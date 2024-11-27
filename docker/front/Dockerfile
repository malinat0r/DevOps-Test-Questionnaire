FROM node:12.22.12-alpine3.15 AS build_front
COPY . /questionnaire-frontend
WORKDIR /questionnaire-frontend
RUN npm install
RUN npm run build:prod

FROM nginx
COPY --from=build_front /questionnaire-frontend/dist/questionnaire-frontend/ /media/www/
COPY /default.conf /etc/nginx/conf.d/

EXPOSE 80
STOPSIGNAL SIGQUIT
CMD ["nginx", "-g", "daemon off;"]