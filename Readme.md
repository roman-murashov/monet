monet
=====

![Architecture](http://i.imgur.com/DbMzzpQ.png)

monet is a distributed image generation system for the
[neural-doodle](https://github.com/alexjc/neural-doodle) project.

monet uses a task queue and publish/subscribe messaging to scale its workers'
pool across a cluster. monet provides an HTTP API to get a list of installed
artworks, add new tasks to the queue, query progress, and get intermediate or
final results.

# Services

* API Endpoint
    * [monet-api](https://github.com/toksaitov/monet-api)
* Task Runner
    * [monet-agent](https://github.com/toksaitov/monet-agent)
* [Nerual-Doodle](https://github.com/alexjc/neural-doodle) Docker Container Images
    * [neural-doodle-cpu](https://github.com/toksaitov/neural-doodle-cpu)
    * [neural-doodle-gpu](https://github.com/toksaitov/neural-doodle-gpu)

## Containerization

* `docker-compose build`: to build all *monet* images

* `docker-compose up`: to start all services

* `docker-compose up -d`: to start the services in the background

* `docker-compose down`: to stop the services

* `docker-compose -f docker-compose.yml -f docker-compose.gpu.yml ...`: to work
  with the GPU variant of the *neural-doodle* image

* `docker-compose -f docker-compose.yml -f docker-compose.development.yml
   [-f docker-compose.gpu.yml] ...`: to mount project directories on the host
  machine under project directories inside containers to allow instant source
  changes throughout development without rebuilds.

## Licensing

*monet* is licensed under the MIT license. See LICENSE for the full license
text.

## Credits

*Neural Doodle* is developed by [Alex J. Champandard](https://github.com/alexjc).

*monet* was created by [Dmitrii Toksaitov](https://github.com/toksaitov).
