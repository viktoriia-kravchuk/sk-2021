## Adresacja w sieciach

![osi-iso](osi.svg)

## Struktura adresu IP

```192.168.100.192```
```255.255.255.0```




### Jak policzyć?
#### Adres sieci

1. `` IP -> BIN ``
2. `` Maska -> BIN ``
3. `` BIN AND (adres x maska)``

#### Adres rozgłoszeniowy

1. `` IP -> BIN ``
2. `` Maska -> BIN ``
3. `` Odwrotność maski ``
4. `` BIN OR adres + odwrotność maski ``



## Podział na równą ilość podsieci

```2^S >= n```

Chcemy 7 -> 2^3 >= 7

Maska -> 255.255.255.11100000 -> 255.255.255.224 -> /27


## Wprowadzenie

| dziesiętnie |  binarnie   | 
| ----------- | -----------  |
| ``10``  | `` 00001010 `` | 
| ``92``  | `` 01011100 `` | 
| ``37``  | `` 00100101 `` | 
| ``240`` | `` 11110000 `` | 
| ``192`` | `` 11000000 `` | 
| ``255`` | `` 11111111 `` | 
| ``128`` | `` 10000000 `` | 
| ``168`` | `` 10101000 `` | 

## 

| binarnie |  dziesiętnie   | 
| ----------- | -----------  |
| ``00100000``  | `` 32 `` | 
| ``11111000``  | `` 248 `` | 
| ``10100000``  | `` 160 `` | 
| ``00110000`` | `` 48 `` | 
| ``10101100`` | `` 172 `` | 
| ``01000000`` | `` 64 `` | 
| ``11111100`` | `` 252 `` | 
| ``01100010`` | `` 98 `` | 
 
## Notacja CIDR
##  
| maska |  /(X) x - liczba bitów   | 
| ----------- | -----------  |
| ``255.255.255.0``   | `` /24 `` | 
| ``255.128.0.0``     | `` /9 `` | 
| ``255.255.252.0``   | `` /22 `` | 
| ``255.255.254.0``   | `` /23 `` | 
| ``255.255.255.240`` | `` /28 ``| 
| ``255.240.0.0``     | `` /12 ``| 
## 

| CIDR |  Maska   | 
| ----------- | -----------  |
| ``/8``    | `` 255.0.0.0 `` | 
| ``/20``   | `` 255.255.240.0 `` | 
| ``/30``   | `` 255.255.255.252 `` | 
| ``/16``   | `` 255.255.0.0 `` | 
| ``/27``   | `` 255.255.255.224 `` | 
| ``/11``   | `` 255.224.0.0 `` | 


## Liczba hostów
## 
| sieć |  liczba   | 
| ----------- | -----------  |
| ``10.0.0.0/8``    | `` 16777214 `` | 
| ``172.16.0.0/16``   | `` 65534 `` | 
| ``192.168.1.0/24``   | `` 254 `` | 
| ``192.168.1.0/27``   | `` 30 `` | 
| ``192.168.1.0/29``   | `` 6 `` | 
| ``192.168.1.0/31``   | `` 0 `` | 

* liczba 0 w masce?


## Parametry na podstawie adresu

Mając dany adres hosta i maskę znajdź:
  * adres sieci
  * początkowy adres hosta w sieci
  * liczbę hostów w zadanej podsieci ```2^(32 bity - bity maski maska) - 2 (siec i broadcast)```
  * końcowy zakres adresu hosta w sieci
  * adres rozgłoszeniowy
##   ## 

| Parametr |  wartość   | 
| ----------- | -----------  |
| ``ip``    | `` 192.168.1.145 `` | 
| ``maska``   | `` 255.255.255.128 `` | 
| ``adres sieci``   | `` 192.168.1.128 `` |
| ``liczba hostów``   | `` 126 `` |
| ``host - min``   | `` 192.168.1.129 `` | 
| ``host - max``   | `` 192.168.1.254 `` | 
| ``broadcast``   | `` 192.168.1.255 `` | 
 
## Zadanie

0. Znajdz wszystkie parametry sieci dla hosta o adresie 172.16.128.64 / 16
##   
| Parametr |  wartość   | 
| ----------- | -----------  |
| ``ip``    | `` 192.168.1.145 `` | 
| ``maska``   | `` 255.255.255.128 `` | 
| ``adres sieci``   | `` 172.16.0.0 `` |
| ``liczba hostów``   | `` 65534 `` |
| ``host - min``   | `` 172.16.0.1 `` | 
| ``host - max``   | `` 172.16.255.254 `` | 
| ``broadcast``   | `` 172.16.255.255 `` | 

1.
  * Podziel sieć ```192.168.1.0/16``` na 16 równych podsieci:
    * 2^n >= Ls
    * 2^n >= 16
    * n = 4
  
##   

| Adres sieci |  zakres hostów   | Adres Rozgłoszeniowy |
| ----------- | -----------  | ----------- |
| ``192.168.0.0``    | `` 192.168.0.1 - 192.168.15.254 `` | `` 192.168.15.255 `` |
| ``192.168.16.0``	| `` 192.168.16.1 - 192.168.31.254 `` | ``	192.168.31.255 `` |
| ``192.168.32.0``	| `` 192.168.32.1 - 192.168.47.254 `` | ``	192.168.47.255 `` |
| ``192.168.48.0``	| `` 192.168.48.1 - 192.168.63.254 `` | ``	192.168.63.255 `` |
| ``192.168.64.0``	| `` 192.168.64.1 - 192.168.79.254 `` | ``	192.168.79.255 `` |
| ``192.168.80.0``	| `` 192.168.80.1 - 192.168.95.254 `` | ``	192.168.95.255 `` |
| ``192.168.96.0``	| `` 192.168.96.1 - 192.168.111.254 `` | ``	192.168.111.255 `` |
| ``192.168.112.0``	| `` 192.168.112.1 - 192.168.127.254 `` | ``	192.168.127.255 `` |
| ``192.168.128.0``	| `` 192.168.128.1 - 192.168.143.254 `` | ``	192.168.143.255 `` |
| ``192.168.144.0``	| `` 192.168.144.1 - 192.168.159.254 `` | ``	192.168.159.255 `` |
| ``192.168.160.0``	| `` 192.168.160.1 - 192.168.175.254 `` | ``	192.168.175.255 `` |
| ``192.168.176.0``	| `` 192.168.176.1 - 192.168.191.254 `` | ``	192.168.191.255 `` | 
| ``192.168.192.0``	| `` 192.168.192.1 - 192.168.207.254 `` | ``	192.168.207.255 `` |
| ``192.168.208.0``	| `` 192.168.208.1 - 192.168.223.254 `` | ``	192.168.223.255 `` |
| ``192.168.224.0``	| `` 192.168.224.1 - 192.168.239.254 `` | ``	192.168.239.255 `` |
| ``192.168.240.0``	| `` 192.168.240.1 - 192.168.255.254 `` | ``	192.168.255.255 `` |

2. 
  * Podziel sieć ``172.16.0.0/16`` na 6 równych podsieci.

3. 
  * Podziel sieć ``192.168.1.0/24``, tak aby każda podsieć miała 11 użytkowników.

4. 
  * Podziel sieć ``10.0.0.0/8`` na 5 podsieci. 
    * Podsieć A ma posiadać 100 000 użytkowników,
    * B – 10 000 użytkowników
    * C – 3 000 użytkowników
    * D – 500 użytkowników
    * E – 2 użytkowników.
    
## Wizualizacja

![krakow siec akademicka](cracow-core.jpeg)


## Materiały

    * http://www.cyfronet.krakow.pl/sieci/13152,artykul,charakterystyka_sieci.html
    * https://www.computerworld.pl/news/Sieci-szkieletowe-polskich-operatorow,394360.html
    * https://cidr.xyz/

    * Lista polskich puli adresów
    * http://42.pl/pl/networks.html?html=1