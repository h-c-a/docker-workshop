# Images

## Building images

A docker image is made of one or more layers. Each layer is built on top of the previous one and they're all immutable. This means you can't modify an existing layer, instead you create a new one made of changes from the previous layer. This is very similar to how `git`'s diff works.

In order to build an image, you will need a `Dockerfile`. Try the calculator app in calculator folder `calculator/build` folder


```
docker build -t add2-image .
```

To see newly build image try

```
docker images
```

Try running the image and run

```
curl http://localhost:3000/add/?id=4%205
```

## use full commands

`One liner to stop / remove all of Docker containers:`

```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

`To remove all images`

```
docker rmi $(docker images -a -q)
```

Link to [`Cheat sheet`](https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes#a-docker-cheat-sheet)


## Understanding layers and leveraging the cache

Notice that if you run the same command again, it will take no time at all. This is because docker caches each layer and doesn't re-build them if the build context and layer creation command didn't change since the last build.

Now change the `FROM node:argon` line to `FROM node:10` so you try how it works with the latest `node.js` version and build it again

You'll see that all layers get rebuilt! This is because you changed the _base layer_; since all layers are just diffs from the previous one, by changing the base layer you invalidate the cache for all layers after it.

Now change the string message in `index.js` and build again.

We now see that the base layer gets reused, but everything else gets re-done, including installing dependencies. In your everyday development workflow, you don't want to reinstall all dependencies just because you changed a single source file, you would only want that if you changed the `package.json` file. So let's tweak the `Dockerfile` to leverage the layer cache:

Change the `COPY [".", "/usr/src/"]` line to `COPY ["package.json", "/usr/src/"]`

And add a new `COPY [".", "/usr/src/"]` after the `RUN npm install` instruction.

Now rebuild (it will rebuild all the first time), then change the text again in `index.js`, and rebuild again.

Faster, right? 😎

You may publish this image by using the command `docker push`, but you'll need an account in [`hub.docker.com`](https://hub.docker.com); you can do that later on your own. For now, let's [learn how to `docker-compose`](https://github.com/h-c-a/docker-workshop/tree/master/3-docker-compose)
