FROM node:9.4.0-alpine

WORKDIR /usr/src/app

COPY package*.json ./yarn.* ./
RUN yarn install

COPY . .

EXPOSE 3000 3001 3002

RUN yarn build

CMD [ "yarn", "start" ]