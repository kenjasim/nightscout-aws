# Deploy nightscout onto an AWS EC2 instance

This repository contains the necessary files and configurations to deploy nightscout and nginx through docker compose onto an AWS EC2 instance.

## Prerequisites

You need to have set up an AWS instance in EC2 and have a domain or sub domian pointing to it and install docker and docker-compose on it. You can find the instructions for that [here](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html).

You also need access to a MongoDB database. You can find instructions on how to set that up [here](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) or you can sign up to MongoDB atlas [here](https://www.mongodb.com/cloud/atlas).

You should also clone this repository onto the EC2 instance.

## Installation

First you should look at the insructions found [here](https://nightscout.github.io/nightscout/setup_variables/) to see what variables you need to set up. There is an example .env file in this repository that you can use as a template. You can also use the following command to generate a random password for the API_SECRET variable:

```bash
openssl rand -base64 32
```

Next you need to replace `{YOUR_DOMAIN_HERE}` in the nginx.conf file with your domain. 

Once you have set up the .env file you can run the following command and follow the prompts to generate the lets encrypt certificates:

```bash
sudo docker compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ -d nightscout.kenanjasim.com
```

Then when you run the following command it will start the nightscout and nginx containers:

```bash
sudo docker compose --profile=nightscout --env-file=env up
```

Then you can bring them down by running:

```bash
sudo docker compose --profile=nightscout --env-file=env down
```

You should then be able to navigate to the domain you set up and see the nightscout site.

## Renewing the certificates

You can renew the certificates by running the following command:

```bash
sudo docker compose --profile=donotstart run --rm certbot renew
```