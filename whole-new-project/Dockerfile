# Use an official Node.js image as the base image with Alpine Linux
FROM node:21-alpine

# Create the react app
RUN npx create-react-app my-react-app

# Set the working directory inside the container
WORKDIR /my-react-app

# Install project dependencies
RUN npm install

# Start your React application
CMD npm start
