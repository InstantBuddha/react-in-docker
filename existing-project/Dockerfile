# Use an official Node.js image as the base image with Alpine Linux
FROM node:21-alpine

# Set the working directory inside the container
WORKDIR /my-react-app

# Copy package.json and package-lock.json
COPY /my-react-app/package*.json ./

# Install project dependencies
RUN npm install

# Copy the rest of your application code
COPY /my-react-app .

# Start your React application
CMD npm start
