<div align="center">
  <h1>Práctica 2.- Construcción de Autómatas Finitos con Interfaz Gráfica para distintos lenguajes</h1>
   <p><i>Juárez Velázquez Erick Daniel 2025630052</i></p>
   <p><i>Placencia Murguia Juan Emilio 2025630024</i></p>
</div>

---

## Introducción
La presente práctica aborda de manera integral el diseño, análisis y simulación de los **Autómatas Finitos** como pilares de la teoría de la computación. La práctica se divide en dos fases fundamentales que conectan la teoría con la implementación práctica:

### Análisis y Conversión en JFLAP
En esta etapa se realiza la implementación de modelos de **Autómatas Finitos No Deterministas (AFND)** y **AFND con transiciones lambda ($\lambda$)**. El enfoque principal es comprender la flexibilidad del no-determinismo y dominar los procesos de conversión sistemática hacia **Autómatas Finitos Deterministas (AFD)**, evaluando en cada paso la eficiencia en términos de número de estados y complejidad de transiciones.

### Desarrollo del Simulador Integral
La segunda parte consiste en la creación de una aplicación robusta con interfaz gráfica (GUI) diseñada para actuar como un entorno de ejecución de AFD. La aplicación permite:
* **Gestión de Formatos**: Importación y exportación de autómatas en archivos `.jff` (JFLAP), `.json` y `.xml`.
* **Definición Dinámica**: Herramientas para que el usuario defina manualmente el alfabeto, los estados, la función de transición (mediante matrices interactivas) y los estados de aceptación.
* **Simulación Visual**: Validación de cadenas con soporte para ejecución paso a paso, permitiendo visualizar el estado actual y la transición tomada en tiempo real.
* **Análisis Lingüístico**: Funcionalidades adicionales para el cálculo de subcadenas, prefijos, sufijos y la generación de lenguajes mediante cerraduras de Kleene ($\Sigma^*$) y positiva ($\Sigma^+$).

## Objetivo
El propósito de esta práctica es profundizar en la comprensión y aplicación de los Autómatas Finitos No Deterministas (AFND) mediante el uso de JFLAP, y desarrollar una aplicación con interfaz gráfica que permita simular Autómatas Finitos Deterministas (AFD) a partir de diferentes formatos de entrada.

## Instrucciones de ejecución

Para poder ejecutar el programa es necesario contar con Java instalado en el sistema. La versión con la que fue desarrollado y probado es OpenJDK 25, aunque debería funcionar sin problema con versiones recientes del JDK (se recomienda JDK 17 en adelante). Para verificar que Java esté instalado correctamente, puedes correr el siguiente comando en la terminal:
```
java --version
```

Una vez confirmado, hay que abrir una terminal y navegar hasta la carpeta del proyecto, que se llama `Lenguajes`. Dentro de ella, entrar a la carpeta `target`, que es donde se encuentra el archivo `.jar` generado:
```
cd ruta/hacia/Lenguajes/target
```

Ya estando en esa ubicación, ejecutar el programa con el siguiente comando:
```
java -jar Lenguajes-1.0-SNAPSHOT.jar
```
Una vez indicada la linea de comando el programa se abrirá automaticamente.
**NOTA:** Se recomienda hacerlo de esta forma ya que ejecutando directamente el archivo en un Explorador de Archivos puede no ejecutarse de forma correcta y mostrar algún error, a lo cual al ser ejecutado mediante la terminal no presentara ningún problema.
## Archivos de los autómatas

En el repositorio de GitHub se encuentra un archivo comprimido llamado Lista2_AFD.zip que contiene todos los ejercicios resueltos de la Lista 2, cada autómata está exportado en los tres formatos requeridos: .jff, .json y .xml.
## Desarrollo de la práctica
### Arquitectura del Autómata

El núcleo del simulador se basa en la representación directa de la quíntupla matemática de un Autómata Finito Determinista. Para lograr esto mediante programación orientada a objetos, la lógica se estructuró en tres componentes principales:

* **Estado:** Define los nodos del grafo. Además de guardar su identificador, almacena sus coordenadas espaciales para el renderizado en la interfaz gráfica y banderas booleanas para determinar si es el estado inicial o si pertenece al conjunto de aceptación.
* **Transición:** Materializa la función de transición, enlazando un estado de origen con uno de destino condicionado a la lectura de un símbolo del alfabeto.
* **Automata:** Es el contenedor principal que agrupa las listas dinámicas de estados y transiciones, gestionando el punto de arranque y ejecutando el motor de validación principal.
Esta parte se puede apreciar en la Figura No. 1.
<p align="center">
  <img src="https://github.com/user-attachments/assets/04d87558-0991-4b17-aca1-620b39795602">
  <br>
  Figura 1
</p>

El proceso de simulación consume la cadena de entrada carácter por carácter. En cada paso, el algoritmo busca una transición válida para el estado actual y el símbolo leído; si al terminar de evaluar toda la cadena el sistema se encuentra posicionado en un estado final, se aprueba la validación. (Ver Figura 2)

<p align="center">
  <img src="https://github.com/user-attachments/assets/0e949f97-e3c1-4b56-bbe4-1a0f372daa1c">
  <br>
  Figura 2
</p>
 
 ### Lienzo Interactivo y Diseño de Grafos

La interfaz permite la construcción visual de autómatas donde se gestiona la interacción del usuario con los elementos del grafo de manera dinámica:

* **Manipulación de Estados:** El usuario puede crear estados haciendo clic sobre el área de trabajo y reubicarlos mediante eventos de arrastre (*drag and drop*). Cada estado se renderiza como un nodo circular con su etiqueta correspondiente.
* **Trazado de Transiciones:** Las conexiones se realizan seleccionando un estado de origen y uno de destino. El sistema detecta automáticamente si se trata de una transición a un estado distinto o un bucle (auto-transición), dibujando curvas o líneas rectas según sea necesario.
* **Renderizado en Tiempo Real:** Se utiliza la API de Java 2D para refrescar constantemente el lienzo, permitiendo que cualquier cambio en la estructura (como mover un estado) actualice automáticamente la posición de las flechas de transición. (Ver Figura 3)

<p align="center">
  <img src="https://github.com/user-attachments/assets/67257bf8-c549-465a-bf51-de52befebe0a">
  <br>
  Figura 3
</p>

Persistencia de Archivos y Compatibilidad

El sistema implementa una capa de gestión de datos robusta que permite la interoperabilidad con herramientas académicas externas. A través de la clase `IOAutomata`, se gestionan tres formatos de serialización distintos:

* **Formato JFF (JFLAP):** Permite la exportación e importación directa con JFLAP. El sistema procesa la estructura XML nativa de estos archivos, mapeando los estados y transiciones para que el diseño sea totalmente funcional en ambas plataformas.
* **Formato JSON:** Se utiliza para una persistencia ligera y moderna, ideal para el intercambio de datos en aplicaciones web o inspección rápida de la estructura del autómata.
* **Formato XML Genérico:** Proporciona una estructura jerárquica clara del autómata, facilitando su lectura por otros sistemas y asegurando que ninguna propiedad del grafo se pierda durante el guardado. (Ver Figura 4)

<p align="center">
  <img src="https://github.com/user-attachments/assets/bcf44856-d192-45a6-926e-be333d3f1bf9">
  <br>
  Figura 4
</p>

Esta capacidad multi-formato asegura que los ejercicios de la **Lista 2** puedan ser verificados, respaldados y compartidos sin depender de un único entorno de ejecución. 

### Motor de Simulación y Validación de Cadenas

El componente central para la evaluación de lenguajes permite verificar la pertenencia de una cadena al lenguaje regular definido por el autómata. Este proceso se gestiona mediante dos modalidades:

* **Validación Individual:** Un algoritmo recorre la estructura del grafo partiendo del estado inicial, consumiendo los símbolos de la cadena de entrada uno a uno. La aceptación se determina si, tras agotar la cadena, el autómata se posiciona en un estado marcado como final.
* **Pruebas Masivas (TestAutomataFrame):** Interfaz diseñada para la evaluación por lotes, donde el usuario puede ingresar múltiples cadenas simultáneamente. El sistema devuelve un reporte tabular de "Aceptada" o "Rechazada", facilitando la validación de los casos de prueba requeridos por la práctica. (Ver Figura 5).

<p align="center">
  <img src="https://github.com/user-attachments/assets/d86b12a5-751a-4735-a09c-b3f708d55fbe">
  <br>
  Figura 5
</p>

### Herramientas de Lenguajes Formales

Como complemento al simulador, se integraron herramientas para el análisis atómico de cadenas y operaciones de cerradura, fundamentales en la teoría de la computación:

* **Análisis de Subcadenas:** Un módulo dedicado a la descomposición de una cadena de entrada para extraer todos sus prefijos, sufijos y subcadenas posibles, incluyendo la cadena vacía (ε).
* **Cerraduras de Kleene y Positiva:** Algoritmos que generan las potencias del alfabeto o lenguaje ($L^*$ y $L^+$), permitiendo visualizar las combinaciones finitas de símbolos hasta un límite definido por el usuario. 
<p align="center">
  <img src="https://github.com/user-attachments/assets/4181d81b-f245-400e-9d69-0b7de6239579">
  <br>
  Figura 6
  <br>
  <img src="https://github.com/user-attachments/assets/efca892f-7343-4c4a-a150-9ee7b7765e34">
  <br>
  Figura 7
</p>
Estas funcionalidades fueron desarrolladas desde la Práctica 1, por lo que su funcionamiento se detalla en el reporte correspondiente.

## Conclusión

Esta práctica nos ayudó a entender de forma más clara cómo un autómata finito pasa de ser un concepto teórico a convertirse en algo que realmente funciona en código. Traducir la quíntupla matemática a clases y estructuras de Java no fue trivial, pero el resultado es un simulador que valida cadenas de forma correcta y predecible.
