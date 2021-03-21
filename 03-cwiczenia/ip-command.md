## Zarządzanie interfejsami z wykorzystamiem programu IP

* stan interfejsu
  * interfejs up (``ip link set eth0 up``)
  * interfejs down (``ip link set eth0 down``)
* adresacja
  * dodaj adres (``ip addr add 192.168.0.1/24 dev eth0``)
  * zmień adres (It looks like the intention here is that ``change`` will only modify an existing address, while ``replace`` will either modify an existing address or create a new one if the specified address does not exist. In practice, it seems as if both change and replace will add the address if it does not already exist.
If you actually want to add a new address and remove an old one, you will need to do that in two steps, using ``ip addr del`` followed by ``ip addr add`` (or the other way around, of course))
  * usuń adres (``ip addr del 192.168.0.1/24 dev eth0``)
  * wyczyść adresy (``ip addr flush dev eth0``)
* routing
  * pobierz trasę dla adresu (``ip route get 1.1.1.1(address)``)

* adresacja fizyczna
  * pokaż adresy interfejsów dostępnych w sieci (``ip addr``)
  * pokaż adresy dla konkretnego interfejsu (``ip addr show dev eth0``)

### ip

| subcommand    |  polecenie   | opis  |
| ------------- |:-------------| :---------------|
|   ``addr``    |                               | infirmacje o adresacji i własnościach interfejsów |
|               |   ``ip addr``                 | informacja o wszystkich interfejsach              |
|               |   ``ip addr show dev enp0s3`` | informacja o konkretnym interfejsie               |
|   ``link``    |    ``ip link set eth0 up``    | zarządza i wyświetla stany wszystkich interfejsów sieciowych  |
|   ``route``   |  ``ip route get 1.1.1.1``     | wyświetla i zmienia tablicę routingu|
|   ``maddr``   |  ``ip maddr add 33:33:00:00:00:01 dev eth0``|  dodaje statyczny adres multiemisji w warstwie łącz|
|               |   ``ip maddr del 33:33:00:00:00:01 dev eth0``| usuwa statycznyy adres multuemisji w warstwie łącz|
|   ``neigh``   | ``ip neigh add 192.168.1.1 lladdr 1:2:3:4:5:6 dev eth0`` | (Add address 192.168.1.1 with MAC 1:2:3:4:5:6 to eth0) dodaje wpis do tablicy ARP (Polecenie neigh manipuluje obiektami sąsiadów, które ustanawiają powiązania między adresami protokołów i adresami warstwy łącza dla hostów korzystających z tego samego łącza)|
|   ``help``    |  | pomoc |

### Zadanie

![zadanie 3](sieci-3.0.svg)

1.
   * Przygotuj konfigurację sieci zgodnie z powyższym diagramem,
      * PC1: ``ip addr add 172.16.100.10/24 dev eth0``, PC2: ``ip addr add 172.16.100.11/24 dev eth0``
   * Przetestuj połączenie poleceniem ping
      * ``ping 172.16.100.11``
   * Przetestuj połączenie z internetem
      * ``ip route google.pl``, ``ping gogle.pl``
   * Sprawdź trasę dla połączeń z adresem IP z poza Twojej sieci lokalnej np. 1.1.1.1
      * ``ip route 1.1.1.1``

1.1

* Zmodyfikuj połączenia sieciowe zgodnie z poniższym schematem
  * Dodanie drugiego adaptera sieciowego PC1
* Ponownie sprawdź trasę dla adresu z poza Twojej sieci lokalnej
  * ``ip route 1.1.1.1``
  
![zadanie 3](sieci-3.1.png)

2.
   * Zainstaluj na komputerze ``PC0`` serwer ``HTTP`` np. ``nginx``
      * ``apk add nginx``
   * skonfiguruj program ``nginx`` tak aby wyświetlał zawartość katalogu ``/var/www``
      * ``service nginx start``
      * ``vi /etc/nginx/conf.d/default.conf``
      * ``http {``
             ``server {``
                        ``location / {``
                                   ``root /var/www;``
                           ``}``
            ``}``
         ``}``
   * przeładuj konfigurację
      * ``nginx -s reload``
   * utwórz własny plik ``index.html`` zawierający tekst
      * ``vi index.html...``
   * Przetestuj komunikację z ``localhost``  wykorzystując konsolowego klienta ``HTTP``, program ``curl``
      * PC2: ``curl 172.16.100.10``
3.
   * Sprawdź który port został zarezerwowany dla komunikacji przez progrmam ``nginx``
      * ``netstat –lntp``

4.
   * Przetestuj komunikację z serwerem HTTP z poziomu komputera ``PC2``
   * czy jest dostepne polecenie curl?
   * np. ``curl {adres_ip_pc_2}``
   * Wykonaj test dla połączenia na innym porcie niż ``80``, np ``curl {adres_ip_pc_2}:8080``
   * czy odpowiedź jest analogiczna jak dla testu z zadania ``2``?

5.
   * Rozbuduj sieć o kolejny komputer ``PC3`` zgodnie z diagramem

![zadanie 3](sieci-3.2.png)

6.
   * Na komputerze ``PC3`` utwórzy plik index.html w katalogu ``/var/www``
   * Na komputerze ``PC3`` uruchom serwer protokołu ``HTTP`` tym razem wykorzystujac python
   * ``python3 -m http.server --directory /var/www --bind 127.0.0.1``
   * Przenieś wykonanie programu do tła ``ctrl + z`` ``bg``
   * Sprawdź połączenie wykorzystując ``curl localhost:port``
   * Sprawdź połączenie wykorzystując ``curl adres_ip:port``
   * Sprawdź zarezerwowane porty korzystając z polecenia ``netstat``

7.
    * Na komputerze ``PC3`` zmodyfikuj sposób uruchamiania serwera http w taki sposób aby możliwe było wyświetlenie strony z komputerów ``PC1`` oraz ``PC2``
    * Przeprowadź test takiej komunikacji

8.
   * Na komputerze ``PC0`` zainstaluj i uruchom w tle program ``chat-server`` z cwiczeń 2
   * Sprawdź zarezerwowane porty korzystając z polecenia ``netstat``
   * Na komputerach ``PC1`` oraz ``PC2`` zainstaluj program ``chat-client``
   * Połącz się z serwerem dostępnym pod adresem IP ``PC0``
