## Introduction

This guide outlines the steps to set up an OSRM (Open Source Routing Machine) server tailored for Colombian logistics challenges. The process draws inspiration from a tutorial by [Afian Anwar](https://www.linkedin.com/in/afian-anwar/), Founder & CEO of [Afi Labs](https://www.afi.io/), which originally focused on creating a geocoding and reverse geocoding service using OpenStreetMap data. You can find the original tutorial [here](https://www.afi.io/blog/building-a-free-geocoding-and-reverse-geocoding-service-with-openstreetmap/). However, modifications have been made to adapt it for Colombian usage, along with adjustments specific to server configurations on Ubuntu.

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
<img width="700" alt="image" src="https://github.com/pablorodn/Diverse-VRP-Models-for-Colombian-E-commerce/assets/113043356/6e992858-aa95-4fed-84db-ee08102df2dc">


