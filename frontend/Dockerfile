FROM node:alpine as builder
WORKDIR '/app'
COPY package.json .
RUN npm install
# builde phase doesn't consider changing the code
# just copy 
COPY . .
RUN npm run build

FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html

