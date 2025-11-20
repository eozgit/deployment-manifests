# --- STAGE 1: Build the Angular application ---
FROM node:20-alpine AS build
WORKDIR /app

# Declare arguments for the repository URL and branch
ARG REPO_URL
ARG BRANCH=main

# 1. Install Git
RUN apk add --no-cache git

# 2. Clone the repository into the working directory
RUN git clone --depth 1 --branch ${BRANCH} ${REPO_URL} .

# Install dependencies
RUN npm install

# Build the Angular application
# **IMPORTANT:** Change 'dist/mtd-itsa-compliance-fe' to match your Angular project name output path
RUN npm run build -- --output-path=./dist/mtd-itsa-compliance-fe

# --- STAGE 2: Serve the application with Nginx ---
FROM nginx:stable-alpine AS final
# Copy the custom Nginx configuration file
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy the built Angular files into the Nginx serving directory
COPY --from=build /app/dist/mtd-itsa-compliance-fe /usr/share/nginx/html

# Default Nginx port
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
