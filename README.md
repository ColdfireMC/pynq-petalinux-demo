# *Setup* de Sistema Básico Con Digilent *PYNQ* #

* Crear un "Block Design" de Vivado.

* Importar *Constraints*, si se desea agregar RTL (Prefiera activar copiar el .xdc dentro del proyecto).

* Agregar un PS "ZYNQ 7 Processing System". Este será el CPU principal del diseño. Los dispositivos de la familia Zynq incluyen un CPU interno. Este bloque lo hace visible al diseño de bloques.
![Agregando un PS](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_181928.png "Agregando un PS")
* Configurar el CPU principal.
    * Ejecutar "Run block Automation". Esto configurara la memoria DRAM y el reloj.
![Autoconfigurar PS](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_182012.png "Autoconfigurar PS")
![Autoconfigurar PS](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_182859.png "Autoconfigurar PS")
Cerrar la ventana con <kbd>OK</kbd>
    * Habilitar Interrupciones externas: El sistema tiene un sistema de interrupciones interno para los *timers* y los controladores integrados, como serial y USB. También dispone de un sistema de interrupciones relacionadas con tejido de FPGA. Perifericos instanciados como IP podrían requerir estas interrupciones Para agregarlos, doble clic al bloque ZYNQ (Esto produce el comando "Re-customize" IP).
    ![Configurando Interrupciones del PS](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_182859.png "Configurando Interrupciones del PS")
        * Ir a la seccion "*Interrupts*".
        * Habilitar "Enable PS-PL Interrupts". Esto habilita las interrupciones provenientes del tejido, que van al CPU.
        * Habilitar "Enable Shared Interrupts". Estas serían las interrupciones que admiten ser compartidas, desde el punto de vista del software.
    * Conectar `FCLK_CLK0` con `M_AXI_GP0_ACLK`. Este reloj tambien sera el reloj principal del bus AXI.
![Conectar Reloj Principal al Bus AXI](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_191246.png "Conectar Reloj Principal al Bus AXI") (Los *Warnings* no generan mayores problemas).
* Agregar y Configurar IP's
    * Agregar *Processor System Reset* y *Run Connection Automation*
    ![*Run Connection Automation*_1](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_191335.png "*Run Connection Automation*")
    ![*Run Connection Automation*_2](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192129.png "*Run Connection Automation*")
     ![*Run Connection Automation*_3](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192143.png "*Run Connection Automation*")
    * Agregar *Concat* (Para empaquetar las líneas de interrupción provenientes del tejido de FPGA), *AXI IIC* (Para el Shield y algunos PMODS) , *AXI QUAD SPI* y 2 *AXI GPIO* (Para luces y botones).
    ![Agregando Concat](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192221.png "Agregando Concat")
    ![Agregando GPIO](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192323.png "Agregando GPIO")
    * Configurar bloque *Concat*: Dado que el tipo de `IRQ_F2P` no es compatible con las líneas de interrupcion, deben concatenarse las líneas en un solo vector lógico. Debe agregarse una entrada por cada interrupción
     * Doble Clic en *Concat*
     * Editar el ampo *Number of Ports*. En este caso serian 4 líneas (Una por cada dispositivo con Interrupción).![Configurando Concat](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192529.png "Configurando Concat")
    * Configurar *AXI GPIO*: Doble clic en el bloque
      * Habilitar Interrupciones. Para evitar *Polling* de los botones, es recomendable habilitar la interrupción del GPIO ![Configurando GPIO 1](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192347.png "Configurando GPIO")
      * Puede seleccionarse a que conectar cada interfaz GPIO, a las conexiones disponibles en el preset de la placa. Si se desea conectar RTL o usar los nombres de los .xdc, debe seleccionarse *Custom* y crear un puerto.![Configurando GPIO 2](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_193423.png "Configurando GPIO") ![Configurando GPIO 3](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_193445.png "Configurando GPIO")
    * Los otros bloques usan en este caso la configuración predeterminada.
* Conectar los bloques de IP: Puede autoconectarse los bloques IP AXI. Nótese que la conexión automática no conecta las interrupciones y solo puede autoconectar un árbol de reloj.
  * Hacer clic en el pop-up *Run Connection Automation*.
  * Seleccionar todos los bloques en las casillas.
  Esto hará que se genere automáticamente un bloque de interconexión y arbitraje AXI y las líneas de dirección y datos del bus.
  * Conectar líneas de Interrupción al CPU principal
    * Conectar La salida del bloque concat al *ZYNQ Processor System*.
    * Conectar cada una de las líneas de interrupción de los bloques IP perifericos agregados al bloque *Concat*.
    ![Conectando Interrupciones](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_193211.png "Conectando Interrupciones")
           
### Opcionales ###
* Agregar BRAM. Puede ser de utilidad si se desea proveer o descargar datos de algun diseño en RTL, evitando el uso del bus AXI.
  * Agregar el bloque *BRAM Controller*.![Agregando BRAM](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200503_235308.png "Agregando BRAM")
  * Agregar el bloque *Block Memory Generator*.![Agregando BRAM](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200503_235423.png "Agregando BRAM")
  Pueden configurarse las caracteristicas de la BRAM, como la cantidad de puertos, el ancho y la capacidad máxima.
  ![Agregando BRAM](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200503_235555.png "Agregando BRAM")
  * Al agregar regiones de memoria, debe verificarse donde fueron mapeadas, especialmente si se agregó memoria o si además del Zynq, por ejemplo, se instanció un coprocesador de apoyo en el tejido de FPGA, para eso debe verse el "Address Editor", accesible desde el menú tools.
  ![Address Editor](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/  Screenshot_20200511_043715.png "Address Editor")
  No Asignar direcciones a una región hará que la síntesis falle. Pueden remapearse regiones del diseño. Este mismo mapa será visible por Petalinux y Xilinx Vitis.

## Generación de Productos ##
* Crear *Wrapper* de HDL(botón derecho sobre el diseño de bloques (el archivo .bd, en la pestaña *Sources*). (Esto encapsula el diseño y lo hace referenciable por el simulador y el sintetizador).
![Generando Wrapper](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_193758.png "Generando Wrapper")
* Generar bitstream (Esto podria tardar hasta una hora)
![Generando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_194026.png "Generando Bitstream")
* Exportar Hardware, incluyendo el bitstream
![Exportando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200420_023711.png "Exportando Bitstream")


## Crear Proyectos Petalinux ##

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
Esto Iniciará un configurador `Kconfig` de la imagen. En muchos casos la configuración predeterminada es satisfactoria, por lo que bastará con seleccionar <kbd>Save</kbd>.

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


## Problemas Conocidos ##

* Los ítems del árbol de Vitis no tienen menús contextuales consistentes, es decir, pese a que las opciones podrían ser no válidas, los menús contextuales no deshabilitarán las opciones. Esto puede resultar confuso, o incluso destruir el proyecto al borrar las configuraciones iniciales. 
* ADVERTENCIA: Cerciorarse que el bitstream que está siendo cargado ya sea en SD o SPI es exactamente el correspondiente a la tarjeta y su diseño. Si se especifica uno diferente podría causar que el FPGA se comporte de manera errática. Algunas tarjetas como Nexys, tienen control sobre el regulador de tensión, lo que permite en términos prácticos, controlar el voltaje del FPGA. Sin embargo en una placa con un FPGA que no esté diseñado para esto, graves daños al regulador, FPGA y a la placa pueden ocurrir si el reguldor es configurado de una manera que no está soportada. Del mismo modo, las interfaces de memoria y los pines a los que se les ha especificado cierto estandar lógico fijo a la placa, podrían fallar y comportarse erraticamente si se especifica un bitstream incorrecto. El FSBL no valida por ningún método que el bitstream corresponde exactamente al modelo de dispositivo y placa, sólo verifica el tipo (FPGA, SOC). No se recomienda probar MicroSD's o imágenes de flash que no se sepa su contenido exacto.
* A veces las ventanas de los menús contextuales olvidan las configuraciones y hay que especificarlas nuevamente. Esto es particularmente notorio en la ventana "Create Boot Image" en la especificación de Offsets y Alineaciones.
* A veces la placa fallará el *flasheo* si se comienza a flashear con JP1 en una posición que no sea JTAG. En ese caso, poner el jumper en la posicion JTAG antes de grabar, luego grabar y final apagar la tarjeta y volver a ponerlo en QSPI.
* Las políticas de *Sleep* del cpu vienen de un modo predeterminado muy agresivo y el CPU al entrar en modo *Idle* sólo puede despertarse con una interrupción de timer interno o una excepción. En https://www.xilinx.com/support/answers/69143.html se describe un método para evitar este problema. 




## Depuración de Aplicaciones y Kernel Linux ##


## Otras Configuraciones del SOC Zynq ##

# Mapa de la estructura del SOC Zynq #
![Mapa de Zynq](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_2020-05-28_21-18-23.png "Mapa de Zynq").
Este mapa resume las porciones configurables (Verdes), del PS de Zynq. Al hacer clic en ellas se puede acceder directamente a las pestañas asociadas.

# Configuración de I/O con el mapa de I/O #

Del mismo modo que puede configurarse el layout de los mapeos de memoria en un PC, el soc Zynq ofrece remapeo de los dispositivos integrados. Debe tenerse en cuenta que solo es posible mapearlos en ubicaciones predeterminadas, y los perifericos internos conectados no pueden remapear sus conexiones al tejido de FPGA.
![Mapa de I/O](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_2020-05-28_21-19-51.png "Mapa de I/O")
Cada bloque representa un rango de decodificación de I/O. No puede haber 2 dispositivos con los mismos rangos. Cuando ese es el caso, el mapa colorea rojo el rango, indicando que la configuración no es Valida.
![Mapa de I/O](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_2020-05-28_21-20-08.png "Mapa de I/O")
Si se desea "hacer externa" una conexión y que salga al Tejido de FPGA, puede seleccionarse el modo EMIO del dispositivo. Al Hacerlo quedará disponible sus entrada/salidas al diseño de bloques.
![Resultado EMIO](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_2020-05-28_21-20-27.png "Resultado EMIO")
Los dispositivos integrados de los SOC, suelen ya estar alambrados de manera fija a los pines correspondientes del dispositivo, y no al tejido. Esto significa que no siempre será posible reemplazar un dispositivo integrado por uno del tejido FPGA, en el lugar, sin cambiar las conexiones. Lamentablemente, de momento, pese a que el archivo de la tarjeta tiene esta infomación, no es posible saber sin prueba y error desde la interfaz de Vivado cual es el caso, hasta que llega el momento de implementar el diseño. En caso desfavorable, el proceso de implementación fallará y mostrará un error como este.
![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_2020-05-29_02-09-04.png "Flasheando Bitstream")
En el caso de desear usar el Shield de PYNQ, debe utilizarse esta característica para tener pines GPIO e i2c, o bien usar un IP de GPIO.


https://es.slideshare.net/ZachPfeffer/beyond-printk-efficient-zynq-ultrascale-mpsoc-linux-debugging-and-development
