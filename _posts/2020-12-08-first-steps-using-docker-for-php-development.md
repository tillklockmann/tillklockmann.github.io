---
layout: post
title: "First steps using Docker container for PHP development"
---

Using docker and its container seems easy to start with but hard if it comes to actually setting up the developing you environment you really want.

To start off easy I used the [official php images from docker hub](https://hub.docker.com/_/php).

Instead of creating a Dockerfile and building your own image based on an official php image, I found it an easy entry to use this docker command:
```
docker run -d -p 8080:80 --name my-apache-php-app -v "$PWD":/var/www/html php:7.2-apache
```
If you execute this command in the root directory of your php project assuming your index.php is located here, you should be able to open your browser on localhost:8080 and see the ouput of your index.php. Any changes in your root directory should also be reflected, due to the ```-v``` volume option.

See my example with a Dockerfile to get the same effect
[here](https://github.com/tillklockmann/php7.1-apache).
