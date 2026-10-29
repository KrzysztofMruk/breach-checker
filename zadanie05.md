Zad.1
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker compose up -d --build

[+] Building 2.1s (23/23) FINISHED
=> [internal] load local bake definitions 0.0s
=> => reading from stdin 1.08kB 0.0s
=> [frontend internal] load build definition from Dockerfile 0.0s
=> => transferring dockerfile: 332B 0.0s
=> [backend internal] load build definition from Dockerfile 0.0s
=> => transferring dockerfile: 367B 0.0s
=> [frontend internal] load metadata for docker.io/library/nginx:alp 0.2s
=> [backend internal] load metadata for docker.io/library/python:3.1 0.2s
=> [backend internal] load .dockerignore 0.0s
=> => transferring context: 81B 0.0s
=> [frontend internal] load .dockerignore 0.0s
=> => transferring context: 66B 0.0s
=> [backend 1/5] FROM docker.io/library/python:3.12-slim@sha256:0210 0.0s
=> => resolve docker.io/library/python:3.12-slim@sha256:02108f5d322d 0.0s
=> [backend internal] load build context 0.0s
=> => transferring context: 63B 0.0s
=> [frontend 1/5] FROM docker.io/library/nginx:alpine@sha256:df221db 0.0s
=> => resolve docker.io/library/nginx:alpine@sha256:df221db836e17540 0.0s
=> [frontend internal] load build context 0.0s
=> => transferring context: 120B 0.0s
=> CACHED [backend 2/5] WORKDIR /app 0.0s
=> CACHED [backend 3/5] COPY requirements.txt . 0.0s
=> CACHED [backend 4/5] RUN pip install --no-cache-dir -r requiremen 0.0s
=> CACHED [backend 5/5] COPY app.py . 0.0s
=> [backend] exporting to image 0.2s
=> => exporting layers 0.0s
=> => exporting manifest sha256:723c8abe015e1a0a8377df0d7de5e7c57208 0.0s
=> => exporting config sha256:a3967ce626e6c335312b5a71538f70c5663d60 0.0s
=> => exporting attestation manifest sha256:a39f01ae0dae77c9b4d8c537 0.0s
=> => exporting manifest list sha256:1cf847f4e1dfde8a72a4114710e3c36 0.0s
=> => naming to docker.io/library/breach-checker-backend:latest 0.0s
=> => unpacking to docker.io/library/breach-checker-backend:latest 0.0s
=> CACHED [frontend 2/5] RUN apk add --no-cache nodejs npm && npm in 0.0s
=> CACHED [frontend 3/5] COPY index.html style.css script.js /usr/sh 0.0s
=> CACHED [frontend 4/5] COPY nginx.conf /etc/nginx/conf.d/default.c 0.0s
=> CACHED [frontend 5/5] RUN htmlhint /usr/share/nginx/html/index.ht 0.0s
=> [frontend] exporting to image 0.3s
=> => exporting layers 0.0s
=> => exporting manifest sha256:efe0408f69caaf444c1ce27c1476a13e2b0a 0.0s
=> => exporting config sha256:94fd23227af693096a79f50a12bc454ba03e9d 0.0s
=> => exporting attestation manifest sha256:080a2c0812368f36e162ef81 0.0s
=> => exporting manifest list sha256:229baa3fd1f0c44a3732fe0cd53100f 0.0s
=> => naming to docker.io/library/breach-checker-frontend:latest 0.0s
=> => unpacking to docker.io/library/breach-checker-frontend:latest 0.0s
=> [backend] resolving provenance for metadata file 0.0s
=> [frontend] resolving provenance for metadata file 0.0s
[+] up 6/6
✔ Image breach-checker-backend Built 2.4s
✔ Image breach-checker-frontend Built 2.5s
✔ Network breach-checker_default Created 0.1s
✔ Container breach-checker-db-1 Healthy 6.2s
✔ Container breach-checker-backend-1 Healthy 11.7s
✔ Container breach-checker-frontend-1 Started 11.9s
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker compose ps
NAME IMAGE COMMAND SERVICE CREATED STATUS PORTS
breach-checker-backend-1 breach-checker-backend "python app.py" backend 13 seconds ago Up 6 seconds (healthy) 5000/tcp
breach-checker-db-1 postgres:16-alpine "docker-entrypoint.s…" db 13 seconds ago Up 12 seconds (healthy) 5432/tcp
breach-checker-frontend-1 breach-checker-frontend "/docker-entrypoint.…" frontend 12 seconds ago Up Less than a second 0.0.0.0:8080->80/tcp, [::]:8080->80/tcp

Zad.2
Po insert
PS C:\Users\kmspo\wirtualizacja\breach-checker> '{"hash":"bc45408273d7e1914b741c3407b0b559764f7876"}' | curl.exe -X POST http://localhost:8080/api/check -H "Content-Type: application/json" -d '@-'
{"found":true}

Po docker compose down + docker compose up -d
PS C:\Users\kmspo\wirtualizacja\breach-checker> '{"hash":"bc45408273d7e1914b741c3407b0b559764f7876"}' | curl.exe -X POST http://localhost:8080/api/check -H "Content-Type: application/json" -d '@-'
{"found":true}

Po docker compose down -v + docker compose up -d
PS C:\Users\kmspo\wirtualizacja\breach-checker> '{"hash":"bc45408273d7e1914b741c3407b0b559764f7876"}' | curl.exe -X POST http://localhost:8080/api/check -H "Content-Type: application/json" -d '@-'
{"found":false}

Przetrwał pierwsze bo nie usuwaliśmy woluminu bazy, a drugiego nie bo usunęliśmy.

Zad.3
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker compose ps
NAME IMAGE COMMAND SERVICE CREATED STATUS PORTS
breach-checker-backend-1 breach-checker-backend "python app.py" backend About a minute ago Up About a minute (healthy) 5000/tcp
breach-checker-backend-2 breach-checker-backend "python app.py" backend 7 seconds ago Up 6 seconds (healthy) 5000/tcp
breach-checker-backend-3 breach-checker-backend "python app.py" backend 7 seconds ago Up 6 seconds (healthy) 5000/tcp
breach-checker-db-1 postgres:16-alpine "docker-entrypoint.s…" db About a minute ago Up About a minute (healthy) 5432/tcp
breach-checker-frontend-1 breach-checker-frontend "/docker-entrypoint.…" frontend About a minute ago Up About a minute 0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker compose exec frontend nslookup backend
Server: 127.0.0.11
Address: 127.0.0.11:53

Non-authoritative answer:

Non-authoritative answer:
Name: backend
Address: 172.19.0.6
Name: backend
Address: 172.19.0.5
Name: backend
Address: 172.19.0.3
Przed restartem
PS C:\Users\kmspo\wirtualizacja\breach-checker> 1..20 | ForEach-Object { '{"hash":"bc45408273d7e1914b741c3407b0b559764f7876"}' | curl.exe -s -X POST http://localhost:8080/api/check -H "Content-Type: application/json" -d '@-' }
{"found":false}
{"found":false}
{"found":false}

Po restarcie:
PS C:\Users\kmspo\wirtualizacja\breach-checker> 1..20 | ForEach-Object { '{"hash":"bc45408273d7e1914b741c3407b0b559764f7876"}' | curl.exe -s -X POST http://localhost:8080/api/check -H "Content-Type: application/json" -d '@-' }
{"found":false}
{"found":false}
{"found":false}

PS C:\Users\kmspo\wirtualizacja\breach-checker> docker compose exec frontend nslookup backend
Server: 127.0.0.11
Address: 127.0.0.11:53

Non-authoritative answer:

Non-authoritative answer:
Name: backend
Address: 172.19.0.3
Name: backend
Address: 172.19.0.6
Name: backend
Address: 172.19.0.5

Dlaczego przed restartem frontendu ruch trafiał do jednej repliki?

Nginx w domyślnej konfiguracji dyrektywy proxy_pass http://backend:5000; rozwiązuje nazwę domenową w DNS tylko raz – w momencie uruchomienia procesu serwera.

Dlaczego skalowanie byłoby niemożliwe, gdyby backend nadal miał ports: "5000:5000"?

Zapis ports: "5000:5000" mapuje port 5000 kontenera bezpośrednio na port 5000 systemu operacyjnego hosta. Ponieważ port na hoście może być zajęty tylko przez jeden proces/gniazdo sieciowe jednocześnie, próba uruchomienia drugiej i trzeciej repliki zakończyłaby się błędem alokacji portu.
