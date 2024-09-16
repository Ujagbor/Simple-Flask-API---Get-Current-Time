# Simple Flask API - Get Current Time

This project is a simple Flask API that returns the current time in JSON format when accessed via a GET request at `/time`.

## Project Structure

my-simple-api/
│
├── app.py                # Flask API code/Flask application file

├── Dockerfile            # Docker configuration to containerize the Flask API

├── requirements.txt      # Python dependencies file

├── README.md             # Project documentation (this file)

└── venv/                 # Virtual environment directory (optional)

### Prerequisites
Before you begin, ensure you have the following installed on your machine:

- Python 3.8+: Install Python

- Docker: Install Docker

- AWS CLI Install AWS CLI

- kubectl Install kubectl

- AWS EKS Setup Set up Amazon EKS

- Terraform (for AWS Infrastructure as Code setup) Install Terraform

- Git (optional, if cloning from a repository): Install Git

Getting Started

#### OPTION A: Creating the Project Locally

**1. Create a Project Directory:**
Create a directory for your project to keep everything organized:

$ pwd

cd local folder

mkdir my-simple-api

cd my-simple-api

**2. Install Flask:**
Install Flask using pip to set up the API framework:
Still, on the bash terminal, copy and paste the code below:

$ pip install Flask

Create a requirements.txt File:
Save your project dependencies so they can be easily installed later:

$ pip freeze > requirements.txt

**3. Create the Flask API**
Create the app.py File:
In your project directory, create a new Python file named app.py:

$ touch app.py

Write the Flask Code:
Open app.py in your code editor( I used VSC) and paste the following code:
I opened a new file, named it app.py, and saved it in the folder I created my-simple-api locally:
Python lang:

$
from flask import Flask, jsonify
from datetime import datetime

app = Flask(__name__)

@app.route('/time', methods=['GET'])
def get_time():
    return jsonify({'current_time': datetime.utcnow().isoformat() + 'Z'})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

Test the API Locally:
Run the Flask application to test it locally:

$ python app.py

Your API will start running on http://127.0.0.1:5000.
To test the endpoint, open a web browser or use curl to visit http://127.0.0.1:5000/time. You should see a response similar to:
Json format:

{"current_time": "2024-08-31T12:34:56.789012Z"}

My Response was: 
P.S: as I reload the URL it automatically updates the time and shows it's working.

$ python app.py
 * Serving Flask app 'app'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5000
 * Running on http://192.168.8.102:5000
Press CTRL+C to quit
127.0.0.1 - - [16/Sep/2024 15:00:02] "GET / HTTP/1.1" 404 -
127.0.0.1 - - [16/Sep/2024 15:00:03] "GET /favicon.ico HTTP/1.1" 404 -
127.0.0.1 - - [16/Sep/2024 15:00:40] "GET / HTTP/1.1" 404 -
127.0.0.1 - - [16/Sep/2024 15:00:44] "GET / HTTP/1.1" 404 -
127.0.0.1 - - [16/Sep/2024 15:06:02] "GET / HTTP/1.1" 404 -
127.0.0.1 - - [16/Sep/2024 15:06:27] "GET / HTTP/1.1" 404 -

$ CTRL+C to quit

**4. Dockerize the Flask API:**
Steps to Create and Add Content to the Dockerfile
**1.	Navigate to the Project Directory:**
Make sure you're inside the project directory where you created your API (my-simple-api).
If not, use the following command to navigate:

$ cd my-simple-api

**I.	Create the Dockerfile:**
If you haven't created the Dockerfile yet, create it by running:

$ touch Dockerfile

**II.	Open the Dockerfile for Editing:**
Open the Dockerfile using a text editor. You can use any text editor you’re comfortable with:
If you are on Linux or Mac, you can use nano:

$ nano Dockerfile
If you are on Windows or using a code editor like VS Code, open the file by either:
Right-clicking the file in your file explorer and selecting "Open with Code/Editor."
Or using the terminal to open it in VS Code:

$ code Dockerfile

**III.	Add the Following Content to the Dockerfile:**
Copy and paste the following code into the Dockerfile:
dockerfile:

$ Use an official Python runtime as a parent image
FROM python:3.9-slim

Set the working directory in the container
WORKDIR /app

Copy the current directory contents into the container at /app
COPY . .

Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

Make port 5000 available to the world outside this container
EXPOSE 5000

Define environment variable
ENV FLASK_APP=app.py

Run app.py when the container launches
CMD ["python", "app.py"]
$

**IV.	Save the Dockerfile:**
After pasting the content, save the file:
If you are using nano, press CTRL + O to save the file, then press Enter.
To exit, press CTRL + X.
If using VS Code or another editor, simply save the file as you normally would (CTRL + S).

**V.	Verify the Dockerfile:**
Use the cat command to verify that the content has been added correctly:

$ cat Dockerfile

You should see the content you added in the terminal output.
That's it! Your Dockerfile is now ready for building the Docker image.

Build the Docker Image:
FIRST, you need to make sure your docker desktop is up and running 
1. Verify if Docker is Running
Docker needs to be running in order to build and run containers. If Docker is not running, you'll get connection errors like this: 

ERROR: error during connect: Head "http://%2F%2F.%2Fpipe%2FdockerDesktopLinuxEngine/_ping": open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.
Steps if any Error:
•	Check if Docker is running:
On Windows, check the system tray for the Docker Desktop icon. It should be visible and running.
If it is not running, start Docker Desktop from your Start menu.
Wait for Docker Desktop to initialize fully (it may take a few minutes).
•	Test Docker Status:
Run the following command to check Docker's status:

$ docker info

If Docker is running properly, it should output information about the Docker environment. If you get an error message, Docker is not running.
•	Build the Docker image from the Dockerfile:

$ docker build -t my-simple-api .

If you still see errors, Select "Restart" or "Quit Docker Desktop" and then relaunch Docker Desktop from the Start menu.
Wait for Docker Desktop to start and stabilize.
Test the Docker Build Again: After Docker has restarted, try building your image again:

$ docker build -t my-simple-api .

Run the Docker Container:
Run the Docker container to test it:

$ docker run -p 5000:5000 my-simple-api

Your API will be available at http://localhost:5000/time, and you can test it just as before.
Test it by pasting 127.0.0.1:5000 in the web browser and the page will show: Not Found xxx..xxx.xx
In the bash terminal, you get a current time update  just as before 
Your result should be something like this:

$ docker run -p 5000:5000 my-simple-api
 * Serving Flask app 'app'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5000
 * Running on http://172.17.0.2:5000
Press CTRL+C to quit
172.17.0.1 - - [16/Sep/2024 14:47:28] "GET / HTTP/1.1" 404 -
172.17.0.1 - - [16/Sep/2024 14:47:47] "GET / HTTP/1.1" 404 -

$ Press CTRL+C to Quit

**5. (Optional) Push the Docker Image to a Container Registry:**
•	Tag the Docker Image:
	Tag your Docker image so it can be pushed to a container registry (e.g., Google Container Registry, Docker Hub):
If to Google container registry : 
$ docker tag my-simple-api gcr.io/my-project-id/my-simple-api:latest

**OR To a Container Registry called **Docker Hub**,** 
To tag your Docker image so it can be pushed to Docker Hub, follow these steps:
**1. Log In to Docker Hub**
If you haven't logged in to Docker Hub on your terminal, run the following command to log in:

$ docker login

Enter your Docker Hub username and password when prompted.
**2. Tag the Docker Image**
To tag the Docker image for Docker Hub, use the following format:

$ docker tag my-simple-api:latest <your-dockerhub-username>/my-simple-api:latest
**3. Push the Image to Docker Hub**
Now, push the tagged image to Docker Hub:

$ docker push <your-dockerhub-username>/my-simple-api:latest

Following these steps, you will have successfully created, containerized, and tested a simple Flask API that returns the current time in JSON format. You’ll then be ready to move on to the infrastructure and deployment steps using Terraform and GKE.

#### OPTION B: CLONING THE REPO

**1. Clone the Repository**
If you haven't already, clone this repository using Git (skip this step in Option A, if you created the project locally):

$ git clone https://github.com/your-username/my-simple-api.git

$ cd my-simple-api

**2. Set Up a Python Virtual Environment (optional but recommended)**
To avoid dependency conflicts, it's a good idea to set up a virtual environment:

Create a virtual environment
$ python3 -m venv venv

Activate virtual environment
$ source venv/bin/activate  # On Windows, use `venv\Scripts\activate`

**3. Install Dependencies**
Install the required dependencies listed in requirements.txt:

$ pip install -r requirements.txt

**4. Running the Flask API Locally**
Once the environment is set up, you can run the Flask API locally:

$ python app.py

The API will be available at http://127.0.0.1:5000. To check the current time, you can send a GET request to the /time endpoint:

$ curl http://127.0.0.1:5000/time

You should receive a JSON response with the current time:

json
$
{
  "current_time": "2024-08-31T12:34:56.789012Z"
}

**5. Dockerizing the Flask API**
- 5.1 Build the Docker Image
To run the API in a container, first build the Docker image:

$ docker build -t my-simple-api .

- 5.2 Run the Docker Container
Run the Docker container:

$ docker run -p 5000:5000 my-simple-api

Now, the API will be available at http://localhost:5000/time. You can send a GET request to the /time endpoint, and it will return the current time in JSON format as before.

**6. Testing the API in the Docker Container**
You can test the API while the Docker container is running by visiting http://localhost:5000/time in your browser or using curl:

$ curl http://localhost:5000/time

**7. Pushing the Docker Image to a Container Registry**
If you want to deploy this API to a cloud platform, you'll need to push the Docker image to a container registry. Here's an example of how to tag and push the image to Google Container Registry (GCR):

- 7.1 Tag the Docker Image
Replace my-project-id with your actual Google Cloud project ID:

$ docker tag my-simple-api gcr.io/my-project-id/my-simple-api:latest

- 7.2 Push the Docker Image to GCR
Make sure you are authenticated with Google Cloud using gcloud:

$ gcloud auth configure-docker
docker push gcr.io/my-project-id/my-simple-api:latest

**8. Deploying to Google Kubernetes Engine (GKE)**
After pushing the Docker image, you can deploy it to Google Kubernetes Engine (GKE). You can use a combination of Terraform and Kubernetes manifests for this. Ensure your Terraform code includes:

Kubernetes Deployment: To run your API on GKE.
Kubernetes Service: To expose the API to the internet.
Refer to the project instructions on how to deploy using Terraform.

### Project Resources
app.py: The main API code written in Python with Flask.
Dockerfile: The Docker configuration to containerize the Flask API.
requirements.txt: List of required Python packages.
README.md: This file, contains setup and usage instructions.

#### Notes
The API is containerized using Docker and ready to be deployed to a cloud platform such as AWmazon Web Services (AWS) using Elastic Kubernetes Service (EKS) or Google Cloud Platform (GCP) using Google Kubernetes Engine (GKE).

**The current time is returned in Coordinated Universal Time (UTC) in ISO 8601 format.**
