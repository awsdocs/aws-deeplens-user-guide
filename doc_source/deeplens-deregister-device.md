# Deregister an AWS DeepLens Device<a name="deeplens-deregister-device"></a>

Deregistering your AWS DeepLens disassociates your AWS account and credentials from the device\. Before you deregister your device, delete the photos or videos that are stored on it\.

**To deregister your AWS DeepLens**

1. Open the AWS DeepLens console at [https://console\.aws\.amazon\.com/deeplens/home?region=us\-east\-1\#firstrun/](https://console.aws.amazon.com/deeplens/home?region=us-east-1#firstrun)\.

1. Remove all projects associated with the device\.

   1. Choose **Devices**, then choose the radio button of the device that you want to deregister\.

   1. In **Projects**, choose **Remove projects**\.
**Important**  
Delete the photos or videos that are stored on the device, using SSH and the SSH password that you set when you registered the device to log on to the device\. Navigate to the folder where the photos or videos are stored and delete them\.

1. Deregister the device\.

   1. Choose **Devices**\.

   1. Choose the name of the device you want to deregister, then choose **Deregister**\.

   1. When the warning appears, choose **Deregister**\.

Your AWS DeepLens is now deregistered\. To use it again, repeat each of these steps:

+ [Register Your AWS DeepLens Device](deeplens-getting-started-register.md)

+ [Connect AWS DeepLens to the Network](deeplens-getting-started-connect.md)

+ [Set Up Your AWS DeepLens Device](deeplens-getting-started-set-up.md)

+ [Verify That Your AWS DeepLens Is Connected](deeplens-getting-started-verify-connection.md)