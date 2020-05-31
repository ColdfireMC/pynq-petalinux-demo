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

## Aplicaciones *Baremetal* Con Xilinx Vitis ##

Después de exportar el hardware, abrir Xilinx Vitis. Al ser similar a Eclipse creará un *Workspace*. Los archivos de código fuente y del proyecto quedarán guardados en este entorno protegido, mientras se desarrolla la aplicación.

* Crear una aplicación *Baremetal* 
 * Ir a "File"->"Project"->"Application". ![Creando proyecto de Aplicación](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200430_021604.png "Creando proyecto de Aplicación")
 * Darle un nombre al proyecto. No ponerle espacios al nombre. ![Creando proyecto de Aplicación](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200430_010058.png "Creando proyecto de Aplicación")
 * Selecionar la plataforma del proyecto. Puede seleccionarse una plataforma existente (Lamentablemente está *buggy* no ocupar) o crear una nueva. ![Nombre proyecto de Aplicación](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200430_021910.png "Nombre proyecto de Aplicación")
 * Al presionar <kbd>Next</kbd>, debe seleccionarse que método será usado para generar la plataforma.![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183750.png "Configurando Plataforma") Seleccionar "Create new platform from Hardware (XSA)". La ventana tiene un campo donde se listan plataformas recientes disponibles en los ejemplos o bien dentro del *Workspace*. Seleccionar "Create new platform from Hardware (XSA)".Si se necesita importar una plataforma de elaboración propia (el presente caso), debe hacer clic en "+" y seleccionar la carpeta donde se encuentra el archivo `.xsa` que se generó al exportar hardware con Vivado.![Importando Hardware](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183803.png "Importando Hardware") Presionar <kbd>Next</kbd>, después de seleccionar la plataforma recién agregada.![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183813.png "Importando Hardware")   
 * Ahora debe seleccionarse el *Domain* de la plataforma. En este caso, en cada campo debe seleccionarse "ps7_cortexa9_0"(Podría haber más de un procesador independiente dentro del sistema que se ha importado), "Standalone" y "C". También debe seleccionarse "Generate Boot components"
   ![Seleccionando Arquitectura](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200430_022033.png "Seleccionando Arquitectura")
 * Al presionar <kbd>Next</kbd>, debe seleccionarse un *Template* para este caso, se seleccionará "Hello World".![Seleccionando Template](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183848.png "Seleccionando Template")
 * Al presionar <kbd>Next</kbd> quedará disponible el entorno de vitis![Estado inicial del proyecto](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183915.png "Estado inicial del proyecto")

En el arbol o **Explorer**, pueden verse los directorios del proyecto.
 * **Release** y **Debug** son los objetivos convencionales de construcción. **Release** por lo general optimizará el ejecutable y no incluirá ni símbolos ni información de depuración. **Debug** incluirá información de depuración y omitirá algunas optimizaciones. Pueden personalizarse los objetivos si se edita el archivo "Makefile" dentro de los directorios.
 * **src** Contiene el codigo fuente del ejecutable que se va a construir. Un proyecto *Baremetal* mínimo necesita de la inicialización de plataforma "platform.c" y de un codigo fuente .c que tenga el procedimiento `main`. Si desea aceder a un código fuente referenciado (por ejemplo `#include <algunarchivo.h>`), puede seguir como si fuera un enlace, el nombre del archivo presionando <kbd>Ctrl</kbd>. El *script* de enlazado "ldscript.ld" contiene la configuración de la estructura del ejecutable .elf. Vitis tiene un parser que permite editarlo de manera gráfica.

### Configurando parámetros de plataforma ###

Como el proyecto solo tiene referenciado el terminal, solo habrá como parámetro configurable, la velocidad de transferencia del *UART*. En "platform.c", debe especirficarse en la la velocidad deseada. Esta es la velocidad que se deberá configurar en el terminal más tarde.![Configuración UART](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_184026.png "Configuración UART")
  
### Configurando estructura del ejecutable ###   

Al seleccionar "ldscript.ld" del árbol aparecerá en la sección principal la configuración del ejecutable ![Configuración Ejecutable](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200430_210611.png "Configuración Ejecutable")
* "Available Memory Regions": Lista las regiones de memoria disponibles en el sistema.
* "Stack and Heap Sizes": Controla el tamaño de tales regiones. El "Stack" almacena las direcciones y las Flags de retorno de las llamadas, por lo tanto, un programa más complejo, requerirá mas "Stack". "Heap" es una estructura de datos tipo montículo que se usa para asignar memoria dinámicamente. Un programa que genere estructuras de datos que crecen con el paso de la ejecución requerirá más "Heap". Los campos permiten establecer el tamaño de ambas secciones. Si es que no se compartirá la memoria con nada más, se recomienda ocuparla completamente. Una buena medida es darle al Stack el 25% o el 50% del total y el resto para el montículo.
* "Section to Memory Region Mapping": Muestra la asociación entre las secciones del ejecutable y las regiones de memoria. En el caso del PS del Zynq, la memoria DRAM está configurada al arranque, por lo tanto puede ocuparse completamente.

### Editando el código fuente ###

En este caso "helloworld.c" es el archivo con el procedimiento `main`. Todo lo que se desea que el programa haga en su cuerpo principal debería ser agregado aquí. Para propósitos de prueba, modificar el programa en la linea
```C
    print("Hello World\n\r");
````
por
```C
 unsigned long count=0;
    while(1)
    {
    printf("Hello World\n\r");
    printf("%lu", count);
    count++;
    }
```
Nótese que al final hay una llamada a `cleanup_platform()` definida en "platform.c". Si se desean agregar más acciones, deben editarse ahí.

### Generando Ejecutable ###

Al presionar "build" (el martillo) se construye la aplicación (holamundo_pynq_system) y el entorno que se le ha asociado, según el objetivo que se haya establecido. Al lado del ícono, aparece una flecha, donde se puede seleccionar el objetivo de compilación ("Debug", "Release" u otro que se haya creado).

### Generando Imagen ###

En el arbol, botón derecho en holamundo_pynq_system.![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200430_224449.png "Generando Bitstream"). 

Aparece la ventana de generación de imagen de arranque. Si se contruyó la imagen correctamente, debería aparecer, si se seleccionar "Import from existing BIF file", las sigueintes particiones de *Flash* ![Generando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200430_224508.png "Generando Bitstream")

Si no se desean agregar más particiones, presionar <kbd>Create Image</kbd>.

![Generando Imagen](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200430_232637.png "Generando Imagen")

### *Flasheando* Imagen ###

En el arbol, botón derecho en holamundo_pynq_system.![Flasheando](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200430_233829.png "Flasheando")
Aparece la ventana de Flasheo. ![Flasheando](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200430_234447.png "Flasheando")
Presionar <kbd>Program</kbd>. Esto tardará bastante tiempo. Asegurarse de que no se interrumpirá el suministro de energía de la placa. De interrumpirse, podría "regrabarse" la memoria cambiando la configuración de los jumpers de la tarjeta de desarrollo a otra que no haga al FPGA arrancar con la memoria SPI (Por ejemplo JTAG).

Para más información sobre las posibles aplicaciones del modo *Baremetal* véase la [API Baremetal de Xilinx](https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/18841745/Baremetal+Drivers+and+Libraries "Altassian Xilinx Baremetal").
## Problemas Conocidos ##
* La "actualización" de plataformas no es capaz de encontrar el bitstream exportado por Vivado, asi que cada vez que se altere la plataforma, debe recrearse nuevamente. Esto rompe parte de la característica de portabilidad automática.
* Cuando se crea primero una plataforma y después la aplicación, con el *Workspace* vacío, el *Wizard* generador de aplicaciones no puede encontrar la definición de plataforma ya creada y queda incompleta y no puede terminarse, o bien, debe seleccionarse una de las incluidas en Vitis y luego reasignar la plataforma manualmente (Cuidado con las inicializaciones, porque si fueron modificadas por el programador, serán totalmente incompatibles).
* Las opciones de los menús contextuales no tienen resultados consistentes. Asegurarse de no seleccionar una opción incorrecta, ya que parecerá que hay un problema o que la imagen se generó incorrectamente.

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


## Depuración de Aplicaciones Baremetal ##

Existen 2 alternativas para depurar una aplicación Baremetal.
* Depuración del programa (Variables, Instrucciones Registros de CPU).
No necesita nada especial de la RTL. El encapsulamiento de JTAG que provee el FTDI instalado en la placa, permite que no sea necesario operar con puerto serial y control de flujo, que si bien, es común su uso para depurar y permite gran parte de la funcionalidad de un depurador, no permite ver "en vivo" el contenido de los registros o la memoria, y tiene un comportamiento "conflictivo" si es que el programador pide paradas arbitrarias al programa desde el depurador. JTAG permite todo esto y más, dependiendo de lo que provea la red de Boundary Scan del CPU.

Ya exportado la descripción de hardware a Vitis, debe cambiarse el objetivo de compilación de la aplicación a Debug.

Puede encargarse desde Vitis que comience la depuración del programa con "Run". Esto descargará la aplicación a la tarjeta usando JTAG. Dicho esto, no se recomienta usar el objetivo debug para grabar el archivo en ROM SPI, debido a que podría ocupar más espacio, ejecutarse lentamente, y el depurador necesita configuración adicional para leer los símbolos desde otras fuentes.

Puede verse lo usual de un depurador: Volcado de memoria, Lista de llamadas, Registros y línea actual de código fuente.

Pueden también usarse comandos adicionales del depurador GDB versionado por Xilinx para efectuar acciones no disponibles desde la GUI o cargar scripts.

* Depuración del sistema completo: Permite depurar, desde el punto de vista programático, ciertos grupos de senales del bus del sistema y de la lógica del tejido de FPGA. Para habilitarlo, debe estar configurado en Vivado el Zynq-PS la habilitación del cross triggering.
![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200514_032817.png "Flasheando Bitstream")
Puede habilitarse para uno o más nucleos del SOC Zynq.
Al salir se podrá ver que el PS tiene lineas disponibles adicionales. ![Cross trigger](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200514_035919.png "Cross trigger")

Al Hacer click en el pop-up "Run Block Automation se agregran tantos System ILA como líneas se agregaron.
A los ILA, deben conectarse las señales y sus relojes pertenecientes al dominio de reloj de la señal correspondiente
Debe notarse que las señales que se van a vigilar, no deberían exceder el reloj local. De hacerlo podria inestabilizarse el circuito, o enviar informacion corrupta hacia el exterior.



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
Los dispositivos integrados de los SOC, suelen ya estar alambrados de manera fija a las patas correspondientes del dispositivo, y no al tejido. Esto significa que no siempre será posible reemplazar un dispositivo integrado por uno del tejido FPGA, en el lugar, sin cambiar las conexiones. Lamentablemente, de momento, pese a que el archivo de la tarjeta tienes esta infomación, no es posible saber sin prueba y error desde la interfaz de vivado cual es el caso, hasta que llega el momento de implementar el diseño. En caso desfavorable, el proceso de implementación fallará y mostrará un error como este.
![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_2020-05-29_02-09-04.png "Flasheando Bitstream")
En el caso de desear usar el Shield de PYNQ, debe utilizarse esta característica para tener pines GPIO e i2c, o bien usar un IP de GPIO.


