# Operationalizing-an-AWS-ML-Project
Udacity Machine Learning Engineer AWS

# Project Summary
In this project, you will complete the following steps:

**Step 1**: Train and deploy a model on Sagemaker, using the most appropriate instances. Set up multi-instance training in your Sagemaker notebook.

**Step 2**: Adjust your Sagemaker notebooks to perform training and deployment on EC2.

**Step 3**: Set up a Lambda function for your deployed model. Set up auto-scaling for your deployed endpoint as well as concurrency for your Lambda function.

**Step 4**: Ensure that the security on your ML pipeline is set up properly.

# Step 1: Training and deployment on Sagemaker
- Creation of the instance
![Alt text](/images/instance.jpg "Creation of instance")
No need for a greater instance the train will be done separatelly by a training job.

- Upload to the S3 bucket
![Alt text](/images/bucket.jpg "Creation of the bucket")
Had to reconfigure the name of the bucket to myudacitybucket

- Training and Deployment

*Logs*
![Alt text](/images/training_sigleinstance_logs.jpg "Training for the endpoint [single instance]")

*Endpoint*
![Alt text](/images/deployment_endpoint.jpg "Deploy of the endpoint [single instance]")
As it is possible to see, it is single instance because of the logs.

- Multi-instance training

*Logs*
![Alt text](/images/training_multiinstance_logs.jpg "Training for the endpoint [multi-instance]")

*Endpoint*
![Alt text](/images/deployment_endpoint_multiinstancetraining.jpg "Deploy of the endpoint [multi-instance]")
As it is possible to see, it is multi-instance because of the 4 logs.

# Step 2: EC2 Training
- Creation of EC2
![Alt text](/images/ec2_creation.jpg "EC2 Instance")
It was chosen the t2.micro because of the budget constraint (t2.micro is free for until a certain usage) and no hurry to train the model.

- Artifact Model
![Alt text](/images/artifact_model_in_ec2.jpg "Artifact Model in EC2")
Since it is not possible to deploy a model like in Sagemaker in EC2, we train the model first and save its artifact.

- Main Differences
The structure of the whole code it is pretty similar, specially regarding the architecture of the network inside ec2train1.py and hpo.py. What it differs a lot btw them is the contruction of the training part, since in hpo.py it has the interface with the "Estimator" in Sagemaker, while ec2train1.py does not. It gives a little bit more work doing this part inside ec2, few parts needs to be done manually in it, especially of you wanted to do a tuning inside ec2 too.

# Step 3: Lambda function setup
- Runtime module
It is possible to invoke a lambda function using the runtime module as follows:

*response=runtime.invoke_endpoint(EndpointName=endpoint_Name,
                                  ContentType="application/json",
                                  Accept='application/json',
                                  Body=json.dumps(event))*
                                  
Of course, in order to use this module, your endpoint must be deployed, and your lambda function with the right permissions.
Lambda functions are really important for micro-services, they are often used because they are a serveless service provided by amazon and are cheap and also easy to use.

# Step 4: Security and testing
- Lambda Function Response to the invokation (list of 33 numbers)
![Alt text](/images/lambda_function.jpg "Test with Lambda Function Invokation")

- IAM dashboard
![Alt text](/images/permissions_iam.jpg "Permissions granted on IAM to invoke")

The IAM Role is secure, it was given only what was needed, nothing more like a FullAcess permission that could cause any breaches in my product here. 

# Step 5: Concurrency and auto-scaling
- Concurrency

The number set for concurrency was 5, it was set a moderate number for it, especially because at first it is not expected to have a high level of traffic this service, and concurrency is also really expensive, but this service will be tracked by the ML Engineers in order to see if it shows/has an upcoming demand to see if it is needed to increase concurrency.
![Alt text](/images/concurrency.jpg "Concurrency")

- Auto-scaling

*Auto-Scaling Number of Instances*
![Alt text](/images/autoscaling.jpg "Auto-Scaling Number of Instances")
It was set this value to match the maximum value for concurrency, it does not make a lot of sense to set an auto-scaling bigger that concurrency, otherwise you would just change bottleneck between concurrency and auto-scaling.

*Auto-Scaling Time to Trigger*
![Alt text](/images/autoscaling_time.jpg "Auto-Scaling Time to Trigger")
Since it is a "new product", and it is not wanted to possibly have a bottleneck right in the beggining, maybe causing the wrong impression of the product, it was set a scale-up/down time of 30 seconds, after few weeks of the release and the number of instances of concurrency and auto-scaling is reconfigured, that could be changed back to the default of 300 seconds or even longer to avoid higher costs.

