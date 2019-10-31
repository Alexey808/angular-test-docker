#### .dockerignore
```$xslt
node_modules
.git
.gitignore
```

#### Dockerfile
```dockerfile
# base image
FROM node:12.2.0

# install chrome for protractor tests
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
RUN apt-get update && apt-get install -yq google-chrome-stable

# set working directory
WORKDIR /app

# add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH

# install and cache app dependencies
COPY package.json /app/package.json
RUN npm install
RUN npm install -g @angular/cli@7.3.9

# add app
COPY . /app

# start app
CMD ng serve --host 0.0.0.0
```

1) `docker build -t example:dev .`  
2) `docker run -v ${PWD}:/app -v /app/node_modules -p 4201:4200 --rm example:dev`  
3) `docker inspect compassionate_wilson | grep Address` смотрим адрес
4) `http://172.17.0.2:4200/`  
