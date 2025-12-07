# Diseño de bases de datos relacionales.
## 1. Etapas del diseño.
![Diseño de bases de datos.](img/ud4_img1.png)

El diseño de una base de datos es un proceso estructurado que se realiza en varias etapas, cada una con un objetivo específico, para garantizar que el resultado final sea un sistema **coherente**, **eficiente** y que satisfaga las **necesidades de información** de la organización. En general distinguimos 3 fases diferentes en el diseño de bases de datos.

### 1. Diseño Conceptual 💡

* **Objetivo Principal:** Crear una representación **completa** y **precisa** de la información que debe almacenar la base de datos, reflejando las reglas y la estructura del **Mundo Real** o de la realidad de la empresa (el dominio de la aplicación).
* **Independencia:** Esta fase es totalmente **independiente del SGBD** (Sistema Gestor de Bases de Datos) que se vaya a utilizar. No se consideran aspectos técnicos como el tipo de *software* o las estructuras de almacenamiento.
* **Modelo Típico:** El modelo más utilizado es el **Modelo Entidad-Relación (E-R)** o alguna de sus extensiones (como E-R Extendido o UML). Este modelo utiliza conceptos de **entidades**, **atributos** y **relaciones** para estructurar la información.
* **Resultado:** Se obtiene el **Esquema Conceptual**, que es la representación de alto nivel del diseño. 

### 2. Diseño Lógico ⚙️

* **Objetivo Principal:** Transformar el **Esquema Conceptual** de alto nivel en un esquema que se adapte al **Modelo de Datos** específico del SGBD seleccionado (por ejemplo, relacional, orientado a objetos, o NoSQL).
* **Dependencia:** Es la primera fase en la que se introduce la dependencia del **modelo de datos** del SGBD.
* **Proceso:** Se aplican reglas de **transformación** para mapear las entidades y relaciones del modelo E-R a las estructuras del modelo lógico elegido (ej., tablas y claves en el modelo relacional).
* **Resultado (Modelo Relacional):** Se obtiene el **Esquema Lógico (Relacional)**, que es una colección de **relaciones (tablas)** con sus **atributos** y las **claves primarias y foráneas** que definen las restricciones de integridad.
* **Normalización:** En esta fase, también se suele realizar la **normalización** para reducir la redundancia y evitar anomalías de actualización.

### 3. Diseño Físico 💾

* **Objetivo Principal:** Determinar la **instrumentación eficiente** del **Esquema Lógico** en un sistema de almacenamiento real, ajustándose a las características internas del SGBD y del *hardware*.
* **Dependencia:** Es la fase más dependiente del **SGBD específico** y del **entorno físico** (sistema operativo, dispositivos de almacenamiento, etc.).
* **Decisiones Clave:** Se toman decisiones relacionadas con la **estructura de almacenamiento** y las **estrategias de acceso** para optimizar el rendimiento:
    * **Definición de Índices:** Creación de **índices primarios y secundarios** para acelerar las consultas.
    * **Asignación de Espacio:** Distribución de las tablas en los dispositivos de almacenamiento.
    * **Organización de Archivos:** Elección de las técnicas de almacenamiento (ej. *Heap*, *B-Tree*, *Hash*).
* **Resultado:** Se obtiene la **Descripción de la Implementación**, que son las sentencias DDL (Lenguaje de Definición de Datos) para crear la base de datos física.


#### 📝 Resumen del Proceso

| Fase | Independencia / Dependencia | Modelo/Concepto Clave | Salida Principal |
| :--- | :--- | :--- | :--- |
| **Conceptual** | Independiente del SGBD y del modelo | Modelo Entidad-Relación | Esquema Conceptual |
| **Lógico** | Dependiente del Modelo de Datos | Tablas y Claves (Modelo Relacional) | Esquema Lógico |
| **Físico** | Dependiente del SGBD y del Hardware | Índices, Almacenamiento, DDL | Implementación Física |

#### 🌐 El Ciclo de Vida Completo del Diseño de Bases de Datos

Si bien el Diseño Conceptual, Lógico y Físico constituyen el **núcleo** técnico del proceso, el diseño de una base de datos es parte de un **ciclo de vida** más amplio. Para que el sistema sea un éxito, es imprescindible considerar las fases iniciales de planificación y las fases finales de implementación y mantenimiento.

Además de las tres etapas fundamentales (Conceptual, Lógico y Físico), el proceso de diseño se enmarca en las siguientes fases:

#### 0. Análisis de Requisitos (Fase Pre-diseño) 🔍

* **Propósito:** Es la fase inicial y más crítica. Su objetivo es entender y documentar exhaustivamente las **necesidades de información** de los usuarios y las **reglas del negocio** (o el dominio de la aplicación).
* **Actividades Clave:** Entrevistas con usuarios, estudio de la documentación existente, identificación de informes y transacciones necesarios, y definición de las **restricciones de integridad**.
* **Resultado:** Un documento de **Especificación de Requisitos de Usuario** y de datos que servirá de base para el Diseño Conceptual.

#### 1. Diseño Conceptual, 2. Diseño Lógico, 3. Diseño Físico. (Fases de Diseño)

#### 4. Implementación y Carga de Datos (Fase de Transición) 🛠️

* **Propósito:** Poner en práctica el Diseño Físico. Se utiliza el DDL (Lenguaje de Definición de Datos) para crear la estructura de la base de datos vacía en el SGBD.
* **Actividades Clave:** Creación de tablas, índices y restricciones; y la **carga inicial de los datos** (a menudo mediante procesos ETL - *Extract, Transform, Load* - para migrar datos de sistemas antiguos).

#### 5. Depuración, Puesta en Marcha y Explotación (Fase Post-diseño) ✅

* **Propósito:** Garantizar que la base de datos y las aplicaciones asociadas funcionen correctamente bajo las condiciones operacionales reales y que se gestionen a largo plazo.
* **Actividades Clave:**
    * **Depuración y Pruebas:** Realización de pruebas unitarias y de integración, validación de la integridad de los datos, y pruebas de rendimiento (*benchmarking*) para asegurar que las consultas sean eficientes.
    * **Puesta en Marcha:** Despliegue del sistema en el entorno de producción.
    * **Explotación y Mantenimiento:** La fase operativa continua, que incluye la administración del sistema (copias de seguridad, gestión de usuarios, monitoreo de rendimiento) y las eventuales **modificaciones** del esquema (*tuning*) a medida que evolucionan las necesidades del negocio.

## 2. Transformación del Modelo E-R al Modelo Relacional.

El proceso de mapeo es la traducción de los componentes conceptuales del modelo entidad/relación a las estructuras lógicas relacionales.  
De forma general la transformación está basada en los tres principios siguientes:

- Todo tipo de entidad se convierte en una relación.
- Todo tipo de interrelación N:M se transforma en una relación.
- Todo tipo de interrelación 1:N se traduce en el fenómeno de propagación de clave o bien se crea una nueva relación.

### 1. Mapeo de Entidades y Atributos

- Cada tipo **entidad** se convierte en una relación.
- Cada **atribulo no multivaluado** de una entidad se transforma en una columna de la relación a la que ha dado lugar la entidad.   
Aplicaremos:
    - Atributos identificadores. El atributo identificador principal pasa a ser la clave primaria de la relación. 

    - Atributos identificadores alternativos. Respecto a los atributos identificadores alternativos se recogen con la cláusula UNIQUE y NOT NULL en su caso.

    - Atributos no identificadores. Estos atributos pasan a ser columnas de la relación,  tienen permitido tornar valores nulos a no ser que se indique lo contrario. 

- **Atributos multivaluados.**  Requieren la creación de una **nueva relación (tabla)** que incluya la clave de la entidad que los contiene. Los atributos multivaluados  **nunca** se representan como una columna única. 
![Diseño de bases de datos.](img/ud4_img2.png){width: "160px"; margin: 0 15px 15px 0;"}

### 2. Mapeo de Interrelaciones (Relaciones)

- **Interrelaciones N:M** (Muchos a Muchos)  
Un interrelación N:M se transforma en una relación que tendrá como clave primaria la concatenación de los AIP de los tipos de entidad que asocia. Tendrá también como columnas los atributos de la relación. Habrá que estudiar si es necesario añadir a la clave primaria atributos de la relación.
![Diseño de bases de datos.](img/ud4_img3.png)

- **Interrelaciones 1:N** (Uno a Muchos)  
Existen dos soluciones para la transformación de una interrelación 1:N, y el estudio de las cardinalidades máximas y mínimas puede ayudarnos a elegir la solución más adecuada.
    
    - Propagar los AIP del tipo de entidad que tiene de cardinalidad máxima 1 a la que tiene N.  
    - Crear una nueva tabla, como si se tratara de una interrelación N:M; sin embargo en este caso, la clave primaria de la relación creada es sólo la clave primaria de la tabla a la que le corresponde la cardinalidad N.  
    - Interrelaciones 1:1. Una interrelación de tipo 1:1 es un caso particular de una N:M o, también de una 1 :N, por  lo que no hay regla fija para la transformación de cada tipo de interrelación al modelo relacional. En el caso de la propagación de clave se podría realizar en los dos sentidos, las cardinalidades máximas y mínimas nos permitirán seleccionar la opción más adecuada. 

![Diseño de bases de datos.](img/ud4_img4.png)

---

![Diseño de bases de datos.](img/ud4_img5.png)

### 2. Mapeo de Jerarquías.
En lo que respecta a los tipos y subtipos presentes en la generalización,  no son objetos que se puedan representar explícitamente en el modelo relacional. Ante un tipo de entidad y sus subtipos caben varias soluciones de transformación al modelo relacional, con la consiguiente pérdida de semántica dependiendo de la estrategia elegida. Destacamos tres: 

  - Opción a: Englobar todos los atributos de la entidad y sus subtipos en una sola relación. En general, adoptaremos esta solución cuando los subtipos se diferencien en muy pocos atributos y las interrelaciones que los asocian con el resto de las entidades del esquema sean las mismas para todos (o casi todos) los subtipos. 

  - Opción b: Crear una relación para cada supertipo y tantas relaciones como  subtipos haya, con sus atributos correspondientes. Esta es la solución adecuada cuando existen muchos atributos distintos entre los subtipos y quieren mantener de todas maneras los atributos comunes a todos ellos en una relación. Habrá que crear las restricciones oportunas. 

  - Opción c: Considerar relaciones distintas para cada subtipo, que contenga además de los atributos propios los atributos comunes. Se elegirá esta opción cuando se dieran las mismas condiciones que en el caso anterior  y los accesos realizados sobre los datos de los distintos subtipos siempre afectan a atributos comunes. 


Es necesario especificar las acciones que el Sistema Gestor de Bases de Dato Relacional (SGBDR) debe tomar si se intenta **borrar** la fila referenciada o **modificar** (actualizar) su clave primaria.

Estas reglas se definen mediante las cláusulas `ON DELETE` y `ON UPDATE`.

**🗑️ Opciones de Borrado** (`ON DELETE`)

| Opción | Descripción |
| :--- | :--- |
| **`CASCADE`** | Cuando la fila referenciada (padre) es **borrada**, las filas que la referencian (hijas) **también son borradas** de forma recursiva. |
| **`SET NULL`** | Cuando la fila referenciada (padre) es **borrada**, la columna de la clave foránea en las filas que la referencian (hijas) se establece en **`NULL`**. *(Requiere que la columna FK acepte nulos).* |
| **`SET DEFAULT`**| Cuando la fila referenciada (padre) es **borrada**, la columna de la clave foránea en las filas hijas se establece en el **valor por defecto** predefinido. |
| **`RESTRICT`** | **Impide** el borrado de la fila referenciada (padre) si existen filas que la referencian (hijas). Si existen filas hijas, la operación de borrado **falla**. (Suele ser la opción por defecto en algunos SGBD si no se especifica nada). |
| **`NO ACTION`** | Similar a `RESTRICT`, pero la comprobación de la restricción se realiza **al final** de la transacción en lugar de inmediatamente. En la práctica, suele comportarse de forma idéntica a `RESTRICT`. |

**✏️ Opciones de Modificación** (`ON UPDATE`)

| Opción | Descripción |
| :--- | :--- |
| **`CASCADE`** | Cuando la clave primaria de la fila referenciada (padre) es **modificada**, el valor de la clave foránea en las filas que la referencian (hijas) **también se actualiza** para reflejar el nuevo valor. |
| **`SET NULL`** | Cuando la clave primaria de la fila referenciada (padre) es **modificada**, la columna de la clave foránea en las filas hijas se establece en **`NULL`**. *(Requiere que la columna FK acepte nulos).* |
| **`SET DEFAULT`**| Cuando la clave primaria de la fila referenciada (padre) es **modificada**, la columna de la clave foránea en las filas hijas se establece en el **valor por defecto** predefinido. |
| **`RESTRICT`** | **Impide** la modificación (actualización) de la clave primaria de la fila referenciada (padre) si existen filas que la referencian (hijas). Si existen, la operación **falla**. |
| **`NO ACTION`** | Similar a `RESTRICT`, la comprobación se retrasa al final de la transacción. |

Generalmente, al trabajar con **claves artificiales** (o *surrogate keys*) en el modelo relacional, es **preferible impedir la modificación** (actualización) de esas claves primarias una vez que tienen referencias en otras tablas.

Esto se debe a las siguientes razones:

**1. Inmutabilidad de las Claves Artificiales**  
Las claves artificiales, como los IDs autoincrementales o UUIDs, tienen como única función ser un identificador interno y estable de la fila, sin significado de negocio. Su valor no depende de ninguna característica del mundo real.  
El principio de una clave artificial es la inmutabilidad. Una vez asignada, nunca debería cambiar.  
En una clave natural (ej., DNI, matrícula), una actualización podría justificarse si el valor natural se corrigiera (ej., un error tipográfico en el DNI), pero en una clave artificial, el valor no tiene lógica externa que corregir.

**2. Eficiencia y Coherencia Transaccional**  
Aunque se puede usar la restricción ON UPDATE CASCADE (actualización en cascada), generalmente se evita su uso en claves primarias:  

  - Rendimiento: Forzar al SGBD a actualizar automáticamente cientos o miles de filas en tablas referenciadas cada vez que se cambia una clave primaria es una operación costosa y puede llevar mucho tiempo, afectando negativamente el rendimiento de la base de datos.

  - Bloqueos: Las operaciones en cascada pueden generar bloqueos de tabla significativos mientras se ejecutan, impidiendo que otros usuarios accedan o modifiquen los datos durante el proceso.

  - Riesgo de Errores: Aunque la propagación asegura la consistencia, el riesgo de introducir un error en una operación masiva y compleja es mayor que simplemente prohibir la modificación.

La práctica estándar es definir la restricción de integridad referencial para las claves foráneas con:

```
ON UPDATE RESTRICT o ON UPDATE NO ACTION
```
Esto impedirá que el SGBD actualice la clave primaria si existen filas que la referencian. La base de datos forzará al diseñador o al programador a no intentar cambiar ese valor.

En resumen, la clave artificial funciona mejor como un ancla inamovible de la fila. Si se necesita modificar la fila, se deben modificar los atributos de datos (nombre, fecha, etc.), no su identificador permanente.


## 3. Grafo relacional.
Un **grafo relacional** es una herramienta visual clave en la fase de **Diseño Lógico** de una base de datos. Es una **representación gráfica y esquemática** del **Esquema Lógico** de la base de datos, una vez que este ha sido transformado del modelo conceptual (como el Modelo E-R) al Modelo Relacional.

Sirve esencialmente como un **mapa de la estructura de las tablas** y las **conexiones lógicas** que existen entre ellas.

#### Componentes

El grafo relacional se compone de los siguientes elementos clave para describir la estructura lógica de la base de datos:

1.  **Nodos (Tablas):** Cada **entidad** o **relación** del modelo conceptual se convierte en un **nodo** del grafo (una tabla o relación en el modelo relacional). Se especifican los atributos de cada tabla, destacando la **clave primaria (PK)**.
2.  **Arcos (Claves Foráneas):** Las **interrelaciones** entre las entidades se representan con **arcos (flechas o líneas)** que conectan los nodos. Estos arcos indican la existencia de una **clave foránea (FK)** que migra de la tabla referenciada a la tabla que la referencia.
3.  **Restricciones:** El grafo a menudo incluye anotaciones sobre las **restricciones de integridad referencial**, como las opciones de borrado y modificación (`ON DELETE`, `ON UPDATE`) que se aplicarán a las claves foráneas.

#### Función.

El propósito principal del grafo relacional es doble:

* **Verificación:** Permite verificar visualmente que todas las reglas del negocio y las relaciones del esquema conceptual se hayan mapeado correctamente a las estructuras del modelo relacional.
* **Documentación:** Sirve como una documentación concisa y de alto nivel del diseño final, facilitando a programadores y administradores entender rápidamente la estructura y las dependencias de la base de datos.

En resumen, es la **visión final de la arquitectura de datos** antes de pasar a la implementación física (código DDL).

Ejemplo

![Diseño de bases de datos.](img/ud4_img6.png)


## 4. Teoría de Normalización.
### 1. Normalización.
La Normalización es un proceso formal y sistemático que se aplica durante el **Diseño Lógico** (después de la transformación del Modelo E-R) para asegurar que la estructura del esquema relacional sea la más **eficiente** y **robusta** posible.

El objetivo principal es la **generación de un conjunto de esquemas relacionales** que permita:

1.  Almacenar la información con la **mínima redundancia** necesaria.
2.  Garantizar la **integridad** y **coherencia** de los datos.
3.  Facilitar la **recuperación** y **manipulación** de la información.

Un esquema de relación mal diseñado conduce a **anomalías** que dificultan el mantenimiento de la base de datos y aumentan los costes de almacenamiento:

| Anomalía | Descripción |
| :--- | :--- |
| **Repetición de la Información** | Duplicidad de datos en múltiples filas, lo que incrementa el espacio de almacenamiento. |
| **Anomalía de Inserción** | Imposibilidad de representar determinada información si no se conoce la clave completa (por ejemplo, no poder añadir una asignatura si ningún alumno la cursa aún). |
| **Anomalía de Borrado** | Pérdida de información importante al borrar una fila que contiene datos redundantes (por ejemplo, al borrar la última fila de un alumno, se pierde la información del profesor que imparte una asignatura). |
| **Anomalía de Actualización** | Procesos de modificación difíciles y costosos, ya que una misma información debe actualizarse en múltiples lugares, creando el riesgo de **inconsistencia** si alguna copia se omite. |

### 2. Dependencias Funcionales (DF)

La normalización se basa enteramente en el concepto de **Dependencia Funcional (DF)**, que son las restricciones semánticas inherentes al contenido de los datos y las reglas del negocio.  
Sea un esquema de relación *R(A, D)*, donde *A* es el conjunto de atributos y *D* el conjunto de dependencias.   

Sean *X* e *Y* dos subconjuntos de *A*, denominados **descriptores**.  
Se dice que **Y depende funcionalmente de X**, y se denota como $X → Y$, si y solo si, a cada valor *x* del atributo (o conjunto de atributos) *X* le corresponde **un único valor** *y* del atributo (o conjunto de atributos) *Y*.

Una dependencia funcional X → Y indica que **cada valor de X determina exactamente un valor de Y**.  
Ejemplo: *DNI → Nombre, Dirección*.  

En el proceso de normalización es necesario diferenciar los siguientes tipos de dependencias funcionales.

* **Dependencia funcional total o completa**: Y depende completamente de X (no de una parte).
* **Dependencia funcional parcial**: Y depende de un subconjunto de X.
* **Dependencia funcional transitiva**: X → Y y Y → Z implican X → Z.

### 3. Formas Normales (FN)

**Proceso de transformación.**

El proceso de normalización descompone un esquema original *R* en un conjunto de esquemas *{R_i}*. Para que este conjunto sea **equivalente** y **mejor** que el original, debe cumplir las siguientes propiedades:

1.  **Conservación de la Información (Libre de Pérdida):** Al realizar la **reunión natural** (Join) de todas las relaciones *{R_i\}*, se debe poder reconstruir la relación original *R* **exactamente**.
2.  **Conservación de las Dependencias:** Todas las dependencias funcionales originales en *R* deben poder ser verificadas en las relaciones *{R_i}*, o inferidas a partir de las dependencias de *{R_i}*. Esto asegura que las restricciones semánticas se mantengan en el modelo.
3.  **Mínima Redundancia (Normalización):** Las relaciones resultantes *{R_i}* deben estar en las **formas normales más avanzadas** posibles, lo que garantiza la mínima redundancia y reduce las anomalías.

Las formas normales son reglas o niveles de calidad que se aplican al diseño de bases de datos relacionales para evitar redundancias, inconsistencias y anomalías al insertar, actualizar o borrar datos.
Cada forma normal es un paso de refinamiento sobre el anterior.

**Primera Forma Normal (1FN)**

La 1FN impone el requisito más básico: la **atomicidad** de los datos.

Un esquema de relación $R$ está en **1FN** si los dominios de todos sus atributos son **atómicos**.  

No existen **atributos multivaluados** (varios valores en una misma celda) ni **atributos compuestos** que no hayan sido separados.  

Para pasar a 1FN se eliminan los atributos multivaluados creando una **nueva relación** que incluye la clave de la relación original y el atributo multivaluado.  

Por definición todas las relaciones están en 1FN.

**Segunda Forma Normal (2FN)**

La 2FN busca eliminar las **dependencias parciales** de la clave primaria.

Una relación *R* está en **2FN** si y solo si:   

  - Está en **1FN**.
  - Cada **atributo no principal** (que no forma parte de ninguna clave candidata) tiene **dependencia funcional completa** (plena) respecto de **cada** clave candidata.

Resuelve anomalías de redundancia y actualización que surgen cuando un atributo no clave depende solo de una **parte** de una clave compuesta.

Para normalizar se descompone la relación en dos o más relaciones, moviendo los atributos que dependen solo de una parte de la clave a una nueva relación cuya clave es esa parte.

**Tercera Forma Normal (3FN)**

La 3FN busca eliminar las **dependencias transitivas**.

Un esquema de relación *R* está en **3FN** si y solo si:  

  - Está en **2FN**.
  - No existe ningún **atributo no principal** que dependa **transitivamente** de **alguna clave** de *R*.

Resuelve anomalías de redundancia y actualización que surgen cuando un atributo no clave depende de otro atributo no clave.

Para normalizar, se descompone la relación extrayendo los atributos que están involucrados en la dependencia transitiva a una nueva relación.

**Forma Normal de Boyce-Codd (FNBC)**

La FNBC es una forma normal más estricta que la 3FN.

Se dice que tina relación se encuentra en FNBC si y sólo si, todo determinante (atributo del que depende otro) es una clave candidata. 

No siempre es posible  transformar un esquema de relación que no esté en FNBC en esquema de relación en FNBC sin que se produzca pérdida de dependencias funcionales.
