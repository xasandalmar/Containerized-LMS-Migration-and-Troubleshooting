Introduction
This section covers the process of creating, configuring, and publishing a containerized version of the LMS frontend application. Container images package your application with all its dependencies, ensuring consistent operation across different environments and simplifying deployment to AWS ECS.

Implementation Steps
Step 1: Create Amazon ECR Repository
Amazon Elastic Container Registry (ECR) provides a secure location to store and manage your container images.

1. Navigate to Amazon ECR via the AWS Console search bar



2. Click "Create repository" 



3. Enter repository details:

Repository name: `edutech-lms-frontend`
Keep all other settings as default


4. Click "Create repository"

5. Note the repository URI (format: ACCOUNT_ID.dkr.ecr.REGION.amazonaws.com/edutech-lms-frontend)

Best Practice: In production environments, enable image tag immutability and vulnerability scanning to improve security and prevent image tag overwriting.

Step 2: Configure Local Development Environment
Set up the necessary tools to build and push container images.

AWS CLI Setup
1. Install AWS CLI:

Windows: Download and run the installer from AWS CLI Download Page
Mac: Run brew install awscli (with Homebrew) or download the installer
Linux: Run sudo apt install awscli (Ubuntu/Debian) or sudo yum install awscli (Amazon Linux/CentOS)
2. Verify AWS CLI installation: aws --version



3. Configure AWS CLI with your credentials: aws configure

4. When prompted, enter:

AWS Access Key ID: [Your access key]
AWS Secret Access Key: [Your secret key]
Default region name: [Your region, e.g., us-east-1]
Default output format: json


Where to find your root access keys?
Sign in to the AWS Management Console with your root credentials
Click on your account name in the top-right corner
Go to "Security credentials" tab
Scroll down to "Access keys" section
5. Click "Create access key" (Note: AWS recommends creating IAM users instead of using root keys for security)
Acknowledge the security warning and complete the process
Save or download the .csv file






Security Note: For production environments, use IAM roles with temporary credentials instead of long-term access keys. For this project, ensure access keys have ECR permissions.

Docker Setup
1. Install Docker:

Windows/Mac: Download and install Docker Desktop from Docker's official website
Linux: Follow distribution-specific instructions:
- Ubuntu: `sudo apt install docker.io docker-compose`
- CentOS/RHEL: `sudo yum install docker`
2. Verify Docker installation: `docker --version`



3. Ensure Docker service is running (Linux only): `sudo systemctl start docker`

4. Test Docker with a simple container: `docker run hello-world`



Step 3: Authenticate to Amazon ECR
1. In the ECR Console, select your newly created repository



2. Click the "View push commands" button to see authentication instructions



3. Open your terminal/command prompt on your local machine

4. Run the AWS CLI authentication command (copy from the console):

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 741448958115.dkr.ecr.us-east-1.amazonaws.com

5. Verify successful authentication with "Login Succeeded" message

Step 4: Acquire and Prepare the Application Source Code
1. First, you need to download the React project to your local machine:

Open your terminal/command prompt
Create and navigate to project directory:
mkdir edutech-project && cd edutech-project



Clone the LMS frontend github repository:
git clone https://github.com/techwithlucy/ztc-projects.git



Move into the project directory:
cd ztc-projects/projects/cloud-engineer-projects/project-2/frontend



Understanding the Project Structure
Before creating the Dockerfile, take a moment to understand what you're working with:

This is a React-based frontend for an educational technology Learning Management System (LMS)
The project contains the source code that needs to be built and containerize
Step 5: Create Dockerfile for React Application
1. While inside the project directory, let's create the Dockerfile using the terminal in VS Code:

Open VS Code's integrated terminal by going to Terminal > New Terminal from the top menu, or by pressing Ctrl+` (backtick)


If the terminal doesn't default to Git Bash, click on the dropdown next to the + sign in the terminal panel and select "Git Bash"


In the terminal, create the Dockerfile with the touch command:
touch Dockerfile

Once created, you'll see the new "Dockerfile" appear in VS Code's Explorer panel


Open the Dockerfile by double-clicking it in the Explorer panel
Add the following content to the Dockerfile:
javascript
# Use Node.js as base image
FROM node:16-alpine

# Set working directory
WORKDIR /app

# Copy package files first for better caching
COPY package*.json ./
RUN npm install

# Copy the rest of the application
COPY . .

# Build the application - necessary for React apps
RUN npm run build

# Install serve to host the built static files
RUN npm install -g serve

# Expose port 3000
EXPOSE 3000

# Start the server
CMD ["serve", "-s", "build", "-l", "3000"]


Technical Note: This multi-stage approach optimizes the container build process. Copying package files before the rest of the code leverages Docker layer caching to avoid reinstalling dependencies when only application code changes.

Save the file by pressing Ctrl+S or selecting File > Save
Understanding the Dockerfile
For beginners, here's what each part of the Dockerfile does:

`FROM node:16-alpine`: Uses a lightweight Node.js image as the base
WORKDIR /app: Creates and sets the working directory inside the container
`COPY package*.json ./`: Copies package.json and package-lock.json files
RUN npm install: Installs all required dependencies
COPY . .: Copies all project files into the container
RUN npm run build: Creates an optimized production build of the React app
`RUN npm install -g serve`: Installs a lightweight web server
EXPOSE 3000: Tells Docker the container will use port 3000
CMD ["serve", "-s", "build", "-l", "3000"]: Command to run when the container starts
Saving the Dockerfile
1. Save the Dockerfile (no file extension needed)

2. Check that the Dockerfile is in the root directory of your project alongside package.json

Once you've completed these steps, you're ready to move on to Step 6 where you'll build the Docker image.

Step 6: Build the Docker Image
1. In your terminal, navigate to your React project directory (if you're not already there):

cd edutech-lms-frontend

2. Build your Docker image with the following command:

docker build -t edutech-lms-frontend .

Note: The period (.) at the end of the command is important! It tells Docker to use the current directory.

Performance Note: Initial builds may take 5-10 minutes depending on internet speed and system resources as Node.js dependencies are downloaded and installed.

3. You'll see a series of steps executing in your terminal as Docker builds the image. Wait for the build process to complete - you'll know it's done when you see a success message and regain control of your terminal prompt.



4. Verify the image was created with: docker images

You should see "edutech-lms-frontend" listed in the output, similar to:



Step 7: Tag and Push the Image to ECR
1. Tag the local image with the ECR repository URI:

 Important: Replace the repository URI with your actual repository URI from Step 1

For example:

docker tag edutech-lms-frontend:latest YOUR_AWS_ACCOUNT_ID.dkr.ecr.YOUR_REGION.amazonaws.com/edutech-lms-frontend:latest

Replace `YOUR_AWS_ACCOUNT_ID` with your 12-digit AWS account number
Replace `YOUR_REGION` with your AWS region (e.g., us-east-1)


2. Push the image to ECR:
Remember to use the same repository URI as in the previous step!

docker push YOUR_AWS_ACCOUNT_ID.dkr.ecr.YOUR_REGION.amazonaws.com/edutech-lms-frontend:latest

3. Monitor the push progress until completion

4. Once complete, you'll see a confirmation message with the image digest.



Step 8: Verify Image in Amazon ECR
1. Return to the ECR Console in your web browser

2. Navigate to the `edutech-lms-frontend` repository



3. Confirm the image appears with the "latest" tag



4. Click on the image to view details:

These details confirm your image was successfully pushed and is ready to use.



Note the complete Image URI for deployment in the next section
Step 9: Image Preparation Checklist
✅ Before proceeding to deployment, ensure you have completed the following:
Created an ECR repository for the LMS frontend

Successfully authenticated Docker with ECR
Created a proper Dockerfile for the React application
Built the Docker image locally without errors
Tagged the image with the correct ECR repository URI
Successfully pushed the image to ECR
Verified the image is available in the ECR repository
Recorded the full image URI for the deployment phase
