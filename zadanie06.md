Zadanie 1:
Wszystkie ok:
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker compose exec backend pytest -v
=========================== test session starts ============================
platform linux -- Python 3.12.15, pytest-8.4.2, pluggy-1.7.0 -- /usr/local/bin/python3.12
cachedir: .pytest_cache
rootdir: /app
collected 7 items

tests/test_check.py::test_leaked_hash_is_found PASSED [ 14%]
tests/test_check.py::test_unknown_hash_is_not_found PASSED [ 28%]
tests/test_check.py::test_missing_hash_field PASSED [ 42%]
tests/test_check.py::test_plaintext_password_is_rejected PASSED [ 57%]
tests/test_check.py::test_uppercase_leaked_hash_is_found PASSED [ 71%]
tests/test_check.py::test_sha1_hash_with_39_characters_is_rejected PASSED [ 85%]
tests/test_check.py::test_sha1_hash_with_non_hex_characters_is_rejected PASSED [100%]

============================ 7 passed in 0.19s =============================

Bez .lower()
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker compose exec backend pytest
=========================== test session starts ============================
platform linux -- Python 3.12.15, pytest-8.4.2, pluggy-1.7.0
rootdir: /app
collected 7 items

tests/test_check.py ....F.. [100%]

================================= FAILURES =================================
**\*\*\*\***\_\_\_**\*\*\*\*** test_uppercase_leaked_hash_is_found **\*\*\*\***\_\_\_\_**\*\*\*\***

client = <FlaskClient <Flask 'app'>>

    def test_uppercase_leaked_hash_is_found(client):
        response = client.post("/check", json={"hash": LEAKED.upper()})

>       assert response.status_code == 200
>
> E assert 400 == 200
> E + where 400 = <WrapperTestResponse streamed [400 BAD REQUEST]>.status_code

tests/test_check.py:27: AssertionError
========================= short test summary info ==========================
FAILED tests/test_check.py::test_uppercase_leaked_hash_is_found - assert 400 == 200
======================= 1 failed, 6 passed in 0.29s ========================

What's next:
Debug this Compose error with Gordon → docker ai "help me fix this compose error"

Nie przchodzi dlatego, że test wysyła LEAKED.upper().,a bez .lower() wielkie litery trafiają do regexu [0-9a-f]{40}, który dopuszcza tylko małe litery.

Zadanie 2:
PS C:\Users\kmspo\wirtualizacja\breach-checker> '{"hash":"7c4a8d09ca3762af61e59520943dc26494f8941b"}' | curl.exe -s -o blad.html -w "HTTP %{http_code}\n" -X POST http://localhost:8080/api/check -H "Content-Type: application/json" -d '@-'
HTTP 500

PS C:\Users\kmspo\wirtualizacja\breach-checker> docker compose ps -a
NAME IMAGE COMMAND SERVICE CREATED STATUS PORTS
breach-checker-backend-1 breach-checker-backend "flask --app app run…" backend 31 minutes ago Up Less than a second (health: starting) 5000/tcp
breach-checker-db-1 postgres:16-alpine "docker-entrypoint.s…" db 31 minutes ago Exited (0) 2 minutes ago
breach-checker-frontend-1 sha256:229baa3fd1f0c44a3732fe0cd53100fccaa8144626ca8b2842abf3471583c0fe "/docker-entrypoint.…" frontend 7 days ago Up 31 minutes 0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker inspect --format '{{.State.Status}} {{.State.ExitCode}}' breach-checker-backend-1
running 0
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker inspect --format '{{.State.Status}} {{.State.ExitCode}}' breach-checker-backend-1
exited 1

Po restarcie:
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker inspect --format '{{.State.Status}} {{.State.ExitCode}}' breach-checker-backend-1
running 0

Zadanie 3:

Przed poprawką:
PS C:\Users\kmspo\wirtualizacja\breach-checker> curl.exe http://localhost:8080/nginx.conf
server {
listen 80;

    # Pliki strony (index.html, style.css, script.js)
    location / {
        root /usr/share/nginx/html;
        index index.html;
    }

    # Wszystko pod /api/ nginx przekazuje do usługi backend w sieci Compose
    location /api/ {
        proxy_pass http://backend:5000/;
    }

}
Po poprawce:
PS C:\Users\kmspo\wirtualizacja\breach-checker> curl.exe -I http://localhost:8080/nginx.conf
HTTP/1.1 404 Not Found
Server: nginx/1.31.6
Date: Thu, 05 Nov 2026 15:23:16 GMT
Content-Type: text/html
Content-Length: 153
Connection: keep-alive

Obraz produkcyjny frontendu nie ma problemu, ponieważ w pliku frontend/Dockerfile użyto instrukcji COPY, która selektywnie kopiuje do /usr/share/nginx/html wyłącznie pliki statyczne strony, a plik nginx.conf trafia do oddzielnego katalogu konfiguracyjnego /etc/nginx/conf.d/.
Montowanie całego katalogu roboczego jako katalog serwera WWW jest ryzykowne, ponieważ mimowolnie udostępnia publicznie poufne pliki źródłowe, skrypty budowania, pliki Dockerfile, konfiguracje serwera, a potencjalnie także repozytorium .git czy pliki .env, co bezpośrednio prowadzi do wycieku wrażliwych danych.
