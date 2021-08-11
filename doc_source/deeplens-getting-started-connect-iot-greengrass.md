# Using the AWS IoT Greengrass Console to View the Output of Your Custom Trained Model \(Text Output\)<a name="deeplens-getting-started-connect-iot-greengrass"></a>

In this step, you sync your AWS DeepLens video input with AWS IoT Greengrass\.

1. In the navigation pane in the AWS DeepLens console, choose **Devices**\.

1. On the **Devices** page, under **Name** choose your AWS DeepLens device\.

1. On the device page, in the **Project output** section copy the MQTT code needed for the AWS IoT Greengrass console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deploy-model-5-iot-greengrass.png)

1. Choose the **AWS IoT console** noted in step 2, [IoT console](https://console.aws.amazon.com/iotv2/home?region=us-east-1#/test)\.

1. Paste the code you copied from step 3 into the **Subscription topic** text box and then choose **Subscribe to topic**\.

**Next Steps**

Now that you have completed the tutorial, there are several different ways that you can [view the video output of your AWS DeepLens device\.](https://docs.aws.amazon.com/deeplens/latest/dg/deeplens-viewing-output.html)

Video output is split into the following streams:
+  The *device stream* is an unprocessed video stream\.
+ The *output stream* is the result of the processing that the model performs on the video frames\.