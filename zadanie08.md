Zadanie 1:
https://github.com/KrzysztofMruk/breach-checker/actions/runs/44576678294

ERROR: failed to build: failed to solve: process "/bin/sh -c htmlhint index.html" did not complete successfully: exit code: 1

https://github.com/KrzysztofMruk/breach-checker/actions/runs/44577154089

W zielonym uruchomieniu job verify czeka na zbudowanie obu obrazów, pobiera je z GHCR oznaczone skrótem SHA commita i uruchamia w Compose. Wcześniej pipeline pobierał z rejestru tylko obraz backendu, a frontend budował lokalnie z kodu repozytorium.

Zadanie 2
https://github.com/KrzysztofMruk/breach-checker/actions/runs/44589764255

https://github.com/KrzysztofMruk/breach-checker/actions/runs/44590128094

Po nieudanym uruchomieniu latest backendu nadal wskazywał poprzedni, poprawnie zweryfikowany obraz, bo pipeline nie uruchomił joba promote. To ważne, bo przy pobieraniu obrazu bez podania tagu Docker używa latest.

Zadanie 3:
PS C:\Users\kmspo\wirtualizacja\breach-checker> docker pull ghcr.io/krzysztofmruk/breach-checker-backend:latest
latest: Pulling from krzysztofmruk/breach-checker-backend
Digest: sha256:b46a8568d620cff02be7be9c796c897e042d45e1acca69f2e0200115c9306f75
Status: Image is up to date for ghcr.io/krzysztofmruk/breach-checker-backend:latest
ghcr.io/krzysztofmruk/breach-checker-backend:latest

PS C:\Users\kmspo\wirtualizacja\breach-checker> docker pull ghcr.io/krzysztofmruk/breach-checker-backend:a8c02ab
a8c02ab: Pulling from krzysztofmruk/breach-checker-backend
Digest: sha256:b46a8568d620cff02be7be9c796c897e042d45e1acca69f2e0200115c9306f75
Status: Downloaded newer image for ghcr.io/krzysztofmruk/breach-checker-backend:a8c02ab
ghcr.io/krzysztofmruk/breach-checker-backend:a8c02ab

PS C:\Users\kmspo\wirtualizacja\breach-checker> docker images --digests ghcr.io/krzysztofmruk/breach-checker-backend
REPOSITORY TAG DIGEST IMAGE ID CREATED SIZE
ghcr.io/krzysztofmruk/breach-checker-backend a8c02ab sha256:b46a8568d620cff02be7be9c796c897e042d45e1acca69f2e0200115c9306f75 b46a8568d620 20 minutes ago 182MB
ghcr.io/krzysztofmruk/breach-checker-backend latest sha256:b46a8568d620cff02be7be9c796c897e042d45e1acca69f2e0200115c9306f75 b46a8568d620 20 minutes ago 182MB

PS C:\Users\kmspo\wirtualizacja\breach-checker> docker push ghcr.io/krzysztofmruk/breach-checker-backend:latest
The push refers to repository [ghcr.io/krzysztofmruk/breach-checker-backend]
05e0eee5572e: Layer already exists
c74288390aeb: Layer already exists
68bced9e0afa: Layer already exists
2dc99d2d0a39: Layer already exists
67aecb86f233: Layer already exists
7f71449c6add: Layer already exists
51b26452e102: Layer already exists
76040c391144: Layer already exists
ad845e7a36de: Layer already exists
latest: digest: sha256:b46a8568d620cff02be7be9c796c897e042d45e1acca69f2e0200115c9306f75 size: 2199

Uwaga! Tu mi prawdopodobnie docker gdzieś przetrzymuje token, przez co nie mogłem się wylogować komendami. Dlatego push przechodzi.

Identyczne sumy kontrolne obu tagów potwierdzają, że latest oraz a8c02ab wskazują na dokładnie ten sam, niezmieniony obraz w rejestrze. Ponieważ pakiet ma ustawioną widoczność publiczną, każdy może go odczytac bez autoryzacji. Zapis wymaga uwierzytelnienia.
