# Creating an AWS DeepLens Inference Lambda Function<a name="deeplens-inference-lambda-create"></a>

In this topic, you create an inference Lambda function that performs three key functions: preprocessing, inference, and postprocessing\. Each step is accompanied by the associated code\. For the complete function code, see [The Completed Lambda Function](#deeplens-inference-lambda-complete)\.

**To create an AWS DeepLens inference Lambda function**

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**, then choose **Blueprints**\.

1. Choose the **Blueprints** box then from the dropdown, choose **Blueprint name** and type **greengrass\-hello\-world**\.

1. When the *greengrass\-hello\-world* blueprint appears, choose it, then choose **Configure**\.

1. In the **Basic information** section:

   1. Type a name for your Lambda function\.

   1. From the **Role** list, choose **Choose an existing role**\.

   1. Choose **AWSDeepLensLambdaRole**, which you created when you registered your device\.

1. Scroll to the bottom of the page and choose **Create function**\.

1. In the function code box, makes sure that the handler is `greengrassHelloWorld.function_handler`\. The name of the function handler must match the name of the Python script that you want to run\. In this case, you are running the `greengrassHelloWorld.py` script, so the name of the function handler is `greengrassHelloWorld.function_handler`\.

1. Delete all of the code in the `GreengrassHelloWorld.py` box and replace it with the code that you generate in the rest of this procedure\.

1. Add the following code to your Lambda function\.

   1. Import these required packages:

      + `os`—Allows your Lambda function to access the AWS DeepLens operating system\.

      + `awscam`—Allows your Lambda function to use the AWS DeepLens Device Library\. For more information, see [Model](deeplens-device-library-awscam-model.md)\.

      + `mo` — Allows your Lambda function to access the AWS DeepLens model optimizer\. For more information, see [AWS DeepLens Model Optimzer API](deeplens-model-optimizer-api.md)\.

      + `cv2`—Allows your Lambda function to access the Open CV library, which contains tools for image preprocessing\.

      + `thread`—Allows your Lambda function to access Python's multi\-threading library\.

      To import these packages, copy and paste the following code into the `GreengrassHello` file\.

      ```
      import os
      import awscam
      import mo
      import cv2
      from threading import Thread
      ```

   1. Create a Greengrass SDK client\. You will use this client to send messages to the cloud\.

      ```
      client = greengrasssdk.client('iot-data')
      ```

   1. Create an AWS IoT topic for your Lambda function's messages\. You can access this topic in the AWS IoT console\.

      ```
      iot_topic = '$aws/things/{}/infer'.format(os.environ['AWS_IOT_THING_NAME'])
      ```

   1. To view the output locally with mplayer, declare a global variable that contains the \.jpeg image that you send to the FIFO file `results.mjpeg`\.

      ```
      jpeg = None
      Write_To_FIFO = True 
      # making Write_To_FIFO = False kills the thread so you cannot view your output over mplayer
      ```

   1. To publish the output images to the FIFO file and view them with mplayer, create a class that runs on its own thread\.

      ```
      class FIFO_Thread(Thread):
                     def __init__(self):
                        '''Constructor.'''
                        Thread.__init__(self)
                  
                     def run(self):
                        fifo_path = "/tmp/results.mjpeg"
                        if not os.path.exists(fifo_path):
                           os.mkfifo(fifo_path)
                        f = open(fifo_path,'w')
                        client.publish(topic=iot_topic, payload="Opened Pipe")
                  
                        while Write_To_FIFO
                           try:
                              f.write(jpeg.tobytes())
                           except IOError as e:
                              continue
      ```

   1. Define an inference class\.

      1. Define an AWS Greengrass inference function\. `input_width` and `input_height` define the width and height of the input in pixels\. To perform inference, the model expects frames of this size\. You can customize these values for the model that you are deploying to AWS DeepLens\.

         ```
         def greengrass_infinite_infer_run():
         input_width  = 224
         input_height = 224
         ```

      1. Name the model\. The name is the prefix of the trained model's `params` and `json` files\. For example, if the files are named `squeezenet_v1.1-0000.params` and `squeezenet_v1.1-0000.json`, the model name is the prefix `squeezenet_v1.1`\.
**Important**  
The model name must match the prefix\. Otherwise, the model can't perform inference, and generates an error\.

         ```
         model_name = 'squeezenet_v1'
         ```

      1. Initialize the model optimizer\. The model optimizer converts the deployed model to clDNN format, which is accessible to the AWS DeepLens GPU\. The model optimizer returns the path to the post\-optimized artifacts\.

         ```
         error, model_path = mo.optimize(model_name, input_width, input_height)
         ```

      1. Load the model into the inference engine\. To use the CPU, instead of the GPU, specify `"GPU":0`\. The CPU is much less efficient, so we don't recommend using it\.

         ```
         model = awscam.Model(model_path,{"GPU":1})
         # You can send a message to AWS IoT to show that the model is loaded.
         client.publish(topic=iot_topic, payload="Model loaded.")
         ```

      1. Define the type of model that you are running\. The options are:

         + `segmentation`—For neural style transfer\.

         + `ssd`—Single shot detector\. For object localization it includes a definition of the locale in the frame that the object occupies by drawing a bounding box around the object\.

         + `classification`—For image classification\.

         Because you are deploying a SqueezeNet model that classifies images, define the model type as `classification`\.

         ```
         model_type = "classification"
         ```

      1. Map the numeric label generated by the model to a human\-readable label\. Because squeezenet\_v1\.1 has 1,000 classifiers, it's unrealistic to create the mapping in code\. Instead, add a text file to the Lambda \.zip file\. You can then load the labels into a list where the index of the list represents the label returned by the network\.

         ```
         with open('sysnet.txt', 'r') as f:
            labels = [l.rstrip() for l in f]
         ```

      1. Define the number of classifiers that you want to see in the output\.

         ```
         topk = 5
         ```

         The value `5` specifies that the top 5 values with the highest probability are output, in descending order\. You can specify any value as long as it's supported by the model\.

      1. Start the FIFO thread so you can view the output with the mplayer\.

         ```
         results_thread = FIFO_Thread()
            results_thread.start()
            # You can publish an "Inference starting" message to the AWS IoT console.
            client.publish(topic = iot_topic, payload = "Inference starting")
         ```

      1. Get the most recent frame from the AWS DeepLens camera\. If the latest frame is not returned, raise an exception\.

         ```
         ret, frame = awscam.getLastFrame()
         if ret == False:
           raise Exception("Failed to get frame from the stream")
         ```

      1. Preprocess the input frame from the camera by making sure that its dimensions match the dimensions of the frame that the model was trained on\. To resize the input frame, specify the input dimensions defined earlier, `input_width` and `input_height`\. Depending on the model that you trained, you might need to perform other preprocessing steps, such as image normalization\.

         ```
         frame_resize = cv2.resize(frame, (input_width, input_height))
         ```

      1. Perform inference on the resized frame\.

         ```
         infer_output = mdel.doInference(frame_resize)
         ```

      1. Parse the results\.

         ```
         parsed_results = model.parseResult(model_type, infer_output)
         ```

      1. Display only the *n* results that have the highest probability\.

         ```
         top_k = parsed_results[model_type][0:topk]
         ```

      1. Send the results to the cloud\.

         First, put the message in JSON format\. This allows other Lambda functions in the cloud to subscribe to the AWS IoT topic and perform actions when they detect an interesting event\.

         ```
         msg = "{"
            prob_num = 0
            for obj in top_k
               if prob_num == topk-1:
                  msg += '"{}":{:.2f}'.format(labels[obj["label"]],obj["prob"])
               else:
                  msg += '"{}":{:.2f},'.format(labels[obj["label"]], obj["prob"])
               prob_num += 1
            msg += "}"
         ```

         Then send it to the cloud\.

         ```
         client.publish(topic="iot_topic, payload = msg)
         ```

      1. Postprocess the image\. In this case add a line of text to the image: a label of the most likely results\.

         ```
         cv2.putText(frame, labels[top_k[0]["label"]], (0,22), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 165, 20), 4) 
         ```

      1. Update the global jpeg variable so you can view the results with mplayer\.

         ```
         global jpeg
         ret, jpeg = cv2.imencode('.jpg', frame)
         # If you want, you can add exception handling as follows. 
         #    Don’t forget to put the preceding code in a try block.
         except Exception as e:
            msg = "Lambda function failed: " + str(e)
            client.publish(topic=iot_topic, payload = msg)
         ```

      1. Run the function and view the results\.

         ```
         greengrass_infinite_infer_run()
         ```

Make sure that you save and publish the function code\. If you don't, you can't view the inference Lambda function that you just created in the AWS DeepLens console\.

For questions or help, see the AWS DeepLens forum at [Forum: AWS DeepLens](https://forums.aws.amazon.com/forum.jspa?forumID=275&start=0)\.

## The Completed Lambda Function<a name="deeplens-inference-lambda-complete"></a>

The following code creates the Lambda function that allows AWS DeepLens to access deployed models\.

```
# ----------------------------------- 
# Copyright Amazon AWS DeepLens, ©2018
# -----------------------------------
import os                      # access to operating system for AWS DeepLens
import awscam                  # access to AWS DeepLens Device Library
import mo                      # access to AWS DeepLens model optimizer
import cv2                     # access to Open CV library
from threading import Thread   # access to Python's multi-threading library

# create a Greengrasscore SDK client
client = greengrasssdk.client('iot-data')

# create AWS IoT for the Lambda function to send messages
iot_topic = '$aws/things/{}/infer'.format(os.environ['AWS_IOT_THING_NAME'])

# global variable to contain jpeg image
jpeg = None
Write_To_FIFO = True 
# making Write_To_FIFO = False kills the thread so you cannot view your output over mplayer

# create a simple class that runs on its own thread so we can publish output images
#    to the FIFO file and view using mplayer
class FIFO_Thread(Thread):
   def __init__(self):
      '''Constructor.'''
      Thread.__init__(self)
            
   def run(self):
      fifo_path = "/tmp/results.mjpeg"
      if not os.path.exists(fifo_path):
         os.mkfifo(fifo_path)
      f = open(fifo_path,'w')
      client.publish(topic=iot_topic, payload="Opened Pipe")
            
      while Write_To_FIFO
         try:
            f.write(jpeg.tobytes())
         except IOError as e:
            continue

# define inference class within the Lambda function
def greengrass_infinite_infer_run():
   input_width  = 224
   input_height = 224

   # define the name of he model
   model_name = 'squeezenet_v1'

# optimize the model into Cl-DNN format
error, model_path = mo.optimize(model_name, input_width, input_height)

# load the model into the inference engine
model = awscam.Model(model_path,{"GPU":1})
# You can send a message to AWS IoT to show that the model is loaded.
client.publish(topic=iot_topic, payload="Model loaded.")

# define the type of model
#   possibilities are:  
#      segmentation - for neural style transfers 
#      ssd - (single shot detector) for object localization
#      classification - for image classification
model_type = "classification"

# load the labels into a list where the index represents the label returned by the network
with open('sysnet.txt', 'r') as f:
   labels = [l.rstrip() for l in f]

# define the number of classifiers to see
topk = 5

# start the FIFO thread to view the output locally
results_thread = FIFO_Thread()
   results_thread.start()
   # you can publish an "Inference starting" message to the AWS IoT console
   client.publish(topic = iot_topic, payload = "Inference starting")

# access the latest frame on the mjpeg stream
ret, frame = awscam.getLastFrame()
if ret == False:
  raise Exception("Failed to get frame from the stream")

# resize the frame to the size expected by the model
frame_resize = cv2.resize(frame, (input_width, input_height))

# do inference on the frame
infer_output = mdel.doInference(frame_resize)

# parse the results and keep the top topk results
parsed_results = model.parseResult(model_type, infer_output)
top_k = parsed_results[model_type][0:topk]

# format the results as JSON and send to the cloud
msg = "{"
   prob_num = 0
   for obj in top_k
      if prob_num == topk-1:
         msg += '"{}":{:.2f}'.format(labels[obj["label"]],obj["prob"])
      else:
         msg += '"{}":{:.2f},'.format(labels[obj["label"]], obj["prob"])
      prob_num += 1
   msg += "}"
client.publish(topic = iot_topic, payload = msg)

# post-process the image to view it on the mplayer 
#    add a line of text to the image: a label of the most likely results  
cv2.putText(frame, labels[top_k[0]["label"]], (0,22), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 165, 20), 4) 

# define a global variable so results can be viewed using mplayer
global jpeg
ret, jpeg = cv2.imencode('.jpg', frame)
# Catch an exception in case something went wrong
except Exception as e:
   msg = "Lambda function failed: " + str(e)
   client.publish(topic=iot_topic, payload = msg)

# run the function and view the results
greengrass_infinite_infer_run()
```