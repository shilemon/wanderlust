FROM node:25

WORKDIR /app

# Copy only package files first for better caching
COPY . .

RUN npm i

# Copy the rest of the app
COPY . .

# Copy environment sample
COPY .env.sample .env.local

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]
