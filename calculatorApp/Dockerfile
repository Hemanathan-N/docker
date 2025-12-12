# we are taking base image, node image contains ubuntu + node
FROM node:20
# we want to set working directory and this working directory is seted then all RUN commands done this directory only
WORKDIR /app
# copying all application files to current directory/working directory
COPY calculator.html
COPY server.js .
# we want mention, the application is running on which port
EXPOSE 5000
#finally we want or run application
CMD ["node", "server.js"]
