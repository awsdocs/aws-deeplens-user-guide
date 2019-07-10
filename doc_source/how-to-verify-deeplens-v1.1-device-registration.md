# Step 5: Verify Your AWS DeepLens 2019 Edition Device Registration<a name="how-to-verify-deeplens-v1.1-device-registration"></a>

After the registration is complete, verify the device registration is successful by checking the device's registration status\. 

**To verify your AWS DeepLens 2019 Edition device registration**

1. Wait for the device registration to complete\. Make sure the **Registration status** under **Device Status** shows **Registered**\.   
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/v1.1-device-completed-registration.png)

1. Optionally, note the MQTT topic Id \(e\.g\., **$aws/things/deeplens\_cTsBnnQxT0mMx45HGLJ4DA/infer**\) displayed under **Project Output**\. You'll need this Id to [view a deployed project output in the AWS IoT console](deeplens-viewing-project-output-json.md)\.