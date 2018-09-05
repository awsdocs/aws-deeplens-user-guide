# Connect to Your AWS DeepLens Device's Wi\-Fi Network<a name="deeplens-getting-started-connect"></a>

To set up your AWS DeepLens device, you must first connect your computer to the device's local Wi\-Fi network, also known as the device's `AMDC-NNNN` network\. When the Wi\-Fi indicator \(the middle LED light\) blinks on the front of the device, this network is active and the device is in setup mode\. 

When you set up your device for the first time, the device is automatically booted into setup mode and ready for your computer to join its `AMDC-NNNN` network\. To update device settings after the initial setup, you must explicitly turn on the device setup mode \(instructions given below\) and then have your computer rejoin the device's Wi\-Fi network\. If the device exits setup mode before you can finish the setup, you must reconnect your computer to the device in the same way\.

While your AWS DeepLens device is in its setup mode and your computer is a member of its Wi\-Fi network, you can open the device setup application, an HTML page hosted on the device's local web server, to configure the device settings\. The device remains in setup mode for up to 2 hours, giving you enough time to finish configuring the device\. When the setup is complete or has exceeded 2 hours, the device will exit setup mode and the Wi\-Fi indicator stops blinking\. The `AMDC-NNNN` network is then deactivated and your computer disconnected from that network and reconnected to its home or office network\. 

**To connect to your AWS DeepLens device's Wi\-Fi network**

1. Look at the bottom of your device and make note of the device's `AMDC-NNNN` network SSID and password\.  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-bottom-ssid-pwd.png)

1. Plug in your AWS DeepLens device to an AC power outlet\. Press the power button on the front of the device to turn the device on\.   
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-plug-in-power-on.png)

1. Wait until the device has entered into setup mode when the Wi\-Fi indicator \(middle LED\) on the front of the device starts to flash\. 

   To return the device to setup mode after the initial setup, press a paper clip into the reset pinhole on the back of the device\. After you hear a click, wait about 20 seconds for the Wi\-Fi indicator to blink\. 

1. Open the network management tool on your computer\. Choose your device's SSID from the list of available Wi\-Fi networks and type the password for the device's network\. The SSID and password are printed on the bottom of your device\. The device's Wi\-Fi network's SSID has the `AMDC-NNNN` format\.   
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-ssid-connect.png)

   On a computer running Windows, choose **Connecting using a security key instead** instead of **Enter the PIN from the router label \(usually 8 digits\)** and then enter your device's Wi\-Fi password\.   
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/type-device-wifi-password-in-windows.png)

After successfully connecting your computer to the device's Wi\-Fi network, you're now ready to launch the device setup application to [configure your device](deeplens-getting-started-set-up.md)\. 

To launch the device setup app, do one of the following:
+ For the initial registration using the AWS DeepLens console, go back to the **Connect to your device** page and choose **Next**\.   
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-registration-connect-to-device.png)
+ For updating the device settings after the initial registration, open a web browser tab and enter `http://deeplens.amazon.net` or `http://deeplens.config` in the address bar\. 
**Note**  
If the above URL doesn't work, your AWS DeepLens device may have the `awscam` software version 1\.3\.5 or earlier installed\. In this case, update the device software and try it again\. Alternatively, navigate to `http://192.168.0.1`, instead of `http://deeplens.amazon.net` or `http://deeplens.config`\. For more information, see [Device Setup URL](troubleshooting-device-registration.md#troubleshooting-device-registration-8)\.