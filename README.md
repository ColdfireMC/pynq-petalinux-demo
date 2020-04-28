# *Setup* Básico Con Digilent *PYNQ* #

* Crear un "Block Design" de Vivado.
* Importar *Constraints*, si se desea agregar RTL (Prefiera activar copiar el .xdc dentro del proyecto.	
* Agregar un PS "ZYNQ 7 Processing System" (CPU principal).
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_181928.png "Agregando un PS")
* Configurar el CPU principal.
    * Ejecutar "Run block Automation". Esto configurara la memoria DRAM y el reloj.
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_182012.png "Autoconfigurar PS")
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_182859.png "Autoconfigurar PS")
Cerrar la ventana con <kbd>OK</kbd>
    * Doble clic al bloque ZYNQ (Esto produce el comando "Re-customize" IP).
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_182859.png "Configurando Interrupciones del PS")
        * Ir a la seccion "*Interrupts*".
        * Habilitar "Enable PS-PL Interrupts".
        * Habilitar "Enable Shared Interrupts".
       
* Conectar `FCLK_CLK0` con `M_AXI_GP0_ACLK`. Este reloj tambien sera el reloj principal del bus AXI.
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_191246.png "Conectar Reloj Principal al Bus AXI"). (Los *Warnings* no generan mayores problemas).
* Agregar y Configurar IP's
    * Agregar *Processor System Reset* y *Run Connection Automation*
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_191335.png "*Run Connection Automation*")
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192129.png "*Run Connection Automation*")
     ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192143.png "*Run Connection Automation*")
    
    * Agregar *Concat*, *AXI IIC*, *AXI QUAD SPI* y 2 *AXI GPIO*.
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192221.png "Agregando Concat")
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192323.png "Agregando GPIO")
    * Configurar bloque *Concat*: Dado que el tipo de `IRQ_F2P` no es compatible con las lineas de interrupcion, deben concatenarse las lineas en un solo vector lógico. Este bloque puede juntar las lineas de interrupción. Debe agregarse una entrada por cada interrupción
         * Doble Clic en *Concat*
         * Editar el ampo *Number of Ports*. En este caso serian 4 lineas (Una por cada dispositivo con Interrupción).
         ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192529.png "Configurando Concat")
    * Configurar *AXI GPIO*: Doble clic en el bloque
      * Habilitar Interrupciones
      ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_192347.png "Configurando GPIO")
      * Puede seleccionarse a que conectar cada interfaz GPIO, a las conexiones disponibles en el preset de la placa. Si se desea conectar RTL o usar los nombres de los .xdc, debe seleccionarse *Custom* y crear un puerto (Véase Agregar RTL).
      ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_193423.png "Configurando GPIO")
      ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_193445.png "Configurando GPIO")
    * Los otros bloques usan en este caso la configuración predeterminada.
* Conectar los bloques de IP: Puede autoconectarse los bloques IP AXI. Nótese que la conexión automática no conecta las interrupciones y solo puede autoconectar un reloj.
  * Hacer clic en el pop-up *Run Connection Automation
  * Seleccionar todos los bloques en las casillas.
  Esto hará que se genere automáticamente un bloque de interconexión y arbitraje AXI y las líneas de dirección y datos del bus.
  * Conectar Lineas de Interrupción
    * Conectar La salida del bloque concat al *ZYNQ Processor System*.
    * Conectar cada una de las líneas de interrupción de los bloques IP perifericos agregados al bloque *Concat*.
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_193211.png "Configurando GPIO")
    
        
    
### Opcionales ###
* Agregar BRAM 
  * Agregar el bloque *BRAM Controller*.
  * Agregar el bloque *Block Memory Generator*.
  Pueden configurarse las caracteristicas de la BRAM, como la cantidad de puertos, el ancho y la capacidad máxima.
* Agregar Interfaz de gestion del Ethernet PHY (Sólo Zybo).
  * Descomentar las lineas 49 y 50 
  ``` 
  set_property -dict { PACKAGE_PIN F16   IOSTANDARD LVCMOS33 } [get_ports eth_int_b]; #IO_L6P_T0_35 Sch=ETH_INT_B
  set_property -dict { PACKAGE_PIN E17   IOSTANDARD LVCMOS33 } [get_ports eth_rst_b]; #IO_L3P_T0_DQS_AD1P_35 Sch=ETH_RST_B
  
  ```
  * Crear puertos con los nombres `eth_int_b` y `eth_rst_b`.
  * Conectar `eth_int_b` a concat y `eth_rst_b`, a `peripheral_aresetn`
  
## Generación de Productos ##
* Crear *Wrapper* de HDL(botón derecho sobre el diseño de bloques (el archivo .bd, en la pestaña *Sources*). (Esto encapsula el diseño y lo hace referenciable por el simulador y el sintetizador).

![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_193758.png "Configurando GPIO")
* Generar bitstream (Esto podria tardar hasta una hora)
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200416_194026.png "Configurando GPIO")
* Exportar Hardware, incluyendo el bitstream
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200420_023711.png "Configurando GPIO")

## Aplicaciones *Baremetal* Con Xilinx Vitis ##

Después de exportar el hardware, Abrir Xilinx Vitis. Al ser similar a Eclipse creará un *Workspace*. Los archivos de código fuente y del proyecto quedarán guardados en este entorno protegido, mientras se desarrolla la aplicación.

* Crear una aplicación *Baremetal* 
 * Ir a "File"->"Project"->"Application"
 * Darle un nombre al proyecto. No ponerle espacios al nombre.
 * Selecionar la plataforma del proyecto. Puede seleccionarse una plataforma existente (Lamentablemente está *buggy* no ocupar) o crear una nueva. ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183739.png "Configurando proyecto de Aplicación")
 
  * Al presionar <kbd>Next</kbd>, debe seleccionarse que método será usado para generar la plataforma. 
  
  ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183750.png "Configurando Plataforma")
  
  Seleccionar "Create new platform from Hardware (XSA)". La ventana tiene un campo donde se listan plataformas recientes disponibles en los ejemplos o bien dentro del *Workspace*. Seleccionar "Create new platform from Hardware (XSA)".Si se necesita importar una plataforma de elaboración propia (el presente caso), debe hacer clic en "+" y seleccionar la carpeta donde se encuentra el archivo `.xsa` que se generó al exportar hardware con Vivado.![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183803.png "Importando Hardware") 
  presionar <kbd>Next</kbd>, después de seleccionar la plataforma recién agregada.![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183813.png "Importando Hardware")   
   * Ahora debe seleccionarse el *Domain* de la plataforma. En este caso, en cada campo debe seleccionarse "Microblaze_0"(Podría haber más de un procesador independiente dentro del sistema que se ha importado), "Standalone" y "C". 
   ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183821.png "Seleccionando Dominio")
   * Al presionar <kbd>Next</kbd>, debe seleccionarse un *Template* para este caso, se seleccionará "Hello World".![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183848.png "Seleccionando Template")
   * Al presionar <kbd>Next</kbd> quedará disponible el entorno de vitis![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_183915.png "Estado inicial del proyecto")

En el arbol o **Explorer**, pueden verse los directorios del proyecto.
* **Release** y **Debug** son los objetivos convencionales de construcción. **Release** por lo general optimizará el ejecutable y no incluirá ni símbolos ni información de depuración. **Debug** incluirá información de depuración y omitirá algunas optimizaciones. Pueden personalizarse los objetivos si se edita el archivo "Makefile" dentro de los directorios.
* **src** Contiene el codigo fuente del ejecutable que se va a construir. Un proyecto *Baremetal* mínimo necesita de la inicialización de plataforma "platform.c" y de un codigo fuente .c que tenga el procedimiento `main`. Si desea aceder a un código fuente referenciado (por ejemplo `#include <algunarchivo.h>`), puede seguir como si fuera un enlace, el nombre del archivo presionando <kbd>Ctrl</kbd>. El *script* de enlazado "ldscript.ld" contiene la configuración de la estructura del ejecutable .elf. Vitis tiene un parser que permite editarlo de manera gráfica.

### Configurando parámetros de plataforma ###

Como el proyecto solo tiene referenciado el terminal, solo habrá como parámetro configurable, la velocidad de transferencia del *UART*. En "platform.c", debe especirficarse en la (línea 49) la velocidad deseada. Esta es la velocidad que se deberá configurar en el terminal más tarde.![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_184026.png "Configuración UART")
  
### Configurando estructura del ejecutable ###   

Al seleccionar "ldscript.ld" del árbol aparecerá en la sección principal la configuración del ejecutable
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_191702.png "Configuración UART").
* "Available Memory Regions": Lista las regiones de memoria disponibles. En este caso se habían creado 2 regiones de memoria, la memoria local y la memoria "Empotrada" en el *FPGA*. 
* "Stack and Heap Sizes": Controla el tamaño de tales regiones. El "Stack" almacena las direcciones y las Flags de retorno de las llamadas, por lo tanto, un programa más complejo, requerirá mas "Stack". "Heap" es una estructura de datos tipo montículo que se usa para asignar memoria dinámicamente. Un programa que genere estructuras de datos que crecen con el paso de la ejecución requerirá más "Heap". Los campos permiten establecer el tamaño de ambas secciones. Si es que no se compartirá la memoria con nada más, se recomienda ocuparla completamente. Una buena medida es darle al Stack el 25% o el 50% del total y el resto para el montículo.
* "Section to Memory Region Mapping": Debido a que la inicialización del sistema viene incorporada en la misma aplicación, no es posible cargar el ejecutable directo en la DRAM, porque no existirá a la hora de extraer el contenido de la *Flash*, o Grabar el *bitstream* por lo tanto, todas las secciones del ejecutable excepto "Heap" y "Stack" deberán mapearse a la memoria BRAM interna. "Heap" y "Stack" deberían mapearse a la memoria DRAM (`mig_7series_0_memaddr`) si se desea tener máxima capacidad. 

### Editando el código fuente ###

En este caso "helloworld.c" es el archivo con el procedimiento `main`. Todo lo que se desea que el programa haga en su cuerpo principal debería ser agregado aquí. Nótese que al final hay una llamada a `cleanup_platform()` definida en "platform.c". Si se desean agregar más acciones, deben editarse ahí.

### Generando Ejecutable ###

Al presionar "build" (el martillo) se construye la aplicación y el entorno, completos, según el objetivo que se haya establecido. Al lado del ícono, aparece una flecha, donde se puede seleccionar el objetivo de compilación ("Debug", "Release" u otro que se haya creado).

### Generando Imagen ###

Del menú "Xilinx" debe seleccionarse "Program FPGA". Luego debe adjuntarse a la imagen, la aplicación recién compilada, en el procesador que aparece. Nótese que podría haber más de un procesador, y que en realidad este depende de la plataforma importada y no de la aplicación. ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_185312.png "Generando Bitstream") Luego de eso debe presionarse <kbd>Generate</kbd>. ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_185301.png "Generando Bitstream") Esto tomará unos instantes.

### *Flasheando* Imagen ###

Del menú "Xilinx" debe seleccionarse "Program Flash". Asegurarse que la tarjeta de desarrollo esté encendida.
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_192405.png "Program Flash") 
* Debe seleccionarse la imagen para grabar. Usando <kbd>Search</kbd> aparecerá la siguiente ventana. ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200425_031005.png "Seleccionando Imagen") Seleccionar "download.bit", que es la imagen que se acaba de generar con el *bitstream* y la aplicación incluida.
* Especificar modelo de flash donde se grabará. Esta normalmente sale en el manual de referencia de la tarjeta. ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200423_192958.png "Flash SPI de Nexys Video") En este caso debe seleccionarse s25fl256xxxxxx0 (Fabricada por Spansion).
* Presionar <kbd>Program</kbd>. Esto tardará bastante tiempo. Asegurarse de que no se interrumpirá el suministro de energía de la placa. De interrumpirse, podría "Regrabarse" la memoria cambiando la configuración de los jumpers a otra que no haga al FPGA arrancar con la memoria SPI.


## Problemas Conocidos ##
* La "actualización" de plataformas no es capaz de encontrar el bitstream exportado por Vivado, asi que cada vez que se altere la plataforma, debe recrearse nuevamente. Esto rompe parte de la característica de portabilidad Automática.
* Cuando se crea primero una plataforma y después la aplicación, con el *Workspace* vacío, el mago generador de aplicaciones no puede encontrar la definición de plataforma ya creada y queda incompleta y no puede terminarse, o bien, debe seleccionarse una de las incluidas en Vitis y luego reasignar la plataforma manualmente (Cuidado con las inicializaciones, porque si fueron modificadas por el programador, serán totalmente incompatibles).
* Pese a que los archivos de Digilent incluyen información sobre los dispositivos de la placa, hay que especificar la *Flash* SPI a mano.


