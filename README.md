# react-in-docker

- [react-in-docker](#react-in-docker)
  - [Installing and running a React project using only docker run commands](#installing-and-running-a-react-project-using-only-docker-run-commands)
    - [Initialize a new project with a docker run command](#initialize-a-new-project-with-a-docker-run-command)
    - [Run an existing project with a docker run command](#run-an-existing-project-with-a-docker-run-command)
  - [Installing and running a React project using a docker-compose.yml + Dockerfile setup](#installing-and-running-a-react-project-using-a-docker-composeyml--dockerfile-setup)
    - [Initialize whole new project with docker-compose.yml](#initialize-whole-new-project-with-docker-composeyml)
    - [Add an existing project](#add-an-existing-project)
  - [Handle permission issues](#handle-permission-issues)
  - [Handling package updates](#handling-package-updates)
  - [React in Docker in production](#react-in-docker-in-production)
  - [Applying changes to original files to be built !IMPORTANT!](#applying-changes-to-original-files-to-be-built-important)
  - [Github Image Repository in docker-compose-prod.yml](#github-image-repository-in-docker-compose-prodyml)

## Installing and running a React project using only docker run commands

In this case no docker-compose.yml or Dockerfile is necessary

### Initialize a new project with a docker run command

Run the following command to init the project in the present folder (with the folder name as project name)

```bash
docker run -it --rm -v $(pwd):/my-react-app -w /my-react-app node:21-alpine sh -c "npx create-react-app ./my-react-app"
```

### Run an existing project with a docker run command

From the same directory where the previous command was run, the following command can be run to start the development server (the directory name needs to be rewritten of course)

```bash
docker run -it --rm --name react-in-docker -p 3000:3000 -v $(pwd):/react-in-docker -w /react-in-docker node:21-alpine sh -c "npm install && npm start"
```
## Installing and running a React project using a docker-compose.yml + Dockerfile setup

There is a bit more work here:

### Initialize whole new project with docker-compose.yml

Build the project:

```bash
docker build -t my-react-app .
```

And then
```bash
docker compose up
```

The files in the container can be copied to your computer.

To sh in:
```sh
docker exec -it whole-new-project sh
```

### Add an existing project

The project files need to be copied, here I have done it with the my-react-app dummy project.

Then, everything is the same, I added a bind mount, so it updates if the local files in the my-react-app folder are changed.

## Handle permission issues
In case permission issues arise as the files were created by the container user and not your OS user, add permissions (replace dan with your username and change the directory path)

```bash
chmod -R u+rwx ~/NEW_PROGRAMMING/react-in-docker
chmod -R g+rx ~/NEW_PROGRAMMING/react-in-docker
chmod -R o+rx ~/NEW_PROGRAMMING/react-in-docker

sudo chown -R dan:dan ~/NEW_PROGRAMMING/react-in-docker
```

or a simpler way for everything in the present directory:

```sh
sudo chmod -R 777 ./
```

## Handling package updates

With the npm-check-updates:

```sh
npx npm-check-updates
```

then update package.json

```sh
npx npm-check-updates -u
```

and then

```sh
npm install
```

## React in Docker in production

Here, I added an nginx to serve the built React project. 

To run the production docker compose, just run:

```sh
docker compose -f docker-compose-prod.yml up
```

The React container exits with code 0 after build and the nginx continues running to serve the built version.

If you would like to run the project on your local computer, open the following in the browser: http://localhost:80

## Applying changes to original files to be built !IMPORTANT!

If you have already built the production ready image, simply rebuilding the project might not be necessary. To make sure all the changes are added:

  1. Remove the already built image
  2. Remove the already created volume
  3. Build the project with the following command to avoid problems created by caching:
  ```bash
  docker compose -f docker-compose-prod.yml build --no-cache
  ```
  4. Just in case clear the cache in your browser too

## Github Image Repository in docker-compose-prod.yml

Replaces the need of building the production image on a server (might be useful with low memory or disk space environments)

After building the image and uploading it to ghcr.io, the following can be modifiedd in docker-compose-prod.yml:

```yml
react-frontend:
    image: ghcr.io/instantbuddha/react-in-docker-production-version:latest
    container_name: react-in-docker-production
```    