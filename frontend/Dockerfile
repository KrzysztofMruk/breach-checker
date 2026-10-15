FROM nginx:alpine
LABEL maintainer="imie.nazwisko@example.com"
LABEL autor="Krzysztof Mruk" data="2026-10-14"
RUN apk add --no-cache nodejs npm && npm install -g htmlhint
COPY . /usr/share/nginx/html
RUN htmlhint /usr/share/nginx/html/index.html
EXPOSE 80
