---
title: "Deploying Streamlit app on Google Cloud Run"
date: 2023-08-17
by: Chirag Sehra
tags: 
    - deployment
    - streamlit
    - Google Cloud Run
    - Serverless Deployment
---
## Prerequisites
- A Google Cloud account
- `gcloud` CLI installed on local system.
- `Docker` installed on local system.
- `Streamlit` application to be deployed

## Dockerizing the Streamlit App
Google Cloud Run runs containerised applications. To prepare Streamlit app for deployment, we will need to create a Docker container. Following are the steps:
1. **Create a `Dockerfile`**: Crafting a `Dockerfile` that specifies the environment and dependencies required for the Streamlit app.
```
FROM python:3.8  
EXPOSE 8080  
WORKDIR /app  
COPY . ./  
RUN pip install -r requirements.txt  
ENTRYPOINT ["streamlit", "run", "main.py", "--server.port=8080", "--server.address=0.0.0.0"]
```


2. **Building the docker image**: Using the docker command line tools, we can build the Docker image of Streamlit app.
```
docker build -t <image_name> .
```

3. **Testing the docker container**: We can run the docker container locally to ensure that our Streamlit app works as expected within the containerised environment.
```
docker run -p 8080:8080 <image_name>
```

## Setup Google Cloud Run
1. Open Google Cloud Console: Access the Google Cloud Console using the GCP account.
2. Enable the Cloud Run API: If you haven't already, enable the Cloud Run API for the GCP account.
3. Deploy using Cloud Console: Following the Cloud Run Interface to deploy the dockerised streamlit app. We can also set environment variables and CPU limitations.
```
gcloud builds submit --tag asia.gcr.io/<PROJECT_ID>/<RANDOM_PROJECT_NAME> 
```

## Deploying the app
1. Search for `Container Registry` and then you would be able to see the recent container image deployed.

![[notes/images/container_registry_image_list.png]]
2. Click `Deploy` and select `Deploy to Cloud Run` to deploy the app.

![[notes/images/container_registry_image_deploument.png]]
3. The app deployment will start and take some time to finish

![[notes/images/cloud_run_app_deployment.png]]

## Testing and Monitoring the App Deployment
After deploying the app, we can test and monitor its usage:
1. Access the deployed app: Once, the deployment is complete, we receive a URL where the Streamlit app is accessible.
2. Monitoring and Troubleshooting: Using the Google Cloud Console, we can monitor the app. This includes checking logs, monitoring usage, and identifying any issues that might arise.
![[notes/images/streamlit_app_monitoring_cloud_run.png]]

## CI/CD
For effecient app updates, we can implement CD:
1. Setting up source control: We can link our Github code repository to Google Cloud Build for automated deployments
2. Configure Triggers: We can configure triggers to build and deploy our app whenever changes are pushed to the repository
![[notes/images/streamlit_app_cloud_run_cd.png]]