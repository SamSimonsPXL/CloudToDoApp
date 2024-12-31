# Stage 1: Build the Angular application
FROM node:16-alpine AS build

# Set working directory
WORKDIR /usr/src/app

# Argument and environment variable for the API URL
ARG APIURL
ENV APIURL=${APIURL}

# Copy the Angular source code to the working directory
COPY . .

# Ensure the environments directory exists before writing to it
RUN mkdir -p /usr/src/app/src/environments \
    && echo "export const environment = {production: true, apiurl: '${APIURL}'};" > /usr/src/app/src/environments/environment.prod.ts

# Install Node.js dependencies
RUN npm install

# Build the Angular application for production
RUN npm run build --configuration=production

# Stage 2: Serve the Angular application using NGINX
FROM nginx:latest

# Copy the custom NGINX configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Copy the built application from the build stage to the NGINX HTML directory
COPY --from=build /usr/src/app/dist/frontend/ /usr/share/nginx/html

# Expose port 80 for the container
EXPOSE 80

# Start the NGINX server
CMD ["nginx", "-g", "daemon off;"]
