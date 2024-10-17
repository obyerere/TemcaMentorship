# TemcaMentorship
Nodejs App, docker build, deploy with Kubernetes or AWS S3
For this App you need docker desktop installed on your pc
To spin up your gocd environment which is the cicd tool
- docker-compose up -d
To stop
- docker-compose down
To access your gocd after it spins up 
http://localhost:8153
You should see the GoCD dashboard. The default credentials are:
Username: admin
Password: badger
Define Build Stage:

    Stage Name: build-stage
    Job Name: build-job
    In the "Tasks" section, add the following tasks:
        Task: Execute a shell script to build the Docker image.
            Command: docker build -t small-app:latest ./small-app
        Task: Execute a shell script to push the Docker image to a container registry (you'll need Docker Hub or another registry account for this step).
            Command: docker tag small-app:latest <your-docker-username>/small-app:latest
            Command: docker push <your-docker-username>/small-app:latest

Define Deploy Stage (for Kubernetes or S3):

    Stage Name: deploy-stage
    Job Name: deploy-job
    For Kubernetes:
        Task: Execute a shell script to deploy to Kubernetes.
            Command: kubectl apply -f deployment.yml
    For S3 (if deploying static files or assets):
        Task: Use AWS CLI to upload artifacts.
            Command: aws s3 cp ./small-app/output s3://<your-bucket-name> --recursive

    Save and Run the Pipeline:
        After defining the stages and jobs, click Save to finalize the pipeline.
        Trigger the pipeline manually by going to the dashboard and clicking on Trigger next to your pipeline.

    Monitor the Pipeline:
        You can monitor each stage as it progresses. The logs will provide detailed information about the build and deployment process.