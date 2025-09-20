# Introducción a las bases de datos.
## 1. Sistemas de información

### 1.1 Información
![información](img/ud1_img1.jpg){: style="float: left; width: 125px; margin: 0 15px 15px 0;"}
La información es considerada, junto con la materia y con la energía, uno de los componentes fundamentales de la naturaleza, siendo vital para el desarrollo de los pueblos.

La importancia de la información provoca una fuerte demanda, por lo que es preciso analizar el marco legal e institucional en el que se inscribe el derecho a la información. El deseo de información es un fin en sí mismo, una necesidad de conocer el entorno socio-económico y cultural en el que nos desenvolvemos con fines de investigación y de toma de decisiones, pues el individuo precisa participar en los asuntos públicos, siendo una reivindicación esencial de nuestra época.

Son muchos los factores que han influido en la transformación del papel que desempeña la información en el marco económico y social. Esto es debido a la elevación del nivel cultural y el deseo de participar en las decisiones públicas por parte del individuo.

Los gobiernos de los distintos países, al reconocer que la información circula para responder a unas necesidades, como son la necesidad de saber, conocer y elegir, no han podido menos que ocuparse del tema, impulsando y planificando sistemas nacionales de información con el fin de asegurar un flujo de este recurso, imprescindible para la evolución científica, económica y social.

Es preciso, sin embargo, reconocer que nos encontramos aún muy lejos de poder satisfacer las necesidades de información, ya que no sólo existen problemas tecnológicos relativos al almacenamiento y acceso a la información, sino que existen también problemas económicos, políticos, administrativos, etc. que impiden el desarrollo de sistemas de información eficientes capaces de atender debidamente las demandas de información.

### 1.2 Cualidades de la información

La información debe contar con una serie de cualidades para que cumpla su función principal, es decir, aportar un conocimiento. Cuando la información pierde una o más de sus cualidades se produce el fenómeno llamado **polución informativa**, análogo a la contaminación atmosférica.

Las cualidades que debe poseer la información para ser realmente útil son:

- **Precisión**: es el porcentaje de información correcta sobre la información total del sistema. En el ámbito de la informática, para que el ordenador aporte unos resultados precisos, es necesario introducir datos igualmente precisos, ya que éste sólo es capaz de mejorar los datos de forma muy limitada (por ejemplo, eliminando datos duplicados).
- **Oportunidad**: tiempo transcurrido desde el momento en que se produjo el hecho que originó el dato hasta el momento en el que la información se pone a disposición del usuario.
- **Compleción**: la información ha de ser completa para poder cumplir sus fines. La compleción absoluta es imposible de conseguir en los sistemas de información por lo que normalmente se busca conseguir un nivel “suficiente” que depende de dos factores: de los datos existentes en el sistema y de los que éste sea capaz de localizar al realizar una consulta concreta.
- **Significativa**: ha de poseer la máxima carga semántica posible, para ello la información debe ser comprensible, interesante y con un volumen justo (ni escasa, ni excesiva).
- **Coherente**: el sistema de información debe carecer de contradicciones, es decir, la información debe ser coherente en sí misma. Esta característica también se conoce como integridad en las bases de datos.
- **Seguridad**: la información debe protegerse para evitar tanto su deterioro como accesos no autorizados. Comprende tres conceptos fundamentales: confidencialidad, disponibilidad e integridad. La seguridad de la información se refiere a la confidencialidad, integridad y disponibilidad de la información y datos, independientemente de la forma los datos pueden tener: electrónicos, impresos, audio u otras formas.  
    La seguridad de la información involucra la implementación de estrategias que cubran los procesos en donde la información es el activo primordial. Cabe mencionar que la seguridad es un proceso continuo de mejora por lo que las políticas y controles establecidos para la protección de la información deberán revisarse y adecuarse, de ser necesario, ante los nuevos riesgos que surjan, a fin de tomar las acciones que permitan reducirlos y en el mejor de los casos eliminarlos.

#### Conceptos de Seguridad

- **Confidencialidad**: La confidencialidad es la propiedad de prevenir la divulgación de información a personas o sistemas no autorizados. La pérdida de la confidencialidad de la información puede adoptar muchas formas. Cuando alguien mira por encima de su hombro, mientras usted tiene información confidencial en la pantalla, cuando se publica información privada, cuando un laptop con información sensible sobre una empresa es robado, cuando se divulga información confidencial a través del teléfono, etc. Todos estos casos pueden constituir una violación de la confidencialidad.
- **Integridad**: Para la seguridad de la información, la integridad es la propiedad que busca mantener los datos libres de modificaciones no autorizadas. La violación de integridad se presenta cuando un empleado, programa o proceso (por accidente o con mala intención) modifica o borra los datos importantes que son parte de la información, así mismo hace que su contenido permanezca inalterado a menos que sea modificado por personal autorizado, y esta modificación sea registrada, asegurando su precisión y confiabilidad. La integridad de un mensaje se obtiene adjuntándole otro conjunto de datos de comprobación de la integridad: la firma digital es uno de los pilares fundamentales de la seguridad de la información.
- **Disponibilidad**: La disponibilidad es la característica, cualidad o condición de la información de encontrarse a disposición de quienes deben acceder a ella, ya sean personas, procesos o aplicaciones. En el caso de los sistemas informáticos utilizados para almacenar y procesar la información, los controles de seguridad utilizada para protegerlo, y los canales de comunicación protegidos que se utilizan para acceder a ella deben estar funcionando correctamente. La alta disponibilidad tiene como objetivo que la información debe estar disponible en todo momento, evitando interrupciones del servicio debido a cortes de energía, fallos de hardware, y actualizaciones del sistema. Garantizar la disponibilidad implica también la prevención de ataque.

A la hora de implantar un sistema de información el objetivo primordial es buscar un equilibrio que permita alcanzar los objetivos propuestos a un coste razonable. Esto se debe, por un lado, a que cuantas más cualidades reúna la información más se incrementarán los costes y, por otro, a que algunas cualidades son incompatibles con otras: una gran precisión puede conllevar una pérdida de oportunidad.

En la actualidad la democratización de las nuevas tecnologías ha provocado una eclosión en la generación de información. Montones y montones de “datos” almacenados en montones y montones de “sitios”. Estas grandes masas de información requieren nuevas herramientas para su manejo: **BIGDATA**.

### 1.3 Definición de un Sistema de Información

Toda organización necesita para su funcionamiento un conjunto de informaciones que han de transmitir entre sus distintos elementos, desde y hacia el exterior. Esta comunicación constituye un sistema de información.

Está diseñado para satisfacer las necesidades de información de una organización compleja y está inmerso en ella.

El SI debe tomar datos del entorno y sus resultados serán la información que dicha organización necesita para su gestión y toma de decisiones; y los directivos deben marcar objetivos para regular el SI.

El control del sistema puede realizarse mediante mecanismos internos (sistemas autorregulados), por mecanismos situados en el entorno, o ambos; teniendo en cuenta la subjetividad que ello supone al poder ampliar los límites del sistema. Estos sistemas interactúan con el entorno, las entradas y el proceso se van adaptando para obtener determinadas salidas.

El controlador del sistema planifica y gobierna el sistema, actuando de acuerdo con la información que recoge la salida, enviando estímulos a la entrada y al procesador para conseguir que la salida correspondiente a los objetivos del sistema.

- **Entradas**: Son los elementos que se consumen o transforman en el proceso. Son la materia prima de los procesos de información, los datos. Los SI, a diferencia de otros sistemas, transforman las entradas sin destruirlas, al quedar almacenadas en bases de datos del sistema.
- **Salidas**: Son los elementos que se crean en el proceso. Son los productos terminados.
- **Procesador**: Es donde se efectúa el tratamiento, y comprende todos los elementos que participan en él sin transformarse ni crearse, excepto las entradas y salidas. Suele estar situado en el orden más bajo de la jerarquía.
- **Retroalimentación**: En ciertos sistemas existe la retroalimentación que va desde la salida a la entrada.

Las definiciones de sistemas de información pueden ser diversas. Destacamos a Langefors y Teichroew. Nosotros lo definimos como un conjunto de elementos ordenados, relacionados entre sí de acuerdo con unas reglas, que aportan al sistema objeto la información necesaria para el cumplimiento de objetivos, para el que tendrá que recoger, procesar y almacenar datos desde la organización o fuentes externas, facilitando la recuperación, elaboración y presentación de los mismos.

En cuanto a sus características destacamos a Bubenko(1980), el cuál cita que pueden agruparse en:

- **Tecnológicas**: Afectan al rendimiento y seguridad del sistema.
- **Funcionales y semánticas**: Se refiere a la eficacia y adaptación del sistema.
- **Económicas**: Coste y eficiencia del sistema.
- **Sociales**: Aquellas que tienen un impacto sobre en entorno social alrededor del sistema.

### 1.4 Componentes de un Sistema de Información

1.  **Contenido (Datos)**: Es el conjunto de datos, que tienen su correspondiente descripción y que están almacenados en un soporte del ordenador. Se pueden diferenciar en:
    1.  **Referenciales**: Estos tipo de datos no contienen en sí misma la información, es decir están formados por referencias de los documentos donde se encuentra verdaderamente la información que buscamos.
    2.  **Factuales**: En estos tipos de datos si se nos devuelve la información que estamos buscando. Dentro de este tipo de datos debemos hacer dos tipos de clasificaciones:
        1.  **Estructurados**: Tienen una estructura o formato en la cual cada campo ocupa una posición fija.
        2.  **No estructurados**: En estos tipos de datos el formato no puede ser fijo. Estos datos no siguen ningún patrón, no poseen definiciones de tipos y no existe el concepto de variables o atributos.
2.  **Equipo Físico (Hardware)**: Es el conjunto de los componentes que integran la parte material de un ordenador. (Unidad central de proceso, equipo periférico,....)
3.  **Soporte lógico (Software)**: Es el conjunto de los componentes lógicos necesarios para hacer posible la realización de una tarea específica, en contraposición a los componentes físicos del sistema.
    1.  **Sistemas operativos**: Estos componentes lógicos incluyen aplicaciones informáticas como el sistema operativo que permite al resto de programas funcionar adecuadamente.
    2.  **Gestión de Datos (SGBD)**: Se realiza mediante los Sistemas Gestores de Bases de Datos.
    3.  **Control de las comunicaciones**: Se llevan varios controles sobre el sistema de información, algunos de ellos son:
        1.  **Control de autorización**: Este módulo comprueba que el usuario tiene los permisos necesarios para llevar a cabo la operación que solicita.
        2.  **Procesador de comandos**: Una vez que el sistema ha comprobado los permisos del usuario, se pasa el control al procesador de comandos.
        3.  **Control de la integridad**: Cuando una operación cambia los datos de la base de datos, este módulo debe comprobar que la operación a realizar satisface todas las restricciones de integridad necesarias.
        4.  **Optimizador de consultas**: Este módulo determina la estrategia óptima para la ejecución de las consultas.
    4.  **Gestor de transacciones**: Este módulo realiza el procesamiento de las transacciones.
        - **Planificador**: Este módulo es el responsable de asegurar que las operaciones que se realizan concurrentemente sobre la base de datos tienen lugar sin conflictos.
        - **Gestor de recuperación**: Este módulo garantiza que la base de datos permanece en un estado consistente en caso de que se produzca algún fallo.
        - **Gestor de buffers o Gestor de Datos**: Este módulo es el responsable de transferir los datos entre memoria principal y los dispositivos de almacenamiento secundario.
    5.  **Tratamientos específicos**: Con la evolución de las bases de datos y su implantación en el mundo de Internet surgió la necesidad de contar con un sistema de administración para controlar tanto los datos como los usuarios, las cuales son las partes más importantes de un sistema de información.
4.  **Usuarios**: Existen 2 grandes grupos:
    - **Usuarios informáticos**:
        - **Programadores de aplicaciones**: que son los responsables de escribir los programas de aplicación de base de datos en algún lenguaje de programación. Estos programas acceden a la base de datos emitiendo la solicitud apropiada al DBMS. Los programas en sí pueden ser aplicaciones convencionales por lotes o pueden ser aplicaciones en línea, cuyo propósito es permitir al usuario final el acceso a la base de datos desde una estación de trabajo o Terminal en línea.
        - **El administrador de base de datos (DBA)**: es el técnico responsable de implementar las decisiones del administrador de datos. También se puede contar como usuario puesto que para saber si funciona la base da datos correctamente debe probarla. Por lo tanto, debe ser un profesional en IT. El trabajo del DBA consiste en crear la base de datos real e implementar los controles técnicos necesarios para hacer cumplir las diversas decisiones de las políticas hechas por el DA. El DBA también es responsable de asegurar que el sistema opere con el rendimiento adecuado y de proporcionar una variedad de otros servicios técnicos.
    - **Usuarios no informáticos**:
        - **Los usuarios finales**: quienes interactúan con el sistema desde estaciones de trabajo o terminales en línea. Un usuario final puede acceder a la base de datos a través de las aplicaciones en línea. Las interfaces proporcionadas por el fabricante están apoyadas también por aplicaciones en línea. La mayoría de los sistemas de base de datos incluyen por lo menos una de estas aplicaciones integradas.
        - **Usuarios con menor nivel técnico**: la mayoría de los sistemas proporcionan además interfaces integradas adicionales en las que los usuarios no emiten en absoluto solicitudes explícitas a la base de datos, sino que en vez de ello operan mediante la selección de elementos en un menú o llenando casillas de un formulario. Estas interfaces controladas por menús o por formularios tienden a facilitar el uso a personas que no cuentan con una capacitación formal en tecnología de la información (IT).
  
## 2. Sistemas de almacenamiento y gestión de la información.
### 2.1 Sistema físico de almacenamiento.
El **sistema físico de almacenamiento** hace referencia a la representación concreta de los datos en los dispositivos hardware (discos duros HDD, unidades de estado sólido SSD, cintas, etc.). Su objetivo es escribir y leer bits (0s y 1s) de la manera más eficiente posible para el hardware, sin preocuparse por el significado lógico de esos datos. Es la capa gestionada por el controlador del disco y el sistema operativo.  

Las características del hardware imponen una serie de limitaciones que las capas superiores (ficheros, SGBD) deben intentar ocultar o mitigar:

1.  **Velocidad de Acceso (La Gran Brecha):**
    *   **Memoria RAM:** Acceso en **nanosegundos** (ns). Volátil.
    *   **SSD (Unidad de Estado Sólido):** Acceso en **decenas de microsegundos** (µs). No tiene partes móviles.
    *   **HDD (Disco Duro Magnético):** Acceso en **milisegundos** (ms). Tiene partes móviles.
    *   **Conclusión:** Un acceso a un HDD puede ser **100,000 veces más lento** que un acceso a la RAM. El objetivo principal de un SGBD es minimizar los accesos al disco.

2.  **Acceso por Bloques (No por Bytes):**
    *   Los discos no leen o escriben bytes individuales. La unidad mínima de transferencia es el **sector** (típicamente 512B o 4KB).
    *   Acceder a un solo byte obliga a leer o escribir todo el sector donde reside. Esto hace que la organización física de los datos sea crucial para el rendimiento.

3.  **Características Mecánicas (En HDD):**
    *   **Tiempo de Búsqueda (Seek Time):** Tiempo que tarda el cabezal de lectura/escritura en moverse al cilindro correcto.
    *   **Latencia Rotacional (Rotational Latency):** Tiempo que tarda el sector deseado en girar hasta quedar bajo el cabezal.
    *   Estos dos factores son el cuello de botella principal en HDDs. Las operaciones son mucho más rápidas si los datos a leer están almacenados en sectores consecutivos, minimizando los movimientos del cabezal.

4.  **Gestión del Espacio y Fragmentación:**
    *   El sistema operativo debe llevar un control de qué bloques del disco están libres y cuáles ocupados.
    *   Con el tiempo, los archivos se fragmentan (sus partes se esparcen por el disco), lo que aumenta los tiempos de acceso aleatorio (el cabezal debe saltar por todo el disco).

#### Unidades de almacenamiento físico.

Para entender cómo se gestionan los datos, es esencial conocer las unidades de trabajo:

| Unidad | Descripción | ¿Quién la gestiona? | Importancia |
| :--- | :--- | :--- | :--- |
| **Bit** | Un 0 o un 1. | Hardware | Unidad atómica de información. |
| **Byte** | 8 bits. | Hardware | Unidad de información direccionable. |
| **Sector** | **Unidad direccionable más pequeña del disco.** Grupo de bytes (ej: 512B, 4KB). Tiene una dirección única. | Controlador del Disco | El disco lee/escribe físicamente sectores completos. |
| **Bloque** | **Unidad mínima de transferencia entre disco y memoria.** Agrupa varios sectores consecutivos (ej: 4KB, 8KB, 16KB). | Sistema Operativo | El SO lee/escribe bloques completos a/desde la RAM. |
| **Página** | **Unidad fundamental de almacenamiento en un SGBD.** Es un bloque de tamaño fijo (ej: 4KB, 8KB) que el SGBD intercambia con el disco. | Sistema Gestor de Bases de Datos (SGBD) | **Concepto clave.** Las tablas, índices y datos se almacenan en páginas. El SGBD gestiona un **buffer pool** en RAM para cachear las páginas usadas frecuentemente y evitar accesos a disco. |
| **Extent** | Conjunto contiguo de páginas (ej: 8 páginas de 8KB forman un extent de 64KB). | SGBD | Se usa para asignar espacio de manera eficiente a objetos grandes (tablas, índices). |
| **Archivo de Datos** | Conjunto de extents y páginas. Un archivo físico en el SO que pertenece a una base de datos. | SGBD / SO | Una base de datos está compuesta por uno o varios archivos de datos (y de log). |

Veamos con un ejemplo cómo interactúan estas unidades:

1.  Un usuario ejecuta la consulta: `SELECT * FROM clientes WHERE id = 123;`
2.  El **SGBD** determina en qué **página** se encuentra el registro con `id=123` (usando un índice).
3.  El SGBD verifica si esa **página** ya está en el **buffer pool** (en RAM).
4.  **Si no está (fallo de página):** El SGBD solicita al **SO** que lea el **bloque** del disco que contiene esa **página**.
5.  El **SO** traduce la solicitud al **controlador del disco**.
6.  El **controlador del disco** localiza el **sector** (o sectores) físico donde reside ese bloque, mueve el cabezal (si es HDD) y transfiere los datos a la RAM.
7.  El **SGBD** recibe la **página** en el buffer pool y luego examina los **registros** dentro de ella para encontrar y devolver la fila con `id=123`.

#### Conclusión: El "Problema" que las Capas Lógicas Resuelven

Trabajar directamente con el almacenamiento físico (calcular sectores, gestionar el movimiento del cabezal) es inviable para desarrollar aplicaciones. Es extremadamente complejo, propenso a errores y muy lento.

**La evolución (Sistema Físico → Ficheros → Bases de Datos)** es una respuesta directa a estos desafíos, añadiendo capas de abstracción que:

1.  **Ocultan la complejidad:** El programador ve "registros" y "tablas", no sectores y cilindros.
2.  **Optimizan el acceso:** Estructuras como **índices** permiten saltar directamente a la página correcta, minimizando los costosos accesos aleatorios al disco.
3.  **Agrupan datos:** Empaquetan registros lógicos en **páginas** físicas para ser transferidos eficientemente.
4.  **Gestionan la concurrencia:** Controlan qué sucede cuando múltiples usuarios necesitan acceder a la misma página simultáneamente.
5.  **Garantizan la consistencia:** Aseguran que las escrituras en el disco se hagan de forma segura incluso ante fallos, usando estructuras como un **log de transacciones**.

Entender la base física es entender **por qué** las bases de datos se diseñan y se comportan como lo hacen. El rendimiento de un SGBD depende, en gran medida, de cómo gestiona esta interacción con el disco.
### 2.2 Sistemas lógicos de almacenamiento.
#### De los ficheros a las bases de datos.
El **Sistema Lógico de Almacenamiento** es una capa de abstracción que se construye sobre el sistema físico para permitir una interacción comprensible y eficiente con los datos. Su objetivo es ocultar la complejidad del hardware (bloques, sectores, direcciones físicas) y presentar modelos de datos estructurados y manejables para usuarios y aplicaciones.

La evolución de estos sistemas lógicos ha seguido esta trayectoria para resolver las limitaciones de cada aproximación previa:

**Almacenamiento Físico → Sistemas de Ficheros → Sistemas de Bases de Datos** 
#### 1. Sistemas de Ficheros

Los sistemas de ficheros fueron la primera capa de abstracción lógica sobre el almacenamiento físico, gestionada por el Sistema Operativo.

Un sistema de ficheros organiza los datos en **archivos** (que contienen registros) y **directorios** (que organizan los archivos). La aplicación es la responsable de interpretar la estructura interna de cada archivo.

**Características Principales:**
    
- **Persistencia:** Los datos sobreviven al apagado de la aplicación.  
- **Abstracción básica:** Ocultan la ubicación física en el disco (bloques/sectores) tras la noción de "archivo" y "ruta".  
- **Control de acceso básico:** Permisos de lectura, escritura y ejecución a nivel de archivo.

**Clasificación según su Estructura Lógica:**

#### A. Ficheros Planos
- **Descripción:** Archivos de texto o binarios sin estructura interna reconocible por el SO. La aplicación debe conocer el formato exacto para interpretarlos.
- **Ejemplos:** `.txt`, `.csv` (valores separados por comas), un archivo JSON.
- **Ventaja:** Máxima simplicidad y portabilidad.
- **Desventaja:** Alta redundancia, nula integridad estructural y acceso ineficiente (siempre secuencial).

#### B. Ficheros Indexados
- **Descripción:** Mejoran los archivos planos añadiendo una estructura auxiliar (índice) que permite acceso rápido a registros específicos mediante una clave.
- **Estructura:** Separan los datos (archivo maestro) de los índices (archivo de índices). El índice relaciona una clave con la posición física del registro en el archivo de datos.
- **Ventaja:** Permiten acceso directo y secuencial. Velocidad de búsqueda mejorada.
- **Desventaja:** Complejidad añadida. El programador debe gestionar la creación, actualización y sincronización de los índices.

#### C. Ficheros de Acceso Directo (o Relativo)
- **Descripción:** Utilizan una función de transformación (hash) para calcular la dirección física directa de un registro a partir de su clave.
- **Ventaja:** Tiempo de acceso constante, ideal para aplicaciones de tiempo real.
- **Desventaja:** Espacio preasignado, posible colisión de claves y uso ineficiente del espacio.

**Limitaciones de los Sistemas de Ficheros (y por qué surgieron las BD):**  
- **Redundancia e inconsistencia:** Datos duplicados en múltiples archivos llevan a inconsistencias.  
- **Aislamiento de datos:** Los datos están dispersos en archivos independientes, dificultando las consultas combinadas.  
- **Dependencia de la aplicación:** La estructura de los datos está embebida en el código de la aplicación. Cambiar la estructura often requiere cambiar la aplicación.  
- **Problemas de integridad:** Es difícil imponer restricciones de integridad (ej: que un cliente tenga una ID única).  
- **Problemas de acceso concurrente:** No hay mecanismos robustos para gestionar múltiples usuarios actualizando el mismo archivo simultáneamente.

#### 2. Sistemas de Bases de Datos

Los Sistemas de Gestión de Bases de Datos (SGBD) surgieron como solución a las limitaciones de los sistemas de ficheros, proporcionando una abstracción lógica mucho más poderosa y robusta.

Un SGBD es un software que sirve como intermediario entre los datos físicos y las aplicaciones/usuario. Proporciona una visión lógica unificada y abstracta de los datos, llamada **esquema**, independiente del almacenamiento físico.

**Características Principales (frente a los sistemas de ficheros):**  

1.  **Abstracción Avanzada:** Ocultan completamente el almacenamiento físico. El usuario ve tablas, vistas y relaciones, no archivos ni bloques.  
2.  **Independencia de los Datos:**  
        - **Independencia Física:** Se puede cambiar la organización física (ej: añadir un índice, migrar a un SSD) sin afectar a las aplicaciones.  
        - **Independencia Lógica:** Se puede cambiar el esquema lógico (ej: añadir una columna) sin afectar a todas las aplicaciones (si no usan esa columna).  
3.  **Reducción de la Redundancia:** Al integrar los datos en una única base de datos, se elimina la duplicación no controlada. La redundancia que queda es deliberada y controlada.  
4.  **Consistencia e Integridad:** Aplican reglas definidas (restricciones, claves foráneas, triggers) para garantizar que los datos sean correctos y válidos en todo momento.  
5.  **Acceso Concurrente Controlado:** Gestionan de forma transparente múltiples usuarios, utilizando mecanismos de transacción (bloqueos, control de concurrencia) para evitar conflictos.  
6.  **Vistas de Seguridad:** Permiten crear vistas personalizadas y seguras de los datos para diferentes usuarios o grupos, restringiendo el acceso a filas o columnas sensibles.

**Clasificación de los Modelos Lógicos en Bases de Datos:**

- **Modelo Relacional:** Organiza los datos en tablas (relaciones) de filas y columnas. Es el más extendido. (Ej: MySQL, PostgreSQL, Oracle).
- **Modelo Documental:** Organiza los datos en documentos flexibles, normalmente en formato JSON/BSON. (Ej: MongoDB).
- **Modelo de Grafos:** Representa los datos como nodos y aristas, ideal para datos interconectados. (Ej: Neo4j).
- **Modelo Clave-Valor:** Almacena datos como pares clave-valor, ideal para cachés y sistemas simples. (Ej: Redis).

#### Resumen Comparativo: La Evolución Lógica

| Característica | Sistema de Ficheros | Sistema de Bases de Datos |
| :--- | :--- | :--- |
| **Abstracción** | Básica (Archivos) | Avanzada (Esquema, Tablas) |
| **Redundancia** | Alta y no controlada | Mínima y controlada |
| **Independencia** | Muy baja (App = Estructura) | Alta (Física y Lógica) |
| **Integridad** | Gestionada por la app | Gestionada por el SGBD |
| **Concurrencia** | Muy difícil de gestionar | Gestionada automáticamente |
| **Consultas** | Secuenciales, programadas | Declarativas (SQL), complejas |
| **Seguridad** | A nivel de archivo | A nivel de tabla, fila, columna |
| **Complexidad** | Para el programador | Para el SGBD (DBMS) |

**Conclusión:** El paso de los sistemas de ficheros a los sistemas de bases de datos representa un salto cualitativo en la gestión de la información. Los SGBD encapsulan toda la complejidad del almacenamiento físico y la gestión de datos, ofreciendo a los desarrolladores un modelo lógico robusto, seguro y eficiente que garantiza la calidad de los datos y simplifica enormemente el desarrollo de aplicaciones.
## 3. Sistemas de bases de datos.

### 3.1. Conceptos.

En un contexto técnico, es esencial precisar la terminología:

*   **Base de Datos (BD):**
    *   **Definición:** Un conjunto estructurado de datos persistente, almacenado en un soporte informático. Es el *activo de información* en sí mismo.
    *   **Naturaleza:** Es pasiva. Son los datos, no el software.
    *   **Analogía:** Los ingredientes y la receta (los datos y el esquema).

*   **Sistema Gestor de Bases de Datos (SGBD o DBMS):**
    *   **Definición:** El software complejo que actúa como un intermediario entre la base de datos, las aplicaciones y los usuarios. Su propósito es proporcionar un entorno eficiente y seguro para la creación, recuperación, actualización y administración de los datos almacenados en la BD.
    *   **Naturaleza:** Es activo. Es el *motor* que manipula la base de datos.
    *   **Analogía:** El chef, los utensilios de cocina y el libro de normas de la cocina (el software y las reglas que manipulan los ingredientes).

*   **Sistema de Base de Datos:**
    *   **Definición:** El sistema completo. Es la unión de los cuatro componentes fundamentales: **los datos (BD)**, **el software (SGBD)**, **el hardware** (servidores, almacenamiento) y **los usuarios** (administradores, desarrolladores, usuarios finales).
    *   **Naturaleza:** Es un ecosistema. Es la suma de todas las partes que interactúan.
    *   **Analogía:** El restaurante completo en funcionamiento (ingredientes, chef, cocina, clientes y personal).

### 3.2. Sistema Gestor de Bases de Datos (SGBD): Núcleo del Sistema

#### **Definición Formal**
Un SGBD es un sistema software de propósito general que facilita el proceso de definición, construcción, manipulación y administración de bases de datos. Proporciona una capa de abstracción que independiza a los programas de aplicación y a los usuarios de los detalles de almacenamiento físico.

#### **Funciones Principales**
1.  **Gestión de Almacenamiento:** Gestiona la asignación de espacio en disco, la estructura de archivos, páginas, y el buffer pool en memoria para optimizar el acceso a datos.
2.  **Procesamiento de Consultas:**
    *   **Parser:** Analiza sintáctica y semánticamente las consultas SQL.
    *   **Optimizador:** Selecciona el **plan de ejecución** más eficiente (elección de índices, algoritmos de join, etc.) para resolver la consulta. Es el "cerebro" del SGBD.
    *   **Motor de Ejecución:** Lleva a cabo el plan generado por el optimizador.
3.  **Gestión de Transacciones:** Garantiza las propiedades **ACID**:
    *   **Atomicidad:** Una transacción se ejecuta por completo o no se ejecuta nada.
    *   **Consistencia:** Toda transacción lleva a la base de datos de un estado válido a otro.
    *   **Aislamiento:** La ejecución concurrente de transacciones produce el mismo estado que una ejecución secuencial.
    *   **Durabilidad:** Los efectos de una transacción confirmada son permanentes.
4.  **Control de Concurrencia:** Implementa mecanismos (como bloqueos multigrado o control de versiones - MVCC) para aislar las transacciones concurrentes y evitar anomalías (lecturas sucias, no repetibles, etc.).
5.  **Gestión de Catálogo (Diccionario de Datos):** Almacena metadatos sobre la estructura de la BD (esquemas, tablas, índices, usuarios, permisos, estadísticas).
6.  **Control de Acceso y Seguridad:** Autentica usuarios y aplica mecanismos de autorización granular (privilegios a nivel de sistema, esquema, tabla, fila e incluso columna).
7.  **Servicios de Copia de Seguridad y Recuperación:** Permite restaurar la base de datos a un estado consistente tras un fallo hardware o software, utilizando copias de seguridad y archivos de log de transacciones.

#### **Estructura y Componentes Arquitectónicos**
La arquitectura modular típica de un SGBD incluye:

*   **Gestor de Archivos:** Gestiona la asignación de espacio en disco y las estructuras de archivos del SO.
*   **Gestor de Almacenamiento:** Responsable del manejo del **buffer pool**, la gestión de páginas y la transferencia de datos entre memoria y disco.
*   **Procesador de Consultas:** Como se describió anteriormente (Parser, Optimizador, Motor de Ejecución).
*   **Gestor de Transacciones:** Coordina las transacciones, gestiona el **log** (registro histórico de modificaciones) y se asegura del cumplimiento de ACID.
*   **Módulo DDL/DML:** Interpreta y ejecuta los comandos del Lenguaje de Definición de Datos (CREATE, ALTER) y del Lenguaje de Manipulación de Datos (SELECT, INSERT, UPDATE, DELETE).
*   **Herramientas de Administración:** Módulos para monitorización, tuning, y mantenimiento (ej: `pgAdmin` para PostgreSQL, `Enterprise Manager` para Oracle).

#### **Clasificación de los SGBD**
1.  **Según el Modelo de Datos:**
    *   **Relacional (RDBMS):** Oracle, MySQL, PostgreSQL, SQL Server.
    *   **NoSQL:**
        *   **Documentales:** MongoDB, Couchbase.
        *   **Clave-Valor:** Redis, DynamoDB.
        *   **Grafos:** Neo4j, Amazon Neptune.
        *   **Columnares:** Cassandra, HBase.
2.  **Según la Licencia:**
    *   **Comerciales/Propietarios:** Requieren pago de licencias. Ofrecen soporte profesional, herramientas avanzadas y alta optimización.
        *   **Oracle Database:** Líder en el mercado empresarial, alto rendimiento, características avanzadas (RAC, Exadata).
        *   **Microsoft SQL Server:** Integración excelente con el ecosistema Microsoft, potente herramienta de BI (SSIS, SSAS, SSRS).
        *   **IBM Db2:** Alto rendimiento en entornos mainframe y distribuidos, fuerte en estabilidad.
    *   **Libres/Open Source:** Código abierto, sin coste de licencia. Comunidad activa y frecuente innovación.
        *   **PostgreSQL:** SGBD relacional open source más avanzado. Cumplimiento extenso de SQL estándar, soporte para tipos de datos avanzados (JSON, geométricos), extensible.
        *   **MySQL:** Muy popular para aplicaciones web. Rápido, fácil de usar, pero con algunas limitaciones en SQL avanzado (mejoradas en versiones recientes).
        *   **MariaDB:** Fork de MySQL creado por los desarrolladores originales, con más características open source y mejoras en motores de almacenamiento.
3.  **Según la Ubicación y Distribución:**
    *   **Centralizados:** Un único servidor SGBD.
    *   **Distribuidos (DDBMS):** Los datos están repartidos en múltiples nodos físicos, manejados por un SGBD que presenta una vista unificada de los datos.
## 4. Bases de datos centralizadas y distribuidas.

### 4.1. Base de Datos Centralizada
**Definición:**
Una base de datos centralizada es aquella en la que todos los datos residen en un único servidor físico (o un clúster de servidores fuertemente acoplados) y son gestionados por una única instancia de un Sistema Gestor de Bases de Datos (SGBD). Todas las operaciones de lectura y escritura se realizan contra esta ubicación única.

**Características Técnicas:**  

- **Arquitectura Única:** Un solo servidor SGBD gestiona todo el esquema de la base de datos.  
- **Localización de Datos:** Todos los datos están almacenados en un único lugar (o un conjunto de discos conectados al mismo servidor).  
- **Gestión de Transacciones ACID:** La atomicidad, consistencia, aislamiento y durabilidad se gestionan de manera sencilla y centralizada.  
- **Punto Único de Acceso:** Los clientes se conectan a un único endpoint o dirección de red.

**Ventajas:**  

- **Simplicidad:** La administración, el backup, la recuperación y la aplicación de políticas de seguridad son más sencillas al estar todo centralizado.
- **Consistencia Fuerte:** Al no existir réplicas o fragmentos, se garantiza fácilmente la consistencia inmediata de los datos.
- **Rendimiento para Datos Locales:** Las transacciones y consultas que acceden a datos relacionados pueden ser muy rápidas al no existir latencia de red interna.

**Inconvenientes:**  

- **Punto Único de Falla (SPOF):** Una caída del servidor o del centro de datos deja toda la aplicación inoperativa.
- **Cuellos de Botella:** Todo el tráfico y el procesamiento convergen en un único punto, lo que puede saturar los recursos (CPU, Memoria, I/O del disco) bajo cargas pesadas.
- **Escalabilidad Limitada:** La escalabilidad es principalmente vertical (*scale-up*): añadir más CPU, RAM o discos más rápidos al servidor, lo que resulta costoso y tiene límites físicos.
- **Problemas de Latencia Geográfica:** Usuarios distribuidos geográficamente experimentan una alta latencia si están lejos del servidor central.

### 4.2. Base de Datos Distribuida (DDBMS)

**Definición:**
Una base de datos distribuida es una colección de múltiples bases de datos lógicamente interrelacionadas, pero distribuidas físicamente en una red de nodos (sitios o centros de datos), y gestionadas por un sistema de gestión de bases de datos distribuidas (DDBMS). El sistema se encarga de que esta distribución sea transparente para el usuario, que puede interactuar con la base de datos como si fuera única.

**Características Técnicas (Principios de C.J. Date):**  

-   **Transparencia de Distribución:** El usuario no necesita saber *dónde* están almacenados físicamente los datos. El DDBMS se encarga de localizarlos y acceder a ellos. Esto incluye transparencia de fragmentación, localización y replicación.  
-   **Autonomía de los Sitios:** Cada nodo (sitio) puede operar de manera independiente y gestionar su base de datos local, participando también en operaciones globales.  
-   **Independencia del Hardware y el SGBD:** Idealmente, los nodos pueden usar hardware y SGBD diferentes (sistemas heterogéneos), aunque en la práctica es más común la homogeneidad.  

**Ventajas:**  

-   **Alta Disponibilidad y Confiabilidad:** La falla de un nodo no implica la caída de todo el sistema. Los datos replicados en otros nodos pueden seguir estando disponibles.
-   **Escalabilidad Horizontal (*Scale-out*):** Se puede aumentar la capacidad añadiendo más nodos/servidores a la red, lo que es más económico y flexible que el *scale-up*.
-   **Mejor Rendimiento y Proximidad:** Los datos pueden colocarse cerca de donde se usan (ej: usuarios en Europa usan un nodo europeo), reduciendo la latencia. La carga de trabajo se distribuye entre varios nodos.
-   **Mayor Autonomía:** Permite que diferentes departamentos o oficinas geográficas controlen sus datos locales.

**Inconvenientes:**  

-   **Complejidad:** El diseño, implementación, administración y debugging son extremadamente complejos.  
-   **Overhead y Coste:** La coordinación entre nodos introduce una sobrecarga en las comunicaciones de red y un mayor consumo de recursos computacionales.  
-   **Retos en la Consistencia:** Garantizar la consistencia de los datos a través de múltiples nodos es un desafío. A menudo se sacrifica la consistencia fuerte inmediata (modelo ACID) por una consistencia eventual en favor de la disponibilidad (Teorema CAP).  
-   **Mayor Dificultad en Seguridad:** La superficie de ataque es mayor al haber múltiples puntos de acceso. La seguridad debe gestionarse de forma coherente en todos los nodos.  

### 4.3. Fragmentación de la Información

La fragmentación es la técnica fundamental para distribuir los datos en un DDBMS. Consiste en dividir una relación (tabla) lógica en partes más pequeñas (fragmentos) que se almacenan en diferentes nodos.

#### **Políticas de Fragmentación:**

1.  **Fragmentación Horizontal:**
    *   **Definición:** Se divide una tabla en subconjuntos de **filas** (tuplas) basándose en un predicado o condición.
    *   **Objetivo:** Localizar datos relevantes en nodos específicos (ej: datos de clientes de una región en el nodo de esa región).
    *   **Ejemplo:** `Clientes_España = σ(Pais = 'España', Clientes)`, `Clientes_Francia = σ(Pais = 'Francia', Clientes)`.

2.  **Fragmentación Vertical:**
    *   **Definición:** Se divide una tabla en subconjuntos de **columnas** (atributos). Cada fragmento debe incluir la clave primaria original para permitir la reconstrucción de las filas.
    *   **Objetivo:** Separar atributos de acceso frecuente o sensibles en diferentes nodos por razones de seguridad o rendimiento.
    *   **Ejemplo:** `Fragmento1 = π(ID_Cliente, Nombre, Email, Clientes)`, `Fragmento2 = π(ID_Cliente, Tarjeta_Crédito, Salario, Clientes)`.

3.  **Fragmentación Mixta (Híbrida):**
    *   **Definición:** Aplicación combinada de las dos técnicas anteriores. Primero se aplica una fragmentación horizontal o vertical y al resultado se le aplica la otra.
    *   **Ejemplo:** Primero fragmentar horizontalmente `Clientes` por `País`, y luego fragmentar verticalmente cada uno de esos fragmentos para separar la información personal de la financiera.

**Principio de Corrección de la Fragmentación:**  

- **Completitud:** Cada dato de la relación global debe asignarse a algún fragmento.  
- **Reconstrucción:** Debe ser posible reconstruir la relación global original a partir de sus fragmentos mediante operaciones relacionales (UNION para horizontal, JOIN para vertical).  
- **Disyunción:** Para evitar redundancia, los fragmentos horizontales deben ser disjuntos entre sí (filas únicas), y los fragmentos verticales deben ser disjuntos en sus atributos (excepto la clave primaria, que se replica para permitir el JOIN).  