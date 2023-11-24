# react-in-docker

## Initialize a new project with a docker run command

Run the following command to init the project in the present folder (with the folder name as project name)

```bash
docker run -it --rm -v $(pwd):/app -w /app node:14-alpine sh -c "npx create-react-app ."
```

In case permission issues arise as the files were created by the container user and not your OS user, add permissions (replace dan with your username and change the directory path)

```bash
chmod -R u+rwx ~/NEW_PROGRAMMING/react-in-docker
chmod -R g+rx ~/NEW_PROGRAMMING/react-in-docker
chmod -R o+rx ~/NEW_PROGRAMMING/react-in-docker

sudo chown -R dan:dan ~/NEW_PROGRAMMING/react-in-docker
```

## Initialize whole new project with docker-compose.yml

Build the project:

```bash
docker build -t my-react-app .
```

And then (builds automatically):
```bash
docker compose up
```

The files in the volume can be edited and they are saved.

## Add an existing project

The project files need to be copied, here I have done it with the my-react-app dummy project.

Then, everything is the same, I changed the settings of the bind mount, so it updates if the local files in the my-react-app folder are changed.