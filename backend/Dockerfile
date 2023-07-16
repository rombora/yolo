FROM node:13.12.0-alpine
WORKDIR /app/backend

COPY package.json package.json
RUN npm install
COPY . .
EXPOSE 5000

CMD ["npm", "start"]
