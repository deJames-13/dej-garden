---
{"dg-publish":true,"permalink":"/programming/docker/demo/","noteIcon":""}
---

**Run an interactive docker image**
```sh
docker run -it alpine
```

**Build docker images**
```sh
docker build -t[tag] hello-docker[directory] .[location]
```

**Port mapping to expose container to localhost**
```sh
# -p [CONTAINER_PORT:LOCAL_PORT]
docker run -p 5173:5173 react-in-docker
```

**Live update docker**
- mounting the current working directory to watch for changes made in code
```sh
# $(pwd) or ./
docker run -p 5173:5173 -v "./:/app" -v /app/node_modules react-in-docker
```
