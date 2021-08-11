# Set Up the Project Data Store in Amazon Simple Storage Service \(S3\)\.<a name="deeplens-getting-started-create-s3"></a>

When creating and deploying a custom image classification model using AWS DeepLens, we recommend storing your training data and the compiled model in an Amazon S3 bucket\. To sign up for Amazon S3, see the Amazon S3 [Getting Started Guide](https://docs.aws.amazon.com/AmazonS3/latest/gsg/GetStartedWithS3.html)\. 

**To create an Amazon S3 bucket**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [ https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\. 

1. Choose **Create Bucket**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/s3-1.png)

1. In the **Bucket name** field, enter a unique DNS\-compliant name for your new bucket\. It must start with `deeplens-` \(for example, `deeplens-my-project`\)\. After creating the Amazon S3 bucket you can't change the name\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/s3-2.png)

1.  Choose **Create**\.

**Note**  
You are not charged for an Amazon S3 bucket until you use it\. To learn more about Amazon S3 features, pricing, and frequently asked questions, see the [ Amazon S3 product page](https://aws.amazon.com/s3/pricing/?nc=sn&loc=4)\. 

**Next Steps**
+ At this point in the tutorial, you should have successfully completed the following:
  + Registering your AWS DeepLens device
  + Created an Amazon S3 bucket
+ Next, you will need to request a service limit increase and then launch an SageMaker Jupyter notebook instance to train your model\. The Jupyter notebook and the required Lambda function are available in the [AWS DeepLens Recipes](https://github.com/aws-samples/aws-deeplens-recipes) repository\. 