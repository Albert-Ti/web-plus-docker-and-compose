# docker build --build-arg="NODE_VERSION=18.20.2" .
ARG NODE_VERSION=16.20.2

FROM node:${NODE_VERSION}-alpine AS base
WORKDIR /app

FROM base AS build
RUN --mount=type=bind,source=package.json,target=package.json \
    --mount=type=bind,source=package-lock.json,target=package-lock.json \
    --mount=type=cache,target=/root/.npm \
    npm ci --include=dev
COPY . .
RUN npm run build

FROM nginx:1.23.1-alpine AS prod
COPY --from=build /app/build /usr/share/nginx/html
COPY ./nginx/default.conf /etc/nginx/conf.d
ENTRYPOINT [ "nginx", "-g", "daemon off;"]