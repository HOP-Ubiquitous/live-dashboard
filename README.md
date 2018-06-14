# Smart Cities Dashboard

## Introduction
Smart Cities Dashboard is a web service interfaces that provides end-users with a simple and user-friendly dashboard allowing a map based representation of the devices connected. Currently the application is able to represent location, real-time sensor information and some interesting aggregated insights in the form of charts. 
The current service version is relying on Google Maps API to create a web service that draws a map of the devices deployed along the world.
- All the devices are shown in real time taking advantage of the Orion’s publish/subscribe mechanism. In this way, updated values for sensors and location are assured.
- The dashboard split the data information between real time data and historical data. The first one is provided as soon as it is gathered. The second one is gathered and aggregated by the STH Comet. 
- The dashboard has been created providing a well-defined user experience. Each point representing a device can be clicked deploying custom tabs regarding the sensors deployed. 

## Architecture
Currently the service architecture is made up of a server that interacts directly with the STH Comet and Orion Context Broker FIWARE components requesting devices data, and a web page in which the data is represented based on a custom map.
![alt text](https://github.com/HOP-Ubiquitous/live-dashboard/raw/master/images/smartcities-dashboard-fiware-architecture.png "High-level architecture")


### Service components
- Fiware IoT Stack: the solution is based on the main FIWARE components. 
  - The device information is gathered through different IoT Agents regarding the IoT protocol used. 
  - This information is provided to Orion Context Broker. Orion will provide the last value information when is requested by the backend and, using its publish/subscribe mechanism, Orion will notify Cygnus each time a value is received.
  - Cygnus will gather and store this information according to the databases connected. In our case MongoDB and the STH Comet service.
  - When requested, STH will provide historical and aggregated data of the different devices registered in Orion.
- SmartCities Backend: this service acts as a bridge requesting and adapting the information provided by Orion Context Broker and STH Comet and sending it to the frontend.
- SmartCities Frontend: this service provides a graphical interface to access and visualize the different devices. Map and chart representations are provided. 

## Configuration

### Frontend configuration 
Three environmental variables have to been taking into account in order create custom configurations of the component:
- SITE_URL → allows dockerized nginx to redirect the incoming traffic base on the domain name
- GOOGLE_MAPS_API → allows the user to consume Google Maps API. This environmental variable is required
- SYNCHRONICITY_BACKEND → backend url.

### Backend configuration
Four environmental variables have to been taking into account in order create custom configurations of the component:
- ORION_URL → Sets the url pointing the Orion instance out.
- ORION_SERVICE → Sets the service header in Orion requests.
- ORION_SERVICEPATH → Sets the service-path header in Orion requests
- DJANGO_DEBUG=False → Enable or disable the django debug flag.

## Build, deploy and run
The component is fully integrated with Docker. This repository provides a docker-compose file allowing build custom images, and official images can be found in HOPU's registries. 

#### Run docker-compose file
```
docker-compose up -d
```

