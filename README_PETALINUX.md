# Crear Proyectos Petalinux #

### Instalando Petalinux ###
Petalinux solo funciona en sistemas con Linux Instalado. Dado conflictos de versiones de bibliotecas, debe instalarse solo en las versiones que estan soportadas por Xilinx. Lamentablemente, a esta fecha, las versiones soportadas estan entrando en "Fin de soporte" o carecen de características o correcciones importantes (Como Centos 6 o Ubuntu 18.04).
Para instalarlo basta con la siguiente secuencia de comandos, después de descargar [Petalinux](https://www.xilinx.com/products/design-tools/embedded-software/petalinux-sdk.html#licensing "petalinux-v2019.2-final-installer.run") desde el sitio de Xilinx, aceptando los términos de contrato
```bash
# mkdir /opt/pkg/petalinux/2019.2/
# chown <su_usuario> /opt/pkg/petalinux/2019.2/
$ ./petalinux-v2019.2-final-installer.run /opt/pkg/petalinux/2019.2
```
### Inicializando Entorno Petalinux ###
Cada vez que vaya a utilizarse, debe inicializarse el entorno actual de trabajo con
```bash
$ source <path-to-installed-PetaLinux>/settings.sh
```
### Crear Proyectos Petalinux ###
El entorno Petalinux necesita que se cree un proyecto, para poder construir Aplicaciones, Imágenes de Arranque, Núcleos (*Kernel*) o bibliotecas compartidas (.o, .so). Hay 2 maneras principales
#### Crear Proyectos Petalinux Desde BSP ####
A veces los fabricantes de tarjetas, entregan un "Paquete de soporte" con extensión .bsp, con una configuración predeterminada. Para poder construir una distribución de petalinux con estos paquetes, deben seguirse los siguientes pasos

```bash
$ source <path-to-installed-PetaLinux>/settings.sh
$ petalinux-create -t project -s <ruta_al_bsp>
```
Deben ser de la misma versión mayor de Petalinux(No puede crearse un proyecto de 2019 con un BSP 2014).

#### Crear Proyectos Petalinux Desde Definición de Hardware Generada con Vivado ####
En este caso, el desarrollador ha construido su propio sistema con Vivado, probablemente incluyendo su propia RTL y condiguración de IP
```bash
$ source <path-to-installed-PetaLinux>/settings.sh
$ petalinux-create --type project --template zynq --name <nombre_del_proyecto>
```
Esto creará una carpeta con el nombre del proyecto, luego debe configurarse el proyecto con la definición de hardware creada con Vivado

```bash
$ cd <carpeta_con_nombre_del_proyecto>
$ petalinux-config --get-hw-description=<carpeta_del_proyecto_de_Vivado>
```
Esto Iniciará un configurador `Kconfig` de la imagen. En muchos casos la configuración predeterminada es satisfactoria, por lo que bastará con seleccionar <kbd>Save</kbd>.![Configurando con Kconfig](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_2020-06-04_15-08-53.png "Configurando con Kconfig")

### Construir Petalinux ###
Para construir el sistema completo predeterminado
```bash
$ cd <carpeta_con_nombre_del_proyecto>
$ petalinux-build
```
![Construyendo Petalinux](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200429_222955.png "Construyendo Petalinux")
Esto podría tardar hasta 30 minutos. Puede ocupar grandes cantidades de espacio en el disco.

### Generar Imagen empaquetada Para SD ###
Para poder cargar el sistema desde una SD, deben empaquetarse las imágenes recién compiladas. Las imágenes construidas van a estar en `<carpeta_con_nombre_del_proyecto>/images/linux`. 
```bash
$ petalinux-package --boot --fsbl <Imagen FSBL> --fpga <bitstream> --u-boot
```
o bien, si solo hay una configuración reciente
```bash
$ petalinux-package
```
Finalmente, los archivos `BOOT.BIN` e `image.ub` en `<carpeta_con_nombre_del_proyecto>/images/linux` deben ser copiados a una SD con formato FAT32. Esto es suficiente para que el Zynq logre arrancar Petalinux. No olvide poner el *Jumper* "JP4" en la posición "SD" en la tarjeta.

### Generar Imagen empaquetada Para SPI ###
Las imágenes construidas van a estar en `<carpeta_con_nombre_del_proyecto>/images/linux`.

La configuración predeterminada no tiene incorporado el arranque por SPI con la imagen linux dentro. De grabarlo en las condiciones normales, el bootloader intentaría cargar el bitstream de la SD o descargarlo por TFTP. El bootloader U-boot, debe ser modificado para poder acceder a la memoria QSPI. Para esto deben configurarse las opciones globales y el componente `u-boot`
```bash
$ cd <carpeta_con_nombre_del_proyecto>
$ petalinux-config --get-hw-description=<carpeta_del_proyecto_de_Vivado>
```
Cerciorarse de que el dispositivo principal es axi_spi_0 en la configuracion de dispositivo de arranque.
Guardar la configuracion con <kbd>Save</kbd> y salir con <kbd>Exit</kbd>. Esto tardará algunos minutos en completarse.
Luego debe configurarse el componente u-boot
```bash
$ cd <carpeta_con_nombre_del_proyecto>
$ petalinux-config -c u-boot
```
Debe habilitarse el arranque por nand SPI y QSPI.

Las imágenes construidas van a estar en `<carpeta_con_nombre_del_proyecto>/images/linux`. 
```bash
$ petalinux-package --boot --fsbl <Imagen FSBL> --fpga <bitstream> --u-boot
```
o bien, si solo hay una configuración
```bash
$ petalinux-package
```
Sin embargo debe notarse que la imagen predeterminada podría exceder ampliamente el tamaño de la memoria. Debe configurarse el Núcleo (*Kernel*) y el "RootFS" para descartar cualquier componente que se considere innecesario, entre ellos, controladores de dispositivos que no va a estar conectados, aplicaciones y *daemons* que no se van a usar.

```bash
$ cd <carpeta_con_nombre_del_proyecto>
$ petalinux-config -c kernel
```
```bash
$ petalinux-config -c rootfs
```
Al hacer cualquiera de estos cambios, la imagen debe reconstruirse. Luego con la tarjeta conectada y con Xilinx Vitis, debe crearse una aplicación del mismo modo que una aplicación baremetal. Asegurarse que se está referenciando la plataforma correcta. 

El template en este caso debe ser "Zynq FSBL" 

![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200512_003740.png "Flasheando Bitstream")

Después de presionar <kbd>Finish</kbd>, el entorno de Vitis estará listo

Del árbol debe seleccionarse la aplicación y construirla. Esto creará la imagen de arranque base .bif.

Nuevamente del arbol debe seleccionarse la aplicación, pero esta vez, debe seleccionarse "Create boot Image"

![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200512_012346.png "Flasheando Bitstream")

En la ventana está disponible la composición inicial de la imagen de arranque .bif. Debe tener al menos

* Un FSBL (First Stage Bootloader, .elf). Este es el primer codigo que se ejecuta, está encargado de inicializar el hardware, valiéndose de las especificaciones de la placa entregadas por el fabricante, y el bitstream con el diseño que había sido creado con Vivado
* Un bitstream (.bit)
* Un bootloader o chainloader. En este caso, petalinux ha implementado u-boot, como su bootloader para linux. u-boot se encargará de inicializar y utilizar los dispositivos de almacenamiento y comunicaciones que se hayan especificado en la configuración.

![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200512_012402.png "Flasheando Bitstream")

Si no está presente alguno de ellos, puede atribuírsele las siguientes razones

* No ha seleccionado el ítem correcto del árbol  (Debe seleccionarse la raíz de la Aplicación).
* No se ha construido el sistema base de la aplicación.
* El template no es el correcto y está esperando que se especifique un código fuente con procedimiento `main`.

Al estar presentes todos los componentes, debe agregársele la imagen de linux. En este caso, se trata de un archivo tipo .ub, que está en `<carpeta_con_nombre_del_proyecto>/images/linux`. Normalmente se llama `image.ub`. Esto puede lograrse con <kbd>Add</kbd>.
![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200512_012422.png "Flasheando Bitstream")
Con <kbd>Browse</kbd> debe especificarse la ubicación de la imagen. Una vez especificada, la imagen debe situarse en el offset `0x520000` ([Zynq 7000 embedded design tutorial]https://www.xilinx.com/support/documentation/sw_manuals/xilinx2019_2/ug1165-zynq-embedded-design-tutorial.pdf "Embedded Design Tutorial PDF Xilinx). 
![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200512_012511.png "Flasheando Bitstream")
Antes de generar la imagen, copiar la ruta del FSBL al portapapeles (Esto debido a un bug intermitente en el comando "Program Flash"). La imagen ahora puede crearse con <kbd>Create Image</kbd>.
![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200512_012539.png "Flasheando Bitstream")
Ahora debe grabarse la imagen. En este momento la tarjeta debe estar conectada y el Servidor de Hardware de Xilinx debe tener la tarjeta reconocida. Seleccionando la aplicacion del arbol, click derecho y seleccionar "Program Flash".
![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200512_020753.png "Flasheando Bitstream")
En el campo "FSBL" debe haber una imagen válida. Si la ventana reporta que la imagen seleccionada no existe, pegar la dirección que se había copiado al portapapeles. 
Una vez especificado el FSBL, presionar <kbd>Program</kbd>
Finalmente, si la imagen Fue grabada exitosamente, después de cargar, un terminal serial debería mostrar el "login" de la distribución petalinux que se ha construido
![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200505_044035.png "Flasheando Bitstream")

## Otras Maneras de Ejecutar Petalinux ##

### Carga directa con Serial/JTAG ###

Es posible cargar directo a la memoria algunas o todas las etapas del arranque directamente desde la máquina de desarrollo usando JTAG. Para  esto, la imagen debe estar compilada y empaquetada previamente, en modo *prebuilt* (paso previo a construir un BSP)
```bash
$ petalinux-package --prebuilt
```
Esto creará el directorio "prebuilt" dentro del proyecto.
Debido a un bug, el comando anterior, no renombra el *bitstream* del FPGA a "download.bit". Esto causa que a la hora de arrancar no encuentre el bitstream y la tarjeta quede en modo FSBL. Asegurarse que en la carpeta "prebuilt/implementation", haya una copia del bitstream, pero con nombre "download.bit", que se encuentra en "prebuilt/images" con el nombre de "system.bit". Luego puede arrancarse el sistema con el siguiente comando:

```bash
$ petalinux-boot --jtag --prebuilt <nivel>
```
Donde nivel indica si se quiere arrancar el FSBL (1), el bootloader (2) y linux (3, kernel y userland).
Este comando tiene numerosas opciones para especificar porciones diferentes del firmware y bitstream. Para más detalles, leer el capítulo 5 de [ug1144](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2020_1/ug1144-petalinux-tools-reference-guide.pdf)

El enlace JTAG tardará unos 2 minutos en cargar la memoria de la tarjeta en el caso de Linux. El proceso tiene la siguiente apariencia



Nótese que el enlace no redirecciona al dispositivo de terminal una vez que la tarjeta tiene el enlace serial disponible. Debe hacerse manualmente con Minicom, Picocom o Teraterm, o configurando uno de los terminales enlazado a tal dispositivo (aunque esto no se recomienda si no es un enlace permanente).



Linux debería verse del siguiente modo si ha cargado correctamente. Nótese que funciona en modo tmpfs si es que no se ha insertado una tarjeta SD que contenga un Filesystem.



### Emulando con QEMU ###

De un modo similar a JTAG, es posible cargar directo a la memoria algunas o todas las etapas del arranque directamente desde la máquina de desarrollo usando la línea de comandos.  Esto podría servir para desarrollo rápido y "co-simular" con Vitis o Vivado. Esta última característica es experimental, pero es prometedora ya que permitiría simular diseños completos (RTL+Software).

```bash
$ petalinux-boot --qemu --prebuilt <nivel>
```

"Nivel" tiene el mismo significado que para JTAG. En este caso, QEMU toma el control total del terminal y solo puede ser terminado con <kbd>Ctrl</kbd><kbd>A</kbd><kbd>X</kbd>. Al ejecutarse, debería tomar la siguiente apariencia. 



QEMU aún no puede emular todos los IP de Xilinx. Una lista exahustiva del soporte está disponible en [ug1169](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2020_1/ug1169-xilinx-qemu.pdf)




## Problemas Conocidos ##

* Los ítems del árbol de Vitis no tienen menús contextuales consistentes, es decir, pese a que las opciones podrían ser no válidas, los menús contextuales no deshabilitarán las opciones. Esto puede resultar confuso, o incluso destruir el proyecto al borrar las configuraciones iniciales. 
* ADVERTENCIA: Cerciorarse que el bitstream que está siendo cargado ya sea en SD o SPI es exactamente el correspondiente a la tarjeta y su diseño. Si se especifica uno diferente podría causar que el FPGA se comporte de manera errática. Algunas tarjetas como Nexys, tienen control sobre el regulador de tensión, lo que permite en términos prácticos, controlar el voltaje del FPGA. Sin embargo en una placa con un FPGA que no esté diseñado para esto, graves daños al regulador, FPGA y a la placa pueden ocurrir si el reguldor es configurado de una manera que no está soportada. Del mismo modo, las interfaces de memoria y los pines a los que se les ha especificado cierto estandar lógico fijo a la placa, podrían fallar y comportarse erraticamente si se especifica un bitstream incorrecto. El FSBL no valida por ningún método que el bitstream corresponde exactamente al modelo de dispositivo y placa, sólo verifica el tipo (FPGA, SOC). No se recomienda probar MicroSD's o imágenes de flash que no se sepa su contenido exacto.
* A veces las ventanas de los menús contextuales olvidan las configuraciones y hay que especificarlas nuevamente. Esto es particularmente notorio en la ventana "Create Boot Image" en la especificación de Offsets y Alineaciones.
* A veces la placa fallará el *flasheo* si se comienza a flashear con JP1 en una posición que no sea JTAG. En ese caso, poner el jumper en la posicion JTAG antes de grabar, luego grabar y final apagar la tarjeta y volver a ponerlo en QSPI.
* Las políticas de *Sleep* del cpu vienen de un modo predeterminado muy agresivo y el CPU al entrar en modo *Idle* sólo puede despertarse con una interrupción de timer interno o una excepción. En https://www.xilinx.com/support/answers/69143.html se describe un método para evitar este problema. 

## Aplicaciones Linux ##

El comando

```bash
$ petalinux-config -c rootfs
```

Abrirá una ventana de Kconfig que ofrecerá paquetes y grupos de aplicaciones que vendrán preinstaladas en la distribución de Petalinux

![Kconfig para rootfs](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200707_022319.png "Kconfig para rootfs")

El menú "apps" lista un par de aplicaciones de ejemplo

![Kconfig para rootfs](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200707_022353.png "Kconfig para rootfs")

Al crear una serie de aplicaciones propias, Kconfig las mostrará en el item "user packages" y puede habilitarse o desabilitarse la instalación de las mismas. dentro del mismo proyecto de Petalinux.


Las aplicaciones serán almacenadas tras construir el proyecto en los archivos rootfs en <raíz del proyecto>/images/linux/. El uso de alguno de estos archivos dependerá de las opciones de empaquetado del proyecto que se utilicen con el comando `petalinux-package`







## Depuración de Aplicaciones y Kernel Linux ##

Para programas mas complejos y desarrollo de controladores y módulos de kernel y hacer mediciones finas de rendimiento, podría ser necesario depurar a nivel de kernel. Esto, en el mejor de los casos, entregará información de que línea del código de qué módulo se está evaluando y el estado de la máquina correspondiente. Esto también podría unirse al análisis lógico de la lógica programable si se instala el hardware y software (handlers e interrupciones) adecuados. Para depurar un sistema Linux, se requiere:

* Compilar el kernel y las aplicaciones en modo "Debug": Debe generarse información de depuración junto con el código. Esto permite al ejecutable proporcionar marcadores para identificar que línea del código se evalúa en este momento y las variables y símbolos del código "Kernel Hacking" ---> "Compile-time checks and compiler options" ---> "Generate Debug Information".

![Agregando Información de depuración](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200708_022033.png "Agregando Información de depuración")

* Des-optimizar el código: (Este paso es más importante de lo que parece) Pese a que el código haya sido compilado con información de depuración, las optimizaciones pueden distorsionar la correspondencia entre las líneas de código máquina, las líneas de ensamblador y las líneas de código fuente.
Para estono basta solo hacer que sea posible depurar, sino que además el código sea "entendible por humanos", permitiendo depurar excepciones e interrupciones relacionadas con controladores y errores derivados de operaciones aritméticas (overflow, underflow, división por cero) y contar con una perspectiva razonable de que instrucción de máquina está siendo representada por el código. Para ello se debe activar la configuración "Kernel Hacking" ---> "Compile-time checks and compiler options" ---> "Generate Readeable Assembler Code".

![Generar Código Máquina Legible](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200708_022033.png "Generar Código Máquina Legible")

Esto es particularmente intenso en máquinas RISC, como ARM, debido a que las acciones de cada una de las instrucciones distan mucho de la complejidad de las líneas del código. 
Además, algunos compiladores tienen optimizaciones a nivel de alineación del código, por lo que algunas instrucciones son espaciadas con otras instrucciones y cambiadas de orden, de modo de aprovechar la caché, el procesamiento fuera de orden y la alineación de la decodificación. Petalinux solo tiene las opciones "Optimize for Size" and "Optimize for Performance" en "General Setup" ---> "Compiler optimization level" --->

![Optimización](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200708_033841.png "Optimización")

"Optimize for Performance" hará el código ser apenas entendible, y podría ser inconveniente para usarlo en flash SPI, por lo que se recomienda mientras se depura usar "Optimize for Size".

* Activar diversos niveles de mensajes, según se requiera: Si se desea depurar servicios y protocolos dentro del kernel, activar mensajería podría ser necesario. Debe notarse que se requiere un enlace de comunicaciones adicional para este fin. Además, puede que sea necesario que el mismo cuente con líneas de control de flujo, cosa que tarjetas como PYNQ, en un giro inesperado del diseño, fue omitida, por lo que tales mensajes solo podrían ser obtenidos con control de flujo por software, o usando un enlace de red Ethernet.

Para acceder

"Kernel Hacking" ---> "Kernel low-level debugging functions" 
![Habilitando funciones de depuración de bajo nivel](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200707_021656.png "Habilitando funciones de depuración de bajo nivel")

"Kernel Hacking" ---> "Compile-time checks and compiler options" ---> "Provide GDB Scripts for GDB debugging".
![Scripts de Apoyo para el depurador](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200708_010942.png "Scripts de Apoyo para el depurador")

* Contar con el código fuente: el código fuente exacto debe estar disponible, para poder tener una lectura acertada. No se debe modificar el código desde el archivo mientras se depura. Hacerlo dejará inválidos los números de línea emitidos por los marcadores y la lectura del código será erronea. Para modificar el código debe evaluarse y ensamblarse la expresión, siempre y cuando no implique desplazar el código.


* Interceptar la máquina en alguna excepción (esto es quizas lo más complejo, si se hace de manera manual), o detenerla en un punto arbitrario. En condiciones normales la máquina corre libremente. Al interceptarla, dependiendo del *handler* que se utilice, podría ser que alguna interrupción detuviera el código. Esto es verdadero en el kernel, pero en otros programas esto debe hacerse de manera manual.

## Uso de QEMU ##











