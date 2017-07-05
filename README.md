# cantiz-anomaly-engine
An API layer to numenta nupic anomaly engine.
# **Getting started with Cantiz Anomaly Engine**

Cantiz anomaly engine has integrated an application programming layer over the NuPIC's *online prediction framework*. This page contains information about how to install and use cantiz anomaly engine.


## **Installing And Using Cantiz anomaly engine**

Cantiz anomaly engine has been developed into a docker image and is available in the docker hub. Dependencies Needed to be  installed are: 
- Docker
- Docker-compose

You can download docker from: [install docker](https://docs.docker.com/engine/installation/) and docker-compose from: [Install docker compose](https://docs.docker.com/compose/install/)

-once you have downloaded both docker and docker compose you can clone the git hub directory:[cantiz anomaly engine](https://github.com/team-attinad/cantiz-anomaly-engine) and get the docker-compose.yml file.

-Within the docker compose file change the local machine directory path under volumes key to a directory path in your system where you want to save file.

Then in terminal run the docker compose file using the command:
```
docker-compose up
```

 

 ### Let us look at how online prediction framework function's

**Swarming** is the name of the process where OPF uses a sample data set and a description of the swarm to create a model of the data set. It uses its classes like "ModelFactory" and "Permutations_runner" to run the swarm and create and save the Best model opted. Read more about the swarming process [here](http://nupic.docs.numenta.org/stable/guides/swarming/running.html)

**Anomaly** detection OPF uses the model created by swarming process to evaluate the new inputs and provides an anomaly score. Read  more how does OPF calculates the anomaly score [here](https://arxiv.org/pdf/1607.02480.pdf)

## How does Cantiz anomaly integrates with OPF

Cantiz anomaly engine allows users to create a persisting project with OPF in a Docker Container. It exposes an API layer using which users can do things like create projects, add description of the data etc. This helps the users to have a more reliable and flexible interface for OPF. Using it as docker container 

### Cantiz anomaly engine does the following things:

**Project Creation** : It allows users to create a project Through an API interface. The API takes project name and version number as arguments. This API provides a storage space mapped to user's project, so that the attributes of the project developed during swarming and detection time are persisted in the docker container.


**Project Description** : Created projects needs a description of the data fields that is used by OPF during swarming. This description is provided as Json file. example of the Json file is given in the API specification. The basic keys needed in the project description are Field name, Field type, Max value, Min Value of the field. The model created during the swarming depends a lot on the max and min value so if the field is a numeric one try to keep the max-min range very realistic. 


**Sample data set** : OPF needs a sample of the data to create the model of the data. so users have to provide sample data as a csv file into the engine which is then fed into the OPF and swarming is started automatically. The format of the csv file is given [here](http://nupic.docs.numenta.org/stable/quick-start/example-data.html). Swarming process along with all this needs a mysql database which is automatically added when the Docker container is made live. 


**Swarm status** : Users can check the status of their swarm using API.


**Anomaly score**: When the swarming is successfully done a model is created , this model is loaded and used to calculate anomaly score in a particular project. Users can again avail this using an API, the anomaly score is printed on the users screen for every data point fed in. Anomaly score is between 0 and 1 so score close to zero means less anomalous and score close to 1 means more anomalous. Initially when the streaming data is fed into the engine the anomaly score will be 1 for few values until the model adjusts it to the cycle. Once a Proper cycle of users data trend is seen by the model more accurate anomaly scores will be given. When the model is fed streaming data for the first time The anomaly scores reported will be point anomalies it will take the model to witness the entire data trend once to provide accurate temporal anomalies.                                                           

                                   
The API specifications are provided [Here](https://attinadsoftware.atlassian.net/wiki/display/CIPDDM/Anomaly+Project+Creation%2C+Configuration+and+Swarming)
 
