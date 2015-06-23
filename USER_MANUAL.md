# Nebula


Try our [application](https://nebula.civicactions.com/) and give us feedback by opening an issue!

And our [api](https://nebulaapi.civicactions.com/) will someday be useful, but is not 
ready now.

This is our application to 18F's Agile BPA RFQ.  However, it is all freely sharable, and we aim to make a product 
truly valuable. Feel free to fork this code.

## Licence

For licence details, including licences of third-party free and open source software incorporated into this repository please see the LICENSE.md file.

## Development environment setup

The development environment is fully self-contained, and is based on Docker and Docker Compose.

### Requirements
1. [Docker](https://www.docker.com/)
1. [Docker Compose](https://docs.docker.com/compose/)

### Instructions

Clone the repository, and change to the project directory:
```bash
git clone git@github.com:CivicActions/nebula.git
cd nebula
```

If you are using boot2docker, make sure it is started up and it's shell environment variable are available before continuing.

To start docker containers, initiate database schemas and import:
```
./bin/build
```
The first run will take a while. Use this command to restart containers if you reboot your workstation.

* The frontend component will be available on port 2086 http://localhost:2086 (if on boot2docker it will be on your boot2docker IP).
* The backend component will be available on port 2095 http://localhost:2095 (if on boot2docker it will be on your boot2docker IP).
* To view logs run `docker-compose logs`.
* To stop the containers run `docker-compose stop`.
* If the backendphp container is modified, rebuild the containers using `docker-compose rm` followed by `./bin/build`.

## Testing

There is a PHPUnit test framework available in the "testing" directory, with tests in the "testing/tests" directory. Guzzle is available for convenient testing of http endpoints.

To execute the tests run:
```bash
./bin/run-tests
```

A JUnit format report.xml is available in the "testing" directory. You can include additional PHPUnit arguments after the above command, as needed.

### Automated Testing

A sample Jenkins configuration is available in the devops/jenkins/testing/config.xml directory. This will, upon each git push to github:
* Bootstrap docker-compose and start the containers.
* Run the tests and record the results.
* Report the result to a Slack channel.

## Deployment

This deploys an instance of the frontend and an instance of the backend on Amazon Web Services instances, then configures DNS (with CDN and SSl) for each IP using Cloudflare. Docker Machine is used to provision and bootstrap the instances. The AWS CLI is used to manage network configuration and a Cloudflare CLI is used to configure Cloudflare (the AWS CLI and Cloudflare tools are run via Docker, so dependencies are minimized).

### Requirements
1. [Docker](https://www.docker.com/)
1. [Docker Compose](https://docs.docker.com/compose/)
1. [Docker Machine](https://docs.docker.com/machine/)
1. Amazon Web Services account and API keys.
1. Cloudflare account and API keys.

### Instructions

Clone the repository, and change to the project directory:
```bash
git clone git@github.com:CivicActions/nebula.git
cd nebula
```

Create the following environment variables, containing your AWS and Cloudflare access details:
```
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
export AWS_VPC_ID=
export AWS_DEFAULT_REGION=
export CLOUDFLARE_EMAIL=
export CLOUDFLARE_TOKEN=
```

Run the ./bin/deploy script to deploy the frontend and backend respectively, where the second parameter is the subdomain to deploy two, and the third is a Cloudflare DNS hosted domain name. For example:
```
./bin/deploy frontend nebula civicactions.com
./bin/deploy backend nebulaapi civicactions.com
```
Will deploy to https://nebula.civicactions.com/ and https://nebulaapi.civicactions.com/.