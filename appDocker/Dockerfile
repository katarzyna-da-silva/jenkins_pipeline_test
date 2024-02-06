# Stage 1: Base Image
FROM python:3.9 AS base
WORKDIR /app

# Install mkdocs and dependencies
RUN pip install mkdocs mkdocs-material

# Copy source files
COPY . .

# Stage 2: Dev Image
FROM base AS dev
# Additional development dependencies can be installed here
# For example, if you need to install additional Python packages for development

# Stage 3: Build Image
FROM dev AS build
# Build MKDocs static files
RUN mkdocs build

# Stage 4: Production Image
FROM nginx:alpine AS prod
WORKDIR /usr/share/nginx/html

# Copy static files from the build stage
COPY --from=build /app/site .

# Stage 5: Initialization Image
FROM base AS init

# Stage 6: Upload to Docker Hub
FROM init AS upload
RUN echo "Uploading to Docker Hub..."
RUN  docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
RUN  docker build -t katarzynadasilva/mkdocs_container .
RUN docker push katarzynadasilva/mkdocs_container
RUN docker build -t katarzynadasilva/mkdocs_container .
RUN docker push katarzynadasilva/mkdocs_container


# Optionally: Stage 7 - Generate Images from a GA (not specified)

# Specify the default stage when the image is run
FROM prod
