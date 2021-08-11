# Create and Publish an AWS DeepLens Inference Lambda Function<a name="deeplens-inference-lambda-create"></a>

This topic explains how to add an AWS Lambda inference function to your custom AWS DeepLens project\. Inference is the step where the model is shown images it has never seen before and asked to make a prediction\. The inference function optimizes the model to run on AWS DeepLens and feeds each camera frame into the model to get predictions\. For the inference function, you use [AWS Lambda](http://aws.amazon.com/lambda) to create a function that you deploy to AWS DeepLens\. The inference function runs locally on the AWS DeepLens device over each frame that comes out of the camera\. 

AWS DeepLens supports Lambda inference funtions written in Python 2\.7 and Python 3\.7\. Python 3\.7 requires requires device software v1\.4\.5\. You will write the Lambda function in the browser and then deploy it to run on AWS DeepLens\. 

**To create and publish an inference Lambda function for your AWS DeepLens project**

1. Download the [AWS DeepLens inference function template](samples/deeplens_inference_function_template.zip) to your computer\. Do not unzip the downloaded file\.

   The zip file contains the following:
   +  `lambda_function.py` — this file contains the Lambda function\. AWS DeepLens will run this file after your AWS DeepLens project has been deployed\. 
   +  `local_display.py` — this file contains a helper class to write the modified frames from the camera into the project video stream\. 
   +  `greengrasssdk` — this folder contains the AWS IoT Greengrass SDK\. It is used to send prediction results back to the cloud or to another IoT device\. 

    You will be modifying `lambda_function.py` to pull frames from the camera, perform inference, and send predictions back through AWS IoT Greengrass\. 

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**, then choose **Author from Scratch**\.

1. In the **Basic information** section:

   1. Under **Function name**, type a name for your Lambda function, for example, `deeplens-object-detection`\. 

      The function name must start with `deeplens`\.

   1. Under **Runtime**, choose **Python 2\.7** or **Python 3\.7**\. 

   1. Under **Permissions**, expand **Choose or create an execution role**, if it's not already expanded\.

   1. Under **Execution role**, choose **Use an existing role**\.

   1. From the **Existing role** dropdown list, choose **service\-role/AWSDeepLensLambdaRole**, which was created when you registered your device\.

1. Scroll to the bottom of the page and choose **Create function**\.

1. In the **Function code** section, do the following: 

   1. Under **Actions**, choose **Upload a \.zip file**\.

   1. Under **Function package**, choose **Upload**, and then select `deeplens_inference_function_template.zip`\. 

1. Choose **Save** \(on the upper\-right corner of the Lambda console\) to load the basic Lambda function into the code editor\.

1. In the Lambda code editor, open `lambda_function.py`:
**Preserve import dependency**  
The `cv2` library has a dependency on the `awscam` module\. If you modify the `lambda_function.py` template, be sure that it continues to import the `awscam` module before the `cv2` library\. 

   ```
   import json
   import awscam
   import mo
   import cv2
   import greengrasssdk
   import os
   from local_display import LocalDisplay
   
   def lambda_handler(event, context):
       """Empty entry point to the Lambda function invoked from the edge."""
       return
   
   def infinite_infer_run():
       """ Run the DeepLens inference loop frame by frame"""
       # Load the model here
       while True:
           # Get a frame from the video stream
           ret, frame = awscam.getLastFrame()
           # Do inference with the model here
           # Send results back to IoT or output to video stream
   infinite_infer_run()
   ```

   Let's examine this code line by line\. 
   + The `json` module lets your Lambda function work with JSON data and send results back through AWS IoT\.
   + The `awscam` module allows your Lambda function to use the [AWS DeepLens device library](deeplens-device-library.md) to grab camera frames, perform inference with an optimized model and parse inference results\. For more information, see [Model Object](deeplens-device-library-awscam-model.md)\. 
   + The `mo` module allows your Lambda function to access the AWS DeepLens model optimizer\. It optimizes the model trained in the cloud to run efficiently on the DeepLens GPU\. For more information, see [Model Optimization \(mo\) Module](deeplens-model-optimizer-api.md)\. 
   + The `cv2` module lets your Lambda function access the [Open CV](https://pypi.org/project/opencv-python/) library used for image preprocessing\. 
   + The `greengrasssdk` module exposes the AWS IoT Greengrass API for the Lambda function to send messages to the AWS Cloud, including sending operational status and inference results to AWS IoT\.
   + The `local_display` file contains a helper class to write to the project video stream\. This helps you draw bounding boxes or text directly onto the DeepLens video stream\. 
   + The `lambda_handler` is typically where you put the code for Lambda functions that are invoked once and then stopped\. We want the Lambda function on AWS DeepLens to run and process frames continuously\. We will leave `lambda_handler` empty and define another function `infinite_infer_run` that can run forever\. 
   + The `infinite_infer_run` function contains a `while` loop that runs forever\. It pulls uses `awscam.getLastFrame` to pull frames from the camera on each iteration\. 

1. In this step, you add code to load the inference model and pass the camera frame to it to get predictions\. For this example, we assume that you have trained a model to differentiate between a dog and a cat\. 

   In the Lambda code editor, under the comment `# Load the model here`, add the following:

   ```
   # Model details
   input_width = 224
   input_height = 224
   model_name = 'image-classification'
   model_type = 'classification'
   output_map = {0: 'dog', 1: 'cat'}
   
   # Optimize the model
   error, model_path = mo.optimize(model_name,input_width,input_height)
   
   # Load the model onto the GPU.
   model = awscam.Model(model_path, {'GPU': 1})
   ```

   AWS DeepLens uses the Intel OpenVino model optimizer to optimize the model trained in the cloud\. `model_name` is the name of the model file you want to load\. For models trained with Amazon SageMaker, the model typically has 2 files: a `<model_name>-symbol.json` file and a `<model_name>-###.params` file\. 

   `input_width` and `input_height` refer to the size of the images used to train your network\. During inference, the same\-sized image is passed in\. 

   `model_type` will tell the `awscam` package how to parse the results\. Other model types include `ssd` \(single shot detector\) and `segmentation`\. 

   `output_map` helps turn the integer results from the model back to human\-readable labels\.

1. In this step, you add code to pass camera frames through the model to get predictions\.

   In the Lambda code editor, under the comment `# Do inference with the model here`, add the following:

   ```
   frame_resize = cv2.resize(frame, (input_height, input_width))
   predictions = model.doInference(frame_resize)
   parsed_inference_results = model.parseResult(model_type, predictions)
   ```

   In this code, we resize the camera frame to be the same size as the image inputs used to trained the model\. Next, we perform inference with the model on this resized frame\. Finally, use the `awscam parseResult` to turn the predictions into a dictionary that can be easily parsed\. For more information about the output of `parseResult`, see [model\.parseResult](https://docs.aws.amazon.com/deeplens/latest/dg/deeplens-device-library-awscam-model-parseresult.html)\. 

1. In this step, you add code to send the results back to AWS IoT\. This makes it easy to record the results in a database or trigger an action based on the predictions\. 

   In the Lambda code editor, in the function `infinite_infer_run`, under `""" Run the DeepLens inference loop frame by frame"""`, add the following:

   ```
   # Create an IoT client for sending to messages to the cloud.
   client = greengrasssdk.client('iot-data')
   iot_topic = '$aws/things/{}/infer'.format(os.environ['AWS_IOT_THING_NAME'])
   
   # Create a local display instance that will dump the image bytes to a FIFO
   # file that the image can be rendered locally.
   local_display = LocalDisplay('480p')
   local_display.start()
   ```

   This code performs two tasks: 
   + Instantiates an AWS IoT Greengrass SDK \(`greengrasssdk`\) to make the inference output available to the AWS Cloud\. This includes sending process info and processed results to an AWS IoT topic \(`iot_topic`\)\. The topic lets you view your AWS DeepLens output as JSON data instead of a video stream\.
   + Starts a thread \(`local_display.start`\) to feed parsed video frames for local display \(`LocalDisplay`\)\. The display can be [on device](deeplens-viewing-device-output-on-device.md) or in a [web browser](deeplens-viewing-device-output-in-browser.md)\. 

   Add the following code after `# Send results back to IoT or output to video stream` to send the results back frame\-by\-frame:

   ```
   # Add the label of the top result to the frame used by local display.
   cv2.putText(frame, output_map[top_k[0]['label']], (10, 70),
   cv2.FONT_HERSHEY_SIMPLEX, 3, (255, 165, 20), 8)
   # Set the next frame in the local display stream.
   local_display.set_frame_data(frame)
   # Send the top k results to the IoT console via MQTT
   cloud_output = {}
   for obj in top_k:
   	cloud_output[output_map[obj['label']]] = obj['prob']
   client.publish(topic=iot_topic, payload=json.dumps(cloud_output))
   ```

1. Choose **Save** to save the updated Lambda function\. Your function should look like the following:

   ```
   import json
   import awscam
   import mo
   import cv2
   import greengrasssdk
   import os
   from local_display import LocalDisplay
   
   def lambda_handler(event, context):
       """Empty entry point to the Lambda function invoked from the edge."""
       return
   
   def infinite_infer_run():
       """ Run the DeepLens inference loop frame by frame"""
       # Create an IoT client for sending to messages to the cloud.
       client = greengrasssdk.client('iot-data')
       iot_topic = '$aws/things/{}/infer'.format(os.environ['AWS_IOT_THING_NAME'])
   
       # Create a local display instance that will dump the image bytes to a FIFO
       # file that the image can be rendered locally.
       local_display = LocalDisplay('480p')
       local_display.start()
       
       # Load the model here
       # Model details
       input_width = 224
       input_height = 224
       model_name = 'image-classification'
       model_type = 'classification'
       output_map = {0: 'dog', 1: 'cat'}
       
       # Optimize the model
       error, model_path = mo.optimize(model_name,input_width,input_height)
       # Load the model onto the GPU.
       model = awscam.Model(model_path, {'GPU': 1})
       while True:
           # Get a frame from the video stream
           ret, frame = awscam.getLastFrame()
           # Do inference with the model here
           # Resize frame to the same size as the training set.
           frame_resize = cv2.resize(frame, (input_height, input_width))
           predictions = model.doInference(frame_resize)
           parsed_inference_results = model.parseResult(model_type, predictions)
   
           k = 2
           # Get top k results with highest probabilities
           top_k = parsed_inference_results[model_type][0:k]
           print(top_k)
           # Send results back to IoT or output to video stream
           # Add the label of the top result to the frame used by local display.
           cv2.putText(frame, output_map[top_k[0]['label']], (10, 70),
           cv2.FONT_HERSHEY_SIMPLEX, 3, (255, 165, 20), 8)
           # Set the next frame in the local display stream.
           local_display.set_frame_data(frame)
           # Send the top k results to the IoT console via MQTT
           cloud_output = {}
           for obj in top_k:
               cloud_output[output_map[obj['label']]] = obj['prob']
           client.publish(topic=iot_topic, payload=json.dumps(cloud_output))
   infinite_infer_run()
   ```

1. From the **Actions** dropdown menu list, choose **Publish new version**\. When you publish a function, it becomes available in the AWS DeepLens console and available to add to your custom project\. 