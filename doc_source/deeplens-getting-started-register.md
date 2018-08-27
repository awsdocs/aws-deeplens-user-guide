# Register Your AWS DeepLens Device<a name="deeplens-getting-started-register"></a>

Before using your AWS DeepLens device for your deep learning computer vision application, you need to register the device with the AWS DeepLens service\. The registration allows you to manage your AWS DeepLens device, including creating and deploying a project and updating the device software, through the AWS Cloud\. 

The registration process involves the following major tasks to be performed on the cloud and on the device, respectively: 
+ Call the AWS DeepLens service to create an identity, to set up access permissions, and to generate a certificate for the device\. You use the AWS DeepLens console to perform this task\.
+ Open a setup application on the device to configure device access to the internet, to upload the certificate \(generated in Task 1\) for the AWS Cloud to authenticate the device, to download the streaming certificate to view device output in a web browser, and, if desired, to create a password for logging into the device\.

In this section, you learn how to perform the first task\. The next two sections will walk you through the steps for the second task\. Before registering your device, make sure that you have completed the prerequisites discussed in [Set Up Your AWS DeepLens Development Environment](deeplens-prerequisites.md)\. 

If you're a visual learner, you may want to check out this [YouTube video](https://youtu.be/X8kP91ADXZ8) 

**Topics**
+ [Register Your AWS DeepLens Device Using the AWS DeepLens Console](#deeplens-start-registering-device-using-console)

## Register Your AWS DeepLens Device Using the AWS DeepLens Console<a name="deeplens-start-registering-device-using-console"></a>

Follow the steps below to start registering your AWS DeepLens device\. If you recycle a device from another user, make sure that the previous user has deregistered the device before registering it again\. 

**To start registering your AWS DeepLens device using the AWS DeepLens console**

1. Sign in to the AWS Management Console for AWS DeepLens at [https://console\.aws\.amazon\.com/deeplens/home?region=us\-east\-1\#firstrun](https://console.aws.amazon.com/deeplens/home?region=us-east-1#firstrun)\.

1. Choose **Register device**\. If you don't see a **Register device** button, choose **Devices** on the main navigation pane\.

1. On the **Name device** page, for **Device name**, type a name for your AWS DeepLens device, and choose **Next**\.

   The device name can have up to 100 characters\. Valid characters are a\-z, A\-Z, 0\-9, and \- \(hyphen\) only\.   
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-registration-name-device.png)

1. On the **Set permissions** page, do the following: 

   1. If this is your first time registering an AWS DeepLens device and you haven't created the [required IAM roles](deeplens-prerequisites.md#deeplens-required-iam-roles) for AWS DeepLens and the AWS services that AWS DeepLens uses in your account, choose **Create roles** for the AWS DeepLens console to create the specified IAM roles for you\. In subsequent registrations or if you have already [created the required IAM roles](deeplens-prerequisites.md#deeplens-required-iam-role-create), you won't be asked to create the roles any more\. You can simply choose **Next** if you don't need to customize device permissions\.  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-registration-set-permissions.png)

   1. This step is optional\. Depending on your project's requirement, override the default **AWSDeepLensGreengrassGroupRole** IAM role to customize the permissions needed to execute Lambda functions on your device\. For a typical first\-time registration, this step is needed and the default device permissions are sufficient\.
**Note**  
 The default role grants permissions for the device Lambda function to access Amazon S3, CloudWatch Logs, AWS DeepLens and Kinesis Video Streams\. To deny permissions to any of the default AWS services or grant permissions to additional AWS services, choose **Customize device configuration**, and then do one of the following:   
If you've already created a customized IAM role of the **AWSDeepLensGreengrassGroupRole** type with customized permissions attached to, choose the role from the drop\-down list\.
Otherwise, choose** Create a role in IAM** to open the IAM console\. Follow the on\-screen instructions to create an IAM role of the **Lambda** type and attach a policy to the role, using the default [https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn:aws:iam::aws:policy/AWSDeepLensLambdaFunctionAccessPolicy$jsonEditor](https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn:aws:iam::aws:policy/AWSDeepLensLambdaFunctionAccessPolicy$jsonEditor) as a template\.
For more information about creating or customizing the **AWSDeepLensGreengrassGroupRole**, see [Create **AWSDeepLensGreengrassGroupRole** Using the IAM Console](deeplens-prerequisites.md#deeplens-required-iam-roles-create-greengrass-group)\.  

![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-register-device-customize-device-configuration.png)

1. Choose **Next**\.

1.  On the **Download certificate** page, choose **Download certificate** to save the device certificate in the default or a selected directory\. 
**Important**  
The downloaded device certificate is a \.zip file\. Don’t unzip it\.   
Certificates aren't reusable\. You must generate a new certificate every time you register \(or register\) your device\.

1. After downloading the certificate, choose **Continue** to start setting up the device\. 

To set up the device, you must first [connect your computer to your device's Wi\-Fi network](deeplens-getting-started-connect.md) and then open a web browser to start the device setup application hosted on the device\. 