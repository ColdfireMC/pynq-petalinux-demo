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

## Depuración de Aplicaciones Baremetal ##

Existen 2 alternativas para depurar una aplicación Baremetal.
* Depuración del programa (Variables, Instrucciones Registros de CPU).
No necesita nada especial de la RTL. El encapsulamiento de JTAG que provee el FTDI instalado en la placa, permite que no sea necesario operar con puerto serial y control de flujo, que si bien, es común su uso para depurar y permite gran parte de la funcionalidad de un depurador, no permite ver "en vivo" el contenido de los registros o la memoria, y tiene un comportamiento "conflictivo" si es que el programador pide paradas arbitrarias al programa desde el depurador. JTAG permite todo esto y más, dependiendo de lo que provea la red de Boundary Scan del CPU.

Ya exportado la descripción de hardware a Vitis, debe cambiarse el objetivo de compilación de la aplicación a Debug.

Puede encargarse desde Vitis que comience la depuración del programa con "Run". Esto descargará la aplicación a la tarjeta usando JTAG. Dicho esto, no se recomienta usar el objetivo debug para grabar el archivo en ROM SPI, debido a que podría ocupar más espacio, ejecutarse lentamente, y el depurador necesita configuración adicional para leer los símbolos desde otras fuentes.

Puede verse lo usual de un depurador: Volcado de memoria, Lista de llamadas, Registros y línea actual de código fuente.

![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-vitis-doc/Screenshot_20200623_223905.png "Flasheando Bitstream")

Pueden también usarse comandos adicionales del depurador GDB versionado por Xilinx para efectuar acciones no disponibles desde la GUI o cargar scripts.

* Depuración del sistema completo: Permite depurar, desde el punto de vista programático, ciertos grupos de senales del bus del sistema y de la lógica del tejido de FPGA. Para habilitarlo, debe estar configurado en Vivado el Zynq-PS la habilitación del cross triggering.
![Flasheando Bitstream](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200514_032817.png "Flasheando Bitstream")
Puede habilitarse para uno o más nucleos del SOC Zynq.
Al salir se podrá ver que el PS tiene lineas disponibles adicionales. ![Cross trigger](https://github.com/ColdfireMC/pynq-petalinux-demo/blob/master/pynq-petalinux-doc/Screenshot_20200514_035919.png "Cross trigger")

Al Hacer click en el pop-up "Run Block Automation se agregran tantos System ILA como líneas se agregaron.
A los ILA, deben conectarse las señales y sus relojes pertenecientes al dominio de reloj de la señal correspondiente
Debe notarse que las señales que se van a vigilar, no deberían exceder el reloj local. De hacerlo podria inestabilizarse el circuito, o enviar informacion corrupta hacia el exterior.



