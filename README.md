# Working-with-ACR

## Create new repositary in your GitHub account
* Login to GitHub and navigate to repositaries to create a new one
* Clone the repo to a local directory and open in VS Code

## Create a new web api using VS code in the cloned directory
```
dotnet new webapi --no-https
```

## Add a Dockerfile
```
Ctrl+Shift+P for Windows
CMD+Shift+P for Windows
```
* Search for docker file to add dockerfile to the workspace
* Push the code to your GitHub repo using the Git extention of VS Code

## Tag and push image locally
```
docker login bcglabacr01.azurecr.io
Username: bcglabacr01
Password: vQign8/X5HO3XhfXFWFW05CtHqpYV+dM
docker images
docker tag <source image> bcglabacr01.azurecr.io/<target image>:<tag>
docker push bcglabacr01.azurecr.io/<target image>:<tag>
```

## Navigate to GitGub and add an action
* Click on Actions
* Create new action using the Docker template -> click on configure
* Use the below yaml to create the CI defination for building and pushing the docker image to Azure container registry:
```
name: Docker Image CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:

  build_push_image:

    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    - name: Build the Docker image
      run: docker build . --file Dockerfile --tag bcglabacr01.azurecr.io/workingwithacr:latest
     
    - name: Azure Container Registry Login
      uses: Azure/docker-login@v1
      with:
        # Container registry username
        username: bcglabacr01
        # Container registry password
        password: vQign8/X5HO3XhfXFWFW05CtHqpYV+dM 
        # Container registry server url
        login-server: bcglabacr01.azurecr.io
        
    - name: Push Image  
      run: docker push bcglabacr01.azurecr.io/workingwithacr:latest
   ```
   ** Commit into the readme file of your repo to trigger the action automatically
