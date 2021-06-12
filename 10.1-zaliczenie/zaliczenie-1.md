# Zadanie 1

Organizacja planuje ulepszyć działanie istniejącej sieci biurowej.

1. Zaprojektuj oraz udokumentuj konfigurację prototypu rozwiązania z wykorzystaniem oprogramowania ``VirtualBox`` lub podobnego. 

## Schemat

![zadanie 1](office.svg)

## Wymagania

W sieci pracują komputery biurowe oraz urządzenia siecowe współdzielące zasoby. Do tej pory organizacja borykała się z ręczna konfiguracją urządzeń oraz adresami IP które dla ludzi z poza kadry technicznej były niezrozumiałe. Postanowiono:

* Wykorzystać usługę DHCP do nadawania adresów w sposób automatyczny dla wszystkich stacji roboczych
* Serwer oraz durządzenia IP tj: drukarka muszą posiadać stałe adresy celem zminimalizowanai potrzeby rekonfiguracji ustawiań klientów
* Wprowadzić translację pomiędzy Adresami IP oraz nazwami domenowymi dla kluczowych zasobów
   - erp.mojaorganizacja.pl
   - drukarka.mojaorganizacja.pl
   - router.mojaorganizacja.pl
* Wszystkie urządzenia łączą się z siecią internet z wykorzystaniem bramy NAT
* Wykorzystać podsieć rozmiaru /22 pozwalającej zaadresować co najmniej 600 urządzeń

## Dokumentacja

 * Charakterystyka rozwiazania
   * Do realizacji zadania wykorzystano oprogramowanie VirtualBox (Linux Alpine). 
 * Adresy sieci IP
     ##

    | Sieć | Adres sieci | Maska|  Zakres hostów   | Adres rozgłoszeniowy |
    | ------------ |----------- | ----------- | -----------  | ----------- |
    | **LAN** | ``120.30.60.0``   | `` 255.255.252.0 `` | `` 120.30.60.1 - 120.30.63.254 `` | `` 120.30.63.255 `` |
    | **WAN** | ``10.0.2.0``   | `` 255.255.255.0 `` | `` 10.0.2.1 - 10.0.2.254 `` | `` 10.0.15.255 `` |

    ##

    | Urządzenie | Adres | Nazwa domenowa| 
    | ------------ |----------- | ----------- | 
    | **Router-NAT** | ``120.30.60.1``   |  `router.ViktoriaK.pl` |
    | **Serwer DHCP** | ``120.30.60.2``   | - |
    | **Drukarka** | ``120.30.60.3``   | `drukarka.ViktoriaK.pl` |
    | **Serwer DNS** | ``120.30.60.4``   | - |
    | **ERP** | ``120.30.60.5``   | ` erp.ViktoriaK.pl ` |
 * Oprogramowanie wykorzystane do realizacji poszczególnych wymagań
      ##

    | Usługa | Wykorzystane oprogramowanie | 
    | ------------ |----------- |
    | **Router-NAT** | linux iptables  |
    | **Serwer DHCP** | dhcp isc |
    | **Serwer DNS** | dnsmasq |
   
 * Kluczowa konfiguracja oprogramowania pozwalająca na odtworzenie stanu po reinstalacji środowiska
    1. Konfiguracja NAT z iptables. Interfejs **eth0** został podłącząny do sieci WAN `10.0.2.15/24` , więc router może uzyskać połączenie a Internetem. Drugi interfejs **eth1** podłącząny do sieci LAN `120.30.60.1/22`. Usługa translacji adresów pomiędzy siecią lokalną i zewnętrzną została zrealizowana za pomocą oprogramowania iptables Masquerade. 
      * ![router-conf](screenshots/router-NAT.png)
    2. Konfiguracja DHCP. Wykorzystano oprogramowanie dhcp isc. Adres interfejsu sieciowego **eth1** jest `120.30.60.2/22`. W pilku `dhcpd.conf` określono właściwości serwera, takie jak: 
         - Zakres przydzielanych adresów - `120.30.60.100 - 120.30.63.100 `, z pewnością pozwała na zaadresowania więcej niż 600 hostów.
         - Opcjonalny router - `120.30.60.1`.
         - Opcjonalny serwer DNS - `120.30.60.4`.

        ![DHCP-conf](screenshots/server_dhcp_configuration.png)

        Po tym odrazu możemy sprawdzić jaki adres zostaje przydzielony stacji roboczej, a także ustawiony router i serwer DNS. Jak widać klient otrzymuje pierwszy dostępny adres z puli `120.30.60.100/22`. Jako bramę ma router LAN i serwer DNS pod adresem `120.30.60.4`.
        ![DHCP-conf-client](screenshots/client_dhcp_configuration.png)

    3. Konfiguracja DNS.  Wykorzystano oprogramowanie dnsmasq. Wprowadzona translacja pomiędzy Adresami IP oraz nazwami domenowymi dla kluczowych zasobów w pliku `/etc/hosts`.

       ![DNS-conf](screenshots/server_dns_configuration.png)

       Po tym odrazu na stacji roboczej możemy sprawdzić czy translacja odbywa się poprawnie.

          ![DNS-conf-client](screenshots/client_names_dns.png)

    4. Konfiguracja interfejsów sieciowych
    * 
         ##
      | Urządzenie | Interfejs | Adres |
      | ------------ |----------- |----------- |
      | **Router-NAT** | eth0 | `10.0.2.15/24`  |
      | **Drukarka** | eth0 | `120.30.60.3/22` |
      | **ERP** | eth0 | `120.30.60.5/22` |
      | **Router-NAT** | eth1 | `120.30.60.1/22`  |
      | **Serwer DHCP** | eth0 | `120.30.60.2/22` |
      | **Serwer DNS** | eth0 | `120.30.60.4/22` |`
    
        Pod koniec, możemy sprawdzić czy wszystko działa dobrze i klient może usyzkać połączenie wewnętrzne w sieci korzystając z nazwy domenowej lub adresu IP, a także zewnętrzne (np. Google itd).

      ![ping-client](screenshots/client-ping.png)


