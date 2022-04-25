# build environment
FROM node:14-alpine as build
WORKDIR /usr/src/app
ENV PATH /usr/src/app/node_modules/.bin:$PATH
COPY package*.json ./
Run npm install
COPY . ./
RUN npm run build

# production environment
FROM nginx:1.12.0-alpine
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY --from=build /usr/src/app/build /usr/share/nginx/html/bookstore
EXPOSE 80
#CMD ["nginx", "-g", "daemon off;"]
