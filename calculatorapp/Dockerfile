# ===== Stage 1: Build Stage =====
FROM node:20 AS build

# Set working directory
WORKDIR /app

# Copy application files
COPY calculator.html .
COPY server.js .

# ===== Stage 2: Production Stage =====
FROM node:20-slim AS production

# Set working directory
WORKDIR /app

# Copy only necessary files from build stage
COPY --from=build /app .

# Expose the port your app will run on
EXPOSE 5000

# Run the application
CMD ["node", "server.js"]
