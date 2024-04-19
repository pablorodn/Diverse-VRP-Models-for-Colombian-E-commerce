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

<img width="800" alt="image" src="https://github.com/pablorodn/Diverse-VRP-Models-for-Colombian-E-commerce/assets/113043356/29767487-c212-496e-81c5-204908375424">

Once you have copied the link, proceed to download the data to your EC2 instance.

```bash
wget http://download.geofabrik.de/south-america/colombia-latest.osm.pbf
```

After downloading the data, you can proceed with the installation and configuration of osrm-backend.


Spin up the osrm-backend image
```bash
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-extract -p /opt/car.lua /data/colombia-latest.osm.pbf || echo "osrm-extract failed"
```
Partition the graph
```bash
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-partition /data/colombia-latest.osrm || echo "osrm-partition failed"

```
Contract the graph
```bash
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-customize /data/colombia-latest.osrm || echo "osrm-customize failed"
```


### Step 6: Running OSRM Docker Container

Now, it's time to run the OSRM Docker container using the following command:

```bash
sudo docker run -t -i -p 5000:5000 -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-routed --algorithm mld /data/colombia-latest.osrm
```
### Success! OSRM Server Running

If everything has been set up correctly, you should see the following output after running the OSRM Docker container:

<img width="700" alt="image" src="https://github.com/pablorodn/Diverse-VRP-Models-for-Colombian-E-commerce/assets/113043356/62ad2ab5-fbed-4bc1-8baa-208203f4b61a">

This screen indicates that the OSRM server is successfully running and ready to handle routing requests. You can now integrate the OSRM API into your applications or use it for routing purposes.


### Testing the OSRM API

This endpoint is exposed on port 5000 within the Docker container, hence it's accessible via the specified URL. You can make requests to this endpoint using your preferred programming language.

To test the OSRM API and retrieve distance and duration matrices between multiple points, you need to provide the coordinates of the points to evaluate. 
You need to replace 'your_ec2_ip_address' with the IP address of your AWS EC2 instance. The API endpoint for this purpose is:
```
http://(your_ec2_ip_address):5000/table/v1/driving/-74.109090,4.659532;-77.28771824600418,1.2284307220812025?annotations=distance,duration
```

This endpoint accepts latitude and longitude coordinates for two or more points separated by semicolons (;). Additionally, you can specify the annotations parameter to indicate which matrices you want in the response (distance and duration in this case).

#### Input Parameters:
- **Coordinates:** Latitude and longitude of each point separated by semicolons. Example: `-74.109090,4.659532;-77.28771,1.228430`


#### Example API Response:

In the response JSON, the "distances" array contains the distance matrix, the "durations" array contains the duration matrix, and the "destinations" and "sources" arrays provide information about each point including its hint, distance, name, and location coordinates.

```
{
  "code": "Ok",
  "distances": [[0, 766225.4], [764959.4, 0]],
  "destinations": [
    {
      "hint": "pBkugAAaLoAUAAAADQAAAG4AAAAzAAAAfwViQc5xD0EeXphCxBwOQhQAAAANAAAAbgAAADMAAAAQAgAAeS-V-10ZRwBeL5X7TBlHAAEAPwlSPa5d",
      "distance": 3.536687349,
      "name": "Calle 25B",
      "location": [-74.109063, 4.659549]
    },
    {
      "hint": "uNwOgNrcDoCYAAAAggAAAAAAAAAAAAAAFTTMQpZVrEIAAAAAAAAAAJgAAACCAAAAAAAAAAAAAAAQAgAAzq5k-4S-EgDarmT7j74SAAAAzxNSPa5d",
      "distance": 1.806400499,
      "name": "Calle 18 A",
      "location": [-77.28773, 1.22842]
    }
  ],
  "durations": [[0, 40960.5], [40307.5, 0]],
  "sources": [
    {
      "hint": "pBkugAAaLoAUAAAADQAAAG4AAAAzAAAAfwViQc5xD0EeXphCxBwOQhQAAAANAAAAbgAAADMAAAAQAgAAeS-V-10ZRwBeL5X7TBlHAAEAPwlSPa5d",
      "distance": 3.536687349,
      "name": "Calle 25B",
      "location": [-74.109063, 4.659549]
    },
    {
      "hint": "uNwOgNrcDoCYAAAAggAAAAAAAAAAAAAAFTTMQpZVrEIAAAAAAAAAAJgAAACCAAAAAAAAAAAAAAAQAgAAzq5k-4S-EgDarmT7j74SAAAAzxNSPa5d",
      "distance": 1.806400499,
      "name": "Calle 18 A",
      "location": [-77.28773, 1.22842]
    }
  ]
}
```

The "hint" field in each destination and source object is a string used internally by OSRM to refer to specific locations. It helps in identifying the corresponding points in subsequent requests or when interpreting the response.

Feel free to modify the input coordinates and explore different scenarios to test the capabilities of the OSRM API.

### Summary and Additional Recommendations

Congratulations on successfully setting up your OSRM server using Docker on an AWS EC2 instance! Here's a summary of the steps you've followed so far:

1. **Creating the EC2 Server on AWS:**
   - You configured an EC2 server in the Amazon Web Services console, choosing an Ubuntu instance with at least 4GB of RAM to ensure optimal performance.

2. **Connecting to the EC2 Instance:**
   - You connected to the EC2 instance both from the AWS console and via SSH, ensuring access to the command line for configurations.

3. **Installing Docker Engine:**
   - You installed Docker Engine on your EC2 instance, setting up the Docker repository and then installing the latest version of Docker. You verified the installation by running a test container.

4. **Installing and Running osrm-backend:**
   - You downloaded OpenStreetMap data for Colombia and then installed and ran the osrm-backend container to process the data and launch the OSRM routing service.


### Additional Recommendations:

- **Security:**
  Ensure to apply proper security practices, such as configuring firewall rules and restricting SSH access only to trusted IP addresses.

- **Monitoring and Maintenance:**
  Implement monitoring tools to monitor the performance and availability of the OSRM server, and establish regular backup procedures for critical data.

- **Scalability:**
  Consider implementing an auto-scaling solution for your EC2 instance, especially if you anticipate an increase in traffic load in the future.

- **Performance Optimization:**
  Experiment with different OSRM configurations and parameters to optimize the server's performance, especially in terms of routing speed and resource consumption.

By following these steps and additional recommendations, you'll be well on your way to effectively maintaining and utilizing your OSRM server on AWS. Good luck with your project!


