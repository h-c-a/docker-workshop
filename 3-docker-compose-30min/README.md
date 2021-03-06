# Docker compose

While docker allows you to easily build and link different apps with different environments and runtimes, doing so with more than just two or three containers quickly becomes tedious. `docker-compose` allows you to work with complex container systems very easily.

First check if you have docker-compose installed (chances are that you have) by doing:

```
docker-compose --version
```

If that fails, [get `docker-compose` for you specific setup](https://docs.docker.com/compose/install/)

For working with `docker-compose`, you will need a `docker-compose.yml` file. Check out the one under the `calculator/compose` repo.

Make sure you aren't running any containers. The from the project root, do:

```
docker-compose up -d
```

Make sure you stop running containers. The from the project root, do:

```
docker-compose down
```

Magic ✨🐳

What just happened was that docker created all containers needed for the whole system, linked them appropriately in a network and bound all required ports.

```
docker-compose ps
```

You can see a simpler variation of `docker ps`, based on the items described in the `docker-compose.yml`. Each of these items are called _services_

Compare it to `docker ps`. They look alike, right? The difference is that containers names are generated as instances of the service, but the service itself is a compose-related concept.

Browse the following from your local terminal and you'll see a familiar results.
```
hcaadmin@Harwin:~% curl http://localhost:3000/add/?id=4%205 Your answer: 9 
hcaadmin@Harwin:~% curl http://localhost:3001/subtract/?id=4%205 The answer is: -1 
hcaadmin@Harwin:~% curl http://localhost:3002/multiply/?id=4%205 The answer is: 20 
hcaadmin@Harwin:~% curl http://localhost:3003/divide/?id=4%205 The answer is: 0.8
```

Interestingly if you  `docker exec -it compose_service_1 bash`  ✨🐳

the services are available by service names:

```
hcaadmin@Harwin:~% curl http://add:3000/add/?id=4%205 Your answer: 9 
hcaadmin@Harwin:~% curl http://subtract:3001/subtract/?id=4%205 The answer is: -1 
hcaadmin@Harwin:~% curl http://multiply:3002/multiply/?id=4%205 The answer is: 20 
hcaadmin@Harwin:~% curl http://divide:3003/divide/?id=4%205 The answer is: 0.8
```