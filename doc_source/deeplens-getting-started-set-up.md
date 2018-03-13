# Set Up Your AWS DeepLens Device<a name="deeplens-getting-started-set-up"></a>

Use your computer to set up your AWS DeepLens device\.

**To set up your device**

1. In a browser, open a new tab and navigate to [http://192\.168\.0\.1](http://192.168.0.1)\.

1. On the **Device** page:

   1. Connect to the network\.

      Choose your local network, type the password, then choose **Next**\. If you are using Ethernet to connect to AWS DeepLens, choose the Ethernet option\.

   1. Upload the certificate\.

      Locate and choose the certificate that you downloaded from the AWS DeepLens console, then choose **Upload certificate**\.

      The certificate is saved as a \.zip file in your `Downloads` directory\. Don't unzip the file\. You attach the certificate as a \.zip file\.

   1. Configure device access\.

      1. **Create a password for the device**—You need this password to access and update your AWS DeepLens\.

      1. SSH server—We recommend disabling SSH\. SSH allows you to log in without using the AWS DeepLens console\.

      1. **Automatic updates**—We recommend enabling this option\. Enabling automatic updates keeps your device's software up\-to\-date\.

   1. Review the settings and finish setting up the device\. 

      To modify settings, choose **Edit** for the setting that you want to change\. 

      Choose **Finish**\.