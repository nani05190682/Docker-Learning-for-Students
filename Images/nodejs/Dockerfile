FROM node:18

# Create app directory
WORKDIR /usr/src/app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN cd /usr/src/app 
RUN npm init -y
RUN npm install

# Copy app source
COPY . .

# Expose the port
EXPOSE 3000

# Start the application
CMD ["node", "app.js"]
