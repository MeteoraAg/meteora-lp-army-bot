FROM node:20-slim AS base
COPY . /app
WORKDIR /app

FROM base AS prod-deps
RUN --mount=type=cache,id=npm,target=/root/.npm npm install --prod --frozen-lockfile

FROM base AS build
ENV NODE_ENV=production
RUN --mount=type=cache,id=npm,target=/root/.npm npm install typescript -g && npm install --frozen-lockfile
RUN npm run build

FROM node:20-slim AS runner
WORKDIR /app
COPY --from=prod-deps /app/node_modules /app/node_modules
COPY --from=build /app/dist /app/dist
CMD [ "node", "/app/dist/index.js" ]