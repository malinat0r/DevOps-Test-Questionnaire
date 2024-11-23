#FROM node:12.22.12
FROM node:12.22.12-alpine3.15
COPY . /questionnaire-backend
WORKDIR /questionnaire-backend
RUN npm install
RUN mv ./src/environments/environment.prod.ts ./src/environments/environment.ts
RUN npm run build
CMD ["npm", "run", "start:build"]
