# Extending any Project's Functionality<a name="deeplens-extend"></a>

In this section, you take the "Hotdog recognition" sample project and add some rule\-based functionality to it to make AWS DeepLens send an SMS notification whenever it detects a hot dog\. Though we use the "Hotdog recognition" sample project in this topic, this process could be used for any project, sample or custom\.

This section demonstrates how to extend your AWS DeepLens projects to interact with other AWS services\. For example, you could extend AWS DeepLens to create:

+ A dashboard and search interface for all objects and faces detected by AWS DeepLens with timelines and frames using Amazon Elasticsearch Service\.

+ Anomaly detection models to detect the number of people walking in front of your store using Kinesis Data Analytics\.

+ A face detection and celebrity recognition application to identity VIPs around you using Amazon Rekognition\. 

In this exercise, you modify the project you previously created and edited \(see [Editing an Existing Model with Amazon SageMaker](deeplens-train-model.md)\) to use the AWS IoT rules engine and an AWS Lambda function\.


+ [Create and Configure the Lambda Function](#deeplens-create-configure-lambda-function)
  + [Create a Lambda Function](#deeplens-create-lambda-function)
  + [Add an AWS IoT Rule](#deeplens-iot-rule)
  + [Configure the Lambda Function](#deeplens-configure-lambda-function)
  + [Test Your Configuration](#deeplens-lambda-test)
  + [Test Using the Hot Dog Project](#deeplens-project-test)
+ [Disable the AWS IoT Rule](#deeplens-extend-disable-iot-rule)

## Create and Configure the Lambda Function<a name="deeplens-create-configure-lambda-function"></a>

Create and configure an AWS Lambda function that runs in the Cloud and filters the messages from your AWS DeepLens device for those that have a high enough probability \(>0\.5\) of being a hot dog\. You can also change the probability threshold\.

### Create a Lambda Function<a name="deeplens-create-lambda-function"></a>

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

   Make sure you have selected the US East \(N\. Virginia\) AWS Region\.

1. Choose **Create function**\.

1. Place your cursor in the **Blueprints** box, then choose **Blueprint name**\. When **Blueprint name:** appears, type and choose **iot\-button\-email**\.

1. Choose **Author from scratch**\.

1. Type a name for the Lambda function, for example, ***<your name>*\_hotdog\_notifier**\.

1. For **Role**, keep **Create a new Role from template\(s\)**\.

1. Type a name for the role; for example, ***<your name>*\_hotdog\_notifier**\.

1. For **Policy Templates**, choose **SNS Publish policy**\.

1. Choose **Create function**\.

### Add an AWS IoT Rule<a name="deeplens-iot-rule"></a>

This AWS IoT rule specifies the source of the data that triggers the action you specify in your Lambda function \(the next step\)\.

1. Scroll down to **aws\-iot**\.

1. For **IoT type**, choose **Custom IoT rule**\.

1. For **Rule**, choose **Create new rule**\.

1. Type a name \(***<your\-name>*\_search\_hotdogs**\) and a description for the rule\.

1. Paste the following into the **Rule query statement** box\. Replace the red text with the AWS IoT topic for your AWS DeepLens\. To find the AWS IoT topic, navigate to **Devices** on your AWS DeepLens, choose your device, then scroll to the bottom of the device detail page\.

   ```
   Select Hotdog from '/$aws/deeplens/KJHFD-DKJO87-LJLKD/inference'
   ```

   This query captures messages from your AWS DeepLens in JSON format:

   ```
   { "Hotdog" : "0.5438" }
   ```

1. Choose **Enable trigger**\.

1. Scroll to the bottom of the page and choose **Create function**\.

### Configure the Lambda Function<a name="deeplens-configure-lambda-function"></a>

Configure the Lambda function by replacing the default code with custom code and adding an environmental variable\. For this project, you also need to modify the custom code that we provide\.

1. In AWS Lambda, choose **Functions**, then choose the name of your function\.

1. On the ***your\-name*\_hotdog\_notifier** page, choose **Configuration**\.

1. In the function code box, delete all of the code\.

1. Paste the following code in the function code box\. You need to change one line in the code to indicate how you want to get notifications\. You do that in the next step\.

   ```
   /**
    * This is a sample Lambda function that sends an SMS notification when your
    * AWS DeepLens device detects a hot dog.
    * 
    * Follow these steps to complete the configuration of your function:
    *
    * Update the phone number environment variable with your phone number.
    */
   
   const AWS = require('aws-sdk');
   
   /**
   * Replace the next line of code with one of the lines of code from the list following this code block.
   */
   const var=process.env.var;
   const SNS = new AWS.SNS({ apiVersion: '2010-03-31' });
   
   exports.handler = (event, context, callback) => {
   console.log('Received event:', event);
   
   // publish message
   const params = {
   Message: 'Your AWS DeepLens device just identified a hot dog. Congratulations!',
   PhoneNumber: phone_number
   };
   if (event.label.includes("Hotdog")
   SNS.publish(params, callback);
   };
   ```

1. Add one of the following lines of code in the location indicated in the code block\. In the next step, you add an environmental variable that corresponds to the code change you make here\.

   + To receive email notifications: **const email=process\.env\.email;**

   + To receive phone notifications: **const phone\_number=process\.env\.phone\_number;**

1. Choose **Environmental variables** and add one of the following:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/deeplens/latest/dg/deeplens-extend.html)

   The key value must match the `const` name in the line of code that you added in the previous step\.

1. Choose **Save and test** \(on the upper right\)\.

### Test Your Configuration<a name="deeplens-lambda-test"></a>

**To test your configuration**

1. Navigate to the AWS IoT console at [https://console\.aws\.amazon\.com/iot](https://console.aws.amazon.com/iot)\.

1. Choose **Test**\.

1. Publish the following message to the topic that you defined in your rule: `{ "Hotdog" : "0.6382" }`\.

   You should receive the SMS message that you defined in your Lambda function: Your AWS DeepLens device just identified a hot dog\. Congratulations\!

### Test Using the Hot Dog Project<a name="deeplens-project-test"></a>

If you haven't already deployed the Hot Dog project, do the following\.

1. Navigate to [https://console\.aws\.amazon\.com/deeplens/home?region=us\-east\-1\#firstrun/](https://console.aws.amazon.com/deeplens/home?region=us-east-1#firstrun) and choose **Projects/Create a project template/Hotdog or Not Hotdog**\.

1. Deploy the project to your device\.

   For more information, see [Creating and Deploying an AWS DeepLens Sample Project](deeplens-create-deploy-sample-project.md)\.

1. Show your AWS DeepLens a hot dog to see if it detects it and sends you the confirmation message\.

To experiment, change the probability threshold for triggering the Lambda function and see what happens\.

## Disable the AWS IoT Rule<a name="deeplens-extend-disable-iot-rule"></a>

Unless you want AWS DeepLens to keep notifying you when it sees a hot dog, disable the AWS IoT rule\.

1. Log in to the AWS IoT console at ["https://console\.aws\.amazon\.com/iot](https://console.aws.amazon.com/iot)\.

1. Choose **Act**, then choose the rule that you created for this exercise, ***<your\-name>*\_search\_hotdogs**\.

1. In the upper\-right corner, choose **Actions**, then choose **Disable**\.