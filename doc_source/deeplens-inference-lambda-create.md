# Create and Publish an AWS DeepLens Inference Lambda Function<a name="deeplens-inference-lambda-create"></a>

Besides importing your custom model, you must create and publish an inference Lambda function to make inference of image frames from the video streams captured by your AWS DeepLens device, unless an existing and published Lambda function meet your application requirements\. 

Because the AWS DeepLens device is an [AWS Greengrass core](http://docs.aws.amazon.com/greengrass/latest/developerguide/gg-core.html) device, the inference function is run on the device in a Lambda runtime as part of the AWS Greengrass core software deployed to your AWS DeepLens device\. As such, you create the AWS DeepLens inference function as an AWS Lambda function\. 

To have all the essential AWS Greengrass dependencies included automatically, you can use the Lambda function blueprint of `greengrass-hello-world` as a starting point to create the AWS DeepLens inference function\. 

The engine for AWS DeepLens inference is the [`awscam` module](deeplens-library-awscam-module.md) of the [AWS DeepLens device library](deeplens-device-library.md)\. Models trained in a supported framework must be optimized to run on your AWS DeepLens device\. Unless your model is already optimized, you must use the model optimization module \([`mo`](deeplens-model-optimizer-api.md)\) of the device library to convert framework\-specific model artifacts to the AWS DeepLens\-compliant model artifacts that are optimized for the device hardware\. 

In this topic, you learn how to create an inference Lambda function that performs three key functions: preprocessing, inference, and post processing\. For the complete function code, see [The Complete Inference Lambda Function](#deeplens-inference-lambda-complete)\.

**To create and publish an inference Lambda function for your AWS DeepLens project**

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**, then choose **Blueprints**\.

1. Choose the **Blueprints** box then from the dropdown, choose **Blueprint name** and type **greengrass\-hello\-world**\.

1. When the *greengrass\-hello\-world* blueprint appears, choose it, then choose **Configure**\.

1. In the **Basic information** section:

   1. Type a name for your Lambda function, for example, `deeplens-my-object-inferer`\. The function name must start with `deeplens`\.

   1. From the **Role** list, choose **Choose an existing role**\.

   1. Choose **AWSDeepLensLambdaRole**, which you created when you registered your device\.

1. Scroll to the bottom of the page and choose **Create function**\.

1. In the function code box, make sure that the handler is `greengrassHelloWorld.function_handler`\. The name of the function handler must match the name of the Python script that you want to run\. In this case, you are running the `greengrassHelloWorld.py` script, so the name of the function handler is `greengrassHelloWorld.function_handler`\.

1. Delete all of the code in the `greengrassHelloWorld.py` box and replace it with the code that you generate in the rest of this procedure\.

   Here, we use the Cat and Dog Detection project function as an example\.

1. Follow the steps below to add code for your Lambda function to the code editor  for the `greengrassHelloWorld.py` file\.

   1. Import dependent modules:

      ```
      from threading import Thread, Event
      import os
      import json
      import numpy as np
      import awscam
      import cv2
      import greengrasssdk
      ```

      where,
      + The `os` module allows your Lambda function to access the AWS DeepLens device operating system\.
      + The `json` module lets your Lambda function work with JSON data\.
      + The `awscam` module allows your Lambda function to use the [AWS DeepLens device library](deeplens-device-library.md)\. For more information, see [Model Object](deeplens-device-library-awscam-model.md)\.
      + The `mo` module allows your Lambda function to access the AWS DeepLens model optimizer\. For more information, see [Model Optimization \(mo\) Module](deeplens-model-optimizer-api.md)\.
      + The `cv2` module lets your Lambda function access the [Open CV](https://pypi.org/project/opencv-python/) library used for image preprocessing, including local display of frames from video feeds originating from the device\.
      + The `greengrasssdk` module exposes the AWS Greengrass API for the Lambda function to send messages to the AWS Cloud, including sending operational status and inference results to AWS IoT\.
      + The `threading` module allows your Lambda function to access Python's multi\-threading library\.

   1.  Append the following Python code as a helper class for local display of the inference results:

      ```
      class LocalDisplay(Thread):
          """ Class for facilitating the local display of inference results
              (as images). The class is designed to run on its own thread. In
              particular the class dumps the inference results into a FIFO
              located in the tmp directory (which lambda has access to). The
              results can be rendered using mplayer by typing:
              mplayer -demuxer lavf -lavfdopts format=mjpeg:probesize=32 /tmp/results.mjpeg
          """
          def __init__(self, resolution):
              """ resolution - Desired resolution of the project stream """
              # Initialize the base class, so that the object can run on its own
              # thread.
              super(LocalDisplay, self).__init__()
              # List of valid resolutions
              RESOLUTION = {'1080p' : (1920, 1080), '720p' : (1280, 720), '480p' : (858, 480)}
              if resolution not in RESOLUTION:
                  raise Exception("Invalid resolution")
              self.resolution = RESOLUTION[resolution]
              # Initialize the default image to be a white canvas. Clients
              # will update the image when ready.
              self.frame = cv2.imencode('.jpg', 255*np.ones([640, 480, 3]))[1]
              self.stop_request = Event()
      
          def run(self):
              """ Overridden method that continually dumps images to the desired
                  FIFO file.
              """
              # Path to the FIFO file. The lambda only has permissions to the tmp
              # directory. Pointing to a FIFO file in another directory
              # will cause the lambda to crash.
              result_path = '/tmp/results.mjpeg'
              # Create the FIFO file if it doesn't exist.
              if not os.path.exists(result_path):
                  os.mkfifo(result_path)
              # This call will block until a consumer is available
              with open(result_path, 'w') as fifo_file:
                  while not self.stop_request.isSet():
                      try:
                          # Write the data to the FIFO file. This call will block
                          # meaning the code will come to a halt here until a consumer
                          # is available.
                          fifo_file.write(self.frame.tobytes())
                      except IOError:
                          continue
      
          def set_frame_data(self, frame):
              """ Method updates the image data. This currently encodes the
                  numpy array to jpg but can be modified to support other encodings.
                  frame - Numpy array containing the image data of the next frame
                          in the project stream.
              """
              ret, jpeg = cv2.imencode('.jpg', cv2.resize(frame, self.resolution))
              if not ret:
                  raise Exception('Failed to set frame data')
              self.frame = jpeg
      
          def join(self):
              self.stop_request.set()
      ```

      The helper class \(`LocalDisplay`\) is a subclass of [https://docs.python.org/2.0/lib/thread-objects.html](https://docs.python.org/2.0/lib/thread-objects.html)\. It encapsulates the process to stream processed video frames for local display on the device or using a web browser: 

      1.  Exposes a constructor \(`__init__(self, resolution)`\) to initiate the `LocalDisplay` class with the specified image size \(`resolution`\)\.

      1. Overrides the `run` method, which will be invoked by the `Thread.start` method, to support continuous writing images to the specified file \(`result_path`\) on the device\. The Lambda function can write to files only in the `/tmp` directory\. To view the images in this file \(`/tmp/results.mjpeg`\), start `mplayer` on a device terminal window as follows:

         ```
         mplayer -demuxer lavf -lavfdopts format=mjpeg:probesize=32 /tmp/results.mjpeg
         ```

      1.  Exposes the `set_frame_data` method for the main inference function to call to update image data by encoding a project stream frame to JPG \(`set_frame_data`\)\. The example code can be modified to support other encodings\. 

      1. Exposes the `join` method to turn waiting threads active by setting the [https://docs.python.org/2.0/lib/event-objects.html](https://docs.python.org/2.0/lib/event-objects.html) object's internal flag to true\.

   1.  Append to the code editor the following Python code that initializes looping through the inference logic, frame by frame:

      ```
      def infinite_infer_run():
          """ Entry point of the lambda function"""
          try:
              # This cat-dog model is implemented as binary classifier, since the number
              # of labels is small, create a dictionary that converts the machine
              # labels to human readable labels.
              model_type = 'classification'
              output_map = {0: 'dog', 1: 'cat'}
      
              # Create an IoT client for sending to messages to the cloud.
              client = greengrasssdk.client('iot-data')
              iot_topic = '$aws/things/{}/infer'.format(os.environ['AWS_IOT_THING_NAME'])
      
              # Create a local display instance that will dump the image bytes to a FIFO
              # file that the image can be rendered locally.
              local_display = LocalDisplay('480p')
              local_display.start()
      
              # The sample projects come with optimized artifacts, hence only the artifact
              # path is required.
              model_path = '/opt/awscam/artifacts/mxnet_resnet18-catsvsdogs_FP32_FUSED.xml'
      
              # Load the model onto the GPU.
              client.publish(topic=iot_topic, payload='Loading action cat-dog model')
              model = awscam.Model(model_path, {'GPU': 1})
              client.publish(topic=iot_topic, payload='Cat-Dog model loaded')
      
              # Since this is a binary classifier only retrieve 2 classes.
              num_top_k = 2
      
              # The height and width of the training set images
              input_height = 224
              input_width = 224
      
              # Do inference until the lambda is killed.
              while True:
                  # inference loop to add. See the next step 
                  ...
      
      
          except Exception as ex:
              client.publish(topic=iot_topic, payload='Error in cat-dog lambda: {}'.format(ex))
      ```

      The inference initialization proceeds as follows: 

      1. Specifies the model type \(`model_type`\), model artifact path \(`model_path`\) to load the model artifact \([`awscam.Model`](deeplens-device-library-awscam-model-constructor.md)\), specifying whether the model is loaded into the device's GPU \(`{'GPU':1}`\) or CPU \(`{'CPU':0}`\)\. We don't recommend using the CPU because it is much less efficient\. The model artifacts are deployed to the device in the `/opt/awscam/artifacts` directory\. For the artifact optimized for DeepLens, it consists of an `.xml` file located in this directory\. For the artifact not yet optimized for DeepLens, it consists of a JSON file and a another file with the `.params` extension located in the same directory\. For optimized model artifact file path, set the `model_path` variable to a string literal: 

         ```
         model_path = "/opt/awscam/artifacts/<model-name>.xml"
         ```

         In this example, `<model-name>` is `mxnet_resnet18-catsvsdogs_FP32_FUSED`\. 

         For unoptimized model artifacts, use the [`mo.optimize`](deeplens-model-optimizer-api-functions_and_objects.md) function to optimized the artifacts and to obtain the `model_path` value of a given model name \(`<model_name>`\):

         ```
         error, model_path = mo.optimize(<model_name>, input_width, input_height)
         ```

         Here, the `model_name` value should be the one you specified when [importing the model](deeplens-import-external-trained.md)\. You can also determine the model name by inspecting the S3 bucket for the model artifacts or the local directory of `/opt/awscam/artifacts` on your device\. For example, unoptimized model artifacts output by MXNet consists of a `<model-name>-symbol.json` file and a `<model-name>-0000`\.params file\. If a model artifact consists of the following two files: `hotdog_or_not_model-symbol.json` and `hotdog_or_not_model-0000.params`, you specify `hotdog_or_not_model` as the input `<model_name>` value when calling `mo.optimize`\.

      1. Specifies `input_width` and `input_height` as the width and height in pixels of images used in training\. To ensure meaningful inference, you must convert input image for inference to the same size\. 

      1. Specifies as part of initialization the model type \([`model_type`](deeplens-device-library-awscam-model-parseresult.md)\) and declares the output map \(`output_map`\)\. In this example, the model type is `classification`\. Other model types are `ssd` \(single shot detector\) and `segmentation`\. The output map will be used to map an inference result label from a numerical value to a human readable text\. For binary classifications, there are only two labels \(`0` and `1`\)\. 

         The `num_top_k ` variable refers to the number of inference result of the highest probability\. The value can range from 1 to the maximum number of classifiers\. For binary classification, it can be `1` or `2`\.

      1. Instantiates an AWS Greengrass SDK \(`greengrasssdk`\) to make the inference output available to the AWS Cloud, including sending process info and processed result to an AWS IoT topic \(`iot_topic`\) that provides another means to view your AWS DeepLens project output, although as JSON data, instead of a video stream\.

      1. Starts a thread \(`local_display.start`\) to feed parsed video frames for local display \(`LocalDisplay`\), [on device](deeplens-viewing-device-output-on-device.md#deeplens-viewing-output-project-stream) or [using a web browser](deeplens-viewing-device-output-in-browser.md)\.

   1.  Replace the inference loop placeholder \(`...`\) above with the following code segment:

      ```
                  # Get a frame from the video stream
                  ret, frame = awscam.getLastFrame()
                  if not ret:
                      raise Exception('Failed to get frame from the stream')
                  # Resize frame to the same size as the training set.
                  frame_resize = cv2.resize(frame, (input_height, input_width))
                  # Run the images through the inference engine and parse the results using
                  # the parser API, note it is possible to get the output of doInference
                  # and do the parsing manually, but since it is a classification model,
                  # a simple API is provided.
                  parsed_inference_results = model.parseResult(model_type,
                                                               model.doInference(frame_resize))
                  # Get top k results with highest probabilities
                  top_k = parsed_inference_results[model_type][0:num_top_k]
                  # Add the label of the top result to the frame used by local display.
                  # See https://docs.opencv.org/3.4.1/d6/d6e/group__imgproc__draw.html
                  # for more information about the cv2.putText method.
                  # Method signature: image, text, origin, font face, font scale, color, and tickness
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

      The frame\-by\-frame inference logic flows as follows: 

      1.  Captures a frame from the DeepLens device video feed \([`awscam.getLastFrame()`](deeplens-device-library-awscam-model-get-last-frame.md)\)\. 

      1. Processes the captured input frame \(`cv2.resize(frame, (input_height, input_width))`\) to ensure that its dimensions match the dimensions of the frame that the model was trained on\. Depending on the model training, you might need to perform other preprocessing steps, such as image normalization\.

      1. Performs inference on the frame based on the specified model: `result = model.doInference(frame_resize)`\.

      1. Parses the inference result: `model.parseResult(model_type, result)`\.

      1.  Sends the frame to the local display stream: `local_display.set_frame_data`\. If you want to show the human\-readable label of the most likely category in the local display, add the label to the captured frame: `cv2.putText`\. If not, ignore the last step\. 

      1. Sends the inferred result to IoT: `client.publish`\.

   1. Choose **Save** to save the code you entered\.

   1. From the **Actions** dropdown menu list, choose **Publish new version**\. Publishing the function makes it available in the AWS DeepLens console so that you can add it to your custom project\.

For questions or help, see the AWS DeepLens forum at [Forum: AWS DeepLens](https://forums.aws.amazon.com/forum.jspa?forumID=275&start=0)\.

## The Complete Inference Lambda Function<a name="deeplens-inference-lambda-complete"></a>

The following code shows the complete Lambda function that, when deployed to the AWS DeepLens device, infers video frames captured from the device to be a cat or dog, based on the model serialized in the model file of `mxnet_resnet18-catsvsdogs_FP32_FUSED.xml` under the `/opt/awscam/artifacts/` directory\. 

```
#*****************************************************
#                                                    *
# Copyright 2018 Amazon.com, Inc. or its affiliates. *
# All Rights Reserved.                               *
#                                                    *
#*****************************************************
""" A sample lambda for cat-dog detection"""
from threading import Thread, Event
import os
import json
import numpy as np
import awscam
import cv2
import greengrasssdk

class LocalDisplay(Thread):
    """ Class for facilitating the local display of inference results
        (as images). The class is designed to run on its own thread. In
        particular the class dumps the inference results into a FIFO
        located in the tmp directory (which lambda has access to). The
        results can be rendered using mplayer by typing:
        mplayer -demuxer lavf -lavfdopts format=mjpeg:probesize=32 /tmp/results.mjpeg
    """
    def __init__(self, resolution):
        """ resolution - Desired resolution of the project stream """
        # Initialize the base class, so that the object can run on its own
        # thread.
        super(LocalDisplay, self).__init__()
        # List of valid resolutions
        RESOLUTION = {'1080p' : (1920, 1080), '720p' : (1280, 720), '480p' : (858, 480)}
        if resolution not in RESOLUTION:
            raise Exception("Invalid resolution")
        self.resolution = RESOLUTION[resolution]
        # Initialize the default image to be a white canvas. Clients
        # will update the image when ready.
        self.frame = cv2.imencode('.jpg', 255*np.ones([640, 480, 3]))[1]
        self.stop_request = Event()

    def run(self):
        """ Overridden method that continually dumps images to the desired
            FIFO file.
        """
        # Path to the FIFO file. The lambda only has permissions to the tmp
        # directory. Pointing to a FIFO file in another directory
        # will cause the lambda to crash.
        result_path = '/tmp/results.mjpeg'
        # Create the FIFO file if it doesn't exist.
        if not os.path.exists(result_path):
            os.mkfifo(result_path)
        # This call will block until a consumer is available
        with open(result_path, 'w') as fifo_file:
            while not self.stop_request.isSet():
                try:
                    # Write the data to the FIFO file. This call will block
                    # meaning the code will come to a halt here until a consumer
                    # is available.
                    fifo_file.write(self.frame.tobytes())
                except IOError:
                    continue

    def set_frame_data(self, frame):
        """ Method updates the image data. This currently encodes the
            numpy array to jpg but can be modified to support other encodings.
            frame - Numpy array containing the image data of the next frame
                    in the project stream.
        """
        ret, jpeg = cv2.imencode('.jpg', cv2.resize(frame, self.resolution))
        if not ret:
            raise Exception('Failed to set frame data')
        self.frame = jpeg

    def join(self):
        self.stop_request.set()

def infinite_infer_run():
    """ Entry point of the lambda function"""
    try:
        # This cat-dog model is implemented as binary classifier, since the number
        # of labels is small, create a dictionary that converts the machine
        # labels to human readable labels.
        model_type = 'classification'
        output_map = {0: 'dog', 1: 'cat'}
        # Create an IoT client for sending to messages to the cloud.
        client = greengrasssdk.client('iot-data')
        iot_topic = '$aws/things/{}/infer'.format(os.environ['AWS_IOT_THING_NAME'])
        # Create a local display instance that will dump the image bytes to a FIFO
        # file that the image can be rendered locally.
        local_display = LocalDisplay('480p')
        local_display.start()
        # The sample projects come with optimized artifacts, hence only the artifact
        # path is required.
        model_path = '/opt/awscam/artifacts/mxnet_resnet18-catsvsdogs_FP32_FUSED.xml'
        # Load the model onto the GPU.
        client.publish(topic=iot_topic, payload='Loading action cat-dog model')
        model = awscam.Model(model_path, {'GPU': 1})
        client.publish(topic=iot_topic, payload='Cat-Dog model loaded')
        # Since this is a binary classifier only retrieve 2 classes.
        num_top_k = 2
        # The height and width of the training set images
        input_height = 224
        input_width = 224
        # Do inference until the lambda is killed.
        while True:
            # Get a frame from the video stream
            ret, frame = awscam.getLastFrame()
            if not ret:
                raise Exception('Failed to get frame from the stream')
            # Resize frame to the same size as the training set.
            frame_resize = cv2.resize(frame, (input_height, input_width))
            # Run the images through the inference engine and parse the results using
            # the parser API, note it is possible to get the output of doInference
            # and do the parsing manually, but since it is a classification model,
            # a simple API is provided.
            parsed_inference_results = model.parseResult(model_type,
                                                         model.doInference(frame_resize))
            # Get top k results with highest probabilities
            top_k = parsed_inference_results[model_type][0:num_top_k]
            # Add the label of the top result to the frame used by local display.
            # See https://docs.opencv.org/3.4.1/d6/d6e/group__imgproc__draw.html
            # for more information about the cv2.putText method.
            # Method signature: image, text, origin, font face, font scale, color, and tickness
            cv2.putText(frame, output_map[top_k[0]['label']], (10, 70),
                        cv2.FONT_HERSHEY_SIMPLEX, 3, (255, 165, 20), 8)
            # Set the next frame in the local display stream.
            local_display.set_frame_data(frame)
            # Send the top k results to the IoT console via MQTT
            cloud_output = {}
            for obj in top_k:
                cloud_output[output_map[obj['label']]] = obj['prob']
            client.publish(topic=iot_topic, payload=json.dumps(cloud_output))
    except Exception as ex:
        client.publish(topic=iot_topic, payload='Error in cat-dog lambda: {}'.format(ex))

infinite_infer_run()
```