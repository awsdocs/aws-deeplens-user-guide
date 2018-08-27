# Verify Your AWS DeepLens Device Registration Status<a name="deeplens-getting-started-verify-connection"></a>

After you finish setting up your device, your computer automatically disconnects from your device's Wi\-Fi network and reconnects to the internet through your home or office network\. This can take a few seconds\. 

When the setup succeeds, the device becomes registered\. You can verify these by choosing **Devices** from the main navigation pane on the AWS DeepLens console, choosing the device you just registered, and inspecting the **Registration status** and **Device status** values in the **Device details** card\. 

Before running the setup, the registration status is **Unregistered** or **Waiting for credentials**\. After the setup, the registration status becomes **Registered** and the device status becomes **Online**\.

![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-device-details-page.png)

When **Registration status** shows **Registered** and **Device status** shows **Online**, your AWS DeepLens device is ready\. You can then proceed to create and deploy an AWS DeepLens project on to the device\.  To do this, follow the instructions given in [Create and Deploy an AWS DeepLens Project](deeplens-create-deploy-project.md)\. For more information, see [Working with AWS DeepLens Projects](deeplens-projects.md)\. 

If your device is not online, verify that the device is connected to the internet and the device certificate is the one you downloaded when the device is registered\. Then, return to [Connect to Your AWS DeepLens Device's Wi\-Fi Network](deeplens-getting-started-connect.md) and repeat the steps for setting up your device and connecting it to the network\.