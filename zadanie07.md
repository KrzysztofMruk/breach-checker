Zadanie 1:

    Z --ignore-unfixed:

    breach-checker-backend:prod (debian 13.7)
    =========================================
    Total: 2 (HIGH: 2, CRITICAL: 0)

    breach-checker-backend:dev (debian 13.7)
    ========================================
    Total: 5 (HIGH: 5, CRITICAL: 0)

    breach-checker-frontend:prod (alpine 3.24.4)
    ============================================
    Total: 3 (HIGH: 3, CRITICAL: 0)

    breach-checker-frontend:przed (alpine 3.24.2)
    =============================================
    Total: 28 (HIGH: 27, CRITICAL: 1)

    Bez --ignore-unfixed:

    breach-checker-backend:prod (debian 13.7)
    =========================================
    Total: 49 (HIGH: 49, CRITICAL: 0)

    breach-checker-backend:dev (debian 13.7)
    ========================================
    Total: 52 (HIGH: 52, CRITICAL: 0)

    breach-checker-frontend:prod (alpine 3.24.4)
    ============================================
    Total: 3 (HIGH: 3, CRITICAL: 0)

    breach-checker-frontend:przed (alpine 3.24.2)
    =============================================
    Total: 28 (HIGH: 27, CRITICAL: 1)


    Użycie flagi --pull podczas budowania zmieniło wynik skanowania,    ponieważ wymusiło pobranie z rejestru najświeższych wersji obrazów  bazowych z zaaplikowanymi najnowszymi łatkami bezpieczeństwa, zamiast korzystania z lokalnej pamięci podręcznej.

Zadanie 2:

    dependency failed to start:     container breach-checker-backend-1  exited (255)

    Flaga read_only: true montuje główny system plików kontenera w trybie tylko do odczytu, co chroni przed trwałą modyfikacją plików aplikacji lub wstrzyknięciem złośliwych skryptów, natomiast cap_drop: [ALL] odbiera procesom wszystkie przywileje jądra Linuksa (Linux capabilities?), a no-new-privileges:true blokuje eskalację uprawnień (np. poprzez wykorzystanie binarek z bitem setuid). Dyrektywa USER appuser w pliku Dockerfile obniża uprawnienia na poziomie tradycyjnego modelu systemu operacyjnego (uruchamia proces jako zwykły użytkownik zamiast root), przez co proces wciąż może zapisywać pliki w miejscach, do których ma prawa dostępu. Z kolei read_only: true jest blokadą nałożoną bezpośrednio na warstwę systemu plików kontenera podczas jego uruchomienia — całkowicie uniemożliwia zapis na głównej warstwie kontenera niezależnie od tego, jakich praw właściciela czy grupy używa dany proces (z wyjątkiem jawnie zamontowanych wolumenów, bind mounts oraz tmpfs).

Zadanie 3:
