======================================
Instalación del Firmware AREDN |trade|
======================================

Los pasos para instalar el firmware en un dispositivo están documentados en el sitio web de AREDN |trade| en la sección `Current Software <https://www.arednmesh.org/content/current-software>`_. En el menú **Software**, seleccione **Download** para acceder a la página *Current Software*.

Se pueden presentar dos casos a la hora de instalar el firmware AREDN |trade|:

1. Si ya tiene una versión de firmware de AREDN |trade| ejecutándose en su dispositivo, puede usar la interfaz web de su computadora para navegar a **Setup > Administration > Firmware Update** para instalar su nuevo firmware. Este proceso se explicará con más detalle en la sección **Configuración Avanzada** de esta guía. Para obtener información adicional vea también *Consejos de Actualización de Firmware* en la sección **Guías Prácticas**.


----------

2. Si está instalando el firmware AREDN |trade| por primera vez en un dispositivo, cada plataforma de hardware puede requerir un procedimiento único.

   .. image:: _images/firmware-install.png
      :alt: Firmware Install Connections
      :align: center


  El diagrama anterior muestra que su ordenador con la imagen de firmware descargada debe estar conectado al nodo mediante cable Ethernet para instalar la imagen AREDN |trade|. Es útil conectar ordenador y nodo a través de un simple switch Ethernet para que éste pueda mantener el enlace con el ordenador mientras se reinicia el nodo.


  Nodos con diferente hardware requerirán métodos diferentes para instalar el firmware AREDN |trade|. Para los dispositivos Ubiquiti, el cliente TFTP de su computadora se conectará al servidor TFTP del nodo para cargar la imagen del firmware. Para los dispositivos TP-LINK y GL-iNET, el navegador web de su ordenador se conectará al servidor web del nodo para cargar la imagen del firmware. Para los dispositivos Mikrotik, su ordenador ejecutará un servidor de arranque remoto y el cliente de arranque remoto del nodo cargará su imagen de arranque desde su computadora. Consulte los procedimientos específicos a continuación para el hardware de su nodo.

Proceso de instalación por primera vez en Ubiquiti
--------------------------------------------------

Los dispositivos **Ubiquiti** tienen un servidor `TFTP <https://en.wikipedia.org/wiki/Trivial_File_Transfer_Protocol>`_ incorporado al que puede cargar la imagen *factory* de AREDN |trade| . Su ordenador debe tener disponible un software de cliente TFTP. Linux y Mac tienen clientes TFTP nativos, pero es posible que deba habilitar u obtener un cliente TFTP para ordenadores con Windows. Si está utilizando Windows, `habilite el cliente TFTP <https://www.trishtech.com/2014/10/enable-tftp-telnet-in-windows-10/>`_  o descargue e instale otro `cliente TFTP independiente <http://tftpd32.jounin.net/tftpd32_download.html>`_  de su elección.

Los diferentes programas de cliente TFTP pueden tener diferentes opciones de línea de comando o indicadores que deben usarse, así que asegúrese de estudiar la sintaxis de comando para su software de cliente TFTP. El ejemplo que se muestra a continuación puede no incluir las opciones específicas requeridas por su programa cliente.

Descargue el archivo *factory* apropiado para su dispositivo siguiendo las instrucciones de la sección **Descarga del firmware de AREDN** de esta documentación.

1. Configure el adaptador de red Ethernet de su ordenador a una dirección IP estática perteneciente a la subred correcta para su dispositivo. Consulte la documentación de su hardware específico para determinar el número de red correcto. Como en el ejemplo a continuación, la mayoría de los dispositivos Ubiquiti tienen una dirección IP predeterminada de 192.168.1.20, por lo que puede darle a su ordenador una IP estática en la red 192.168.1.x con una máscara de red de 255.255.255.0. Por ejemplo, configure su adaptador Ethernet a una dirección IP estática de 192.168.1.100.


  Puede elegir cualquier número para el cuarto octeto, siempre que no sea el mismo de la dirección IP del nodo. Por supuesto, también debe evitar usar 192.168.1.0 y 192.168.1.255, que son direcciones reservadas para identificar la red en sí y la dirección de broadcast de esa red. Otros dispositivos pueden tener diferentes direcciones IP o subredes predeterminadas, por lo tanto, seleccione una IP estática para su ordenador que lo coloque en la misma subred pero que no entre en conflicto con la IP predeterminada del dispositivo.

2. Conecte un cable Ethernet desde su computadora al switch, y otro cable desde el puerto LAN del adaptador PoE al switch.

3. Ponga el dispositivo Ubiquiti en modo TFTP manteniendo presionado el botón "reset" mientras conecta el cable Ethernet de su nodo en el puerto POE del adaptador PoE.

4. Continúe presionando el botón "reset" del dispositivo durante aproximadamente 30 a 45 segundos hasta que vea los LED en el nodo alternando en un patrón 1-3, 2-4, 1-3, 2-4, entonces suelte el botón de "reset".

5. Abra una ventana de comandos en su ordenador y ejecute un comando de transferencia de archivos para enviar el firmware AREDN a su dispositivo. Apunte a la dirección IP predeterminada de su nodo Ubiquiti, como 192.168.1.20 o 192.168.1.1 para AirRouters. El siguiente es un ejemplo de comandos TFTP que transfieren la imagen del firmware a un nodo:

  >>>
  [Linux/Mac]
  > tftp 192.168.1.20
  > bin                 [Transferir en modo "binario"]
  > trace on            [Muestra la transferencia en progreso]
  > put <ruta completa al archivo de firmware>
    [Por ejemplo, put /temp/aredn-3.19.3.0-ubnt-nano-m-xw-factory.bin]
  -----------------------------------
  [Windows con una única línea de comandos]
  > tftp -i 192.168.1.20 put C:\temp\aredn-3.19.3.0-ubnt-nano-m-xw-factory.bin

  El cliente TFTP debe indicar que los datos se están transfiriendo y que la transferencia se ha completado.

6. Observe los LED durante aproximadamente 2-3 minutos hasta que el nodo haya terminado de reiniciarse. El reinicio se completa cuando la luz del LED 4 (la más a la derecha) está encendida y se mantiene en verde.

7. Configure la interfaz de red Ethernet de su ordenador para usar DHCP para obtener una dirección IP del nodo. Es posible que deba desconectar y volver a conectar el cable Ethernet del ordenador para forzarla a obtener una nueva dirección IP del nodo.

8. Después de que el nodo se haya reiniciado, abra un navegador web e ingrese la siguiente URL: http://localnode.local.mesh Algunos ordenadores pueden tener rutas de búsqueda de DNS configuradas que requieren que use el `nombre de dominio cualificado completo (FQDN) <https://en.wikipedia.org/wiki/Fully_qualified_domain_name>`_ para resolver *localnode* a la dirección IP del nodo mesh.

9. Navegue a la página *Setup* y configure el "firstboot" del nuevo nodo "firstboot" como se describe en la sección **Basic Radio Setup (Configuración básica de radio)**.


Proceso de instalación por primera vez en TP-LINK 
-------------------------------------------------
Proceso Recomendado
+++++++++++++++++++

Actualmente los dispositivos **TP-LINK** permiten utilizar la interfaz de usuario del navegador web *PharOS* preinstalada por el fabricante para cargar y aplicar nuevas imágenes de firmware. Esta es la forma más fácil de instalar el firmware AREDN |trade|. Vaya a la sección *Setup* para seleccionar y cargar nuevo firmware. Consulte la documentación de TP-LINK para su dispositivo si tiene dudas sobre el uso de su interfaz de usuario incorporada.

Proceso Alternativo
+++++++++++++++++++

Los dispositivos TP-LINK también llevan incorporado un cliente :abbr:`TFTP (Trivial File Transfer Protocol)` y `Bootp <https://en.wikipedia.org/wiki/Bootstrap_Protocol>`_ que les permite obtener nuevo firmware de una fuente externa. Su ordenador debe ejecutar un servidor TFTP / Bootp para proporcionar imágenes de firmware al nodo. En ciertas situaciones, es posible que necesite usar este método para actualizar el firmware o para restaurar un archivo de recuperación de TP-LINK siguiendo los pasos a continuación.

*Preparación*

1. Descargue el archivo TP-LINK *factory* apropiado y renómbrelo como ``recovery.bin`` 

2. Configure el adaptador de red Ethernet de su ordenador a una dirección IP estática que sea miembro de la subred correcta para su dispositivo. Consulte la documentación de su hardware especéfico para determinar el número de red correcto. Como en el ejemplo a continuación, la mayoría de los dispositivos TP-LINK usan la subred 192.168.0.x de manera predeterminada, por lo que puede darle a su ordenador una IP estática como 192.168.0.100 con una máscara de red de 255.255.255.0.

  Puede elegir cualquier número para el cuarto octeto, siempre que no sea la misma que la dirección IP del nodo y no esté dentro del rango de direcciones DHCP que proporcionará en el paso 2 a continuación. Por supuesto, también debe evitar usar 192.168.0.0 y 192.168.0.255, que son direcciones reservadas que identifican la red en sí y la dirección de difusión de esa red. Otros dispositivos pueden tener diferentes direcciones IP o subredes predeterminadas, así que seleccione una IP estática para su ordenador que la coloque en la misma subred.


3. Conecte un cable Ethernet desde su ordenador al switch, y otro cable desde el puerto LAN del adaptador PoE al switch.


*Procedimiento para Linux*

1. Cree un directorio en su ordenador llamado ``/tftp`` copie allí el archivo TP-LINK ``recovery.bin``.

2. Determine el nombre de la interfaz Ethernet de su ordenador con ``ifconfig``. Será la interfaz que estableció más arriba en 192.168.0.100. Utilizará este nombre de interfaz en el comando a continuación como el nombre después de  ``-i`` y debe sustituir su nombre de usuario de inicio de sesióndespués de ``-u``. Utilice una ``dhcp-range`` (rango de direcciones) IP que también estén en la misma subred que el ordenador: por ejemplo 192.168.0.110,  192.168.0.120 como se muestra a continuación.


3. Hágase ``root`` y abra una ventana de terminal para ejecutar el siguiente comando dnsmasq:

  >>>
  (root)# dnsmasq -i eth0 -u joe --log-dhcp --bootp-dynamic \
        --dhcp-range=192.168.0.110,192.168.0.120 -d -p0 -K \
        --dhcp-boot=recovery.bin --enable-tftp --tftp-root=/tftp/

4. Con la unidad PoE apagada, conecte un cable Ethernet desde el nodo TP-LINK al puerto POE.

5. Presione el botón reset en el TP-LINK y manténgalo presionado mientras enciende la unidad PoE. Continúe presionando el botón de reinicio hasta que vea la información de salida en la terminal del ordenador donde ejecutó el comando dnsmasq, lo que debería ocurrir después de unos 10 segundos. Suelte el botón de reinicio cuando el ordenador comience a comunicarse con el nodo. Cuando vea el mensaje "sent" (enviado), esto indica que el nodo TP-LINK ha descargado la imagen exitosamente y se reiniciará. Ahora puede hacer <ctrl> C o kill dnsmasq.


*Procedimiento para Windows*

Necesitará el software `Tiny PXE <http://reboot.pro/files/file/303-tiny-pxe-server/>`_ en su ordenador con Windows. Descargue este software y extráigalo en su ordenador.

1. Navegue a la carpeta donde extrajo el software *Tiny PXE* y edite el archivo ``config.ini``. Directamente debajo de la etiqueta ``[dhcp]``, agregue la siguiente línea: ``rfc951=1`` luego guarde y cierre el archivo.

2. Copie la imagen del firmware ``recovery.bin`` en la carpeta ``files`` debajo de la ubicación del directorio del servidor Tiny PXE.

3. Inicie el servidor Tiny PXE exe y seleccione su IP de interfaz Ethernet de la lista desplegable llamada ``Option 54 [DHCP Server]``, asegurándose de marcar la casilla ``Bind IP``. En la sección "Boot File", ingrese ``recovery.bin`` en el campo *Filename* y desactive la casilla de verificación "Filename if user-class = gPXE or iPXE". Haga clic en el botón *Online* en la parte superior de la ventana de Tiny PXE.


.. image:: _images/tiny-pxe-tpl.png
  :alt: Tiny PXE Display
  :align: center

4. Con la unidad PoE apagada, conecte un cable Ethernet desde el nodo TP-LINK al puerto POE. Mantenga presionado el botón de reinicio en el nodo mientras enciende la unidad PoE.

5. Continúe presionando el botón reset hasta que vea ``TFTPd: DoReadFile: recovery.bin`` en la ventana de log de Tiny PXE.

6. Suelte el botón reset del nodo y haga clic en el botón *Offline* en Tiny PXE. Ha terminado de usar Tiny PXE cuando el nodo ha leído la imagen del firmware.

*Pasos Finales de la Configuración*

1. Configure la interfaz de red Ethernet de su ordenador para usar DHCP para obtener una dirección IP del nodo.

2. Después de que se reinice el nodo, abra un navegador web e ingrese la siguiente URL: ``http://localnode.local.mesh`` Algunos ordenadores pueden tener rutas de búsqueda de DNS configuradas que requieren que use el `fully qualified domain name (FQDN) <https://en.wikipedia.org/wiki/Fully_qualified_domain_name>`_  para resolver el n*localnode* a la dirección IP del nodo de la malla.

3. Navegue a la página *Setup* y configure el "firstboot" del nuevo nodo  como se describe en la sección **Basic Radio Setup (Configuración básica de radio)** 


Proceso de instalación por primera vez en Mikrotik
--------------------------------------------------

Los dispositivos **Mikrotik** se deben flashear usando unos pasos que son similares al proceso alternativo TP-LINK descrito anteriormente. Su ordenador debe ejecutar un servidor TFTP / Bootp para proporcionar imágenes de firmware a los nodos Mikrotik. Los nodos Mikrotik requieren un proceso de **instalación en dos partes** : primero, instale y arranque el archivo mikrotik-vmlinux-initramfs correcto con la extensión **elf**, y luego use la interfaz de administración de usuario AREDN |trade| solo-en-memoria para completar la instalación del archivo mikrotik-rb con la extensión **bin**.


**Preparación**

1. Descargue los archivos apropiados de Mikrotik **elf** y **bin**. Cambie el nombre del archivo *elf* a ``rb.elf`` y mantenga el archivo bin disponible para más adelante.
2. Configure el adaptador de red Ethernet de su ordenador a una dirección IP estática que sea miembro de la subred correcta para su dispositivo. Consulte la documentación de su hardware específico para determinar el número de red correcto. Como en el ejemplo a continuación, la mayoría de los dispositivos Mikrotik usan la subred 192.168.1.x de manera predeterminada, por lo que puede darle a su ordenador una IP estática como 192.168.1.100 con una máscara de red de 255.255.255.0.

  Puede elegir cualquier número para el cuarto octeto, siempre que no sea la misma que la dirección IP del nodo y no esté dentro del rango de direcciones DHCP que proporcionará en el paso 2 a continuación. Por supuesto, también debe evitar usar 192.168.1.0 y 192.168.1.255, que son direcciones reservadas que identifican la red en sí y la dirección de difusión de esa red. Otros dispositivos pueden usar diferentes subredes predeterminadas, como las unidades QRT que usan 192.168.88.x. Seleccione una IP estática para su ordenador que la coloque en la misma subred que su dispositivo.

3. Conecte un cable Ethernet desde su ordenador al switch, y otro cable desde el puerto LAN del adaptador PoE al switch. Si está flasheando un dispositivo Mikrotik hAP ac lite, conecte el cable Ethernet desde *Port 1* del Mikrotik al switch.

**Procedimiento de Linux**

1. Cree un directorio en su ordenador llamado ``/tftp`` copie ahí el archivo ``rb.elf``.

2. Determine el nombre de la interfaz Ethernet de su ordenador con ``ifconfig``. Será la interfaz que establezca en 192.168.1.100. Utilizará este nombre de interfaz en el comando que se muestra más abajo como el nombre después de ``-i`` y debe escribir su nombre de usuario de inicio de sesión después de ``-u``. Utilice un rango ``dhcp-range`` de direcciones IP que también esté en la misma subred que el ordenador: por ejemplo 192.168.1.110,192.168.1.120 como se muestra a continuación.

3. Hágase ``root`` y abra una ventana de terminal para ejecutar el siguiente comando dnsmasq:

  >>>
  (root)# dnsmasq -i eth0 -u joe --log-dhcp --bootp-dynamic \
        --dhcp-range=192.168.1.110,192.168.1.120 -d -p0 -K \
        --dhcp-boot=rb.elf --enable-tftp --tftp-root=/tftp/

4. Con la unidad PoE apagada, conecte el nodo Mikrotik al puerto POE. Mantenga presionado el botón **reset** del Mikrotik mientras enciende la unidad PoE o el dispositivo hAP.

5. Continúe presionando el botón de reinicio hasta que vea la información de salida en la ventana del ordenador donde ejecutó el comando dnsmasq, que debería aparecer después de unos diez segundos. Suelte el botón de reinicio cuando el ordenador comience a comunicarse con el nodo. Cuando vea el mensaje "enviado", esto indica éxito, y el nodo ha descargado la imagen y se reiniciará. Ahora puede hacer <ctrl> C o kill al proceso dnsmasq.

**Procedimiento en Windows**

You will need `Tiny PXE <http://reboot.pro/files/file/303-tiny-pxe-server/>`_ software on your Windows computer. Download this software and extract it on your computer.

1. Navigate to the folder where you extracted the *Tiny PXE* software and edit the ``config.ini`` file.  Directly under the ``[dhcp]`` tag, add the following line:  ``rfc951=1`` then save and close the file.

2. Copy the ``rb.elf`` file into the ``files`` folder under the Tiny PXE server directory location.

3. Start the Tiny PXE server exe and select your Ethernet interface IP from the dropdown list called ``Option 54 [DHCP Server]``, making sure to check the ``Bind IP`` checkbox. Under the "Boot File" section, enter ``rb.elf`` into the the *Filename* field, and uncheck the checkbox for "Filename if user-class = gPXE or iPXE". Click the *Online* button at the top of the Tiny PXE window.

.. image:: _images/tiny-pxe-mik.png
  :alt: Tiny PXE Display for Mikrotik
  :align: center

4. With the PoE unit powered off, connect the Mikrotik node to the POE port. If you are flashing a Mikrotik hAP ac lite device, connect the LAN cable from *Port 1* of the Mikrotik to the dumb switch.

5. Press and hold the reset button on the node while powering on the PoE unit or the device. Continue holding the reset button until you see ``TFTPd: DoReadFile: rb.elf`` in the Tiny PXE log window.

6. Release the node’s reset button and click the *Offline* button in Tiny PXE.  You are finished using Tiny PXE when the firmware image has been read by the node.

**Final Configuration Steps**

1. After booting the AREDN firmware image the node should have a default IP address of 192.168.1.1. Change your computer’s Ethernet interface to DHCP mode to obtain an IP address from the node. For the hAP ac lite, pull the Ethernet cable from the WAN port (1) on the Mikrotik and insert it into one of the LAN ports (2,3,4). You should be able to ping the node at 192.168.1.1. If this does not work, then something is wrong. Don't proceed until you can ping the node. You may need to disconnect and reconnect your computer's network cable to ensure that your IP address has been reset. Also, you may need to clear your web browser's cache in order to remove cached pages remaining from your node's previous firmware version.

2. In a web browser, open the node’s Administration page ``http://192.168.1.1/cgi-bin/admin`` (user = 'root' password = 'hsmm') and navigate to the *Setup > Administration > Firmware Update* section. Select the **bin** file you previously downloaded and click the *Upload* button.

   As an alternative to using the node's web interface, if your node has plenty of free memory you can copy the **bin** file to the node and run a command line program to install the image. This will allow you to see any error messages that are not displayed when using the web interface upgrade procedure. Execute the following commands from your computer:

   >>>
   my-computer:$ scp -P 2222 aredn-firmware-filename.bin root@192.168.1.1:/tmp
   my-computer:$ ssh -p 2222 root@192.168.1.1
   ~~~~~~~ after logging into the node with ssh ~~~~~~~
   node:# sysupgrade -n /tmp/aredn-firmware-filename.bin

3. After the node reboots, navigate to the node’s *Setup* page and configure the new “firstboot” node as described in the **Basic Radio Setup** section.

GL-iNET First Install Process
------------------------------

**GL-iNET** devices allow you to use the manufacturer's pre-installed *OpenWRT* web interface to upload and apply new firmware images. Check the GL-iNET documentation for your device if you have questions about initial configuration. Both GL-iNET and AREDN devices provide DHCP services, so you should be able to connect your computer and automatically receive an IP address on the correct subnet. GL-iNET devices have a default IP address of 192.168.8.1, so if for some reason you need to give your computer a static IP address you can use that subnet.

After the GL-iNET device has been booted and configured, navigate to the *Upgrade* section and click *Local Upgrade* to select the AREDN |trade| "sysupgrade.bin" file you downloaded for your device. Be sure to uncheck/deselect the "Keep Settings" checkbox, since GL-iNET settings are incompatible with AREDN. After the device has rebooted to the AREDN |trade| image, you should be able to navigate to ``http://192.168.1.1`` for the firstboot or NOCALL page to appear.

If for some reason your GL-iNET device gets into an unusable state, you should be able to recover using the process documented here:
`GL-iNET debrick procedure <https://docs.gl-inet.com/en/2/troubleshooting/debrick/>`_

Troubleshooting Tips
--------------------

One common issue can occur when installing firmware using a web browser interface. The browser cache stores data for the URLs that have been visited, but IP addresses and other parameters often change during the install process. It is possible for the cache to contain information that doesn’t match the latest settings for the URL, so the browser may block the connection setup and display an ERR_CONNECTION_RESET message. Clearing the web browser's cache will allow the latest URL settings to be registered so you can continue with the install process.

Instead of a *Connection Reset* message, sometimes a *Bad Gateway* message may appear. This is an `HTTP Status Code <https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml>`_ that can mean any of several things. Often it indicates a network communication issue between a web browser and a web server. During AREDN |trade| firmware installs you can usually resolve a *Bad Gateway* issue by doing one or more of the following things:

* Refresh or Reload the URL for your node.
* Clear your browser cache and delete cookies.
* Close your browser and restart a new session.
* Use a different web browser program or a *Safe Mode / Incognito* browser window.
* Unplug and reconnect the Ethernet cable from your computer to ensure that your machine has received a new DHCP IP address on the same subnet as the node's updated IP.

If for some reason the node's web interface does not work, you may be able to use a command line program to install the firmware image. You must first copy the firmware *bin* file to the node, then log into the node and use the *sysupgrade* program to install the image as illustrated below.

>>>
my-computer:$ scp -P 2222 aredn-firmware-filename.bin root@192.168.1.1:/tmp
my-computer:$ ssh -p 2222 root@192.168.1.1
~~~~~~~ after logging into the node as root (hsmm) ~~~~~~~
node:# sysupgrade -n /tmp/aredn-firmware-filename.bin

Additional questions and troubleshooting assistance can usually be obtained by creating a post on the AREDN |trade| `online forum <https://www.arednmesh.org/forum>`_, which has an active community of helpful and experienced operators.

Post-Install Steps
------------------

Once your device is running AREDN |trade| firmware, you can display its web interface by connecting your computer to the LAN port on the :abbr:`PoE (Power over Ethernet)` and navigating to the following URL: ``http://localnode.local.mesh``  Some computers may have DNS search paths configured that require you to use the `fully qualified domain name (FQDN) <https://en.wikipedia.org/wiki/Fully_qualified_domain_name>`_ to resolve *localnode* to the mesh node's IP address. Each node will serve its web interface on both port 80 and 8080.

By default AREDN |trade| devices run the :abbr:`DHCP (Dynamic Host Control Protocol)` service on their LAN interface, so your computer will receive an IP address from the node as soon as it is connected with an Ethernet cable. Ensure that your computer is set to obtain its IP address via :abbr:`DHCP (Dynamic Host Control Protocol)`. You may also need to clear your web browser's cache in order to remove cached pages remaining from your node's previous firmware version.

.. |trade|  unicode:: U+00AE .. Registered Trademark SIGN
   :ltrim:
