FROM alpine

LABEL maintainer="sh_goutam@yahoo.com"

RUN apk add --update nodejs nodejs-npm

COPY . /src

WORKDIR /src

RUN npm install

EXPOSE 8080

ENTRYPOINT ["node", "./app-express.js"]