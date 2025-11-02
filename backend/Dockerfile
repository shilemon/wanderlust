# Use the official Node.js 25 image as the base
FROM node:25

# Set the working directory inside the container
WORKDIR /app

# Copy all files from the current directory to the container's /app directory
COPY . .

# Install project dependencies using npm
RUN npm i

# Copy the sample environment file and rename it to .env
COPY .env.sample .env

# Expose port 8080 for the application
EXPOSE 8080

# Start the application using npm
CMD ["npm", "start"]
