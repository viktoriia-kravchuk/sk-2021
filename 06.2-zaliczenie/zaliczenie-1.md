# Zadanie 1

Organizacja planuje rozpoczęcie działalności w 3 budynkach, w każdym z nich przewiduje do 1000 urządzeń IP

1. Zaprojektuj oraz udokumentuj prototyp rozwiązania z wykorzystaniem oprogramowania ``CISCO Packet Tracer``, ``VirtualBox`` lub podobnego.

## Schemat

![zadanie 1](stage-01.svg)

## Zawartość

* Adresy poszczególnych sieci IP

    ##

    | Sieć | Adres sieci | Maska|  Zakres hostów   | Adres rozgłoszeniowy |
    | ------------ |----------- | ----------- | -----------  | ----------- |
    | **LAN 1** | ``195.225.180.0``   | `` 255.255.252.0 `` | `` 195.225.180.1 - 195.225.183.254 `` | `` 195.225.183.255 `` |
    | **LAN 2** | ``185.4.212.0``	| `` 255.255.252.0 `` | `` 185.4.212.1 - 185.4.215.254 `` | ``185.4.215.255`` |
    | **LAN 3** | ``91.201.44.0`` | `` 255.255.252.0 `` | `` 91.201.44.1 - 91.201.47.254 `` | `` 91.201.47.255 `` |

* Adresacja linków pomiędzy routerami

    * link LAN 1 - LAN 2
        ## 

        | Sieć | Adres | Maska|
        | ------------ |----------- | ----------- |
        | **LAN 1 ip_1** | ``20.10.0.1``| `` 255.255.255.252 `` |
        | **LAN 2 ip_1** | ``20.10.0.2``| `` 255.255.255.252`` | 

    * link LAN 1 - LAN 3
        ## 

        | Sieć | Adres | Maska|
        | ------------ |----------- | ----------- |
        | **LAN 1 ip_2** | ``20.10.0.5``| `` 255.255.255.252 `` |
        | **LAN 3 ip_1** | ``20.10.0.6``| `` 255.255.255.252 `` | 
    
    * link LAN 2 - LAN 3
        ## 

        | Sieć | Adres | Maska|
        | ------------ |----------- | ----------- |
        | **LAN 2 ip_2** | ``20.10.0.9`` | `` 255.255.255.252 `` |
        | **LAN 3 ip_2** | ``20.10.0.10``| `` 255.255.255.252 `` | 

* Tablica routingów na poszczególnych routerach

    * Tablica routera sieci **LAN 1** 
        ## 

        | Network |  Destination address | Mask| Next hoop|
        | ------------ |----------- | ----------- |----------- |
        | **LAN 2** | ``185.4.212.0``| `` 255.255.252.0 `` |  ``20.10.0.2`` |
        | **LAN 3** | ``91.201.44.0`` | `` 255.255.252.0 ``| ``20.10.0.6`` | 

    * Tablica routera sieci **LAN 2** 
        ## 

        | Network |  Destination address | Mask| Next hoop|
        | ------------ |----------- | ----------- |----------- |
        | **LAN 1** | ``195.225.180.0``| `` 255.255.252.0 `` |  ``20.10.0.1`` |
        | **LAN 3** | ``91.201.44.0`` | `` 255.255.252.0 ``| ``20.10.0.9`` | 

    * Tablica routera sieci **LAN 3** 
        ## 

        | Network |  Destination address | Mask| Next hoop|
        | ------------ |----------- | ----------- |----------- |
        | **LAN 1** | ``195.225.180.0``| `` 255.255.252.0 `` |  ``20.10.0.5`` |
        | **LAN 2** | ``185.4.212.0`` | `` 255.255.252.0 ``| ``20.10.0.10`` |


    

