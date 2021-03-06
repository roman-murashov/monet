monet
=====

![Example](http://i.imgur.com/sz2Viam.jpg)

monet is a distributed image generation system for the
[neural-doodle](https://github.com/alexjc/neural-doodle) project.

monet uses a task queue and publish/subscribe messaging to scale its workers'
pool across a cluster. monet provides an HTTP API to get a list of installed
artworks, add new tasks to the queue, query progress, and get intermediate or
final results.

# Services

![Architecture](http://i.imgur.com/DbMzzpQ.png)

* API Endpoint
    * [monet-api](https://github.com/toksaitov/monet-api)
* Task Runner
    * [monet-agent](https://github.com/toksaitov/monet-agent)
* [Nerual-Doodle](https://github.com/alexjc/neural-doodle) Docker Container Images
    * [neural-doodle-cpu](https://github.com/toksaitov/neural-doodle-cpu)
    * [neural-doodle-gpu](https://github.com/toksaitov/neural-doodle-gpu)

## Prerequisites

* *Docker*, *Docker Compose* `>= 1.11`, `>= 1.7.0`

## Containerization

* `docker-compose up`: to start all services

* `docker-compose up -d`: to start the services in the background

* `docker-compose down`: to stop the services

* `docker-compose exec monet-api npm run gulp`: to recreate the `artworks`
  collection inside the `monet-artwork-db` container and import sample data from
  the `artworks` directory to the service database

* `docker-compose -f docker-compose.yml -f docker-compose.gpu.yml ...`: to work
  with the GPU variant of the *neural-doodle* image

* `docker-compose -f docker-compose.yml -f docker-compose.development.yml
   [-f docker-compose.gpu.yml] ...`: to mount project directories on the host
  machine under project directories inside containers to allow instant source
  changes throughout development without rebuilds

## Working in a Swarm Cluster Environment

* `docker-compose pull`: to download pre-built images from Docker Hub on every
  node

* `docker-compose up [-d]`: to start all services

* `docker-compose exec monet-api npm run gulp`: to import sample
  data from the `artworks` directory to the service database

* `docker-compose scale monet-agent=<number of instances>`: to start a specific
  number of instances of the *monet-agent* and spread them across the cluster

## Docker Hub

* [toksaitov/monet-api](https://hub.docker.com/r/toksaitov/monet-api)
* [toksaitov/monet-agent](https://hub.docker.com/r/toksaitov/monet-agent)
* [toksaitov/neural-doodle](https://hub.docker.com/r/toksaitov/neural-doodle)

## Sample Usage

To get a list of artworks available to transfer styles from

```bash
curl http://localhost:8080/artworks
```

To submit a doodle file `doodle.png` for processing

```bash
(echo -n '{ "artworkID": "<some artwork ID returned from the previous step>", "map": "'; base64 doodle.png; echo '" }') |
  curl -H "Content-Type: application/json" -d @- http://localhost:8080/process
```

To check the progress and get intermediate or final results

```bash
curl http://localhost:8080/tasks/<task ID returned from the previous step>
```

On Windows and OS X setups with Docker Toolbox replace *localhost* with an IP
address of a virtual machine used by Docker.

For example, to get an IP address of the default Docker virtual machine,
execute the following

```bash
docker-machine ip default
```

## Licensing

*monet* is licensed under the MIT license. See LICENSE for the full license
text.

## Credits

*Neural Doodle* is developed by [Alex J. Champandard](https://github.com/alexjc).

*monet* was created by [Dmitrii Toksaitov](https://github.com/toksaitov).
