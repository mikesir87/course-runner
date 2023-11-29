---
title: Background
---

So, the first question is... why do we even need to persist data in the first place?

First off, remember that containers are sandboxed environments that simply run a process. That process could be a database, a web app, a game server, or almost anything else!

When that sandboxed environment is created, the only starting files that are in the environment are those that are provided by the container image. So, if we run the following:

```docker
docker run -d -p 8080:80 --name nginx-demo nginx
```

The container image for `nginx` is downloaded (if it didn't exist locally yet). That image contains everything needed to run nginx... its binaries, config files, dependencies, and more. Once it's running you can open your browser to [http://localhost:8080](http://localhost:8080) and you'll see the default nginx landing page. Cool!

Now, let's make a change to that file. Let's make it simply say hello world by replacing the default `index.html` file in the image:

```docker
docker exec nginx-demo sh -c 'echo "<h1>Hello world!</h1>" > /usr/share/nginx/html/index.html'
```

Now, if you refresh the page, you'll see the updated message!

But, let's kill our container and restart it.

```docker
docker rm -f nginx-demo
docker run -d -p 8080:80 --name nginx-demo nginx
```

If we look at the page again, our change is lost! Why? Well, we started a new container and the new environment used only the container used only the files found in the container image.
