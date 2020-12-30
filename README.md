# Hadoop on Docker
This repository contains all the files to install an Hadoop simple cluster on Docker.   
This architecture is not meant to a be production system. The purpose here is to provide an Hadoop cluster infrastructure for tests only.
By using this code, you will deploy a Hadoop cluster composed of one master node and two slave nodes. Each nodes is hosted in a standalone container in a Docker infrastructure that can be either local on your computer or on a server in the Cloud.  
The base version provide only HDFS and YARN. It can be used as base-image to add more components to the configuration (Spark, Hive, Pig, ...). In the future update, I will provide the Dockerfile to install these components.  

## Supported Versions
Base system: Hadoop 2.10.1

## Repository Structure
### Base
Contains the scripts and Dockerfile to create an Hadoop base image and to deploy automatically the cluster on your Docker environment.
The dockerfile is used to build the image is based on the lastest Ubuntu image (20.04 at the time of writing). In addition to this OS, the Dockerfile will install Hadoop version 2.10.1, copy the config files (these config files are originally provided by https://github.com/kiwenlau/hadoop-cluster-docker) and finally it will format the nodename.  
The image is built with the command:  

`$ sudo docker build -t <image name and tag> <path to the Dockerfile>`

Once the image is built, you can verify if it is correctly done by running:

`$ sudo docker image ls`

The image must appear in the list.

*__WARNING:__ The image is quite big (around 2.4 GB)*  

one can use the start-containers shell script to deploy automatically the cluster. The syntax of the command is:

`$ sudo ./start-containers -i <hadoop image name>`

This script check is the containers are already deployed, if yes it will remove them and deploy them again from the image. Once the container are started, it will execute a script on the master node to start hdfs and yarn on the master and to add the two slave nodes to the configuration.  
Once the start of the cluster is done, the script executes the command jps (java ps) on each nodes and display the command output. Use this output to check that the following process are running ont the nodes.  
  
__Master Node :__  
ResourceManager  
NameNode  
SecondaryNameNode  
  
__Slave Nodes:__  
(On each nodes)  
DataNode  
NodeManager  

To check that the containers are deployed and running use the following command:

`$ sudo docker ps --filter "name=hadoop" --filter "status=running"`  

The output must show three containers (*hadoop-master*, *hadoop-slave1* and *hadoop-slave2*) running. From this point your hadoop configuration is up and running.
