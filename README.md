# react-in-docker

- [react-in-docker](#react-in-docker)
  - [Installing and running a React project using only docker run commands](#installing-and-running-a-react-project-using-only-docker-run-commands)
    - [Initialize a new project with a docker run command](#initialize-a-new-project-with-a-docker-run-command)
    - [Run an existing project with a docker run command](#run-an-existing-project-with-a-docker-run-command)
  - [Installing and running a React project using a docker-compose.yml + Dockerfile setup](#installing-and-running-a-react-project-using-a-docker-composeyml--dockerfile-setup)
    - [Initialize whole new project with docker-compose.yml](#initialize-whole-new-project-with-docker-composeyml)
    - [Add an existing project](#add-an-existing-project)
  - [Handle permission issues](#handle-permission-issues)

## Installing and running a React project using only docker run commands

In this case no docker-compose.yml or Dockerfile is necessary

### Initialize a new project with a docker run command

Run the following command to init the project in the present folder (with the folder name as project name)

```bash
docker run -it --rm -v $(pwd):/app -w /app node:14-alpine sh -c "npx create-react-app ."
```

### Run an existing project with a docker run command

From the same directory where the previous command was run, the following command can be run to start the development server (the directory name needs to be rewritten of course)

```bash
docker run -it --rm --name react-in-docker -p 3000:3000 -v $(pwd):/react-in-docker -w /react-in-docker node:14-alpine sh -c "npm install && npm start"
```
## Installing and running a React project using a docker-compose.yml + Dockerfile setup

There is a bit more work here:

### Initialize whole new project with docker-compose.yml

Build the project:

```bash
docker build -t my-react-app .
```

And then (builds automatically):
```bash
docker compose up
```

The files in the volume can be edited and they are saved.

### Add an existing project

The project files need to be copied, here I have done it with the my-react-app dummy project.

Then, everything is the same, I changed the settings of the bind mount, so it updates if the local files in the my-react-app folder are changed.

## Handle permission issues
In case permission issues arise as the files were created by the container user and not your OS user, add permissions (replace dan with your username and change the directory path)

```bash
chmod -R u+rwx ~/NEW_PROGRAMMING/react-in-docker
chmod -R g+rx ~/NEW_PROGRAMMING/react-in-docker
chmod -R o+rx ~/NEW_PROGRAMMING/react-in-docker

sudo chown -R dan:dan ~/NEW_PROGRAMMING/react-in-docker
```