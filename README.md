A Docker micro image for uhttpd, a tiny, lightweight web server. The image is built using progrium/busybox to keep the footprint as small as possible. If you need a quick and small static HTTP server, give this one a go!

Getting the Image
This image is hosted on the Docker index as a trusted build and can be pulled down with:

    docker pull androw/uhttpd
Usage
To run a simple detached container:

    docker run -d -p 80 androw/uhttpd
If you want to serve the contents of the current directory in the container (warning, the directory will be local to the system running the docker daemon):

    docker run -d -p 80 -v `pwd`:/www androw/uhttpd
Alternatively, you can use the data container pattern by creating a "data" container and mounting its volumes into the web server container. Let's use named containers to make this easier:

    docker run -v /www --name www_data busybox true
    docker run -d -p 80 --volumes-from www_data --name www androw/uhttpd
For fun, we'll run a container just to create an index file:

    docker run --rm --volumes-from www_data busybox \
        /bin/sh -c 'echo "<h1>Hey there</h1>" > /www/index.html'