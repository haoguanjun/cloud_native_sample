# Cloud Native Sample

##

https://github.com/haoguanjun/cloud_native_sample

## 教程：容器化 .NET 应用

[Tutorial: Containerize a .NET app](https://docs.microsoft.com/en-us/dotnet/core/docker/build-container?tabs=windows)

```bash
#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to FROM  mcr.microsoft.com/dotnet/aspnet:6.0 AS base
RUN sed -i 's/TLSv1.2/TLSv1.0/g' /etc/ssl/openssl.cnf

WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
# Copy csproj and restore
COPY *.csproj ./
RUN dotnet restore

COPY . ./

FROM build AS publish
RUN dotnet publish -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

ENTRYPOINT ["dotnet", "mywebapi.dll"]
```

[Deploy a .NET Core API with Docker](https://dotnetplaybook.com/deploy-a-net-core-api-with-docker/)

[Configure GitHub Actions](https://docs.docker.com/ci-cd/github-actions/)

```yaml
name: Docker Image CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:

  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
    
    - name: Login to Docker Hub
      uses: docker/login-action@v1
      with:
          username: ${{ secrets.DOCKER_HUB_USERNAME }}
          password: ${{ secrets.DOCKER_HUB_ACCESS_TOKEN }}
    
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v1
    
    - name: Build and push
      uses: docker/build-push-action@v2
      with:
          context: ./src/mywebapi 
          file: ./src/mywebapi/Dockerfile
          push: true
          tags: ${{ secrets.DOCKER_HUB_USERNAME }}/mywebapi:latest
   
```

## Reference

* [Tutorial: Create and share a Docker app with Visual Studio Code](https://docs.microsoft.com/en-us/visualstudio/docker/tutorials/docker-tutorial)
* [Tutorial: Persist data in a container app using volumes in VS Code](https://docs.microsoft.com/en-us/visualstudio/docker/tutorials/tutorial-persist-data-layer-docker-app-with-vscode)
* [Tutorial: Deploy your Docker app to Azure](https://docs.microsoft.com/en-us/visualstudio/docker/tutorials/tutorial-deploy-docker-app-azure)
* [Tutorial: Create multi-container apps with MySQL and Docker Compose](https://docs.microsoft.com/en-us/visualstudio/docker/tutorials/tutorial-multi-container-app-mysql)