## Komunikacja osi/iso

![osi-iso](osi-iso.png)

## Konfiguracja route


* routing
    * dodaj trasę default  (`` ip route add default via 192.168.0.1 dev eth0 ``)
    * dodaj trasę przez bramę (`` ip route add 192.168.1.0/24 via 192.168.1.1 ``)
    * dodaj trasę przez interfejs (`` ip route add 192.168.1.0/24 dev em1 ``)
    * usuń trasę (`` ip route delete 192.168.1.0/24 via 192.168.1.1 ``)
    * zmień trasę (`` ip route replace 192.168.1.0/24 dev em1 ``)
    * pobierz trasę dla adresu (`` ip route get 192.168.1.5 ``)
     
### ip 
| subcommand    |  polecenie   | opis  |
| ------------- |:-------------| :---------------| 
|   ``route``    |                               | |
|               |   ``ip route add``             | |


### Zastosowania

* Wydzielenie osobnej sieci dla urządzeń sieciowych
* Separacja urządzeń gościnnej sieci WIFI do osobnej sieci
* Zapewnienie komunikacji pomiędzy odrębnymi sieciami lokalnymi z wykorzystaniem VPN

![zadanie 4](example-network.svg)

### Zadanie

![zadanie 4](cwiczenia4.svg)

1.
   * Przygotuj konfigurację sieci zgodnie z powyższym diagramem,:
      * PC0: 
         * `` ip addr flush eth0 ``,  `` ip addr flush eth1 ``
         * `` ip link set up eth0 ``, `` ip link set up eth1 ``
         * ``ip addr add 172.16.100.1/24 dev eth0 ``, ``ip addr add 10.0.10.1/24 dev eth1 ``
         * `` ip route ``, `` ping 10.0.10.10 ``
         * `` vi etc/sysctl.d/10-ipforward.conf `` > ``net.ipv4.ip_forward=1 ``
      * PC1:
         * `` ip addr flush eth0 ``
         * `` ip addr add 172.16.100.10/24 dev eth0 ``
         * `` ip route add 10.0.10.0/24 via 172.16.100.1 ``
         * `` ip route get 10.0.10.10 ``
         * `` ping 10.0.10.10 ``
         * `` echo "hello world" > index.html ``
         * `` python3 -m http.server ``
      * PC2:
         * `` ip addr flush eth0 ``
         * `` ip addr add 10.0.10.10/24 dev eth0 ``
         * `` ip route add default via 10.0.10.1 ``
         * `` ip route get 172.16.100.10 ``
         * `` ping 172.16.100.10 ``
         * `` curl 172.16.100.10:8000/index.html ``

   * Przetestuj połączenie pomiędzy wszystkimi elementami sieci
   * Dlaczego połączenie może nie działać: trzeba włączyć funkcję routera na komputerze PC0
      * stworzyć plik `` vi etc/sysctl.d/10-ipforward.conf `` > ``net.ipv4.ip_forward=1 ``
      * zmienić konfigurację pliku `` echo 1 > /proc/sys/net/ipv4/ip_forward ``
      * `` sysctl net.ipv4.ip_forward=1 ``
2. Przygotuj konfigurację tak aby została załadowana poprawnie po ponownym uruchomieniu systemu
   * Patrz ``utrwalanie statycznej konfiguracji cwiczenia 2``
   * zwróć uwagę na różnice pomiędzy dydtrybucjami systemu
3. Zainstaluj, uruchom i przetestuj działanie aplikacji ``chat``
   * aplikacja dostępna w serwisie github ``https://github.com/jkanclerz/client-server-chat`` lub preinstalowana na maszynie dostępnej w zasobach kursu
   * PC1: `` echo "hello world" > index.html ``, `` python3 -m http.server ``
   * PC2: `` curl 172.16.100.10:8000/index.html ``

### Zadanie do domu

1. Przygotuj konfigurację z zadania 1 wykorzystując inny system operacyjny na komputerze pełniącym rolę routera.
  * debian / centos / inny
  * zapewnij poprawną komunikację pomiędzy PC3 -> PC1
  
2. Dodaj kolejną sieć do komputera pełniącego funkcję routera
   * Sprawdź poprawność komunikacji pomiędzy innymi obszarami sieci
   * Zweryfikuj działanie programu chat pomiędzy różnymi segmentami sieci

![zadanie 4](todo.svg)