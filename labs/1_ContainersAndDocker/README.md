


## Objectives

In this lab, you will:

- Pull an image from Docker Hub
- Run an image as a container using `docker`
- Build an image using a Dockerfile
- Push an image to IBM Cloud Container Registry


1. Open a terminal window by using the menu in the editor: `Terminal > New Terminal`.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/env&#x26;cmdlinetools_1.png" width="800"> <br>

2. Verify that `docker` CLI is installed.

```
docker --version
```

You should see the following output, although the version may be different:

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/env&#x26;cmdlinetools_2.png"> <br>

3. Verify that `ibmcloud` CLI is installed.

```
ibmcloud version
```

You should see the following output, although the version may be different:

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/env&#x26;cmdlinetools_3.png"> <br>

4. Change to your project folder. 
>> **Note: If you are already on the '/home/project' folder, please skip this step.**

```
cd /home/project
```

5. Clone the git repository that contains the artifacts needed for this lab, if it doesn't already exist.

```
[ ! -d 'CC201' ] && git clone https://github.com/ibm-developer-skills-network/CC201.git
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/env&#x26;cmdlinetools_5.png"> <br>

6. Change to the directory for this lab by running the following command. `cd` will change the working/current directory to the directory with the name specified, in this case **CC201/labs/1_ContainersAndDcoker**.
 

```
cd CC201/labs/1_ContainersAndDocker/
```

7. List the contents of this directory to see the artifacts for this lab.

```
ls
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/env&#x26;cmdlinetools_6.png">  <br>

::page{title="Pull an image from Docker Hub and run it as a container"}

1. Use the `docker` CLI to list your images.

```
docker images
```

You should see an empty table (with only headings) since you don't have any images yet.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/pullimg_ctr_1.png"> <br>

2. Pull your first image from Docker Hub.

```
docker pull hello-world
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/pullimg_ctr_2.png"> <br>

3. List images again.

```
docker images
```

You should now see the `hello-world` image present in the table.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/pullimg_ctr_3.png"> <br>

4. Run the `hello-world` image as a container.

```
docker run hello-world
```

You should see a **'Hello from Docker!'** message.

There will also be an explanation of what Docker did to generate this message.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/pullimg_ctr_4.png"> <br>

5. List the containers to see that your container ran and exited successfully.

```
docker ps -a
```

Among other things, for this container you should see a container ID, the image name (`hello-world`), and a status that indicates that the container exited successfully.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/pullimg_ctr_5.png"> <br>

6. Note the CONTAINER ID from the previous output and replace the **<container_id>** tag in the command below with this value. This command removes your container.

```
docker container rm <container_id>
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/pullimg_ctr_6.png"> <br>

7. Verify that that the container has been removed. Run the following command.

```
docker ps -a
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/pullimg_ctr_7.png"> <br>

Congratulations on pulling an image from Docker Hub and running your first container! Now let's try and build our own image.

::page{title="Build an image using a Dockerfile"}

1. The current working directory contains a simple Node.js application that we will run in a container. The app will print a hello message along with the hostname. The following files are needed to run the app in a container:

- app.js is the main application, which simply replies with a hello world message.
- package.json defines the dependencies of the application.
- Dockerfile defines the instructions Docker uses to build the image.

2. Use the Explorer to view the files needed for this app. Click the Explorer icon (it looks like a sheet of paper) on the left side of the window, and then navigate to the directory for this lab: `CC201 > labs > 1_ContainersAndDocker`. Click `Dockerfile` to view the commands required to build an image.

![Dockerfile in Explorer](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/dockerfile-explorer.png)

>> **You can refresh your understanding of the commands mentioned in the Dockerfile below:**<br>
>> The FROM instruction initializes a new build stage and specifies the base image that subsequent instructions will build upon. <br>
>> The COPY command enables us to copy files to our image. <br>
>> The RUN instruction executes commands. <br>
>> The EXPOSE instruction exposes a particular port with a specified protocol inside a Docker Container. <br>
>> The CMD instruction provides a default for executing a container, or in other words, an executable that should run in your container. <br>

3. Run the following command to build the image:

```
docker build . -t myimage:v1
```

As seen in the module videos, the output creates a new layer for each instruction in the Dockerfile.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/buildimg_3.png"> <br>

4. List images to see your image tagged `myimage:v1` in the table.

```
docker images
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/buildimg_4.png"> <br>

Note that compared to the `hello-world` image, this image has a different image ID. This means that the two images consist of different layers -- in other words, they're not the same image.

You should also see a `node` image in the images output. This is because the `docker build` command pulled `node:9.4.0-alpine` to use it as the base image for the image you built.

::page{title="Run the image as a container"}

1. Now that your image is built, run it as a container with the following command:

```
docker run -dp 8080:8080 myimage:v1
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/run_img_as_detached_ctr_3.png"> <br>

The output is a unique code allocated by docker for the application you are running. 

2. Run the `curl` command to ping the application as given below.

```
curl localhost:8080
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/run_img_as_ctr_3.png"> <br>

If you see the output as above, it indicates that **'Your app is up and running!'.**

4. Now to stop the container we use `docker stop` followed by the container id. The following command uses `docker ps -q` to pass in the list of all running containers:

```
docker stop $(docker ps -q)
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/run_img_as_ctr_4.png"> <br>

5. Check if the container has stopped by running the following command.

```
docker ps
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/run_img_as_ctr_5.png"> <br>

::page{title="Push the image to IBM Cloud Container Registry"}

1. The environment should have already logged you into the IBM Cloud account that has been automatically generated for you by the Skills Network Labs environment. The following command will give you information about the account you're targeting:

```
ibmcloud target
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/push_img_1.png"> <br>

2. The environment also created an IBM Cloud Container Registry (ICR) namespace for you. Since Container Registry is multi-tenant, namespaces are used to divide the registry among several users. Use the following command to see the namespaces you have access to:

```
ibmcloud cr namespaces
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/push_img_2.png"> <br>

You should see two namespaces listed starting with `sn-labs`:

- The first one with your username is a namespace just for you. You have full _read_ and _write_ access to this namespace.
- The second namespace, which is a shared namespace, provides you with only Read Access
 

3. Ensure that you are targeting the region appropriate to your cloud account, for instance `us-south` region where these namespaces reside as you saw in the output of the `ibmcloud target` command.

```
ibmcloud cr region-set us-south
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/push_img_3.png"> <br>

4. Log your local Docker daemon into IBM Cloud Container Registry so that you can push to and pull from the registry.

```
ibmcloud cr login
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/push_img_4.png"> <br>

5. Export your namespace as an environment variable so that it can be used in subsequent commands.

```
export MY_NAMESPACE=sn-labs-$USERNAME
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/push_img_5.png"> <br>

6. Tag your image so that it can be pushed to IBM Cloud Container Registry.

```
docker tag myimage:v1 us.icr.io/$MY_NAMESPACE/hello-world:1
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/push_img_6.png"> <br>

7. Push the newly tagged image to IBM Cloud Container Registry.

```
docker push us.icr.io/$MY_NAMESPACE/hello-world:1
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/push_img_7.png"> <br>

> **Note:** If you have tried this lab earlier, there might be a possibility that the previous session is still persistent. In such a case, you will see a **'Layer already Exists'** message instead of the **'Pushed'** message  in the above output. We recommend you to proceed with the next steps of the lab.

8. Verify that the image was successfully pushed by listing images in Container Registry.

```
ibmcloud cr images
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/push_img_8.png"> <br>

Optionally, to only view images within a specific namespace.

```
ibmcloud cr images --restrict $MY_NAMESPACE
```

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cc201/labs/1_ContainersAndDocker/images/push_img_9.png"> <br>

You should see your image name in the output.
