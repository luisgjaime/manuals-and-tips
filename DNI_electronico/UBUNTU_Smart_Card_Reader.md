# UBUNTU 18.04: Instalar Smart Card Reader (ACR38)

Para tener un lector de tarjetas funcionando, tenemos que instalar algunos paquetes en Linux.

1. Ejecute este comando:

     ```sudo apt install libpcsclite1 pcscd pcsc-tools```

2. Para comprobar que nuestro *card reader* funciona, podemos hacerlo ejecutando en el Terminal: 

    ```pcsc_scan```

    El resultado debería ser parecido a uno de los siguientes, sino es que algo ha ido mal.
    
    ```
    Using reader plug'n play mechanism
    Scanning present readers...
    0: ACS ACR38U 00 00
 
    Mon Jun  1 20:38:05 2020
    Reader 0: ACS ACR38U 00 00
    Card state: Card removed,
    ```
   
    o:
    
    ```
    PC/SC device scanner
    V 1.4.16 (c) 2001-2009, Ludovic Rousseau <ludovic.rousseau@free.fr>
    Compiled with PC/SC lite version: 1.5.3
    Scanning present readers...
    0: SCM SCR 3310 (21120839GXXXXX) 00 00

    Mon Aug 15 11:47:42 2011
     Reader 0: SCM SCR 3310 (21120839GXXXXX) 00 00
      Card state: Card inserted,
      ATR: 3B 7D 96 00 00 80 XX XX XX XX XX XX XX XX XX XX XX XX

    ATR: 3B 7D 96 00 00 80 XX XX XX XX XX XX XX XX XX XX XX XX
    + TS = 3B --> Direct Convention
    + T0 = 7D, Y(1): 0111, K: 13 (historical bytes)
      TA(1) = 96 --> Fi=512, Di=32, 16 cycles/ETU
        250000 bits/s at 4 MHz, fMax for Fi = 5 MHz => 312500 bits/s
      TB(1) = 00 --> VPP is not electrically connected
      TC(1) = 00 --> Extra guard time: 0
    + Historical bytes: 80 31 80 65 B0 XX XX XX XX XX XX XX XX
      Category indicator byte: 80 (compact TLV data object)
        Tag: 3, len: 1 (card service data byte)
          Card service data byte: 80
            - Application selection: by full DF name
            - EF.DIR and EF.ATR access services: by GET RECORD(s) command
            - Card with MF
    ```

## Problemas conocidos

Si al ejecutar `pcsc_scan`, el resultado es similar a este:

```
SCardListReader: Cannot find a smart card reader. (0x8010002E)
Waiting for the first reader...
```
    
El controlador no está bien instalado. Para el **modelo ACR38**, en Ubuntu 18.04 es necesario instalar algunos 
paquetes adicionales
    
1. Descargue e instale el controlador ACR38 manualmente desde Smart Card Readers. 
[Enlace directo de descarga](https://www.acs.com.hk/download-driver-unified/11929/ACS-Unified-PKG-Lnx-118-P.zip)
    
2. Descomprima el controlador descargado y abra una terminal en ese directorio + instale el paquete debian. Para
Ubuntu 18 sería:
    ``` 
    cd ubuntu/bionic
    sudo dpkg -i libacsccid1_1.1.8-1~ubuntu18.04.1_amd64.deb
    ```

3. Reinicia `pcsc`

    ```sudo service pcscd restart```

4. Prueba a lanzar de nuevo `pcsc_scan`

# Fuentes

- https://help.ubuntu.com/community/CommonAccessCard
- https://askubuntu.com/questions/12535/why-doesnt-my-acr38-smartcard-reader-work
