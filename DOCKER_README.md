Dockerization & Local Deployment (instructions)

This repository contains a MERN todo app (Client and Server). The Dockerfiles and docker-compose are provided to build, run, and test locally.

1) Prerequisites
- Docker installed and running
- (Optional) Docker Hub account if you want to push images

2) Run locally with docker-compose (recommended)
From project root:

```powershell
# build and start containers
docker-compose up --build -d

# show logs
docker-compose logs -f

# stop and remove
docker-compose down
```

Access
- Client (vite preview): http://localhost:3000
- Server API: http://localhost:4000 (e.g. /api/user/)

3) Build images manually

```powershell
# build server image
docker build -t <your-dockerhub-username>/todo-server:latest -f Server/Dockerfile ./Server
# build client image and set API base URL to the server container hostname (for local compose use http://server:4000)
docker build -t <your-dockerhub-username>/todo-client:latest --build-arg VITE_API_BASE_URL=http://server:4000 -f Client/Dockerfile ./Client
```

4) Run containers manually

```powershell
# run server
docker run -d --name todo-server -p 4000:4000 <your-dockerhub-username>/todo-server:latest

# run client (serve build with vite preview)
docker run -d --name todo-client -p 3000:3000 <your-dockerhub-username>/todo-client:latest
```

5) Push images to Docker Hub
Replace <your-dockerhub-username> and login first:

```powershell
# login
docker login
# tag (if not tagged during build)
docker tag todo-server:latest <your-dockerhub-username>/todo-server:latest
docker tag todo-client:latest <your-dockerhub-username>/todo-client:latest
# push
docker push <your-dockerhub-username>/todo-server:latest
docker push <your-dockerhub-username>/todo-client:latest
```

Notes & tips
- Vite reads import.meta.env at build time. We pass `VITE_API_BASE_URL` as a build-arg to the Client Dockerfile which writes a `.env` before `npm run build` so the client code picks the correct backend URL.
- If running client outside Docker and hitting server via localhost, set `VITE_API_BASE_URL=http://localhost:4000` in `Client/.env` and restart the dev server.
- If you want a runtime-configurable client (set API URL without rebuilding), you'll need a small runtime config fetch or server-side proxy; ask me and I can add it.
