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
![Alt text](/images/training_sigleinstance_logs.jpg "Training for the endpoint [single instance]")
![Alt text](/images/deployment_endpoint.jpg "Deploy of the endpoint [single instance]")
As it is possible to see, it is single instance because of the logs.

- Multi-instance training
![Alt text](/images/training_multiinstance_logs.jpg "Training for the endpoint [multi-instance]")
![Alt text](/images/deployment_endpoint_multiinstancetraining.jpg "Deploy of the endpoint [multi-instance]")
As it is possible to see, it is multi-instance because of the 4 logs.





