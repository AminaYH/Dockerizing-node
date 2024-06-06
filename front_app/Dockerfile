FROM node:latest
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
RUN npm install

RUN npm run build

EXPOSE 8000
CMD [ "npm", "run", "preview" ]
