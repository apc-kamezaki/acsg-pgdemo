FROM node:14-alpine as builder
ARG AI_INSTRUMENTATION_KEY
ENTRYPOINT [ "npm" ]
WORKDIR /app
COPY . ./
RUN npm install -g npm && \
    npm install --no-optional --unsafe-perms && \
    npm run build:product

FROM nginx:1.21 as final
RUN mkdir /usr/share/nginx/spa-client
COPY --from=builder /app/nginx/conf.d/default.conf /etc/nginx/conf.d/
COPY --from=builder /app/dist /usr/share/nginx/spa-client/
RUN touch /var/run/nginx.pid && \
  chown -R nginx:nginx /var/run/nginx.pid && \
  chown -R nginx:nginx /var/cache/nginx
