Zadanie 1
Z przeglądarki:
http://localhost:5000/health
{"status":"ok"}

Z konsoli:
StatusCode : 200
StatusDescription : OK
Content : {"status":"ok"}

RawContent : HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://localhost:5000
Connection: close
Content-Length: 16
Content-Type: application/json
Date: Sun, 25 Oct 2026 13:08:46 GMT
Server: Werkzeug/3.1.9 ...
Forms : {}
Headers : {[Access-Control-Allow-Origin, http://localhost:5000],
[Connection, close], [Content-Length, 16], [Content-Typ
e, application/json]...}
Images : {}
InputFields : {}
Links : {}
ParsedHtml : mshtml.HTMLDocumentClass
RawContentLength : 16

Zadanie 2:
docker run --rm --network breach-net healthcheck-probe
backend: OK

docker run --rm healthcheck-probe
backend: UNREACHABLE

docker stop backend
docker run --rm --network breach-net healthcheck-probe
backend: UNREACHABLE

Zadanie 3
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker exec backend cat /tmp/notatka.txt
notatka

PS C:\Users\kmspo\wirtualizacja\breach-checker> docker exec backend cat /tmp/notatka.txt
cat: /tmp/notatka.txt: No such file or directory

Zawartość pliku historia.txt
Sun Oct 25 13:43:04 UTC 2026
Sun Oct 25 13:43:05 UTC 2026

Dlaczego plik w /tmp przetrwał restart, ale nie rm?
Plik zapisany w /tmp trafił do warstwy zapisywalnej kontenera. Podczas zwykłego restartu kontener nie przestaje istnieć - zatrzymuje się i wznawia działanie z tą samą warstwą, natomiast polecenie docker rm bezpowrotnie niszczy cały kontener wraz z całą jego ulotną warstwą zapisu.
Dlaczego historia.txt przetrwała, choć oba kontenery zostały usunięte?
Plik ten został zapisany w wolumenie , a wolumeny żyją w przestrzeni zarządzanej bezpośrednio przez Dockera na hoście, całkowicie poza cyklem życia kontenerów. Usunięcie kontenera (nawet z flagą --rm) nie usuwa powiązanego z nim wolumenu, dzięki czemu kolejny kontener mógł go podmontować i odczytać wcześniejsze wpisy.
Czy lista wycieków przetrwałaby docker rm i dlaczego?
To zależy od architektury bazy: jeśli dane bazy (lub samej aplikacji) są zapisywane w wolumenie, to dane przetrwają usunięcie kontenera. Jeśli aplikacja lub baza działa bez podpiętego wolumenu i trzyma stan wyłącznie w swojej ulotnej warstwie kontenera lub pamięci RAM, usunięcie kontenera spowoduje bezpowrotną utratę danych.
