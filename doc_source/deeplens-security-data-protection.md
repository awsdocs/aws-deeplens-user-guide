# Data Protection<a name="deeplens-security-data-protection"></a>

 AWS DeepLens does not collect data from the device\. Video frames from the camera and inference results are confined to the device locally\. As a user, you control how and where to publish inference results to other AWS services in the project's Lambda function\. Your account remains the sole ownership of the affected data regardless which service it is published to\. 

 The AWS DeepLens supports hardware\-level full\-disk encryption for the data storage on device\. You must configure data encryption on the device\. 

 For the purpose of device registration and maintenance, communications between your AWS DeepLens device and you computer requires HTTPS connection, except for connecting to the device softAP endpoint in the device setup mode that requires authentication with password\. The default password is printed at the bottom of the AWS DeepLens device\. 

 For on\-device disk encryption, keys are stored in a hardware component known as Trusted Platform Module \(TPM\)\. 

Models are never deleted from the device\. To remove data from the device, you must connect to the AWS DeepLens device using SSH and manually remove data\. You can also perform a factory reset\. 