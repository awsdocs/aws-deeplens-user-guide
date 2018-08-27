# Update Your AWS DeepLens Device<a name="deeplens-manual-updates"></a>

When you set up your device, you had the option to enable automatic updates \(see [Set Up Your AWS DeepLens Device](deeplens-getting-started-set-up.md)\)\. If you enabled automatic updates, you need only to reboot the device to get the software updated on your device\. If you didn't enable automatic updates, you need to manually update your device periodically\.

**To manually update your AWS DeepLens on the device**

1. Plug in your AWS DeepLens and turn it on\.

1. Use a micro HDMI cable to connect your AWS DeepLens to a monitor\. 

1. Connect a USB mouse and keyboard to your AWS DeepLens\.

1. When the login screen appears, sign in to the device using the SSH password you set when you registered it\.

1. Start your terminal and run each of the following commands:

   ```
   sudo apt-get update
   sudo apt-get install awscam
   sudo reboot
   ```

**To manually update your AWS DeepLens using an SSH terminal**

1. Find your IP address by either logging into Ubuntu\. or looking at your Wi\-Fi router\. 

1. Start a terminal and type:

   ```
   ssh aws_cam@IP-address
   ```

1. Run each of the following commands:

   ```
   sudo apt-get update
   sudo apt-get install awscam
   sudo reboot
   ```