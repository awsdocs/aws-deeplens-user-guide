# Register Your AWS DeepLens Device<a name="deeplens-getting-started-register"></a>

Use your computer's browser to register your device and download a certificate\.

Before you continue, complete the [Prerequisites](deeplens-prerequisites.md)\.

**To register AWS DeepLens**

1. Sign in to the AWS Management Console and open the AWS DeepLens console at [https://console\.aws\.amazon\.com/deeplens/home?region=us\-east\-1\#firstrun](https://console.aws.amazon.com/deeplens/home?region=us-east-1#firstrun)\.

1. Choose **Register device**\.

1. For **Device name**, type a name for your AWS DeepLens, then choose **Next**\.

   Use only alphanumeric characters and dashes \(\-\)\.

1. If this is your first time registering an AWS DeepLens device, create the following AWS Identity and Access Management \(IAM\) roles\. They give AWS DeepLens the permissions it needs to perform tasks on your behalf\.

   If you have already created these roles, skip to step 5\.

   1. **IAM role for AWS DeepLens**

      From the list, choose **AWSDeepLensServiceRole**\. If **AWSDeepLensServiceRole** isn't listed, choose **Create role in IAM** and follow these steps in the IAM console\.

      1. Accept the **DeepLens** service and **DeepLens** use case by choosing **Next: Permissions**\.

      1. Accept the **AWSDeepLensServiceRolePolicy** policy by choosing **Next: Review**\.

      1. Accept the role name **AWSDeepLensServiceRole** and the provided description by choosing **Create role**\. Do not change the role name\. 

      1. Close the IAM window\.

   1. **IAM role for AWS Greengrass service**

      From the list, choose **AWSDeepLensGreengrassRole**\. If **AWSDeepLensGreengrassRole** isn't listed, choose **Create role in IAM** and follow these steps in the IAM console\.

      1. Accept the **Greengrass** service and **Greengrass** use case by choosing **Next: Permissions**\.

      1. Accept the **AWSGreengrassResourceAccessRolePolicy** policy by choosing **Next: Review**\.

      1. Accept the role name **AWSDeepLensGreengrassRole** and the provided description by choosing **Create role**\. Do not change the role name\. 

      1. Close the IAM window\.

   1. **IAM role for AWS Greengrass device groups**\.

      From the list, choose **AWSDeepLensGreengrassGroupRole**\. If **AWSDeepLensGreengrassGroupRole** isn't listed, choose **Create role in IAM** and follow these steps in the IAM console\.

      1. Accept the **DeepLens** service and the **DeepLens \- Greengrass Lambda** use case by choosing **Next: Permissions**\.

      1. Accept the **AWSDeepLensLambdaFunctionAccessPolicy** policy by choosing **Next: Review**\.

      1. Accept the role name **AWSDeepLensGreengrassGroupRole** and the provided description by choose **Create role**\. Do not change the role name\. 

      1. Close the IAM window\.

   1. **IAM role for Amazon SageMaker**

      From the list, choose **AWSDeepLensSagemakerRole**\. If **AWSDeepLensSagemakerRole** isn't listed, choose **Create role in IAM** and follow these steps in the IAM console\.

      1. Accept the **SageMaker** service and the **SageMaker \- Execution** use case by choosing **Next: Permissions**\.

      1. Accept the **AmazonSageMakerFullAccess** policy by choosing **Next: Review**\.

      1. Accept the role name **AWSDeepLensSageMakerRole** and the provided description by choosing **Create role**\. Do not change the role name\. 

      1. Close the IAM window\.

   1. **IAM role for AWS Lambda**

      From the list, choose **AWSDeepLensLambdaRole**\. If **AWSDeepLensLambdaRole** isn't listed, choose **Create role in IAM** and follow these steps i the IAM console\.

      1. Accept the **Lambda** service and the **Lambda** use case by choosing **Next: Permissions**\.

      1. Accept the **AWSLambdaFullAccess** policy by choosing **Next: Review**\.

      1. Accept the role name **AWSDeepLensLambdaRole** and the provided description by choosing **Create role**\. Do not change the role name\. 

      1. Close the IAM window\.

1. In AWS DeepLens, on the **Set permissions** page, choose **Refresh IAM roles**, then do the following:

   + For **IAM role for AWS DeepLens**, choose **AWSDeepLensServiceRole**\.

   + For **IAM role for AWS Greengrass service**, choose **AWSDeepLensGreengrassRole**\.

   + For **IAM role for AWS Greengrass device groups**, choose **AWSDeepLensGreegrassGroupRole**\.

   + For **IAM role for Amazon SageMaker**, choose **AWSDeepLensSagemakerRole**\.

   + For **IAM role for AWS Lambda**, choose **AWSDeepLensLambdaRole**\.
**Important**  
Attach the roles exactly as described\. Otherwise, you might have trouble deploying models to AWS DeepLens\.
If any of the lists do not have the specified role, find that role in step 4, follow the directions to create the role, choose **Refresh IAM roles**, and return to where you were in step 5\.

1. Choose **Next**\.

1. On the **Download certificate** page, choose **Download certificate**, then choose **Save File**\. Note where you save the certificate file because you need it later\.  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-save-certificate-dialog.png)

   After the certificate has been downloaded, choose **Register**\.
**Important**  
The certificate is a \.zip file\. You attach it to AWS DeepLens in \.zip format, so don’t unzip it\.   
Certificates aren't reusable\. You need to generate a new certificate every time you register your device\.