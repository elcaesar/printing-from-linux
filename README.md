# Pasos para instalar desde Linux Debian una impresora conectada a PC con Windows

## En la PC Windows

1. Asegurarse que la impresora conectada a Windows esté **Compartida**

2. Obtener los siguientes datos de la PC de Windows:

    a. Nombre de la impresora compartida: ej: **hp1020**
    
    b. Usuario y contraseña del equipo con Windows: ejemplo: user: **cesar** pass: **1234**
    
    c. Nombre del Grupo de trabajo o Dominio al que pertenece la pc Windows: ejemplo: **archivos.com**
    
    d. Dirección IP de la PC Windows ej: **192.168.2.11**
    
    e. Verificar que en Windows que esté activado **Activar la detección de redes** y el item **Activar el uso compartido con protección con contraseña**



## En Linux

1. Buscar, descargar e instalar los drivers de la impresora del sitio oficial del fabricante:

    Por ejemplo para la HP 1020 (HPLIP):
    [HP Linux Imaging and Printing](https://developers.hp.com/es/hp-linux-imaging-and-printing)

2. Instalar Samba (Windows)

    `sudo apt-get install samba samba-client smbclient samba-common smbnetfs samba samba-common-bin`

3. Verificar status (debe decir **active running**)

    `systemctl status smbd`  
    `systemctl status nmbd`

4. Si el servicio de Samba no se inicia:

    `sudo systemctl start smbd`  
    `sudo systemctl start smbd`

5. Instalar e iniciar CUPS: (debe decir **active running**)

    `sudo apt-get install cups cups-ipp-utils`  
    `sudo systemctl start cups`

6. Hacer una copia backup del archivo **smb.conf**

    `sudo cp /etc/samba/smb.conf /etc/samba/smb_copia.conf`

7. Abrir smb.conf 

    `sudo nano /etc/samba/smb.conf`

8. Modificar la línea donde se especifica el Grupo de trabajo/Dominio  al que pertenece la PC Windows

    `workgroup = workgroup`  *cambiar por* `workgroup = archivos.com` *segun el ejemplo descripto*

9. Reiniciar la PC Linux

10. Ingresar al servidor de CUPS

    `http://localhost:631/admin` (si solicita, ingresar user y pass del usuario linux)

11. Pulsar en **Agregar Impresora** (si solicita, ingresar user y pass del usuario linux)

12. En la sección de Impresoras de Red, elegir el item **Windows printer via SAMBA** y pulsar Siguiente

13. En el campo Conexión ingresar el siguiente formato:

    `smb://user:pass@server/printer` *por ejemplo:* `smb://cesar:1234@192.168.2.11/hp1020` *siguiendo el ejemplo anterior*

    luego pulsar Siguiente

14. Escribir un nombre significativo de la impresora compartida, luego pulsar Siguiente

15. En esta pantalla definir los drivers de la impresora que fueron previamente instalados, 
    seleccionando la marca y el modelo correspondiente. luego pulsar "Añadir Impresora"

16. La impresora ya aparece en la lista de Impresoras en la Configuración del Sistema

## Video instructivo

[![Instructivo Linux](https://i.ytimg.com/vi/gW0nK99CTww/maxresdefault.jpg)](https://www.youtube.com/watch?v=gW0nK99CTww)

https://youtu.be/gW0nK99CTww?si=TZeeEaY6iw_RDE3h

