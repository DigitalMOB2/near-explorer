FROM mhart/alpine-node:12 AS build

WORKDIR /near-explorer/frontend
#RUN chown -R nobody: ..
#USER nobody
ENV HOME=/tmp
COPY ./package.json ./package-lock.json ./
RUN npm install --prod && \
    cp -a node_modules ../production_node_modules && \
    npm install

COPY ./ ./

# XXX: render.com always copies files as owned by UID 1000, GID 1000
#USER root
#RUN chown nobody: .
#USER nobody

RUN npm run build


FROM mhart/alpine-node:slim-12

WORKDIR /near-explorer/frontend
ENV HOME=/tmp \
    PORT=3000
COPY --from=build /near-explorer/production_node_modules ./node_modules
COPY --from=build /near-explorer/frontend/public ./public
COPY --from=build /near-explorer/frontend/next.config.js ./
COPY --from=build /near-explorer/frontend/.next ./.next

CMD ["/bin/sh", "-c", "exec ./node_modules/.bin/next start --port $PORT"]
