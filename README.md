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
