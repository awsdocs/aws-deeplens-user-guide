# Train a Model in Amazon SageMaker<a name="deeplens-getting-started-launch-sagemaker"></a>

To begin creating your custom image classification model using your registered AWS DeepLens device you need to launch an Amazon SageMaker instance and request a service limit increase\. 

## Getting Started with an Amazon SageMaker Instance<a name="getting-started-sagemaker-instance"></a>

SageMaker is a fully managed machine learning service that enables data scientists and developers to build and train machine learning models using a Jupyter notebook instance\. 

To create a custom image classification model, we need to use a graphics processing unit \(GPU\) enabled training job instance\. GPUs are excellent at parallelizing the computations required to train a neural network for this project\. 

In order to access a GPU\-enabled training job instance, you must submit a request for a service limit increase to the [ AWS Support Center](https://console.aws.amazon.com/support/home?#/case/create)\. 

**Note**  
Jupyter notebooks are open source web applications that you can use to create and share documents that contain live code, equations, visualizations, and narrative text\. The AWS DeepLens Jupyter notebook in this repo contains code that demonstrates how to create machine learning solutions with Amazon SageMaker and AWS DeepLens\. 

## Request a GPU\-enabled Amazon SageMaker Training Instance<a name="request-gpu-enabled-sagemaker-instance"></a>

1. Open the [AWS Support Center console](https://console.aws.amazon.com/support/home#/case/create)\.

1. On the **AWS Support Center** page, choose **Create Case** and then choose ** Service limit increase**\.

1. In the **Case classification** panel under **Limit type**, search for Amazon SageMaker\.

1. In the **Request** panel, choose the **Region** that you are working in\. For **Resource Type**, choose **SageMaker Training**\. 

1. For **Limit** choose **ml\.p2\.xlarge instances**\.

1. For **New Limit Value**, verify that the value is **1**\.

1. In **Case description**, provide a brief explanation of why you need the **Service limit increase**\. For example, I need to use this GPU\-enabled training job instance to train a deep learning model using TensorFlow\. I'll use this model on an AWS DeepLens device\. 

1.  In **Contact options**, provide some details about how you would like to be contacted by the AWS service support team on the status of your **Service limit increase** request\. 

1. Choose **Submit**\.

## Create an SageMaker Notebook Instance<a name="create-sagemaker-instance"></a>

1. Sign in to the [SageMaker console](https://console.aws.amazon.com/sagemaker/home?region=us-east-1#/dashboard)

1. In the navigation pane, choose **Notebook instances**\.

1. On the **Notebook instances** page, choose **Create notebook instance**\.  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/get-started-5-notebook-create.png)

1. On the **Create notebook instance** page, enter your name in **Notebook instance name**, and then choose **ml\.p2\.xlarge**\.   
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/get-started-6-notebook-name.png)

1. Choose your **IAM Role** to set up the correct permissions and encryption as follows: 
   + If you have an existing Amazon SageMaker IAM role, select that IAM role from the list\.
   + If you are new to **Amazon SageMaker**, create an IAM role by choosing **Create a new role**\. On the **Create an IAM role** page, choose **Any S3 bucket** to give your new IAM role access to your S3 bucket\. Choose **Create Role**\.  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/get-started-7-notebook-iam.png)

1. On the **Create a notebook instance** page, choose **IAM Role**, and then choose your IAM role from the list\. 

1. Open the **Git repositories** panel\. and then, under the **Default repository** drop down, choose **clone a public gitHub repository**\. 

1.  Copy `https://github.com/aws-samples/aws-deeplens-recipes/` and paste it into the field\. It contains the Jupyter notebook required for this custom project\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/sagemaker-create-notebook.png)

1. Choose **Create notebook instance**\.

   In a few minutes, Amazon SageMaker will launch the notebook instance\. When ready the status will change from **Pending** to **In service**\. 

1. On the ** Notebook instances** page, choose **Open Jupyter** to launch your newly created Jupyter notebook\. 

1. Navigate to the `static/code/trash-sorter/` directory and open the `aws-deeplens-custom-trash-detector.ipynb`\.  
![\[alt_text\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/jupyter-launch-page.png)

1. Follow the prompts in the Jupyter notebook to train your model\.
**Note**  
Jupyter notebook contain a mixture of code and markdown cells\. In a notebook, each cell can be run and modified\. To modify a cell, double\-click it and make your change\. To run a cell, press ShiftEnter\.   
While the cell is running, an **asterisk \(\*\)** appears in brackets to the left of the cell that you selected\. When the cell has finished running, the asterisk is replaced by an output number, and a new output cell is generated beneath the original cell\.   
Alternatively, you can also run a cell by selecting it and then choosing **Run** on the toolbar\.

**Next steps**
+ At this point in the tutorial, you should have now successfully completed the following:
  + Registering \. your AWS DeepLens device
  + Created an Amazon S3 bucket
  +  Trained an image classification model SageMaker

Next, you will learn how to deploy this model to run on AWS DeepLens