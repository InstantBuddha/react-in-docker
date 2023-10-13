# react-in-docker

## Initialize whole new project

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