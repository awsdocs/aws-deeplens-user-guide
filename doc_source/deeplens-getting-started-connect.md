# Connect to Your AWS DeepLens Device's Wi\-Fi Network<a name="deeplens-getting-started-connect"></a>

To set up your AWS DeepLens device, you must first connect your computer to the device's local area Wi\-Fi network, also known as the device's `AMDC-NNNN` network\. When this network is active, the Wi\-Fi indicator \(the middle LED light\) on the front of the device flashes\. The device is said to be in the setup mode and disconnected from the internet\. The device will remain in the setup for 2 hours for you to finish configuring or updating the device settings\. After finishing the setup or exceeding the allotted time delay, the device will exit the setup mode, when the Wi\-Fi indicator stays on and not blinking\. Upon a successful registration, the device will be in the operational mode and connected to the internet\. 

When you set up your device for the first time, the device is booted into the setup mode\. The device's `AMDC-NNNN` network becomes visible from the list of available Wi\-Fi networks on your computer\. You can connect your computer to the Wi\-Fi network by choosing the SSID and type the password of the `AMDC-NNNN` network\. To update the device settings after the initial setup, you must reconnect to the device's `AMDC-NNNN` network after turning on the device's setup mode, again\. If, for some reason, the device exits the setup mode before you can finish the setup, you must reconnect your computer to the device in the same way\.

**To connect to your AWS DeepLens device's Wi\-Fi network**

1. Start your AWS DeepLens device by plugging the power cord into an outlet and the other end into the back of your device\. Turn the device on by pressing the power button on the front of the device\. 

1. Wait until the device has entered into the setup mode when the Wi\-Fi indicator on the front of the device starts to flash\. 

   To return the device into its setup mode after it has exited the setup mode, press a paper clip into the reset pinhole on the back of the device and, after hearing a click, wait for about 20 seconds for the Wi\-Fi indicator to blink\. 

1. Connect your computer to the device's Wi\-Fi network\. On your computer, choose the device's SSID from the list of available networks and type the password for the device's network\. The SSID and password are printed on the bottom of your device\. The device's Wi\-Fi network's SSID has the `AMDC-NNNN` format\.  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-ssid-connect.png)
**Note**  
To enter your device's Wi\-Fi password on a computer running Windows, you may need to choose **Connecting using a security key instead** over **Enter the PIN from the router label \(usually 8 digits\)**\.   
     

![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/type-device-wifi-password-in-windows.png)

After successfully connecting to the device's Wi\-Fi network, you're now ready to [set up your device or update the settings](deeplens-getting-started-set-up.md)\.