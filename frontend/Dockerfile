# syntax=docker/dockerfile:1.4

ARG NODE_VERSION=18.17.1

################################################################################
# Use node image for base image for all stages.
FROM node:${NODE_VERSION}-alpine AS base

# Set working directory
WORKDIR /src/app

################################################################################
# Create a stage for installing production dependencies.
FROM base AS deps

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci --omit=dev

################################################################################
# Create a stage for building the application.
FROM base AS build

# Copy dependencies from deps stage
COPY --from=deps /src/app/node_modules ./node_modules

# Copy application source
COPY . .

# Build the application
RUN npm run build

################################################################################
# Create the final stage to run the application with minimal runtime dependencies.
FROM node:${NODE_VERSION}-alpine AS final

# Set working directory
WORKDIR /src/app

# Use production node environment by default
ENV NODE_ENV=production

# Copy application and dependencies from build stage
COPY --from=build /src/app .

# Expose the port that the application listens on
EXPOSE 3000

# Start the application
CMD ["npm", "start"]