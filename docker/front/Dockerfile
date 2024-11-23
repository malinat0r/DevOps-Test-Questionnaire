#FROM node:12.22.12
#FROM node:12.22.12-bullseye-slim
FROM node:12.22.12-alpine3.15
COPY . /questionnaire-frontend
WORKDIR /questionnaire-frontend
RUN npm install
# By default, Angular's development server (ng serve) binds to localhost (or 127.0.0.1), which makes it accessible only within the container. You need to make sure Angular binds to all IP addresses (0.0.0.0) so that it can be accessed from outside the container.
RUN sed -i 's/"start": "ng serve"/"start": "ng serve --host 0.0.0.0 --port 4200 --disableHostCheck"/' ./package.json
RUN cp ./src/environments/environment.prod.ts ./src/environments/environment.ts
RUN npm run build:prod
CMD ["npm", "run", "start"]
