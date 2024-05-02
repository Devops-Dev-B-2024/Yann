FROM node:18-alpine

WORKDIR /home/node/app

COPY ./e-bakery/ ./

RUN npm install

EXPOSE 8000

CMD ["node", "server.js"]
