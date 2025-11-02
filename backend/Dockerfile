FROM node:25

WORKDIR /app

COPY . .

RUN npm i

COPY .env.sample .env

EXPOSE 8080

CMD ["npm", "start"]
