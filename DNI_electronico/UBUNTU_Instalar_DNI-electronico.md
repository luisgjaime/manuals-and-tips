# UBUNTU 18.04: Instalar DNI electrónico y Smart Card Reader

Esta guía la escribo tras expermintar varios problemas a la hora de tener funcionando 
[DNI electrónico](https://www.dnielectronico.es/PortalDNIe/) en Ubuntu 18.04. No pretende sustituir a la instalación 
oficial, pero sí ayudar en alguno de los problemas que te puedes encontrar.

---
*Nota*: Tenemos que tener un *Smart Card Reader* funcionando. Esta otra guía explica cómo instalar un 
[Smart Card Reader (ACR38)](./UBUNTU_Smart_Card_Reader.md).
---


## 1 Instalar Firefox v68

El certificado no se puede utilizar a menos que se utilice la versión 68 de Firefox. Para ello, primero descargarla 
de aquí: [1716 - ¿Cómo instalar mozilla firefox 68 si tengo instalada la versión 69 o superior?](https://www.sede.fnmt.gob.es/web/sede/preguntas-frecuentes/acerca-de-mozilla-firefox/-/asset_publisher/fVZppcBHj0oa/content/1716-como-instalar-mozilla-firefox-68-si-tengo-instalada-la-version-69-?inheritRedirect=false&redirect=https%3A%2F%2Fwww.sede.fnmt.gob.es%3A9440%2Fweb%2Fsede%2Fpreguntas-frecuentes%2Facerca-de-mozilla-firefox%3Fp_p_id%3D101_INSTANCE_fVZppcBHj0oa%26p_p_lifecycle%3D0%26p_p_state%3Dnormal%26p_p_mode%3Dview%26p_p_col_id%3Dcolumn-2%26p_p_col_count%3D1)

Seguimos los siguientes pasos para poder **lanzar dos versiones de Firefox
dentro del mismo sistema operativo**: 

1. Una vez completada la descarga, extraiga los archivos en su escritorio (`~/Desktop`).
2. Abra una terminal y ejecute firefox -p -no-remote. Se abrirá el administrador de perfiles.
3. Presione *Crear nuevo perfil* en la nueva ventana que se abrirá, haga clic en "Siguiente", luego asigne un nombre a 
su nuevo perfil de Firefox (por ejemplo: `mi-perfil-firefox68`) y haga clic en "Finalizar" y presione "Salir" para 
cerrar el administrador de perfiles.
4. Ahora arrancamos el nuevo firefox (68) con el nuevo perfil. Escriba su terminal: 

    ```~/Desktop/firefox/firefox -p mi-perfil-firefox68 -no-remote``` 
    
    (**Muy importante usar `-no-remote`**)

5. No olvidar **desactivar las actualizaciones autómaticas** de Firefox en Preferencias/General/Actualizaciones de 
Firefox. Marcando la opción "Buscar Actualizaciones, pero permitirle elegir si instalarlas".


## 2 Crear un acceso directo en el escritorio (Opcional)

Si queremos dejar esta instalación como otra versión de Firefox y tener un acceso directo en el escritorio, se puede
crear de la siguiente manera.

---
*Nota*: Esta carpeta `firefox68`, puede estar localizada donde queramos de nuestro sistema.

---

1. Crea un nuevo archivo en el escritorio que se llame por ejemplo `firefox-68.desktop`. Desde línea de comandos sería:

    ```gedit ~/Desktop/Firefox-68.desktop```

2. Copia y pega el siguiente texto, y modifica la información que cambie con respecto a tu configuración:

    ```
    #!/usr/bin/env xdg-open
    [Desktop Entry]
    Version=1.0
    Type=Application
    Terminal=false
    Exec=~/Desktop/firefox68/firefox -p mi-perfil-firefox68 -no-remote
    Name=Firefox v68
    Comment=Mozilla Firefox v68 (AutoFirma)
    Icon=/usr/share/icons/HighContrast/48x48/apps/firefox.png
    ```

3. Haz el archivo ejecutable. Desde línea de comandos sería:

    ```chmod ugo+x ~/Desktop/Firefox-68.desktop```


## 3 Instalar libpkcs11-dnie

Para esta parte del manual, seguir los pasos que encontramos en la 
[página oficial del DNIe](https://www.dnielectronico.es/PortalDNIe/PRF1_Cons02.action?pag=REF_1100).

1. Descargar el ejecutable más adecuado para nuestro sistema 
(*GNU/Linux Ubuntu 18.04 (LTS) Bionic Beaver y 19.04 Disco Dingo*) que se puede descargar [aquí](https://www.dnielectronico.es/PortalDNIe/PRF1_Cons02.action?pag=REF_1112). 

2. Si no sabemos como instalar un `.deb`, seguir el manual que se puede descargar 
[aquí](https://www.dnielectronico.es/PDFs/manuales_instalacion_unix/Manual_de_Instalacion_de_Opensc_DNIe_version_2_1.pdf).

## 4 Configurar el navegador para usar libpkcs11-dnie

Para usar el DNI electrónico se requiere:

1. Instalar el Módulo de Seguridad PKCS#11
    - Para instalar el módulo PCKS#11 debe ir a Editar/Preferencias/Avanzado/Cifrado/Dispositivos de seguridad
    - Seleccione "Cargar"
    - Dele un nombre al módulo. (Por ejemplo "DNIe Modulo PKCS #     11")
    - Indique manualmente la ruta del módulo: /usr/lib/libpkcs11-dnie.so
    - Pulse el botón "Aceptar"

2. Instalar el Certificado Raíz de la Autoridad de Certificación del DNIe
    - Para instalar el certificado raíz ir a Editar/Preferencias/Avanzado/Cifrado/Ver certificados
    - Seleccione "Importar".
    - Indique manualmente la ruta del certificado raíz: /usr/share/libpkcs11-dnie/ac_raiz_dnie.crt
    - El asistente le pedirá que establezca la confianza para el certificado.
    - Marque las tres casillas de confianza.
    - Pulse el botón "Aceptar"


## 5 Validar que nuestro DNIelectrónico funciona

En la [página oficial](https://www.dnielectronico.es/PortalDNIe/PRF1_Cons02.action?pag=REF_320&id_menu=15) se puede 
comprobar si hemos configurado correctamente nuestro navegador. Recomiendo usar el enlace de 
[FNMT](https://www.sede.fnmt.gob.es/certificados/persona-fisica/verificar-estado).

Si nos pide la contraseña de nuestro DNI quiere decir que todo se ha instalado correctamente.

# Problemas encontrados

Si obtenemos al verificar un error de este tipo: 

```
Error 500--Internal Server Error
From RFC 2068 Hypertext Transfer Protocol -- HTTP/1.1:
10.5.1 500 Internal Server Error

The server encountered an unexpected condition which prevented it from fulfilling the request.
```

posiblemente habremos cometido algún error en el paso 4 de este tutorial. 

# Fuentes

- https://support.mozilla.org/en-US/questions/974208
- https://www.dnielectronico.es/PortalDNIe/
- https://www.sede.fnmt.gob.es/certificados/persona-fisica/verificar-estado
