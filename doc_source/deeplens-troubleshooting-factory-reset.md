# Restoring your AWS DeepLens device to factory settings<a name="deeplens-troubleshooting-factory-reset"></a>

After using your AWS DeepLens, you may want to reset the device to its factory settings\. Resetting the AWS DeepLens requires multiple steps\. First, you must partition a USB drive into two parts and make the first partition bootable\. Next, you also need to download the required files for the factory restore\. These files are greater than 10 GB combined\. Finally, The second procedure explains how to use the partitioned flash drive and its contents to restore your AWS DeepLens to its factory settings\. 
+ [Partitioning a USB drive into two parts\.](#deeplens-device-factory-reset-preparation)
+ [Using a partitioned USB drive to restore your AWS DeepLens device](#deeplens-device-factory-reset-instructions)

## Preparing to perform factory reset on your AWS DeepLens device<a name="deeplens-device-factory-reset-preparation"></a>

### <a name="Introduction"></a>

Resetting your AWS DeepLens completely removes any data on the device and returns it to its factory settings\. This process requires additional hardware\. This topic explains what you need to get started and walks you through the process\. 

### Prerequisites<a name="factory-reset-prerequisites"></a>

Before starting, make sure you have the following items ready:
+ 1 USB drive, 16 GB or larger
+ A DeepLens \(v1\.0\) or a AWS DeepLens 2019 Edition\(v1\.1\) device\.

  To learn more about AWS DeepLens versions, see [AWS DeepLens Device Versions](deeplens-versions.md)\.
+ A computer to facilitate preparation: 
  + You can set up your AWS DeepLens camera as a Linux computer using a pointer, keyboard, monitor, HDMI to micro HDMI cable, and a USB hub\.
  + You can also connect your AWS DeepLens to a personal computer via SSH\. For more information, see [How to Connect Your Device to Your Home or Office Wi\-Fi Network When the Wi\-Fi SSID or Password Contains Special Characters?](troubleshooting-device-registration.md#troubleshooting-device-wifi-connection)\.

**Note**  
It is possible to use a micro SD with a USB\-based card reader, but this is *not recommended*\. A factory reset cannot be initiated if the card slot on the back of the AWS DeepLens device is in use\. 

### Preparation<a name="preparation"></a>

You must complete the procedure in this section before performing a factory reset\. At a high level, the preparation process includes the following\. 
+ Formatting a USB flash drive into the following two partitions:
  + **1st partition**: FAT32 of 2 GB
  + **2nd partition**: NTFS of at least 9 gb
+ Making the USB flash drive bootable:
  + Download the required [customized Ubuntu ISO image](https://s3.amazonaws.com/deeplens-public/factory-restore/Ubuntu-Live-16.04.3-Recovery.iso) required for both 1\.0 and 1\.1, of AWS DeepLens\.
  + Use the downloaded image to turn the first partition of your flash drive into a bootable device\. 
+ Copy the factory restore files on a USB drive:
  + Download the compressed factory restore package for your version of AWS DeepLens:
    + [AWS DeepLens v1\.0](https://s3.amazonaws.com/deeplens-public/factory-restore/DeepLensFactoryRestore.zip)
    + [AWS DeepLens v1\.1](https://s3.amazonaws.com/deeplens-public/factory-restore/DeepLensFactoryRestore_hw_1_1.zip)\.
  + Unzip the package and put the unpacked files onto the 2nd partition \(formatted as NTFS\) of the USB drive\. 

You only do this once to set up your USB flash drive\. After that, you can use the same flash drive to reset your AWS DeepLens as needed\. 

There are multiple ways to partition and format a USB drive\. Depending on the computer you use, specific tasks may differ from one operating system to another\. 

The following procedure requires that you have set up your AWS DeepLens device as an Ubuntu computer and uses two third\-party applications, GParted and UNetbootin\. GParted is a partition\-editing application and UNetbootin creates bootable USB drives \.

The same instructions apply if you are using a personal computer running the Ubuntu operating system\.

**To partition a USB drive and make it bootable by using an Ubuntu computer**

1. Open an Ubuntu terminal window and run the following commands to install and launch GParted\.

   ```
   sudo apt-get update; sudo apt-get install gparted
   sudo gparted
   ```

1. Open the GParted console\.   
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/DeepLens_System_Restore_GParted.png)

1. In the GParted console, choose **/dev/sda** from the drop\-down menu in the upper\-right corner, and then delete any existing partitions\. 

1. Next, delete any existing partitions\.

1. To create the FAT32 partition, choose the file icon and then set the parameters as follows\. 

   **Free space preceding:** **1**

   **New size:** **2048**

   **Free space following:** **28551**

   **Align to:** **MiB**

   **Create as:** **Primary Partition**

   **Partition name:**

   **File system:** **fat32**

   **Label:** **BOOT**  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/DeepLens_System_Restore_Instruction.jpg)

1. Then, choose **Add**\.

1. To create the NTFS partition, choose the file icon and then set the parameters as follows\. 

   **Free space preceding:** **0**

   **New size:** **28551**

   **Free space following:** **0**

   **Align to:** **MiB**

   **Create as:** **Primary Partition**

   **Partition name:** 

   **File system:** **nfts**

   **Label:** **Flash**  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/DeepLens_System_Restore_NTFS.jpg)

1. Then, choose **Add**\.

After successfully partitioning your USB drive into the FATS32 and NTFS partitions, the USB drive partitions will appear in the GParted console\. 

![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/DeepLens_System_Restore_USB_Success.jpg)

Now you are ready to make the partitioned USB drive bootable using UNetbootin\.

**To make a USB drive bootable using UNetbootin**

1. Download the [ customized Ubuntu ISO image](https://s3.amazonaws.com/deeplens-public/factory-restore/Ubuntu-Live-16.04.3-Recovery.iso)\.

1. Open an Ubuntu terminal window\.

1. Run the following commands to install and launch Unetbootin\.

   ```
   sudo apt-get update; sudo apt-get install unetbootin
   sudo unetbootin
   ```

1. In the UNetbootin window choose **Diskimage**\.

1. Then, choose **ISO**\.

1. Next, choose the **\. \. \.** to open the file explorer\.

1. Then, choose the customized Ubuntu ISO file you downloaded previously\.

1. For **Type**, choose **USB Drive**\.

1. For **Drive**, choose `/dev/sda1`\.

1. Then, choose **OK**\.  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/DeepLens_System_Restore_UNetbootin.jpg)
**Note**  
The customized Ubuntu image might be more recent than what's shown here\. If it is, use the most recent version of the Ubuntu image\.

Now that you have successfully created a bootable USB drive\. Use the procedure below to copy the factory restore files to the NTFS partition of your USB drive\. 

**Note**  
You must use the factory restore package associated with the correct version of your AWS DeepLens device\.

**To copy the factory restore files to the NTFS partition of your usb drive**

1. Download the compressed factory restore package for your version of AWS DeepLens\.
   + [AWS DeepLens v1\.0](https://s3.amazonaws.com/deeplens-public/factory-restore/DeepLensFactoryRestore.zip)
   + [AWS DeepLens v1\.1](https://s3.amazonaws.com/deeplens-public/factory-restore/DeepLensFactoryRestore_hw_1_1.zip)

1. Unzip the downloaded package\.

   ```
   unzip your-file-name.zip
   ```

1. Copy the following uncompressed files to the second \(NTFS\) partition of the USB drive:

   **For AWS DeepLens v1\.0**

   *Image files*
   + DeepLens\_image\_1\.3\.22\.bin
   + DeepLens\_1\.3\.22\.md5

   *Script file*
   + usb\_flash

   **For AWS DeepLens v1\.1**

   *Image files*
   + image\_deepcam\_19WW19\.5tpm
   + image\_deepcam\_19WW19\.5\_tpm\.md5
   + APL\_1\.0\.15\_19WW19\.5\_pcr\_fuse\.dat
   + pubkey\.der\.aws

   *Script file*
   + usb\_flash
   + seal\_and\_luksChangeKey

After completing these procedures you should have successfully partitioned your USB flash drive into two partitions and added the necessary files to each partition\. 

## Restoring your AWS DeepLens device to factory settings<a name="deeplens-device-factory-reset-instructions"></a>

Follow the instructions to restore your AWS DeepLens device to its factory settings\. The procedures below assume you have successfully completed the steps outline in [Preparing to perform factory reset on your AWS DeepLens device](#deeplens-device-factory-reset-preparation)\.

These instructions *require* you to connect your AWS DeepLens device to a monitor and keyboard\.

**Warning**  
 All data previously stored on your AWS DeepLens device will be erased\.<a name="deeplens-device-factory-reset-procedure"></a>

**To restore your AWS DeepLens device to its factory settings**

1. Insert the prepared USB drive into your AWS DeepLens device and power it on\. Repeatedly press the *ESC* key to enter BIOS\. 

1. From the BIOS window on your monitor, choose **Boot From File**\.

1. Then choose **USB VOLUME**\.

1. Then choose **EFI**\.

1. Then choose **BOOT**\.

1. Then choose **BOOTx64\.EFI**\.

1. Next, your AWS DeepLens device reset will start automatically\.

   This occurs when the power LED indicator starts to flash and a terminal window pops up to display your progress\. No further user input is necessary at this point\.
**Note**  
If an error occurs and the recovery fails, restart the procedure from step 1\. For detailed error messages, see the *result\.log* file that's generated on the USB flash drive\. 

1. After the power LED light stops flashing and the terminal closes, signaling that the factory reset is complete\. The device then reboots itself automatically\.

 Your device is now restored\. You can disconnect your USB flash drive from AWS DeepLens\.