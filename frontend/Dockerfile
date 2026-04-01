FROM node:24-alpine AS build

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .

RUN npm run build

FROM node:24-alpine AS development

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .

ENV NODE_ENV=development

EXPOSE 3000

CMD ["sh", "-c", "test -d node_modules/nuxt || npm ci; exec npm run dev -- --host 0.0.0.0"]

FROM node:24-alpine AS prod

WORKDIR /app

COPY --from=build /app/.output /app/.output

ENV NODE_ENV=production

EXPOSE 3000

CMD [ "node", ".output/server/index.mjs" ]
