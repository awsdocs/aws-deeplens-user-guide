# Set Up Your AWS DeepLens Device<a name="deeplens-getting-started-set-up"></a>

Before getting into the detailed steps, let's have an overview of what's involved in setting up a device\. When you set up your AWS DeepLens device or update the device settings, while your computer is connected to your device's `AMDC-NNNN` network, you'll perform the following tasks: 
+ Setting up the device's network connection to the internet\. This is necessary for the device to be operational\. 
+ Uploading to your AWS DeepLens device the security certificate that the AWS DeepLens service generated for you at the start of [your device registration](deeplens-getting-started-register.md)\. This device certificate is required for the AWS Cloud to authenticate the device and to communicate with the AWS DeepLens service, the AWS Greengrass service, and other AWS services\. 
**Note**  
When reregistering a device, you must upload the newly generated device certificate to override the existing one\.
+ Download the streaming certificate from your device\. You must upload this streaming certificate to each supported web browser in order to [view device output using the web browser](deeplens-viewing-device-output-in-browser.md)\. Without the streaming certificate, you can only [view the device output using `mplayer`](deeplens-viewing-device-output-on-device.md) after logging in to the device\.
+ Create a password for you or an administrator\. You use this password to log in to the device to manage updates and troubleshoot device issues\. Save this password in a secure location\. You might not be able to override this password by deregistering and then reregistering the device\.
+  Enable or disable SSH connection\. Enabling SSH is optional and lets you log into the AWS DeepLensdevice without using a monitor with a micro\-USB cable, a USB mouse and a USB keyboard\. To use SSH to remotely log in to the device, the device must be in the setup mode\. For instructions about how to turn on the setup mode,  see [Connect to Your AWS DeepLens Device's Wi\-Fi Network](deeplens-getting-started-connect.md)\.
+  Enable automatic software updates to keep the latest software and configurations automatically deployed to your device\. The newly deployed software packages and device settings are installed when the device is rebooted\. 

To perform these tasks, you open a web browser on your computer connected to the device's `AMDC-NNNN` network; You then navigate to the device setup application hosted by the web server on your AWS DeepLens device\. 

The following procedure prescribes the step\-by\-step instructions for setting up your device\. Before starting, locate the device certificate \(ZIP\) file\.

You can run the setup on a different computer from the one you use to start the registration and download the device certificate\. In this case, copy the device certificate to the setup computer\.<a name="deeplens-set-up-device-procedure"></a>

**To set up your AWS DeepLens device**

1.  Turn on the device setup mode and connect your computer to the device's `AMDC-NNNN` network\. 
**Note**  
If you're continuing on the same computer with which you went through the steps given in [Register Your AWS DeepLens Device Using the AWS DeepLens Console](deeplens-getting-started-register.md#deeplens-start-registering-device-using-console), your computer should be already connected to the device's Wi\-Fi network\. If you run the setup from a different computer, follow these [instructions](deeplens-getting-started-connect.md) to connect to the device's Wi\-Fi network\.

1.  Open a web browser\. If this is your first time to register your device, go to `http://deeplens.amazon.net` to open the device setup app\. Thereafter, you can open the device setup app by pointing your browser to `http://deeplens.config`\.
**Note**  
If the above URL doesn't work, your AWS DeepLens device may have the `awscam` software version 1\.3\.5 or earlier installed\. In this case, navigate to `http://192.168.0.1`, instead of `http://deeplens.amazon.net` or `http://deeplens.config`\. For more information, see [Device Setup URL](troubleshooting-device-registration.md#troubleshooting-device-registration-8)\.
**Note**  
If you're continuing on the same computer you used to go through the steps given in [Register Your AWS DeepLens Device Using the AWS DeepLens Console](deeplens-getting-started-register.md#deeplens-start-registering-device-using-console), Choose **complete the setup** to open the setup app\.  

![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-device-setup-guide-page.png)

1. On the **Device setup** page, complete the following steps\.

   1. Under **Network connection**, choose **Connect to the network**\. Then, choose the name \(i\.e\., SSID\) and type the password of your home/office Wi\-Fi that'll connect your device to the internet, then choose **Next**\. You can also choose Ethernet to connect your AWS DeepLens device to the internet\.

      If your office or home Wi\-Fi network's SSID or password has special characters, including single quotes, you may be blocked while trying to set up the network connection\. To troubleshoot this issue, see [How to Connect Your Device to Your Home or Office Wi\-Fi Network When the Wi\-Fi SSID or Password Contains Special Characters?](troubleshooting-device-registration.md#troubleshooting-device-wifi-connection)\.

   1. Under **Device certificate**, choose **Upload the device certificate**\. Then, locate and choose the certificate that you downloaded from the AWS DeepLens console when [registering the device](deeplens-getting-started-register.md), then choose **Upload certificate**\.
**Note**  
The downloaded  device certificate is a \.zip file\. Upload the unzipped device certificate file as\-is\.  

   1. Under **Streaming certificate**, choose **Download the streaming certificate**, to save the streaming certificate required to [view device output using a supported web browser](deeplens-viewing-device-output-in-browser.md)\. 
**Note**  
If you delete or lose the saved streaming certificate, you can always come here to download it again\.

   1. Under **Device access**, complete the following steps to configure device access\.

      1. For **Create a password for the device**, type a password\. You need this password to log in to your device to maintain and update your AWS DeepLens device\. You can log in remotely using SSH, if enabled \(below\), or locally using a hardwired monitor, a USB mouse and/or a USB keyboard\.

      1. For **SSH server**, choose **Enable** or **Disable**\. If enabled, SSH allows you to log in to your device using SSH terminal on your Mac or Linux computer or using PuTTY or another SSH client on your Windows computer\.  

      1. For **Automatic updates**, choose **Enable** or **Disable**\. Automatic updates keeps the most recent software deployed to your device\. You will need to reboot the device to have the updates installed\. 

   1. Review the settings and choose **Finish** to finish setting up the device and to terminate your connection to the device's Wi\-Fi network\.

      To change settings, choose **Edit** for the setting that you want to change, and then choose **Finish**\.   
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-device-setup-complete.png)

**Note**  
 To ensure the setup completes successfully, make sure that AWS DeepLens device has access to ports 8883 and 8443 and is not blocked by your network firewall policy\.   
Some user has reported that after choosing **Finish**, the AWS DeepLens device's connection to the internet was repeatedly turned on and off\. This may leave an impression that the registration failed, when it is successful\. The issue is most likely caused by the internet\-bound Wi\-Fi network\. Restart the Wi\-Fi network might resolve the issue\.

This completes the device registration\. You're ready to deploy a project to your AWS DeepLens device when the device becomes both **Registered** and **Online**\. The [next](deeplens-getting-started-verify-connection.md) step shows you how to verify the device status\. 