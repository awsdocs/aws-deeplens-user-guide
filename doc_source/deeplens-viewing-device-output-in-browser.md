# View Your AWS DeepLens Project Output in a Browser<a name="deeplens-viewing-device-output-in-browser"></a>

**Note**  
 To view a project's output streams in a supported browser, your device must have the `awscam` software version 1\.3\.9 or higher installed\. For information to update the device software, see [Update Your AWS DeepLens Device](deeplens-manual-updates.md)\. <a name="deeplens-view-device-stream-in-browser-proc"></a>

**To view unprocessed device streams or processed project streams in a supported web browser**

1. If you have not already downloaded the streaming certificate from your device when [registering your device](deeplens-getting-started-set-up.md) or if you have lost the downloaded copy, follow the steps below to download the streaming certificate\.

   1. [Connect to your device's `ADMC-NNNN` Wi\-Fi network](deeplens-getting-started-connect.md)\.

   1. Start the device setup app at `http://deeplens.config`\. 

   1. Download the streaming certificate for viewing a project's output streams, following the on\-screen instructions\.

1. Import into your supported web browser the streaming certificate you downloaded during the [device registration](deeplens-getting-started-set-up.md#deeplens-set-up-device-procedure)\.

   1. For FireFox \(Windows and Mac Sierra or higher\), follow these steps\.:

      1. Choose **Preferences** in FireFox\. 

      1. Choose **Privacy & Security**\.

      1. Choose **View certificate**\.

      1. Choose the **Your Certificates** tab\.

      1. Choose **Import**\.

      1. Choose the downloaded streaming certificate to load into FireFox\.

      1.  When prompted to enter your computer's system password, leave the password field blank and follow the on\-screen instruction to finish importing the streaming certificate\.
**Note**  
Depending on the version of the FireFox you use, you may need to follow steps below, instead:  
From **Preferences**, choose **Advanced**\.
Choose the **Certificates** tab\.
Choose **View Certificates**\.
On **Certificate Manager**, choose the **Your certificates** tab\.
Choose **Import**\.
Navigate to and open the downloaded streaming certificate\. 
When prompted for a password, choose **OK** without entering one\.

   1. For Chrome \(Mac Sierra or higher\), follow these steps:

      1. On your Mac, double\-click the downloaded streaming certificate to add it to **System** under **Keychains** and **My Certificate** under **Category**\.

         Alternatively, open and unlock the **Keychain Access** app, choose **System** under **Keychains** on the left and **My Certificate** under **Category**\. Drag and drop the downloaded streaming certificate into the window\.

      1. Enter your computer's system password\.

      1.  On the next screen leave the **Password** field blank and then choose **OK**\.

   1. For Chrome \(Windows and Linux\), follow these steps:

      1. In your Chrome browser, open **Settings** and choose **Advanced settings**\.

      1. In **Privacy and security**, choose **Manage certificates**\. 

      1. Choose **Import**\.

      1. Navigate to the folder containing the downloaded streaming certificate and choose it to load into Chrome\.

      1. When prompted for your computer's system password, leave the password field blank and follow the on\-screen instructions to finish importing the streaming certificate\.

1. View output streams: 

   1. Open the AWS DeepLens console at [https://console\.aws\.amazon\.com/deeplens/](https://console.aws.amazon.com/deeplens/)\.

   1. In the navigation pane, choose **Devices**\.

   1.  For **Devices**, choose your AWS DeepLens device\. 

   1.  From the device details page, choose **View output**\.

   1.  On the **View output** page, choose **View stream**\. 

      This will open your AWS DeepLens project's output viewer in a separate browser tab\.  

      Alternatively, you can open the output viewer by navigating to `https://<your-device-ip-address>:4000`\. You can find your device's IP address on the device details page in the AWS DeepLens console\.

      When prompted, choose **OK** to confirm the selection of the imported streaming certificate, choose to add security exception to the newly opened output stream viewer URL, and follow the on\-screen instructions to finish opening the stream viewer\.

   1. Choose **Live stream** to view the unprocessed device stream and choose **Project stream** to view the processed project stream\. 