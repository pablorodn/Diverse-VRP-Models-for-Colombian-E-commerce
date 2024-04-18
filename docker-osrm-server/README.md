## Introduction

This guide outlines the steps to set up an OSRM (Open Source Routing Machine) server. The process draws inspiration from a tutorial by [Afian Anwar](https://www.linkedin.com/in/afian-anwar/), Founder & CEO of [Afi Labs](https://www.afi.io/), which originally focused on creating a geocoding and reverse geocoding service using OpenStreetMap data. You can find the original tutorial [here](https://www.afi.io/blog/building-a-free-geocoding-and-reverse-geocoding-service-with-openstreetmap/). However, modifications have been made to adapt it for Colombian usage, along with adjustments specific to server configurations on Ubuntu.

## Step 1: Creating the EC2 Server on AWS

The first step is to create an EC2 server from the Amazon Web Services (AWS) console. It's important to note that the server should be an Ubuntu instance, not one of the Amazon Machine Images (AMIs). Additionally, if you choose a server from the free tier, the memory capacity may not support the installation of the server. Therefore, it's suggested to deploy it on a `t2.medium` instance, which has 4GB of RAM.

Here's a comparison of the characteristics of the `t2.medium` instance compared to the free tier:
- `t2.medium`: 4GB RAM
- Free tier: Memory capacity varies, typically less than 1GB

For a detailed tutorial on how to create the server on AWS, you can refer to this [YouTube video](https://www.youtube.com/watch?v=lZbxBathlpg) by Soy Adan Najera.

## Step 2: Connecting to the EC2 Instance

After creating the EC2 instance, you can connect to it either directly from the AWS console or through SSH by logging in from your terminal.

### Connecting via AWS Console:
1. Navigate to the EC2 Dashboard in the AWS Management Console.
2. Select your instance from the list of instances.
3. Click on the "Connect" button and follow the on-screen instructions to connect using the browser-based SSH client.

### Connecting via SSH:
1. Open your terminal.
2. Use the SSH command to connect to your instance. The command typically looks like this:

Replace `/path/to/your/key.pem` with the path to your private key file and `your-instance-public-ip` with the public IP address of your EC2 instance.

3. Once connected, authenticate as the root user by running: `sudo -i`


After connecting to the instance, you can proceed with the necessary configurations.

### Console Image:
<img width="700" alt="image" src="https://github.com/pablorodn/Diverse-VRP-Models-for-Colombian-E-commerce/assets/113043356/9c8dee52-f5c2-4f14-a147-afcbae6ad1b1">


"I will now begin basing my setup on the tutorial mentioned,
### Step 3: Install Docker Engine

To install Docker Engine on EC2 for the first time, you should begin by setting up the Docker repository with apt (Advanced Package Tooling), a command-line package management tool used in Debian-based Linux distributions, including Ubuntu. Once this is done, you'll be able to install and keep Docker up-to-date by using the apt-get command."
During the execution of these commands, you may be prompted with a message asking if you want to continue (Y/n). It's important to always respond with "Y" to ensure the process continues without interruptions.

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

```

After executing these commands, you should install the latest version of Docker. Use the following commands in the console:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```
To verify that everything is functioning correctly, execute the following command in the console:

```bash
sudo docker run hello-world
```
If you have successfully installed Docker, you should see the following output:

<img width="700" alt="image" src="https://github.com/pablorodn/Diverse-VRP-Models-for-Colombian-E-commerce/assets/113043356/f9552384-90a5-46b6-8531-9cdab5b7e9cb">

### Step 4: Installing osrm-backend and Running the OSRM API

Now, you're ready to move forward with installing osrm-backend and launching the OSRM API. In this step, we'll guide you through the process, ensuring a smooth setup.
### Step 5: Downloading OSM Data for Colombia

The next step involves downloading the OpenStreetMap (OSM) data for Colombia. You can obtain the data from the OSRM website. 

Please follow these steps:

1. Navigate to the OSRM website by clicking [here](https://download.geofabrik.de/south-america/colombia-latest.osm.pbf).
   
2. Copy the link to the Colombia OSM data from the webpage.

Once you have copied the link, proceed to download the data to your EC2 instance.

```bash
wget http://download.geofabrik.de/south-america/colombia-latest.osm.pbf
```

After downloading the data, you can proceed with the installation and configuration of osrm-backend.

### Console Image:

<img width="800" alt="image" src="https://github.com/pablorodn/Diverse-VRP-Models-for-Colombian-E-commerce/assets/113043356/29767487-c212-496e-81c5-204908375424">

