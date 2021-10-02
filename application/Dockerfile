FROM node:14 as build

WORKDIR /app
COPY . /app

RUN npm install
RUN npm run build

FROM node:14-alpine

WORKDIR /app
COPY --from=build /app/dist dist
COPY --from=build /app/node_modules node_modules
CMD [ "node", "dist/main.js" ]