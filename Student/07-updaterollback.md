# Challenge 7: Updates and Rollbacks

[< Previous Challenge](./06-deploymongo.md) - **[Home](../README.md)** - [Next Challenge >](./08-storage.md)

## Introduction

It's time to update your application, what do you do in a containerized world? Kubernetes makes it easy to safely rollout your new version carefully.

## Description

In this challenge you'll be deploying a v2 of the FabMedical application to your Kubernetes cluster. We're going to do this using two different update strategies: "Rolling Update" and "Blue/Green Deployment".

- We have staged an updated version of the app on Docker Hub with id and version:
	- **whatthehackmsft/content-web:v2**
	- **whatthehackmsft/content-api:v2**
- **NOTE:** If you have been building your docker container images from source code and deploying to an Azure Container Registry, you can find v2 of the source code in your Challenge 7 Resources folder.
- Version 2 of FabMedical stores its data in MongoDB.  We have provided a container image with an initialization script called “content-init” that loads the database with the sample content (Student/Resources/Challenge 7).  The container runs as a Kubernetes Job.  The container image is available on Dockerhub at:
	- **whatthehackmsft/content-init**
	- Use the content-init “Job” yaml provided to run the initialization of MongoDB for our new version of the app.
	- You should verify that the MongoDB contains the FabMedical data after content-init job has completed.  Hint:
    	- Connect to the mongodb pod
    	- Use the mongodb command `show dbs`
	- Logs for content-init will provide the detailed logs showing whether it was able to successfully connect and add the contents to the MongoDB. You can use kubectl to check the logs.
- Perform a rolling update of content-web on your cluster to the new version of content-web
    - **NOTE:** The new version of content-api will need to know how to reach the MongoDB server. You will need to pass it an environment variable named: **MONGODB_CONNECTION** set to its URL:
    	- `mongodb://mongodb:27017/contentdb`
	- Two options for updating the image used by the deployment:  1) `kubectl set image`, or 2) Edit the deployment
	- With kubectl and its watch feature you should be able to see new pods with the new version come online and the old pods terminate.
	- At the same time, hit the front page to see when you’re on the new version by refreshing constantly until you see the conference dates updated to 2019. 
- Now we are going to roll back this update.
	- Again, this is done from the command-line using a (different) kubectl command.
	- Confirm that we are back to the original version of the app by checking that the conference dates are back to 2017.
- Perform the update again, this time using the blue/green deployment methodology.
	- This time make sure you update BOTH content-web and content-api.
	- You will need a separate deployment file using different tags.
	- Cut over is done by modifying the app’s service to point to this new deployment.


## Success Criteria

1. You are running v2 of the application.
1. MongoDB has been seeded with speaker and session data.
1. You've upgraded content-web as a rolling update.
1. You've rolled back content-web.
1. You've upgraded both content-web and content-api as a blue/green deployment.