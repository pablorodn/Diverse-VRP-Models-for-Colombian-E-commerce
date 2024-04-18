# Diverse-VRP-Models-for-Colombian-E-commerce
Repository dedicated to providing a variety of Vehicle Routing Problem (VRP) models tailored specifically to address the logistics challenges of e-commerce businesses in Colombia
funciona
# Diverse Last-Mile Delivery Solutions 

This repository is dedicated to providing a variety of Vehicle Routing Problem (VRP) models tailored specifically to address the logistics challenges of last-mile deliveries for e-commerce businesses operating in Colombia. The Last-Mile Delivery Problem involves optimizing the routes of vehicles to efficiently serve customers located in dense urban areas, taking into account factors such as traffic congestion, road restrictions, and specific delivery time windows.

## About the Problem
The Last-Mile Delivery Problem (LMDP) in Bogotá poses unique challenges due to the city's complex urban landscape, diverse traffic patterns, and varying customer demands. Efficiently navigating these challenges is crucial for e-commerce businesses to meet customer expectations while minimizing transportation costs and environmental impact.

## Our Approach
In this repository, we explore and propose various VRP models and solutions tailored to the specific requirements of last-mile delivery operations in Colombia. Our goal is to provide e-commerce businesses with practical and efficient routing strategies that consider factors such as Colombia's traffic conditions, delivery time windows, vehicle capacity limits, and environmental considerations.

## Contributions Welcome
We invite contributions from researchers, developers, and logistics experts to collaborate on developing and refining VRP models and solutions tailored to Bogotá's last-mile delivery challenges. Together, we can work towards enhancing the efficiency, sustainability, and reliability of e-commerce logistics in Bogotá and beyond.

## Step 1: Setting up the OSRM Server

The first crucial step is establishing the OSRM server. This server acts as a fundamental tool in our logistics solution, providing us with crucial data to tackle optimization challenges efficiently. You can follow the step-by-step guide provided in the repository folder [here](https://github.com/pablorodn/Diverse-VRP-Models-for-Colombian-E-commerce/tree/main/docker-osrm-server), which contains detailed instructions for creating the server with Docker enabled.

This server's significance lies in its capability to generate estimated time and distance matrices between nodes or points of interest. These matrices serve as foundational data for solving the Traveling Salesman Problem (TSP) and its extension to Vehicle Routing Problems (VRP). By having access to accurate estimations of travel times and distances, we can optimize routes, minimize costs, and enhance overall logistics efficiency significantly.






### RUN DOCKER Command
sudo docker run -t -i -p 5000:5000 -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-routed --algorithm mld /data/colombia-latest.osrm
