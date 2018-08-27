# View Your AWS DeepLens Project Output in the AWS IoT Console<a name="deeplens-viewing-project-output-json"></a>

1. Open the AWS DeepLens console and make sure to choose the **US N\. Virginia** region\.

1.  From the AWS DeepLens primary navigation pane, choose **Devices** and then choose your AWS DeepLens project\.

1.  From the **Device details** page of selected device, copy the **MQTT** topic value, of the `$aws/things/deeplens_<uuid>/infer` format\. You will need to paste this AWS IoT topic ID in the AWS IoT console that you'll open next\.

1. Open the [AWS Management Console for IoT Core](https://console.aws.amazon.com/iot/home?region=us-east-1#/home) and make sure to choose **US North Virginia** region\.

1.  From the primary navigation pane, choose **Test**\.

1.  Under **Subscription topic** on the **MQTT client** page, paste the AWS IoT topic ID \(coped in **Step 3** above\) into the **Subscription topic** input field\. Then, choose **Subscribe to topic**\.

1. Inspect the display of the results that your inference Lambda function publishes by calling the `greengrasssdk.publish` method\. The following AWS IoT console display shows example results published by the [Cat\-and\-dog\-recognition sample project function](deeplens-inference-lambda-create.md):  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-view-project-output-json-result.png)