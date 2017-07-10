# Installation

The Value Set Workbench includes a [Docker Compose](https://docs.docker.com/compose/)-based system for deploying the suite of tools comprising the Value Set Workbench.

The following components will be installed:

* [valueset-workbench](https://github.com/valuesetworkbench/valueset-workbench) - The Value Set Workbench web-based user interface.
* [arangodb-service](https://github.com/valuesetworkbench/arangodb-service) - A [CTS2-compliant](http://www.omg.org/spec/CTS2/) terminology service.
* [valueset-standardizer](https://github.com/valuesetworkbench/valueset-standardizer) - An automated value set standardization service.
* [valueset-automap](https://github.com/valuesetworkbench/valueset-automap) - An automated value set mapping service.
* An [Elasticsearch](https://www.elastic.co/products/elasticsearch) service.
* An [ArangoDB](https://www.arangodb.com/) graph database.

## Prerequisites
* [Docker](https://docs.docker.com/engine/installation/) >= 1.12.0
* [docker-compose](https://docs.docker.com/compose/install/) >= 1.8.0

## Installation Steps
```
git clone https://github.com/valuesetworkbench/valueset-workbench-docker.git
cd valueset-workbench-docker
./start.sh
```
By default, when started the Value Set Workbench will be available at https://localhost/

On a Mac, you will need to substitute ```localhost``` with the IP address the Docker Machine. You can find this address by running ```docker-machine ip```.

## Configuration Options

### External Host Name
If you are deplying the Value Set Workbench in a production environment, you may need to tell it the hostname where it will be accessed. This is necessary for several functions including OAuth2 callbacks.

You may sepecify an external host via the ```start.sh``` script:
```
./start.sh yourhost.com
```

If you do not provide an external host parameter, default behaviour depends on the deployment operating system:

* **Linux** - ```localhost```
* **OSX** - the result of ```docker-machine ip```

### Custom OAuth2
The Value Set Workbench allows integrates with your existing OAuth2 strategy. See [here for details](custom_oauth).

### Custom SSL
By default, the Value Set Workbench will deploy with a self-signed SSL certificate. To use your own, place your custom Private Key and Certificate in the following locations:

* **Private key:** ```valueset-workbench-docker/ssl/key.pem```
* **Certificate:** ```valueset-workbench-docker/ssl/ca.pem```