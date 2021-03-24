
**DOCKER LAB**

Create an account on github.com, make sure you'veinstalled git on your laptop, if not go to:

    http://git-scm.com/downloads

Install docker on your laptop:

    https://docs.docker.com/get-started/

Get an account on docker hub: 

    https://hub.docker.com/

**#PART 1**

1>It’s time to build your first container 

2> Open a terminal on your laptop

3>Login to docker hub using your username and password. It should prompt you for username/password

    % docker login -u <your_dockerhub_id>
    Login Succeeded

4>Let’s make sure docker is working properly. Enter the following command

    %  docker version

Let’s get a list of available docker commands using the help utility. Enter the following command: 

    % docker help | more


Start with an existing "Official" image on docker hub.
The pull command fetches the ubuntu linux "Official" image from docker hub and saves it to your laptop. 

You can then see a list of images pulled to your laptop with 

    %docker image ls  



You can specify the version number (ubuntu:20.04) or just use (ubuntu:latest) to specify the latest version of the image


Go to the docker hub on the browser (hub.docker.com), select the "Explore" tab and search for "ubuntu". Look for Official Image.


    % docker pull ubuntu:latest
    Using default tag: latest
    latest: Pulling from library/ubuntu
    ba3557a56b15: Pull complete
    Digest:     sha256:a75afd8b57e7f34e4dad8d65e2c7ba2e1975c795ce1ee22fa34f8cf46f96a3be     
    Status: Downloaded newer image for ubuntu:latest
    docker.io/library/ubuntu:latest
    % docker image ls
    REPOSITORY TAG IMAGE ID CREATED SIZE
     ---
    ubuntu latest 28f6e2705743 5 days ago
    5.61MB

Now that we have the image pulled to your laptop, let's run the image in a docker container. 

This command runs the ubuntu image in a container in "interactive" mode and opens a "bash" shell into the container.

    % docker run -it ubuntu bash root@364d57dac987:/#

You can now try out basic linux commands in the shell. Note the change in your prompt.

Open up a second terminal session and enter:

     % docker container ls

You should see the running container shown in the command.
    
 
 
**##PART 2**

1> Open a terminal on your laptop

2> Create a directory(if not already, you dont need to follow the path precisely, I just like things more organized) similar to:

     % mkdir /Users/<your username>/github.com/<your username>
3> cd into the folder:

    % cd /Users/<your username>/github.com/<your username>

4> Fork this project, from github into your account and then clone it to your laptop :https://github.com/amitrathod101/getting-started

    %git clone git@github.com:amitrathod101/getting-started.git

    Cloning into 'getting-started'...
    remote: Enumerating objects: 12, done.
    remote: Counting objects: 100% (12/12), done.
    remote: Compressing objects: 100% (12/12), done.
    remote: Total 461 (delta 4), reused 2 (delta 0), pack-reused 449
    Receiving objects: 100% (461/461), 3.60 MiB | 1.49 MiB/s, done.
    Resolving deltas: 100% (228/228), done.        


 This is a simple to-do list manager. Dont worry about what the code does and what language was this written in. Let's focus on the docker concepts here! 

5> The git clone command pulls down the source code from github and places it onto your laptop:

6> Change into the directory and into the app folder

    % cd getting-started
    % cd app

7> Let's create a file called Dockerfile in getting-started/app folder where the package.json file resides. 

Note the uppercase D and lowercase f, ofcourse without the "." and no extensions. You could use "vi" or any text editor you like, just copy the text from FROM till the CMD end. 

A Dockerfile is simply a text-based script of instructions that is used to create a container image. It will be traversed from the TOP to the BOTTOM. 

Again : Create a file named Dockerfile in the same folder as the file package.json with the following contents. package.json file can be found in getting-started/app.

    FROM node:12-alpine
    RUN apk add --no-cache python g++ make
    WORKDIR /app
    COPY . .
    RUN yarn install --production
    CMD ["node", "src/index.js"]


Writing a Dockerfile is the first step to containerizing an application. You can think of this Dockerfile as a list of commands or a a step-by-step recipe on how to build our image. This one takes the following steps:

- Starts with a base image of node with a tag 12-alpine using the FROM statement.
- In the RUN statement, we are using the alpine package manager to add python, g++ and make. Note that we are using the --no-cache option here in order to make sure our container footprint is small. Reduce the flab!
- We are setting the working directory as /app using the WORKDIR statement. 
- Now are are COPYing everything in the current folder(which is /app) into the WORKDIR.
- Now in the RUN statement, we are advising to run yarn to install the dependencies. We are using the --production flag here so that yarn will not install any package listed in devDependencies in the package.json file. 
- Finally the CMD statement specifies the default command to run when starting a container from this image.

8> Now we build our very own docker image from the source code using the command:

    %docker build -t getting-started .

- Using the -t option helps provide a tag to the image.
- getting-started after the -t tells it the name of the image
- . tells the command the location of the Dockerfile. Remember we were running the command from the getting-started/app folder which has the Dockerfile.
- Dont forget the .  

9> It may take some time for the command to finish. Wait for it!

10> Now that we have an image, let’s run the application! Run the command:

    %docker run -dp 3000:3000 getting-started
- -d command tells docker to run the command in the detached mode. You can also explicitly type --detach
- -p tells the port mapping information. The port of the left is the port on your computer and the port on the right is the port on the container. 
- You could also give a name to your container using --name option, Its optional.

11> Voila! You did the following:
- Had your code ready
- Created your Dockerfile
- Built a Docker Image
- Created a contanier from the Docker Image!!

12> Now open a tab on your browser and copy this link, you should be able to add items in the to-do list. Make sure it is working by adding things in it, checking it or even deleting it. 

     http://localhost:3000/

13> Let's see the list of containers running on your laptop using the command: 

    % docker container ls

14> Now let's see the image using the command:

    % docker image ls

15> Before we push it to your docker account account, you will have to tag it. Running without the tag results in a tag called latest. 

    % docker tag getting-started your_dockerhub_id/getting-started
 
16> Finally we push it to hub.docker.com using 

    % docker push your_dockerhub_id/getting-started:latest

17> If it complains about login, I would best recommend to logout and then login again. 

    % docker logout
    % docker login -u your_dockerhub_id

18> Go to hub.docker.com and check out your docker image!

19> Stop the running container

    % docker container stop first_four_alphanumeric_from_containerID from Step 13.
- If you would have used a --name option, you can use the name here. 
- Generally speaking you should copy the CONTAINER ID from the docker container ls command, it also takes in the first 4 characters. Just like git! 

20> Finally, let’s get rid of the container and image 

    % docker image rm  getting-started
- you can use a -f to force the removal. 

That's it from me!