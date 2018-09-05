# Troubleshooting Device Registration Issues<a name="troubleshooting-device-registration"></a>

Use the following guidelines to troubleshoot issues with AWS DeepLens device registration\.

**Topics**
+ [How to Turn your Device Back to its Setup Mode to Complete Device Registration or Update Device Settings?](#troubleshooting-device-registration-1)
+ [How to Connect Your Device to Your Home or Office Wi\-Fi Network When the Wi\-Fi SSID or Password Contains Special Characters?](#troubleshooting-device-wifi-connection)
+ [What IAM Roles to Choose When None Is Available?](#troubleshooting-device-registration-2)
+ [How to Fix an Incomplete Certificate \(\.zip\) File?](#troubleshooting-device-registration-3)
+ [How to Fix an Incorrectly Uploaded Certificate File?](#troubleshooting-device-registration-4)
+ [How to Resolve the Maximum User Limits Exceeded Restriction for Devices, Projects, or Models?](#troubleshooting-device-registration-5)
+ [How to Fix Failed Deregistration of the AWS DeepLens Device?](#troubleshooting-device-registration-6)
+ [How to Resolve Failed Registration of the AWS DeepLens Device?](#troubleshooting-device-registration-7)
+ [How to Open the Device Configuration Page When the Device's Local Address \(192\.168\.0\.1\) Is Not Accessible?](#troubleshooting-device-registration-8)
+ [How to Make the Device's Wi\-Fi Network Visible on Your Computer When the Device Wi\-Fi Indicator Is Blinking or After Your Computer Has Connected to the Device?](#troubleshooting-device-registration-9)
+ [How to Fix an Unsuccessful Device Wi\-Fi Reset?](#troubleshooting-device-registration-10)

## How to Turn your Device Back to its Setup Mode to Complete Device Registration or Update Device Settings?<a name="troubleshooting-device-registration-1"></a>

If you don't see your device's Wi\-Fi network SSID among the available Wi\-Fi networks on your computer and the device's Wi\-Fi indicator \(the middle LED light\) does not blink, you cannot connect to the device to set up the device and to complete the registration\. 

Your device is in its setup mode when the Wi\-Fi indicator \(the middle LED light\) on the front of the device blinks\. When you start registering your device, the device is booted into the setup mode after you power it on\. Only in the setup mode is the device's Wi\-Fi \(`AMDC-NNNN`\) network visible in the list of available Wi\-Fi networks on your computer\. After 30 minutes, the device exits the setup mode, and the Wi\-Fi indicator stays on and stops blinking, 

Your device must be in the setup mode for you to connect your computer to the device, to configure the device to complete the registration or to update the device settings, or to view the output of a deployed project\. If you don't finish the setup within the allotted time, you must reset the device back to its setup mode and then reconnect your computer to the device's Wi\-Fi network\. 

After connecting your computer to the device's Wi\-Fi network, navigate to `http://deeplens.config` from a web browser to open the device configuration page and to complete setting up your device\. For an initial registration, you may want type `http://deeplens.amazon.net` in the browser's address bar to open the configuration page\.

For detailed instructions to connect to your device's Wi\-Fi network, see [Connect to Your AWS DeepLens Device's Wi\-Fi Network](deeplens-getting-started-connect.md)\.

## How to Connect Your Device to Your Home or Office Wi\-Fi Network When the Wi\-Fi SSID or Password Contains Special Characters?<a name="troubleshooting-device-wifi-connection"></a>

 If your home or office network Wi\-Fi name \(SSID\) or password contains special characters \(including single quotes, back slashes, or white spaces\), you might not be able to connect your AWS DeepLens device to the home or office Wi\-Fi network when setting up your device on the device configuration page \(`http://deeplens.amazon.net` or `http://deeplens.config`\)\.  

To work around this, follow the steps below to use SSH commands to log into the device and connect it to your home or office Wi\-Fi network\. 

**To connect your DeepLens device to your home or office Wi\-Fi network when the Wi\-Fi SSID and password contains special characters**

1. Connect to your device's Wi\-Fi network\. 

   1. Power on your DeepLens and press the power button on the front to turn on the device\. Skip this step if the device is already turned on\.

   1. Wait until the device enters into the setup mode when the Wi\-Fi indicator on the front of the device starts to flash\. If you skipped the previous step, you can turn the device into its setup mode by pressing a paper clip into the reset pinhole on the back of the device and, after feeling a click, wait for about 20 seconds for the Wi\-Fi indicator to blink\.

   1. Connect your computer to the device's Wi\-Fi network by selecting the device Wi\-Fi network SSID \(of the `AMDC-NNNN` format\) from the list of available networks and type the network password\. The SSID and password are printed on the bottom of your device\. On a Windows computer, choose **Connecting using a security key instead** when prompted for **Enter the PIN from the router label \(usually 8 digits\)**\.

1.  After the Wi\-Fi connection is established, open a web browser on your computer and type `http://deeplens.amazon.net/#deviceAccess` in the address bar\.

1. In the **Configure device access** page, choose **Enable** under **SSH server**\. Create a device password if you haven't already done so\. Choose **Save**\.
**Important**  
 After choosing **Save** do not choose **Finish**\. Otherwise, you'll be disconnected from the device and can't proceed in the following step\.  
If you've chosen **Finish**, follow **Step 1\-3** above to get back to the device's Wi\-Fi network before continuing to the next step\.

1. While your computer is connected to the device \(`AMDC-NNNN`\) network Wi\-Fi connection, open a terminal on your computer and run the following SSH command to log in to your device:

   ```
   ssh aws_cam@deeplens.amazon.net
   ```

   Type the device password\. 
**Note**  
 On a computer running Windows, you can use PuTTY or a similar SSH client to execute the SSH command\.

1. After you have logged in, run the following shell command in the terminal:

   ```
   sudo nmcli device wifi con '<wifi_ssid>' password '<wifi_password>' ifname mlan0
   ```

   Replace `<wifi_ssid>` and `<wifi_password>` with the SSID and the password for the Wi\-Fi connection, respectively\. 

   You must enclose the SSID and password in single quotes\. If the SSID or password contains a single quote, escape each instance by prefixing a back slash \(`\`\) and enclosing them within a pair of single quotes\. For example, if a single quote appears in the original `<wifi_ssid>` as `Jane's Home Network`, the escaped version is `Jane'\''s Home Network`\. 

   After the command has run, verify that your device is connected to the internet by pinging any public website\.

1. Go to `http://deeplens.amazon.net` or `http://deeplens.config` on your browser to continue setting up the device, as prescribed by [**Step 3\.b** in Set Up Your Device](deeplens-getting-started-set-up.md#deeplens-set-up-device-procedure)\.  

   When prompted, update the device software to version 1\.3\.10 or higher\. The update ensures that your AWS DeepLens device can connect to your home or office Wi\-Fi network from the AWS DeepLens console so you won't have to repeat this workaround\. 

## What IAM Roles to Choose When None Is Available?<a name="troubleshooting-device-registration-2"></a>

If [the required IAM roles](deeplens-required-iam-roles.md) are not available in your account when you register your device using the AWS DeepLens console for the first, you will be prompted to create the roles with a single click of button\. If you wish to create the required IAM roles  without using the AWS DeepLens console, follow the instructions in [Create IAM Roles for Your AWS DeepLens Project](deeplens-required-iam-roles.md#deeplens-required-iam-role-create)\.

## How to Fix an Incomplete Certificate \(\.zip\) File?<a name="troubleshooting-device-registration-3"></a>

If you download a certificate \.zip file and forget to choose the **Finish** button, you get an incomplete and, therefore, unusable certificate file\. To fix this, delete the downloaded zip file and reregister your device\.

## How to Fix an Incorrectly Uploaded Certificate File?<a name="troubleshooting-device-registration-4"></a>

If you accidentally uploaded an incorrect certificate \(\.zip\) file, upload the correct certificate \(\.zip\) file to overwrite the incorrect one\.

## How to Resolve the Maximum User Limits Exceeded Restriction for Devices, Projects, or Models?<a name="troubleshooting-device-registration-5"></a>

Deregister the device, delete a project, or delete a model before adding new one\.

## How to Fix Failed Deregistration of the AWS DeepLens Device?<a name="troubleshooting-device-registration-6"></a>

 Stop deregistration and reregister the device\. 

**To stop deregistering and then reregister an AWS DeepLens device**

1. Turn the device back to its setup mode by inserting a small paper clip into the reset pinhole at the back of the device\.

1. Reregister the device with a new device name\. 

1. Upload the new certificate to replace the old one\.

## How to Resolve Failed Registration of the AWS DeepLens Device?<a name="troubleshooting-device-registration-7"></a>

Device registration often fails because you have incorrect or insufficient permissions\. Make sure that the correct IAM roles and permissions are set for the AWS services you use\. 

For example, when setting the permissions in the AWS DeepLens console, you must already have created an IAM role with the required permissions for AWS Greengrass and associated this role with the **IAM role for AWS Greengrass** option\. If registration fails, make sure that you've created and specified the correct roles, then reregister the device\.

Specifying an existing Lambda role for the **IAM role for AWS Lambda** can also cause registration to fail if the role name doesn't start with `AWSDeepLens`\. Specifying a role whose name doesn't begin with `AWSDeepLens` doesn't affect the deployment of Lambda functions\.

To set up the roles correctly, follow the instructions in [Register Your AWS DeepLens Device](deeplens-getting-started-register.md) and check with the AWS DeepLens console tips for each role\.

## How to Open the Device Configuration Page When the Device's Local Address \(192\.168\.0\.1\) Is Not Accessible?<a name="troubleshooting-device-registration-8"></a>

If your AWS DeepLens device is running software \(`awscam`\) version 1\.3\.6 or later, open the device configuration page by navigating to `http://deeplens.amazon.net` \(the first time you register the device\) or `http://deeplens.config` in a browser window on your computer\. Your computer must be [connected to the device's \(`AMDC-nnnn`\) Wi\-Fi network](deeplens-getting-started-connect.md)\.

If your device is running an earlier version of the `awscam` package, update o the latest version by running the following commands from a terminal on the device:

```
sudo apt update
sudo apt install awscam
```

If you don't want to update the `awscam` package to the latest version yet, navigate to `http://192.168.0.1` to launch the device setup app\. If your home or office Wi\-Fi network router also uses `192.168.0.1` as the default IP address, reset the router's IP address, for example, to `192.168.1.0`\. For instructions on resetting your router's IP address, see the documentation for your router\. For example, to change a D\-Link router IP address, see [ How do I change the IP Address of my router?](http://www.dlink.com/uk/en/support/faq/routers/mydlink-routers/dir-605l/how-do-i-change-the-ip-address-of-my-router)\. 

If your device's Wi\-Fi network is unresponsive, reset the device and connect to it again\. To reset the device, press a pin on the **RESET** button on the back of the device, and wait until the Wi\-Fi indicator \(on the front of the device\) blinks\. 

## How to Make the Device's Wi\-Fi Network Visible on Your Computer When the Device Wi\-Fi Indicator Is Blinking or After Your Computer Has Connected to the Device?<a name="troubleshooting-device-registration-9"></a>

When you can't see your device's Wi\-Fi network on your laptop even though the middle LED light on the device is blinking or if your computer is still connected to the device Wi\-Fi network after the middle LED light stops blinking, refresh the available Wi\-Fi networks on your computer\. Usually, this involves turning off the laptop Wi\-Fi and turning it back on\. 

It takes up to 2 minutes to reboot a device and for  the device Wi\-Fi network to be ready to accept connection requests\.

## How to Fix an Unsuccessful Device Wi\-Fi Reset?<a name="troubleshooting-device-registration-10"></a>

If the device's Wi\-Fi indicator \(middle LED light\) isn't blinking after you reset the device by pressing a pin into the reset pinhole on the back of it, connect the device to the internet with an Ethernet cable, connect the device to a monitor\. a keyboard and a mouse, log in to the device, open a terminal window, and run the following commands to reset the `awscam` package:

```
sudo systemctl status softap.service
```

If the command returns error code 203, reinstall `awscam-webserver` by running the following commands on your AWS DeepLens device:

```
sudo apt-get install --reinstall awscam-webserver
sudo reboot
```

The successful reboot sets up the device's Wi\-Fi network, and the middle LED light on the device will start to blink\.