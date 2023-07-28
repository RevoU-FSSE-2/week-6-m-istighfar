# Dockerize Project - Step-by-Step Guide

In this guide, I will walk you through the process of dockerizing a project using a Dockerfile. Dockerizing your project allows you to package all the dependencies and configurations required to run your application inside a Docker container, making it portable and reproducible across different environments.

## Step 1: Docker Installation

### System Requirements

Before installing Docker Desktop on your Windows machine, ensure that your system meets the following requirements:

- Windows 10 Pro, Enterprise, or Education (64-bit), with at least version 1903 (Build 18362) or higher.
- Hyper-V enabled (Most Windows editions have this feature, but check your version to be sure).
- For hardware virtualization, your CPU must support one of the following virtualization extensions: Intel VT-x or AMD-V.

### Install Docker Desktop

Follow these steps to install Docker Desktop on your Windows machine:

1. Download the Docker Desktop installer from the official website: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

2. Double-click the downloaded installer file (e.g., `Docker Desktop Installer.exe`) to start the installation process.

3. You might be prompted by User Account Control (UAC) to allow the installer to make changes. Click "Yes" to proceed.

4. The installation wizard will guide you through the process. Read and accept the license agreement, then select the installation options you prefer.

5. By default, the installer will enable the "Use Windows containers instead of Linux containers" option. This setting allows you to run Windows containers. If you need Linux containers in the future, you can switch this option from the Docker Desktop system tray menu.

6. Click "Install" to begin the installation.

7. Docker Desktop will download and install all the necessary components. This may take some time depending on your internet connection.

8. Once the installation is complete, the Docker Desktop application will launch, and you should see a Docker icon in your system tray.

### Verify Installation

After installing Docker Desktop, verify that Docker is running correctly:

1. Click on the Docker icon in the system tray to open the Docker Desktop application.

2. The application will take a few moments to start Docker services. Once it's ready, you should see a message indicating that Docker is running.

3. Open a PowerShell or Command Prompt window, and run the following command to check the Docker version:

   ```
   docker --version
   ```

   ![ss version](/readme-images/vers.png)

## Step 2: Set Up Your Project

Before you start dockerizing your project, make sure your project is structured and configured correctly to run outside the container. Ensure that all the necessary dependencies and configurations are in place.

For demonstration purposes, let's assume you have a simple Node.js web application with the following structure:

```
W6 ASSIGNTMENT/
  ├── app.js
  ├── package.json
```

The `app.js` file contains your Node.js application code, and `package.json` holds the project's dependencies.

## Step 3: Create a Dockerfile

Next, you'll need to create a `Dockerfile` in the root directory of your project. The `Dockerfile` defines the instructions to build a Docker image for your project.

Here's a sample `Dockerfile` for our Node.js web application:

```Dockerfile
FROM node:18

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --omit=dev

# Bundle app source
COPY . .

EXPOSE 3001
CMD [ "node", "app.js" ]
```

## Step 4: Build the Docker Image

With the `Dockerfile` in place, you can now build the Docker image for your project. Open a terminal or Command Prompt in the root directory of your project and execute the following command:

```
docker build -t w6 .
```

Explanation:

- `docker build`: Command to build a Docker image.
- `-t w6`: Tags the image with the name `w6`.
- `.`: The `.` indicates the current directory, where the `Dockerfile` is located.

This will start the image build process, which may take a while depending on your project's size and dependencies.

After the build process completes, verify that the Docker images were successfully created by running:

```bash
docker images
```

You should see the `w6` image listed in the output.


## Step 5: Run the Docker Container

Once the image is built successfully, you can run a Docker container based on that image. Use the following command:

```
docker run -p 3002:3001 w6
```

Explanation:

- `docker run`: Command to run a Docker container.
- `-p 3002:3001`: Maps port 3002 from the host machine to port 3001 inside the container. Adjust this if your application uses a different port.
- `w6`: The name of the Docker image to use for the container.

## Step 5: Verify the Running Container

Your Docker container should now be running. To verify that your project is running correctly inside the container, open a web browser and navigate to `http://localhost:3002` (or the port you specified in the `docker run` command). You should see your Node.js web application in action.

![ss local](/readme-images/running.png)

## Conclusion

Congratulations! You've successfully dockerized your project using a Dockerfile, built a Docker image, and ran a container to execute your application inside it. Docker containers provide a consistent and isolated environment, making it easy to deploy your project across various platforms and environments.

Keep in mind that this guide covered a simple Node.js web application as an example. The process may vary depending on your project's technology stack and requirements, but the basic principles remain the same. Happy Dockerizing! 🐳
