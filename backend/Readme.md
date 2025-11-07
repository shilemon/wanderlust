🚀 Overview

This Dockerfile uses two stages:

Stage 1 – backend-builder

Based on the full node:25 image.

Installs dependencies and prepares the application.

Stage 2 – Final Runtime Image

Based on the lighter node:25-slim image.

Copies only the built app and necessary dependencies from the first stage.

Exposes port 8080 and runs the app with npm start.

🧱 What Happens Here:

node:25 → used for building (has all tools and compilers).

node:25-slim → lightweight runtime base image.

COPY --from=backend-builder → brings only the necessary files from the first stage.

EXPOSE 8080 → defines the application port.

CMD ["npm", "start"] → starts the Node.js server.

Run the following command to build the Docker image:

docker build -t my-app .

▶️ Run the Container

Once the image is built, run it with:

docker run -d -p 8080:8080 --name node-app my-app
