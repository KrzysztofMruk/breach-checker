Zad.1
Komenda:
docker inspect --format '{{json .Config.Labels}}' krzysztof-frontend:v1
Wynik:
{"autor":"Krzysztof Mruk","data":"2026-10-14","maintainer":"imie.nazwisko@example.com"}

Komenda:
docker inspect --format '{{json .Config.Labels}}' krzysztof-frontend:latest
Wynik:
{"autor":"Krzysztof Mruk","data":"2026-10-14","maintainer":"imie.nazwisko@example.com"}

---

Komenda:
docker history krzysztof-frontend:v1

IMAGE CREATED CREATED BY SIZE COMMENT
0e4ac892f4bb 19 minutes ago EXPOSE [80/tcp] 0B buildkit.dockerfile.v0
<missing> 19 minutes ago RUN /bin/sh -c htmlhint /usr/share/nginx/htm… 4.1kB buildkit.dockerfile.v0
<missing> 19 minutes ago COPY . /usr/share/nginx/html # buildkit 32.8kB buildkit.dockerfile.v0
<missing> 19 minutes ago RUN /bin/sh -c apk add --no-cache nodejs npm… 102MB buildkit.dockerfile.v0
<missing> 19 minutes ago LABEL autor=Krzysztof Mruk data=2026-10-14 0B buildkit.dockerfile.v0
<missing> 19 minutes ago LABEL maintainer=imie.nazwisko@example.com 0B buildkit.dockerfile.v0
<missing> 3 weeks ago RUN /bin/sh -c set -x && apkArch="$(cat … 52.4MB buildkit.dockerfile.v0
<missing> 3 weeks ago ENV ACME_VERSION=0.4.1 0B buildkit.dockerfile.v0
<missing> 3 weeks ago ENV NJS_RELEASE=1 0B buildkit.dockerfile.v0
<missing> 3 weeks ago ENV NJS_VERSION=1.0.1 0B buildkit.dockerfile.v0
<missing> 3 weeks ago CMD ["nginx" "-g" "daemon off;"] 0B buildkit.dockerfile.v0
<missing> 3 weeks ago STOPSIGNAL SIGQUIT 0B buildkit.dockerfile.v0
<missing> 3 weeks ago EXPOSE map[80/tcp:{}] 0B buildkit.dockerfile.v0
<missing> 3 weeks ago ENTRYPOINT ["/docker-entrypoint.sh"] 0B buildkit.dockerfile.v0
<missing> 3 weeks ago COPY 30-tune-worker-processes.sh /docker-ent… 16.4kB buildkit.dockerfile.v0
<missing> 3 weeks ago COPY 20-envsubst-on-templates.sh /docker-ent… 12.3kB buildkit.dockerfile.v0
<missing> 3 weeks ago COPY 15-local-resolvers.envsh /docker-entryp… 12.3kB buildkit.dockerfile.v0
<missing> 3 weeks ago COPY 10-listen-on-ipv6-by-default.sh /docker… 12.3kB buildkit.dockerfile.v0
<missing> 3 weeks ago COPY docker-entrypoint.sh / # buildkit 8.19kB buildkit.dockerfile.v0
<missing> 3 weeks ago RUN /bin/sh -c set -x && addgroup -g 101… 5.72MB buildkit.dockerfile.v0
<missing> 3 weeks ago ENV DYNPKG_RELEASE=1 0B buildkit.dockerfile.v0
<missing> 3 weeks ago ENV PKG_RELEASE=1 0B buildkit.dockerfile.v0
<missing> 3 weeks ago ENV NGINX_VERSION=1.31.6 0B buildkit.dockerfile.v0
<missing> 3 weeks ago LABEL maintainer=NGINX Docker Maintainers <d… 0B buildkit.dockerfile.v0
<missing> 3 weeks ago CMD ["/bin/sh"] 0B buildkit.dockerfile.v0
<missing> 3 weeks ago ADD alpine-minirootfs-3.24.2-x86_64.tar.gz /… 9.08MB buildkit.dockerfile.v0

Komentarz:
Z obrazu bazowego pochodzą wszystkie warstwy oprócz pierwszych pięciu (moje kończą się na <missing> 19 minutes ago LABEL maintainer=imie.nazwisko@example.com 0B buildkit.dockerfile.v0). Niektóre mają rozmiar 0B, ponieważ są to tylko labele. Widzę tylko moją wartość, ponieważ ją ją zmieniłem jako ostatni przed utworzeniem mojego obrazu
