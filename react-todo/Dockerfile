FROM node:20-alpine as builder
WORKDIR /app
COPY . .
RUN npm i
FROM node:20-alpine as runner
WORKDIR /app
COPY --from=builder /app .
EXPOSE 5173
CMD ["npm","run","dev"]
