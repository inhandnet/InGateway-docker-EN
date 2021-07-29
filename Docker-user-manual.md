# InGateway902 Docker user manual
InGateway902 series edge computing gateway (IG902 for short) supports manage docker images. You can publish your docker images to IG902 to quickly deploy and run applications developed by yourself. In order to introduce how to use IG902's Docker environment, this document will demonstrate how to run an Nginx image on IG902. This image is used for open source reverse proxy server for HTTP, HTTPS, SMTP, POP3 and IMAP protocols, and load balancer, HTTP cache And web server.  <br/>
Docker is an open source application container engine that allows developers to package their applications and dependencies into a portable container and then publish it to any popular Linux machine or Windows machine. It can also be virtualized. The container is completely with the sandbox mechanism, there will be no interface between each other.

  - [1. Prepare IG902 Hardware and Network Environment](#prepare-ig902-hardware-and-network-environment)
    - [1.1 Connect IG902 to the Power Source and to a PC with a Network Cable](#connect-ig902-to-the-power-source-and-to-a-pc-with-a-network-cable)
    - [1.2 Access the IG902](#set-lan-parameters)
    - [1.3 Connect IG902 to the Internet](#set-wan-parameters)
    - [1.4 Update the firmware](#update-the-firmware)
  - [2. Enable and configure Docker manager](#enable-and-configure-docker-manager)
    - [2.1 Install Docker SDK and enable Docker manager](#install-docker-sdk-and-enable-docker-manager)
    - [2.2 Configure Docker Manager--Portainer](#configure-docker-manager-portainer)
      - [2.2.1 Access Portainer](#access-portainer)
      - [2.2.2 Add docker image](#add-docker-image)
      - [2.2.3 Configure and deploy container](#configure-and-deploy-container)
  - [Appendix](#appendix)
    - [Use Serial port for communication in container](#call-the-serial-port-in-the-container-for-communication)
    - [Set the container to run permanently](#set-the-container-to-run-permanently)
    - [Run Ubuntu in IG902](#run-ubuntu-in-ig902)
    - [Build the image through a container (create an image to save the container configuration)](#build-images-from-containers)
    - [How to download docker images from gitlab / github](#how-to-download-docker-images-from-gitlab-github)
  - [FAQ](#faq)
    - [Q1：It prompted succeed after pulling image on the "Images" page, but the image is not shown on the "Images" page.](#q1)


<a id="prepare-ig902-hardware-and-network-environment"> </a>  

## 1. Prepare IG902 Hardware and Network Environment

<a id="connect-ig902-to-the-power-source-and-to-a-pc-with-a-network-cable"> </a>  

### 1.1 Connect IG902 to the Power Source and to a PC with a Network Cable
Connect IG902 to the power source and to a PC with an Ethernet cable according to the topology diagram.   

![](images/2020-01-21-10-08-56.png)

<a id="set-lan-parameters"> </a>  

### 1.2 Access the IG902
Acess the IG902 by referring to [Access the IG902](http://manual.ig.inhandnetworks.com/en/latest/IG902-Quick-Start-Manual.html#set-lan-parameters).

<a id="set-wan-parameters"> </a>  

### 1.3 Connect IG902 to the Internet
Connect IG902 to the Internet by referring to [Connect IG902 to the Internet](http://manual.ig.inhandnetworks.com/en/latest/IG902-Quick-Start-Manual.html#set-wan-parameters).

<a id="update-the-firmware"> </a>  

### 1.4 Update the firmware
To obtain the latest firmware version of IG902 and updated functions, please visit the [Resource](https://www.inhandnetworks.com/downlist/cid-148/). To update the IG902 firmware, see [Update the IG902 software version](http://manual.ig.inhandnetworks.com/en/latest/IG902-Quick-Start-Manual.html#update-the-software).<font color=#FF0000> (The firmware version should be 2.0.0.r12057 and above)</font>

<a id="enable-and-configure-docker-manager"> </a>  

## 2. Enable and configure Docker manager

<a id="install-docker-sdk-and-enable-docker-manager"> </a>  

### 2.1 Install Docker SDK and enable Docker manager
The Docker SDK integrates the operating environment and docker image manager required to run the docker image. Before using Docker, you must install the Docker SDK. To obtain the Docker SDK, please visit the [Resource](https://www.inhandnetworks.com/downlist/cid-148/).  </br>
- Step 1: If you already have the Docker SDK, choose Edge Computing > Docker Manager page of IG902, close the Docker Manager and import the Docker SDK.  

  ![](images/2020-08-04-14-33-23.png)  

- Step 2: After importing, IG902 will automatically install the Docker SDK. The installation process usually takes 1-2 minutes. Please be patient. After successful installation, select Enable Docker Manager and click Submit.  

  ![](images/2020-08-04-14-34-15.png)  
   
- Step 3: Then you can modify the port number and login password to access the Docker manager.  

  ![](images/2020-08-04-14-36-47.png)

<a id="configure-docker-manager-portainer"> </a>  

### 2.2 Configure Docker Manager--Portainer
IG902 uses Portainer to build, manage and maintain Docker images and containers. For a detailed introduction and instructions on Portainer, please see the [Portainer official website](https://www.portainer.io/overview/). This document will show you how to add and deploy an Nginx docker image on IG902.

<a id="access-portainer"> </a>  

#### 2.2.1 Access Portainer
- Step 1: Click Portainer's access button, and Portainer will prompt you to enter your username and password. At this time, copy the user name and the set password from the Edge Computing > Docker Manager page of IG902 and click Login.  

  ![](images/2020-08-04-14-37-36.png)  

  ![](images/2020-01-21-14-36-08.png)  
   
- Step 2: After the login is successful, as shown in the figure below, select Local to use the Portainer to manage the docker image on the IG902, and then click Connect.  

  ![](images/2020-01-14-16-20-37.png)  
   
- Step 3: On the Home page of Portainer, select local to manage the docker image on IG902.  

  ![](images/2020-01-14-16-21-43.png)  
   
  Then you will jump to the local dashboard, where you can get an overview of the IG902's containers and images.  

  ![](images/2020-01-14-16-22-43.png)

<a id="add-docker-image"> </a>  

#### 2.2.2 Add docker image
There are two ways to add docker images for Portainer：
- Method 1: Enter the Local > Images page of Portainer. click "Import" to import the image.   
  
  ![](images/2020-06-29-14-50-49.png)  
   
- Method 2: Choose Local > Images page of Portainer and download the nginx docker image from DockerHub. (The time required to download the image varies depending on the size of the image; please be patient when the docker image is large)  

  ![](images/2020-01-21-15-24-52.png)  
   
  After the docker image is downloaded, you can see the corresponding docker image information in Local > Images as shown below:   

  ![](images/2020-01-21-15-28-04.png)  

<font color=#FF0000>Note: The architecture of IG902 CPU is linux/arm/v7. Only images that support linux/arm/v7 architecture can run normally in IG902. Images  of other architectures，such as window/amd64, may not be imported, pulled, or run successfully in IG902.</font>  

![](images/2020-06-28-15-20-14.png)

<a id="configure-and-deploy-container"> </a>  

#### 2.2.3 Configure and deploy container
- Step 1: Choose Local > Containers page of Portainer and click Add container to add a new container.  

  ![](images/2020-01-13-18-08-05.png)  
   
- Step 2: Configure the operating parameters for the container and deploy the container.  

  ![](images/2020-04-27-14-58-45.png)  
   
- Step 3: The container will run automatically after deployment. You can view the container running status on Portainer's Local > Containers page.  

  ![](images/2020-01-13-18-16-28.png)  
   
- Step 4: After entering the Nginx access link (IP address + port number of IG902) configured in the container in the browser, you can see the Nginx welcome page.This shows that the Nginx docker image has been running on the IG902 normally. Now, you have completed adding and deploying an Nginx docker image on the IG902.  

  ![](images/2020-01-14-17-42-52.png)

## Appendix

<a id="call-the-serial-port-in-the-container-for-communication"> </a>  

### Use Serial port for communication in container

When deploying the container, add **Volume Mapping** to the **Advanced Container Settings > Volumes** page of the Portainer. The following figure maps the files in the dev directory of IG902 to the dev directory in the container (The corresponding interface file is included in the dev directory of IG902).  

![](images/2020-07-31-16-41-20.png)    

Enable **Privileged mode** on the **Advanced Container Settings > Runtime & Resouces** page of the Portainer (if it is not enabled, using the serial port will prompt that there is no operation authority).  

![](images/2020-07-31-16-42-05.png)

At this time you can deploy the container. Enter the dev directory of the container's console, you can find the interface files such as `ttyO1` and `ttyO3`.  

![](images/2020-06-29-10-56-56.png)  

<a id="set-the-container-to-run-permanently"> </a>  

### Set the container to run permanently
On the **Advanced Container Settings > Restart Policy** page of the Portainer, set the **Restart policy** state as **Always**. Then the container will automatically restart as long as it stops running.  

![](images/2020-06-29-10-41-54.png)  

<a id="run-ubuntu-in-ig902"> </a>  

### Run Ubuntu in IG902
- Step 1: Pull the Ubuntu image on the **Local > Images** page of the Portainer, as shown below：  
  
  ![](images/2020-06-29-13-51-58.png)  

- Step 2: Go to **Local > Containers** page of the Portainer, click **Add container** to add a new container. Select the Ubuntu image downloaded in the previous step as the containers image. At the same time, click **Advanced Container Settings > Command & Logging**, check **Interactive & TTY** in the **Console**. After the configuration is complete, click **Deploy the container** to deploy the container.  
  
  ![](images/2020-06-29-13-45-37.png)  

  ![](images/2020-06-29-13-52-14.png)

- Step 3: After deployment, on the **Local > Containers** page of Portainer, you can see that the container is running. Click on **Exec Console** to log in to the console.  
  
  ![](images/2020-06-29-13-56-39.png)  

  Click on **Connect** in **Execute** page, and then enter the container to run the corresponding command.  

  ![](images/2020-06-29-13-57-40.png)  

  ![](images/2020-06-29-13-58-57.png)  

<a id="build-images-from-containers"> </a>  

### Build the image through a container (create an image to save the container configuration)
When the corresponding development or operating environment has been configured in the container, if you need to save the environment configuration, you can create a new image based on the container's changes. The method is as follows (take the ping tool installation in the Ubuntu container as an example):

- Step 1: Configure the development or runtime environment of the container.

  Run the `apt-get update` and `apt-get install inetutils-ping` commands to install the ping tool.  

  ![](images/2020-06-29-14-39-40.png)  

  ![](images/2020-06-29-14-41-05.png)  

- Step 2: Create an image based on the container.   
  
  Click on the container name to enter the details of the container page.  

  ![](images/2020-06-29-14-42-28.png)  

  Configure the name of the image in the **Create Image** on the details page and click on **Create**.  

  ![](images/2020-06-29-14-43-06.png)

- Step 3: Use the created image to deploy the container.  

  After the image is created, you can view it in **Local > Images** page of Portainer.  

  ![](images/2020-06-29-14-45-39.png)

  Then deploy an ubuntu container using the image on the **Local > Containers** page of Portainer. As shown below:  

  ![](images/2020-06-29-14-46-48.png)  

  Log in to the console of the container, you can use your ping command.  

  ![](images/2020-06-29-14-47-58.png)

<a id="how-to-download-docker-images-from-gitlab-github"> </a>  

### How to download docker images from gitlab / github 
Choose Local > Registries page of Portainer and click Add registry to add a docker mirror repository (must be a public repository).  

![](images/2020-01-19-10-39-19.png)  

Then select Custom registry and configure the mirror repository information. After configuration, click Add registry.  

![](images/2020-01-21-15-40-36.png)  

After the mirror repository is successfully added, you can see the web page as shown below:  

![](images/2020-01-21-15-41-25.png)  

After the addition is successful, you can select the configured image repository when pulling the docker image.  

![](images/2020-01-21-15-41-59.png)

## FAQ

<a id="q1"> </a>  

### Q1：It prompted succeed after pulling image on the "Images" page, but the image is not shown on the "Images" page.

A1: That is because the architecture of IG902 CPU is linux/arm/v7. Only images that support linux/arm/v7 architecture can run normally in IG902. Images of other architectures, such as window/amd64, may not be imported, pulled, or run successfully in IG902.  

![](images/2020-06-28-15-25-10.png)  

![](images/2020-06-28-15-25-46.png)