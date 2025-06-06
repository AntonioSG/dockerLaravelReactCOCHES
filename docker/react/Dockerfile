# Build Stage
FROM node:alpine AS build
RUN apk add --no-cache git
RUN git clone https://github.com/AntonioSG/ReactCochesFront.git
#RUN npm install -g vite
WORKDIR /ReactCochesFront
#COPY vite-project/package*.json ./
COPY .env.example .env
RUN npm install
#COPY vite-project .
RUN npm run build
 
# Production Stage
FROM nginx:stable-alpine AS production
COPY --from=build /ReactCochesFront/dist /usr/share/nginx/html