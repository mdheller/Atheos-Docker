# [Atheos IDE](https://atheos.io/), updated from [Codiad](http://codiad.com/)

## What is Atheos


![Screenshot: Atheos](/docs/atheos.png?raw=true "Atheos")

Atheos is an updated and currently maintained fork of Codiad, a web-based IDE framework with a small footprint and minimal requirements. 

Codiad was built with simplicity in mind, allowing for fast, interactive development without the massive overhead of some of the larger desktop editors. That being said even users of IDE's such as Eclipse, NetBeans and Aptana are finding Codiad's simplicity to be a huge benefit. While simplicity was key, we didn't skimp on features and have a team of dedicated developer actively adding more.

Atheos is expanding on that mentality as much as possible, trying to minimizing it's footprint even further while maximizing functionality and performance. The major goal of Atheos will be a complete rewrite of every line of code, from the bottom to the top, including plugins; Reason being that Codiad was built over a long period of time, and older files still use much older standards. Codiad has a lot of technical debt piled up that needs addressing.

For more information on the project please check out **[the docs](https://www.atheos.io/docs)** or **[the Atheos Website](http://www.atheos.io)**


### Features of this image

  * **Simple**: Based on Ubuntu, contains `docker` and `docker-compose` binaries, and is easy to extend so as to include required development tools.
  * **Performant**: Using Nginx + PHP-FPM (very performant).
  * **Secure**:
      * Runs as non-root (Nginx run as `nginx` and PHP-FPM run as UID `2743` by default once started).
      * Includes a brute-force attack protection.



## How to use this image

    docker run --rm -p 8080:80 \
        -e ATHEOS_UID=$UID -e ATHEOS_GID=$GID \
        -v $PWD/code:/code \
        -v /etc/localtime:/etc/localtime:ro \
        -v /var/run/docker.sock:/var/run/docker.sock:ro \
        atheos/atheos

Then open your browser at `http://localhost:8080`.

**Parameters:**

  * `-p 80` ‒ the port to expose.
  * `-e ATHEOS_UID` and `-e ATHEOS_GID` ‒ *(optional)* sets the user / group ID to use for PHP
    (i.e. it'll be the user / group under which all Codiad users will execute commands if they use the Terminal plugin or such).
  * `-v /code` ‒ *(optional)* persists your configuration and installed plugins (you may also use a Docker volume).
  * `-v /etc/localtime` ‒ *(optional)* used for timesync.
  * `-v /var/run/docker.sock` ‒ *(optional)* allows to **build and run Docker images** from within Codiad
    (e.g. using the Terminal plugin or Macros plugin). It also gives often nearly `root` access to your Codiad
    users so use it with care. If you see client API incompatible, you may try to mount also `-v /usr/bin/docker:/usr/bin/docker:ro`.
    Just ensure that user `ATHEOS_UID:ATHEOS_GID` has read access to that socket file.


### User / Group Identifiers

TL;DR - The `ATHEOS_UID` and `ATHEOS_GID` values set the user / group you'd like your container to 'run as'. This can be a user you've created or even root (not recommended).

Part of what makes our containers work so well is by allowing you to specify your own PUID and PGID. This avoids nasty permissions errors with relation to data volumes (-v flags). When an application is installed on the host OS it is normally added to the common group called users, Docker apps due to the nature of the technology can't be added to this group. So we added this feature to let you easily choose when running your containers.


### Setting up your projects

  * Store your projects somewhere below `/code/`, for data persistence (or mount another volume).


### Extending the capabilies

You can easily extend to include tool you may need and have them ready
whenever you re-create your container. Just create a `Dockerfile` like:

    FROM hlsiira/atheos
    RUN apt update && apt install -y build-essential python

Now you can just build and use your new image:

    $ docker build -t atheos .
    $ docker run --rm -p 8080:80 atheos


## Feedbacks

Suggestions are welcome on our [GitHub issue tracker](https://github.com/Atheos/Atheos-Docker/issues).
