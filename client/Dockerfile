FROM node:13.12.0-alpine
WORKDIR /app/client

COPY package.json package.json
RUN npm install
COPY . .
EXPOSE 3000

CMD ["npm", "start"]
