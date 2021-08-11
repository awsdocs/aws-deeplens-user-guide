# `awscam` Module for Inference<a name="deeplens-library-awscam-module"></a>

Use the `awscam` module to grab video frames from your AWS DeepLens device and load a deep learning model into the inference engine of the device\. Use the model to run inference on captured image frames\. 

When you create an AWS Lambda inference function to use on your AWS DeepLens project, you use the `awscam` module\. The `awscam` module requires the `cv2` module\. The `cv2` module has a dependency on the `awscam` module, so you must import the `awscam` module before importing the `cv2` module\. 

**Topics**
+ [`awscam.getLastFrame` Function](deeplens-device-library-awscam-model-get-last-frame.md)
+ [Model Object](deeplens-device-library-awscam-model.md)