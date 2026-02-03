# Administración de BD (DDL-DCL)

## 1. Introducción al Diseño Físico de Bases de Datos

Una vez definido y validado el esquema lógico de una base de datos, el siguiente paso crítico en el ciclo de vida del desarrollo de bd, consiste en su **implantación física** utilizando un Sistema Gestor de Bases de Datos Relacional (SGBDR).

Esta fase no es una mera traducción directa; implica transformar el diseño lógico en estructuras de almacenamiento reales y eficientes utilizando un lenguaje formal, estándar y universal: **SQL (Structured Query Language)**.

En esta unidad estudiaremos el esquema físico de una base de datos relacional desde una perspectiva técnica y profesional, abordando los siguientes pilares:

* **Definición de Estructuras:** Creación de tablas y selección de **tipos de datos adecuados** para optimizar el espacio y la integridad.
* **Integridad y Reglas de Negocio:** Establecimiento de claves primarias, ajenas y restricciones (`constraints`) que aseguren la calidad de la información.
* **Seguridad y Abstracción:** Gestión de usuarios, permisos y creación de **vistas** para proteger y simplificar el acceso a los datos.
* **Rendimiento y Optimización:** Implementación de **índices** y otros mecanismos físicos para garantizar tiempos de respuesta rápidos en entornos de producción.

Para proporcionar una visión completa y realista del mercado laboral, nos centraremos en dos de los SGBD más relevantes de la actualidad:

1. **[MySQL:](https://dev.mysql.com/doc/refman/8.4/en/){target=blank}** El referente en el ámbito del código abierto (*open-source*) y entornos web.
2. **[Oracle Database:](https://docs.oracle.com/en/database/oracle/oracle-database/index.html){target=blank}** El estándar de facto en entornos corporativos y de gran escala.

Analizaremos sus elementos comunes basados en el estándar ISO/ANSI, pero también sus **principales diferencias**, lo que nos permitirá adaptarnos a cualquier entorno de trabajo profesional.

---

**🔗 Recursos de interés**  

* **[DB-Engines Ranking](https://db-engines.com/en/ranking){target=blank}:** Un ranking actualizado de los motores de bases de datos más utilizados en el mundo.
* **[Introducción al estándar SQL (Wikipedia)](https://es.wikipedia.org/wiki/SQL){target=blank}:** Un repaso rápido a la historia y componentes del lenguaje.
---
## 2. El Esquema Físico de una Base de Datos

El **esquema físico** constituye el nivel más bajo de abstracción en la arquitectura de una base de datos. Mientras que el esquema lógico define *qué* datos se almacenan y qué relaciones existen entre ellos, el esquema físico determina **cómo** se organizan, almacenan y gestionan realmente en el soporte físico del SGBD.

### 2.1. El concepto de Esquema (Schema) y Catálogo

En el diseño físico, el **Esquema** no es solo un concepto teórico; es un objeto lógico dentro del SGBD que funciona como un contenedor para agrupar tablas, vistas, índices y otros elementos relacionados.

La implementación de este concepto varía según el sistema gestor:

* **En Oracle:** Un esquema está ligado intrínsecamente a un **usuario**. Al crear un usuario en el sistema, Oracle crea automáticamente un esquema con el mismo nombre para que este almacene sus objetos.
* **En MySQL:** El término `SCHEMA` es, a efectos prácticos, un sinónimo de `DATABASE`. Crear un esquema equivale a crear una base de datos independiente.

En el ámbito de las bases de datos, el **catálogo** (también llamado **Diccionario de Datos** o **Metadatos**) es el lugar donde el SGBD almacena toda la información sobre la estructura y la gestión del propio sistema.

En pocas palabras: **es la base de datos que describe a la propia base de datos.**

 🔍 ¿Qué contiene exactamente?

El catálogo no guarda los datos de los alumnos o profesores, sino la "receta" de cómo están organizados:

* **Estructura:** Nombres de tablas, columnas, tipos de datos y restricciones (PK, FK, Checks).
* **Objetos lógicos:** Definiciones de vistas, índices, disparadores (triggers) y procedimientos.
* **Seguridad:** Usuarios existentes, roles asignados y los privilegios (DCL) de cada uno.
* **Estadísticas:** Tamaño de las tablas y uso de almacenamiento (cuotas).

🛠️ ¿Cómo se consulta?

Los usuarios no suelen modificar el catálogo directamente con `INSERT` o `DELETE`, sino que el sistema lo actualiza automáticamente cuando ejecutas comandos DDL (`CREATE`, `ALTER`, `DROP`).

Para consultarlo, cada sistema tiene sus propias vistas:

**En MySQL:** Se encuentra en la base de datos especial `information_schema`.  

Ejemplo: `SELECT * FROM information_schema.tables;`


**En Oracle:** Se consulta a través de vistas de sistema como `USER_TABLES`, `ALL_TABERNACLES` o `DBA_USERS`.  

Ejemplo: `SELECT * FROM user_constraints;`



> Cuando se intenta borrar una tabla y el sistema le da un error porque existe una Clave Foránea, es el **Catálogo** quien le "avisa" al motor de la base de datos que esa relación existe y debe protegerse.

### 2.2. Dominios: La base del tipado de datos

Antes de proceder a la creación de tablas, el diseño físico debe considerar los **dominios**. Un dominio es el conjunto de valores válidos (el "universo" de posibilidades) que puede tomar un atributo.

Podemos distinguir dos tipos principales:

* **Dominios predefinidos:** Son los tipos de datos que ofrece el SGBDR de forma nativa (ej. `INTEGER`, `VARCHAR`, `DATE`).
* **Dominios definidos por el usuario:** El estándar SQL permite crear tipos de datos personalizados para asegurar la consistencia semántica (ej. un dominio `D_COD_POSTAL` que valide un formato específico de 5 dígitos).

La creación de dominios varía significativamente según el motor elegido, reflejando distintas filosofías de diseño:  

* **PostgreSQL y Firebird:** Son los más fieles al estándar SQL. Ambos permiten el uso de la sentencia `CREATE DOMAIN`, permitiendo definir un tipo de dato con nombre propio y restricciones asociadas (por ejemplo, un dominio `D_PRECIO` como un `DECIMAL` siempre mayor que cero) que luego puede ser reutilizado en múltiples tablas. Firebird, en particular, destaca por su robustez histórica en este aspecto, facilitando la mantenibilidad del esquema físico.
* **Oracle Database:** No implementa `CREATE DOMAIN` de forma directa, pero ofrece una alternativa mucho más potente mediante **Tipos de Objetos** (`CREATE TYPE`). Esto permite al diseñador crear estructuras de datos complejas y métodos asociados, orientando el diseño físico hacia el modelo objeto-relacional.
* **MySQL:** Es el más restrictivo en este punto, ya que carece de una sentencia para definir dominios personalizados. En su lugar, el diseñador debe definir el tipo de dato básico en cada columna y aplicar restricciones `CHECK` o tipos `ENUM` para validar que los valores pertenecen al dominio deseado.
---
**🔗 Recursos de interés.** 

* **[Dominios en PostgreSQL](https://www.postgresql.org/docs/current/sql-createdomain.html){target=blank}:** Documentación oficial sobre la creación de dominios.
* **[Language Reference de Firebird - Dominios](https://firebirdsql.org/file/documentation/html/en/refdocs/fblangref30/firebird-30-language-reference.html%23fblangref30-ddl-domn){target=blank}:** Guía técnica sobre cómo Firebird gestiona los tipos definidos por el usuario.
* **[Tipos de datos definidos por el usuario en Oracle](https://docs.oracle.com/en/database/oracle/oracle-database/19/adobj/basic-components-of-oracle-objects.html){target=blank}:** Introducción a los tipos de objetos en Oracle.

### 2.3. Componentes del Esquema Físico

La implementación del esquema físico mediante el lenguaje SQL implica la definición de varios elementos críticos para el funcionamiento del sistema:

* **Definición de tablas:** Creación de las estructuras base de almacenamiento (`CREATE TABLE`).
* **Elección de tipos de datos y dominios:** Selección del formato físico más eficiente para cada atributo, equilibrando precisión y ahorro de espacio.
* **Integridad Referencial:** Declaración formal de **claves primarias** (`PRIMARY KEY`) y **foráneas** (`FOREIGN KEY`).
* **Restricciones de Integridad:** Reglas automáticas para validar la calidad del dato, como `NOT NULL`, `UNIQUE` y cláusulas `CHECK`.
* **Vistas:** Capas de abstracción que presentan los datos de forma simplificada o protegida.
* **Gestión de Seguridad:** Definición de **usuarios y privilegios** para controlar el acceso al esquema (sentencias `GRANT` y `REVOKE`).
* **Estructuras de Optimización:** Creación de **índices** diseñados para minimizar la carga de lectura/escritura (E/S) y acelerar las consultas.

---

**🔗 Recursos de interés**

* **[Diferencias entre Schema y Database (MySQL)](https://dev.mysql.com/doc/refman/8.0/en/glossary.html%23glos_schema){target=blank}:** Glosario oficial sobre la terminología en MySQL.
* **[Arquitectura de Esquemas en Oracle](https://docs.oracle.com/en/database/oracle/oracle-database/19/cncpt/introduction-to-oracle-database.html){target=blank}:** Documentación oficial sobre la gestión de usuarios y esquemas.

---

## 3. El Estándar SQL: Evolución y Normalización

El **SQL (Structured Query Language)** es el lenguaje universal para la gestión de Sistemas de Gestores de Bases de Datos Relacionales (SGBDR). Su éxito radica en su capacidad de abstracción y en el esfuerzo de estandarización liderado por organismos internacionales.
Para cerrar esta sección introductoria y dar una visión técnica completa, es fundamental explicar que SQL no es un lenguaje monolítico, sino que se divide en varios sublenguajes especializados según la tarea que realizan sobre el esquema físico.  

Aunque nos referimos a SQL como un único lenguaje, éste se divide funcionalmente en tres subconjuntos que permiten gestionar el ciclo de vida completo de la base de datos:

* **DDL (Data Definition Language):** Es el lenguaje de **definición** de datos. Es el protagonista del diseño físico, ya que incluye sentencias como `CREATE`, `ALTER` y `DROP`, que permiten crear, modificar y borrar la estructura de las tablas, esquemas, índices y dominios.
* **DML (Data Manipulation Language):** Es el lenguaje de **manipulación** de datos. Una vez definida la estructura, este sublenguaje permite gestionar el contenido mediante sentencias como `INSERT`, `UPDATE`, `DELETE` y la fundamental `SELECT` para la recuperación de información.
* **DCL (Data Control Language):** Es el lenguaje de **control**. Se encarga de la seguridad y la integridad, permitiendo a los administradores gestionar el acceso al esquema físico mediante las sentencias `GRANT` (otorgar permisos) y `REVOKE` (retirar permisos).

---

**🔗 Recursos de interés.**

* **>>[Diferencias entre DDL, DML, DCL y TCL](https://www.geeksforgeeks.org/sql/sql-ddl-dql-dml-dcl-tcl-commands/){target=blank}:** Una guía rápida para diferenciar cada tipo de comando.
  
---

### 3.1. Organismos de Estandarización

Para garantizar la interoperabilidad entre diferentes sistemas (como Oracle, PostgreSQL, MySQL o SQL Server), el lenguaje es supervisado principalmente por:

* **ANSI** (American National Standards Institute).
* **ISO** (International Organization for Standardization).

### 3.2. Hitos en la Evolución del Estándar

A lo largo de las décadas, el estándar ha evolucionado para adaptarse a las nuevas necesidades de procesamiento de datos:

| Versión | Año | Aportaciones Clave |
| --- | --- | --- |
| **SQL-86** | 1986 | Primera ratificación del estándar por ANSI. |
| **SQL-92 (SQL2)** | 1992 | Incrementó sustancialmente la capacidad semántica del esquema relacional. Añadió nuevos tipos de datos, operadores (`JOIN` explícitos) y una mejor gestión de errores. |
| **SQL:1999 (SQL3)** | 1999 | Introdujo características del modelo **objeto-relacional**, triggers, y consultas recursivas. |
| **SQL:2016** | 2016 | Añadió soporte para **JSON**, permitiendo que las bases de datos relacionales manejen datos semiestructurados. |
| **SQL:2023** | 2023 | La versión más reciente. Introduce las **Property Graph Queries (SQL/PGQ)**, permitiendo consultar datos relacionales como si fueran una base de datos de grafos. |

### 3.3. Cumplimiento del Estándar (Compliance)

Aunque existe un estándar oficial, es importante destacar que la mayoría de los SGBDR comerciales y de código abierto implementan el estándar de forma parcial o añaden extensiones propias (dialectos).

> **Nota:** Un SGBDR moderno suele cumplir al 100% el "Core SQL", pero las funcionalidades avanzadas pueden variar entre fabricantes.

---

**🔗 Recursos de interés.**

* **[ISO/IEC 9075](https://www.iso.org/standard/76583.html){target=blank}:** Página oficial de la ISO donde se publican las partes del estándar SQL (en inglés).
* **[Documentación de PostgreSQL sobre cumplimiento de SQL](https://www.postgresql.org/docs/current/features.html){target=blank}:** Un excelente ejemplo de cómo un motor de BD documenta su fidelidad al estándar.
* **[SQL:2023 - Wikipedia](https://en.wikipedia.org/wiki/SQL:2023){target=blank}:** Resumen detallado de las últimas incorporaciones al lenguaje.

---

## 4. Lenguaje de definición de datos (DDL).

El **lenguaje de definición de datos (DDL)** permite crear, modificar y eliminar los objetos que forman parte del esquema físico de una base de datos. Las sentencias DDL actúan sobre la estructura y no sobre los datos almacenados.

Las principales sentencias DDL son: `CREATE`, `ALTER`, `DROP` y `TRUNCATE`.

### 4.1 Esquemas.

En el diseño físico, la gestión de esquemas nos permite estructurar el espacio de trabajo. Utilizaremos `CREATE`, `ALTER` y `DROP` para la gestión.

Aunque el estándar SQL define el esquema como un contenedor lógico, su implementación física varía drásticamente entre los gestores más utilizados, lo que influye en cómo se organiza el diseño:

* **MySQL:** Los términos `SCHEMA` y `DATABASE` son **equivalentes y sinónimos**. En este motor, un esquema es el contenedor de mayor nivel; no existe una subdivisión jerárquica entre ambos. Al ejecutar `CREATE SCHEMA`, se está creando físicamente una base de datos independiente en el servidor.
* **PostgreSQL:** Sigue fielmente el estándar al mantener una separación clara. En una misma **Instancia** pueden existir varias **Bases de Datos**, y cada base de datos puede contener múltiples **Esquemas** (como si fuesen carpetas). Esto permite, por ejemplo, separar en una misma BD las tablas de `ventas` de las de `contabilidad` de forma totalmente lógica.
* **Oracle Database:** Utiliza un enfoque basado en el usuario. Un esquema en Oracle **es el conjunto de objetos que pertenecen a un usuario** específico. No se crea un esquema de forma independiente; cuando se crea un usuario en la base de datos, se genera automáticamente un esquema asociado a él. Por tanto, en Oracle, el concepto de "esquema" y "usuario" son, a efectos prácticos, la misma entidad.

#### A. Creación (CREATE SCHEMA)

Define el espacio de nombres inicial. Según el estándar SQL-92, puede incluir la cláusula de autorización.

```sql
-- Sintaxis básica
CREATE SCHEMA <nombre_esquema> [AUTHORIZATION <usuario>];

-- Ejemplo:
CREATE SCHEMA biblioteca AUTHORIZATION bibliotecario_admin;

```

#### B. Modificación (ALTER SCHEMA)

La modificación de un esquema es limitada. Generalmente, no se puede cambiar "qué es" un esquema, pero sí **quién lo posee** o **cómo se llama**.

* **Cambio de nombre:** Útil durante reestructuraciones de la base de datos.
* **Cambio de propietario:** Esencial cuando un administrador o desarrollador deja el proyecto.

```sql
-- Sintaxis general (varía según SGBD, ejemplo en PostgreSQL/SQL Server)
ALTER SCHEMA biblioteca RENAME TO archivo_historico;
ALTER SCHEMA archivo_historico OWNER TO nuevo_administrador;

```

> **Nota:** En **MySQL**, no existe una sentencia `ALTER SCHEMA` para cambiar el nombre directamente; normalmente se requiere crear un esquema nuevo y mover las tablas.

#### C. Borrado (DROP SCHEMA)

Elimina el esquema del sistema. Esta es una operación crítica y definitiva. Existen dos modos de borrado que el estándar define:

1. **RESTRICT (por defecto):** El esquema solo se borra si está **vacío**. Si contiene una sola tabla o vista, el sistema devolverá un error.
2. **CASCADE:** El sistema borra el esquema y **todos los objetos que contenga** (tablas, índices, disparadores, etc.) de forma automática.

```sql
-- Borrado seguro (solo si está vacío)
DROP SCHEMA biblioteca RESTRICT;

-- Borrado total (elimina todo su contenido)
DROP SCHEMA biblioteca CASCADE;

```

---

#### Resumen de comandos por SGBD

| Operación | MySQL | Oracle | PostgreSQL |
| --- | --- | --- | --- |
| **Crear** | `CREATE SCHEMA` / `DATABASE` | `CREATE USER` | `CREATE SCHEMA` |
| **Renombrar** | No permitido directamente | `RENAME` (de usuario) | `ALTER SCHEMA ... RENAME TO` |
| **Borrar** | `DROP SCHEMA` | `DROP USER ... CASCADE` | `DROP SCHEMA ... CASCADE` |

---

**🔗 Recursos de interés.**

* **[PostgreSQL ALTER SCHEMA](https://www.postgresql.org/docs/current/sql-alterschema.html){target=blank}:** Ejemplo de cómo cambiar nombres y dueños de esquemas.
* **[MySQL DROP DATABASE/SCHEMA](https://dev.mysql.com/doc/refman/8.0/en/drop-database.html){target=blank}:** Advertencias sobre el borrado de esquemas en MySQL.
* **[Oracle DROP USER](https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/DROP-USER.html){target=blank}:** Explicación de por qué en Oracle se borra el usuario para borrar el esquema.

---
### 4.2 Tipos de datos nativos.

Antes de definir dominios o tablas, es necesario conocer los tipos de datos que el SGBD proporciona de forma nativa. Aunque el estándar SQL define tipos universales, cada motor implementa sus propias variaciones para optimizar el almacenamiento físico.

Los tipos de datos se agrupan en cuatro grandes categorías:

#### A. Tipos Numéricos

Se dividen en números enteros y números de coma fija o flotante (decimales).

* **`INTEGER` / `INT`:** Para números enteros.
* **`DECIMAL(p,s)` / `NUMBER(p,s)`**: Para una precisión exacta (ideal para dinero). La `p` es la precisión (dígitos totales) y la `s` la escala (decimales).
* **`FLOAT` / `REAL`**: Para cálculos científicos donde se requiere un gran rango pero se permite una ligera imprecisión.

#### B. Tipos de Cadena (Alfanuméricos)

* **`CHAR(n)`**: Cadena de longitud fija. Si no se llena, el SGBD añade espacios.
* **`VARCHAR(n)`**: Cadena de longitud variable. Solo ocupa lo que se escribe (hasta un máximo `n`).
> **Diferencia clave:** En **Oracle** se recomienda usar `VARCHAR2` en lugar de `VARCHAR`.


* **`TEXT` / `CLOB`**: Para textos muy largos que superan los límites de un varchar (como el contenido de un artículo).

#### C. Tipos de Fecha y Tiempo

* **`DATE`**: Almacena año, mes y día. (Nota: En Oracle también incluye la hora).
* **`TIME`**: Almacena solo la hora.
* **`TIMESTAMP`**: Combinación de fecha y hora, a menudo con precisión de milisegundos.

En el diseño físico de bases de datos, la elección entre `DATE` y `TIMESTAMP` es una decisión de arquitectura que afecta tanto al almacenamiento como a la funcionalidad de la aplicación. La diferencia fundamental reside en la **granularidad**, el **rango** y el **tratamiento de la ubicación geográfica**:

* **Precisión y Contenido:** Mientras que el tipo **`DATE`** está orientado a registrar fechas absolutas sin necesidad de precisión horaria (año, mes y día), el tipo **`TIMESTAMP`** está diseñado para capturar instantes exactos, incluyendo horas, minutos, segundos y, habitualmente, fracciones de segundo (milisegundos o microsegundos). Es importante notar una particularidad de **Oracle**: su tipo `DATE` rompe la norma al incluir siempre la hora, mientras que en **MySQL** o **PostgreSQL** son tipos estrictamente separados.
* **Contexto Geográfico (Zonas Horarias):** Esta es la diferencia más crítica para aplicaciones globales. El tipo `DATE` es agnóstico a la ubicación (un cumpleaños es el mismo día en Tokio que en Madrid). Sin embargo, el **`TIMESTAMP`** suele ser "consciente" de la zona horaria (*timezone aware*); el SGBD convierte la hora local a una hora universal (UTC) para almacenarla y la reconvierte a la hora local del usuario al consultarla.
* **Ciclo de Vida del Registro:** En motores como **MySQL**, el `TIMESTAMP` posee la capacidad de "auto-actualización", permitiendo que el propio sistema grabe la hora exacta de creación o modificación de una fila sin que el programador tenga que enviarla en la consulta SQL.

#### Ejemplos de uso en el diseño de tablas:

1. **Uso de `DATE` (Eventos del calendario):**
En una tabla de empleados, la columna `fecha_nacimiento` debe ser `DATE`. No nos interesa el milisegundo en que nació el empleado, ni queremos que la fecha cambie si el administrador de la base de datos consulta la tabla desde una zona horaria distinta.
2. **Uso de `TIMESTAMP` (Auditoría y Logística):**
En una tabla de `pedidos_online`, la columna `fecha_pago` debe ser `TIMESTAMP`. En un sistema de alta concurrencia, saber si un pago entró a las `14:05:01.125` o a las `14:05:01.128` puede ser vital para resolver disputas o procesar el inventario. Además, si el cliente está en México y el servidor en España, el `TIMESTAMP` garantiza que ambos vean el momento relativo correcto a su hora local.

#### D. Otros tipos

* **`BOOLEAN`**: Verdadero/Falso (Soportado en PostgreSQL, emulado como `TINYINT(1)` en MySQL o `NUMBER(1)` en Oracle).
* **`BLOB`**: Para datos binarios (imágenes, PDFs, archivos).

#### Comparativa Rápida de Tipos de Datos

Para el diseño físico, es vital saber traducir los tipos entre motores:

| Tipo Lógico | MySQL | Oracle | PostgreSQL |
| --- | --- | --- | --- |
| Entero | `INT` | `NUMBER` | `INTEGER` |
| Decimal exacto | `DECIMAL` | `NUMBER` | `NUMERIC` |
| Cadena variable | `VARCHAR` | `VARCHAR2` | `VARCHAR` |
| Fecha y hora | `DATETIME` | `DATE` | `TIMESTAMP` |
| Booleano | `TINYINT(1)` | `NUMBER(1)` | `BOOLEAN` |

---
**🔗 Enlaces de interés.**

* **[Tipos de datos en MySQL](https://dev.mysql.com/doc/refman/8.0/en/data-types.html){target=blank}:** Guía oficial sobre rangos y almacenamiento.
* **[Oracle Data Types](https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/Data-Types.html){target=blank}:** Documentación sobre los tipos nativos en Oracle 19c.
* **[PostgreSQL Data Types](https://www.postgresql.org/docs/current/datatype.html){target=blank}:** Referencia sobre la amplia variedad de tipos en Postgres.

---
### 4.3 Dominios.
 Los **dominios** son fundamentales para garantizar la integridad de los datos desde su raíz, ya que definen las reglas que deben cumplir los valores de una columna antes siquiera de insertarlos en una tabla.

Un **dominio** es un tipo de datos definido por el usuario que puede incluir restricciones de integridad (cláusulas `CHECK`), valores por defecto y reglas de nulidad. Su gran ventaja es la **reutilización**: una vez definido, puedes aplicarlo a múltiples columnas en diferentes tablas, asegurando que todas sigan la misma lógica de negocio.

#### A. Creación (CREATE DOMAIN)

La sentencia de creación establece el tipo de dato base y las reglas que lo gobiernan.

```sql
-- Sintaxis estándar SQL-92
CREATE DOMAIN <nombre_dominio> [AS] <tipo_base>
  [DEFAULT <valor_defecto>]
  [CHECK (<condicion>)];

-- Ejemplo: Dominio para correos electrónicos con validación básica
CREATE DOMAIN d_email AS VARCHAR(100)
  CHECK (VALUE LIKE '%@%.%');

-- Ejemplo: Dominio para calificaciones escolares
CREATE DOMAIN d_nota AS DECIMAL(4,2)
  DEFAULT 0.00
  CHECK (VALUE BETWEEN 0 AND 10);

```

#### B. Modificación (ALTER DOMAIN)

El diseño físico es evolutivo. La sentencia `ALTER DOMAIN` permite ajustar las reglas de validación o cambiar valores por defecto sin necesidad de reconstruir las tablas que utilizan dicho dominio.

* **Cambiar el valor por defecto:** `ALTER DOMAIN d_nota SET DEFAULT 5.0;`
* **Eliminar el valor por defecto:** `ALTER DOMAIN d_nota DROP DEFAULT;`
* **Añadir una nueva restricción:** `ALTER DOMAIN d_email ADD CONSTRAINT email_min_length CHECK (LENGTH(VALUE) > 5);`
* **Eliminar una restricción:** `ALTER DOMAIN d_email DROP CONSTRAINT email_min_length;`

#### C. Borrado (DROP DOMAIN)

Elimina la definición del dominio del esquema. Al igual que con los esquemas, el estándar define dos comportamientos:

1. **RESTRICT:** Solo permite el borrado si el dominio no está siendo utilizado por ninguna columna de ninguna tabla.
2. **CASCADE:** Elimina el dominio y, en las columnas que lo usaban, sustituye el dominio por el tipo de datos base subyacente, manteniendo la integridad de los datos existentes pero perdiendo las reglas del dominio.

```sql
DROP DOMAIN d_email CASCADE;

```

Es importante comprender que no todos los motores implementan esta parte del estándar SQL:

* **PostgreSQL y Firebird:** Soportan plenamente `CREATE DOMAIN`. Son los motores ideales para enseñar este concepto de forma pura.
* **MySQL:** **No soporta** dominios. Para lograr un efecto similar, el diseñador debe definir manualmente el tipo de dato y la restricción `CHECK` en cada columna de cada tabla.
* **Oracle:** No utiliza la sintaxis `CREATE DOMAIN`. En su lugar, emplea `CREATE TYPE` para crear tipos de datos de objeto, lo cual es un concepto más avanzado de la base de datos objeto-relacional.

---

**🔗 Recursos de interés.**

* **[PostgreSQL Documentation: CREATE DOMAIN](https://www.postgresql.org/docs/current/sql-createdomain.html){target=blank}:** Referencia completa sobre la creación y gestión de dominios.
* **[Firebird SQL: Working with Domains](https://www.firebirdsql.org/refdocs/langrefupd15-create-domain.html){target=blank}:** Guía práctica de uso en Firebird.
* **[SQL Standard Check Constraints](https://www.w3schools.com/sql/sql_check.asp){target=blank}:** Cómo emular dominios en sistemas como MySQL mediante restricciones de columna.

---

### 4.3 Tablas.

La tabla es el objeto central del esquema físico donde se almacenan los datos. Su definición establece la estructura de filas y columnas, así como las reglas de integridad que el SGBD debe validar automáticamente.

En el diseño de tablas, existen dos mecanismos clave para simplificar la inserción de datos y evitar errores de nulidad:

1. **Columnas Autoincrementales:** Se utilizan principalmente para las Claves Primarias. Permiten que el sistema genere automáticamente un número único y correlativo cada vez que se inserta un nuevo registro.  
   En **MySQL**, se utiliza la palabra clave `AUTO_INCREMENT`.  
   En **Oracle**, se emplea la cláusula `GENERATED AS IDENTITY`.

1. **Valores por Defecto (`DEFAULT`):** Esta cláusula permite definir un valor que se asignará automáticamente a una columna si el usuario no especifica uno durante la inserción.


#### A. Creación (CREATE TABLE)

Define el nombre de la tabla, sus columnas, tipos de datos y restricciones iniciales.

**Sintaxis General:**

```sql
CREATE TABLE <nombre_tabla> (
    <columna1> <tipo_dato> [DEFAULT <valor>] [restricciones_columna],
    <columna2> <tipo_dato> [DEFAULT <valor>] [restricciones_columna],
    ...
    [CONSTRAINT <nombre_restriccion> <tipo_restriccion> (<columnas>)]
);

```

**Ejemplo práctico:**

```sql
CREATE TABLE Libros (
    isbn VARCHAR(13) PRIMARY KEY,
    titulo VARCHAR(200) NOT NULL,
    precio DECIMAL(6,2) CHECK (precio > 0),
    id_editorial INT
);

```

#### B. Modificación (ALTER TABLE)

Permite modificar la estructura física de la tabla sin perder los datos almacenados.

**Sintaxis General:**

```sql
ALTER TABLE <nombre_tabla>
    [ADD <columna> <tipo_dato> [restricciones]]
    [DROP COLUMN <columna>]
    [MODIFY | ALTER COLUMN <columna> <nuevo_tipo>]
    [ADD CONSTRAINT <nombre_restriccion> <definicion_restriccion>];

```

**Ejemplos de uso:**

* **Añadir una columna:** `ALTER TABLE Libros ADD num_paginas INT;`
* **Eliminar una columna:** `ALTER TABLE Libros DROP COLUMN id_editorial;`
* **Cambiar un tipo de dato (PostgreSQL):** `ALTER TABLE Libros ALTER COLUMN titulo TYPE VARCHAR(300);`
* **Añadir una clave foránea:** `ALTER TABLE Libros ADD CONSTRAINT fk_edit FOREIGN KEY (id_editorial) REFERENCES Editoriales(id);`

#### C. Borrado (DROP TABLE)

Elimina de forma permanente la tabla y todos los registros que contiene del soporte físico.

**Sintaxis General:**

```sql
DROP TABLE <nombre_tabla> [RESTRICT | CASCADE];

```

**Opciones de borrado:**

* **RESTRICT (Valor por defecto):** No permite borrar la tabla si hay otros objetos (como vistas o claves foráneas en otras tablas) que dependan de ella.
* **CASCADE:** Borra la tabla y elimina automáticamente cualquier restricción o relación de integridad que dependa de ella.

**Ejemplos:**

* `DROP TABLE Libros;` (Borrado simple).
* `DROP TABLE Libros CASCADE;` (Borrado forzado eliminando dependencias).

#### D. Sentencia TRUNCATE.

La sentencia `TRUNCATE` elimina todos los registros de una tabla manteniendo su estructura. Se considera una sentencia DDL ya que opera directamente sobre la estructura física de la tabla. 

```sql
TRUNCATE TABLE nombre_tabla;
```

> **Dato importante:** Si solo deseas vaciar los datos pero mantener la tabla para volver a usarla, utiliza `TRUNCATE TABLE <nombre_tabla>;`, que es mucho más eficiente que borrar y recrear.

### 4.4 Gestión de Restricciones (CONSTRAINTS)

Las restricciones son reglas que el SGBD aplica a las columnas o tablas para asegurar la integridad de los datos. Aunque pueden definirse durante la creación de la tabla, suelen gestionarse de forma independiente para facilitar el mantenimiento del diseño físico.

Las restricciones pueden definirse a dos niveles:

1. **Nivel de Columna:** Se definen junto al tipo de dato (ej. `NOT NULL`, `UNIQUE`).
2. **Nivel de Tabla:** Se definen al final de la tabla y pueden involucrar varias columnas (ej. `PRIMARY KEY` compuesta). **Si la restricción afecta a más de una columna (es compuesta), obligatoriamente debe ser a nivel de tabla.**

Se pueden distinguir los siguientes tipos principales:

* **PRIMARY KEY:** Identificador único (no nulo, no repetido).
* **FOREIGN KEY:** Mantiene la integridad referencial entre tablas, permitiendo acciones como `ON DELETE CASCADE` o `ON UPDATE SET NULL`.
* **NOT NULL:** Es la única restricción que solo puede definirse a nivel de columna. Obliga a que el campo tenga siempre un valor, prohibiendo los vacíos legales en datos críticos como nombres o fechas de registro.

* **UNIQUE:** Garantiza que no existan valores duplicados en una columna (o conjunto de ellas), pero a diferencia de la PRIMARY KEY, sí permite valores nulos (dependiendo del motor). Es ideal para datos alternativos de identificación como el DNI, el pasaporte o el correo electrónico.
* **CHECK:** Valida condiciones lógicas (ej. `sueldo > 1000`).

#### A. Creación de Restricciones 

**Restricciones de Clave Primaria (PRIMARY KEY)**

Identifica de forma única cada registro.

* **A nivel de columna:** Directo y sencillo para claves de un solo atributo.
* **A nivel de tabla:** Permite definir claves compuestas (varias columnas) y asignar un nombre a la restricción.

```sql
-- NIVEL DE COLUMNA
CREATE TABLE Autores (
    id_autor INT PRIMARY KEY, -- Clave simple
    nombre VARCHAR(100)
);

-- NIVEL DE TABLA (Recomendado para claves compuestas)
CREATE TABLE Ejemplares (
    id_libro INT,
    numero_copia INT,
    CONSTRAINT pk_ejemplar PRIMARY KEY (id_libro, numero_copia)
);

```

**Restricciones de Clave Foránea (FOREIGN KEY)**

Establece el vínculo de integridad referencial con otra tabla.

* **A nivel de columna:** Se usa la palabra clave `REFERENCES`. No permite definir acciones complejas en todos los motores.
* **A nivel de tabla:** Es la forma estándar y completa, permitiendo definir qué ocurre al borrar o actualizar (`CASCADE`, `SET NULL`, etc.).

```sql
-- NIVEL DE COLUMNA
CREATE TABLE Libros (
    isbn VARCHAR(13) PRIMARY KEY,
    id_editorial INT REFERENCES Editoriales(id) -- Relación simple
);

-- NIVEL DE TABLA (Con integridad referencial avanzada)
CREATE TABLE Prestamos (
    id_prestamo INT PRIMARY KEY,
    id_socio INT,
    CONSTRAINT fk_socio_prestamo 
        FOREIGN KEY (id_socio) REFERENCES Socios(id_socio)
        ON DELETE CASCADE -- Si se borra el socio, se borran sus préstamos
);

```
**Restricciones NOT NULL/UNIQUE**

```sql
-- NIVEL DE COLUMNA
CREATE TABLE Usuarios (
    id_usuario INT PRIMARY KEY,
    email VARCHAR(100) NOT NULL UNIQUE, -- Obligatorio y no repetido
    telefono VARCHAR(20) UNIQUE         -- No puede repetirse, pero puede ser nulo
);

-- NIVEL DE TABLA (Unique compuesto)
-- Ejemplo: Un socio no puede reservar la misma pista a la misma hora
CREATE TABLE Reservas (
    id_reserva INT PRIMARY KEY,
    id_pista INT,
    fecha_hora TIMESTAMP,
    CONSTRAINT uq_pista_hora UNIQUE (id_pista, fecha_hora)
);
```
**Restricciones de Verificación (CHECK)**

Valida que los datos cumplan una condición booleana.

* **A nivel de columna:** La condición solo puede referirse a la columna actual.
* **A nivel de tabla:** Puede comparar varias columnas de la misma fila entre sí.

```sql
-- NIVEL DE COLUMNA
CREATE TABLE Productos (
    id_prod INT PRIMARY KEY,
    precio DECIMAL(10,2) CHECK (precio > 0) -- El precio debe ser positivo
);

-- NIVEL DE TABLA (Comparación entre columnas)
CREATE TABLE Ofertas (
    id_oferta INT PRIMARY KEY,
    precio_original DECIMAL(10,2),
    precio_descuento DECIMAL(10,2),
    -- El descuento no puede ser mayor que el precio original
    CONSTRAINT ck_descuento_logico CHECK (precio_descuento < precio_original)
);

```

#### B. Modificación de Restricciones (ALTER TABLE)

No se puede "editar" una restricción directamente. En SQL, modificar una restricción significa **añadir una nueva** o **sustituir la anterior**.

**Sintaxis General:**  

`ALTER TABLE <nombre_tabla> ADD CONSTRAINT <nombre_regla> <definicion>;`

**Ejemplos prácticos:**

1. **Añadir una restricción de valor (CHECK):**
Queremos asegurar que el stock de la tabla `Productos` nunca sea negativo tras haber creado la tabla.
```sql
ALTER TABLE Productos 
ADD CONSTRAINT ck_stock_positivo CHECK (stock >= 0);

```


2. **Añadir una Clave Foránea a una tabla ya existente:**
```sql
ALTER TABLE Empleados 
ADD CONSTRAINT fk_departamento 
FOREIGN KEY (id_dept) REFERENCES Departamentos(id);

```

**C. Borrado de Restricciones (DROP CONSTRAINT)**

El borrado es esencial cuando las reglas de negocio cambian o cuando necesitamos realizar una carga masiva de datos y queremos desactivar las comprobaciones temporalmente.

**Sintaxis General:**  

`ALTER TABLE <nombre_tabla> DROP CONSTRAINT <nombre_regla>;`

*Nota: En MySQL, para claves foráneas se usa `DROP FOREIGN KEY <nombre>`, y para la clave primaria `DROP PRIMARY KEY`.*

**Ejemplos prácticos:**

1. **Eliminar una restricción de chequeo:**
Si la empresa decide que ahora puede tener stock negativo (por preventas), borramos la regla:
```sql
ALTER TABLE Productos DROP CONSTRAINT ck_stock_positivo;

```


2. **Eliminar una Clave Foránea:**
```sql
ALTER TABLE Empleados DROP CONSTRAINT fk_departamento;
```
---

### 4.5 Aserciones.
Para cerrar los elementos del esquema relacional en SQL-92, abordaremos las **aserciones**. Como hemos comentado, son el mecanismo de integridad más potente, ya que no se limitan a una sola tabla, sino que supervisan el estado global de la base de datos.  

Una aserción es un objeto del esquema que define una condición de integridad que debe cumplirse siempre en todo el sistema. A diferencia de las restricciones de tabla (`CHECK`), las aserciones se utilizan para reglas de negocio que involucran múltiples tablas.

Algunos SGBD que intentan aproximarse al estándar permiten desactivarlas temporalmente, pero no es una función universal.

Es fundamental recalcar un punto que mencionamos anteriormente pero que debe quedar registrado en este bloque:

* **Soporte Real:** Aunque es un elemento clave de la teoría de bases de datos relacionales (SQL-92), motores como **MySQL, Oracle y PostgreSQL no las soportan** directamente debido a su complejidad de cómputo.
* **La Alternativa Práctica:** En el mundo profesional, cuando un diseñador necesita implementar lo que sería una aserción, recurre a los **Triggers** (disparadores). Un trigger se programa para ejecutarse antes de un `INSERT` o `UPDATE` y realizar la comprobación manualmente, lanzando un error si la condición no se cumple.

#### A. Creación (CREATE ASSERTION)

Define la regla lógica que la base de datos debe validar tras cada transacción.

**Sintaxis General:**

```sql
CREATE ASSERTION <nombre_asercion>
    CHECK (<condicion_logica>);

```

Para justificar el uso de una **Aserción**, debemos buscar un caso donde la regla sea **global** y no dependa de una acción individual, sino del **estado total de la base de datos**.

Imagina una biblioteca que tiene una política de diversidad: *"El número total de libros de 'Ciencia Ficción' no puede superar nunca el 50% del inventario total de la biblioteca"*. Esta regla no se puede poner en un `CHECK` de la tabla `Libros`, porque para saber si puedes insertar **un** libro nuevo de Ciencia Ficción, necesitas saber cuántos hay en total de **todas** las demás categorías.

```sql
CREATE ASSERTION limite_diversidad_genero
CHECK (
    (SELECT COUNT(*) FROM Libros WHERE genero = 'Ciencia Ficcion') 
    <= 
    (SELECT COUNT(*) FROM Libros) * 0.5
);

```

#### B. Modificación (ALTER ASSERTION)

El estándar **no contempla** `ALTER ASSERTION`.  

Si la biblioteca cambia su política y ahora permite un 60%, el proceso físico sería:

```sql
-- 1. Eliminamos la vigilancia anterior
DROP ASSERTION limite_diversidad_genero;

-- 2. Creamos la nueva regla con el 60% (0.6)
CREATE ASSERTION limite_diversidad_genero
CHECK (
    (SELECT COUNT(*) FROM Libros WHERE genero = 'Ciencia Ficcion') 
    <= 
    (SELECT COUNT(*) FROM Libros) * 0.6
);

```

#### C. Borrado (DROP ASSERTION)

```sql
DROP ASSERTION <nombre_asercion>;

```

Si la biblioteca decide eliminar esta restricción de diversidad:

```sql
DROP ASSERTION limite_diversidad_genero;

```

### 4.6 Vistas

Una vista es un objeto del esquema que permite presentar los datos de una o varias tablas de forma personalizada. 

**A. Creación (CREATE VIEW)**

Al crear una vista, definimos un nombre para el objeto y la consulta que lo genera.

**Sintaxis General:**

```sql
CREATE VIEW <nombre_v_ista> [(<alias_columnas>)]
AS <consulta_select>
[WITH CHECK OPTION];

```
**La cláusula WITH CHECK OPTION** asegura que los datos añadidos a través de la vista cumplan con el `WHERE` de la misma.

**Ejemplos prácticos:**

1. **Vista de Simplificación:** Ocultamos la complejidad de un `JOIN` para que el usuario solo vea lo necesario.
```sql
CREATE VIEW v_libros_editoriales AS
SELECT L.titulo, E.nombre_editorial, L.precio
FROM Libros L
JOIN Editoriales E ON L.id_editorial = E.id;

```


2. **Vista de Seguridad:** Mostramos solo los empleados de un departamento específico.
```sql
CREATE VIEW v_nomina_informatica AS
SELECT nombre, puesto 
FROM Empleados
WHERE departamento = 'Informatica';

```



**B. Modificación (CREATE OR REPLACE VIEW)**

En la mayoría de los SGBD, para modificar una vista no se utiliza el comando `ALTER`, sino que se "sobrescribe" la definición existente.

**Sintaxis General:**

```sql
CREATE OR REPLACE VIEW <nombre_vista> AS
<nueva_consulta_select>;

```

**Ejemplo:**
Si queremos añadir el ISBN a nuestra vista anterior:

```sql
CREATE OR REPLACE VIEW v_libros_editoriales AS
SELECT L.isbn, L.titulo, E.nombre_editorial, L.precio
FROM Libros L
JOIN Editoriales E ON L.id_editorial = E.id;

```

**C. Borrado (DROP VIEW)**

Elimina la definición de la vista del esquema. Es importante recordar que **borrar una vista no borra los datos** de las tablas originales, solo elimina el acceso virtual.

**Sintaxis General:**

```sql
DROP VIEW <nombre_vista> [CASCADE | RESTRICT];

```

**Ejemplo:**

```sql
DROP VIEW v_nomina_informatica;

```

---

#### Ventajas del uso de Vistas en el Diseño Físico

* **Seguridad:** Permite mostrar a un usuario ciertas columnas de una tabla pero ocultar otras sensibles (como sueldos o contraseñas).
* **Independencia de datos:** Si la estructura física de las tablas cambia (por ejemplo, dividimos una tabla en dos), podemos modificar la vista para que las aplicaciones sigan viendo los datos igual, sin tener que reprogramarlas.
* **Simplicidad:** Los usuarios no necesitan saber hacer `JOINs` complejos; simplemente consultan la vista como si fuera una tabla normal: `SELECT * FROM v_libros_editoriales;`.


### 4.7 Índices. 

Los **índices** son estructuras de datos adicionales que se crean sobre las tablas para mejorar la velocidad de recuperación de la información. En el diseño físico, representan el equilibrio entre **velocidad de lectura** (consultas más rápidas) y **velocidad de escritura** (inserciones más lentas, ya que hay que actualizar el índice).

Un índice funciona como el índice alfabético de un libro: permite al SGBD encontrar una fila sin tener que leer toda la tabla.

**A. Creación (CREATE INDEX)**

Los índices se crean sobre una o varias columnas que se utilicen frecuentemente en las cláusulas `WHERE`, `JOIN` o `ORDER BY`.

**Sintaxis General:**

```sql
CREATE [UNIQUE] INDEX <nombre_indice>
ON <nombre_tabla> (<columna1> [ASC | DESC], ...);

```

**Ejemplos prácticos:**

1. **Índice simple:** Para acelerar la búsqueda de libros por su título.
```sql
CREATE INDEX idx_titulo ON Libros(titulo);

```


2. **Índice único:** Asegura que no haya valores duplicados (el sistema lo crea automáticamente para la `PRIMARY KEY`).
```sql
CREATE UNIQUE INDEX idx_dni_socio ON Socios(dni);

```
3. **Índice compuesto:** Útil si buscamos frecuentemente por apellidos y nombre combinados.
```sql
CREATE INDEX idx_nombre_completo ON Socios(apellido1, nombre);

```

**B. Modificación (ALTER INDEX / REBUILD)**

A diferencia de las tablas, los índices no suelen "modificarse" para cambiar sus columnas. Si quieres cambiar las columnas de un índice, debes borrarlo y crearlo de nuevo. Sin embargo, en entornos profesionales existen comandos para **reorganizar** el índice si se ha fragmentado.

**Sintaxis (Depende del SGBD):**

* **Oracle / SQL Server (Reconstrucción):**
```sql
ALTER INDEX idx_titulo REBUILD;

```

* **PostgreSQL (Reindexar):**
```sql
REINDEX INDEX idx_titulo;

```

* **MySQL:** No tiene un `ALTER INDEX` directo para cambiar la estructura; se usa `DROP` y `ADD`.


**C. Borrado (DROP INDEX)**

Eliminar un índice libera espacio en disco y acelera las operaciones de `INSERT` y `UPDATE` sobre esa tabla.

**Sintaxis General:**

```sql
-- Estándar y PostgreSQL
DROP INDEX <nombre_indice>;

-- MySQL (requiere especificar la tabla)
ALTER TABLE <nombre_tabla> DROP INDEX <nombre_indice>;

-- SQL Server
DROP INDEX <nombre_indice> ON <nombre_tabla>;

```

**Ejemplo:**

```sql
DROP INDEX idx_titulo;

```
Importacia de los índices.

- Mejoran el rendimiento de las consultas.
- Son especialmente útiles en columnas utilizadas en condiciones y relaciones.
- Incrementan el coste de las operaciones de modificación de datos.

A nivel de diseño físico podemos señalar algunas de las situaciones en las que **no** conviene poner un índice:

* **Tablas muy pequeñas:** El SGBD tarda más en leer el índice que en leer toda la tabla.
* **Columnas con poca diversidad:** (Ej. una columna "Sexo" con solo dos valores). El índice no ayuda a filtrar eficazmente.
* **Tablas con inserciones constantes:** Cada vez que haces un `INSERT`, el SGBD tiene que detenerse a ordenar el índice, lo que penaliza el rendimiento.

---
## 5. Lenguaje de Control de Datos (DCL)

El **Lenguaje de Control de Datos (DCL)** es la parte de SQL encargada de la seguridad y la integridad de la base de datos. Su función principal es controlar el acceso a los datos mediante la gestión de identidades y la asignación de permisos.

En un entorno profesional, el acceso no se otorga de manera indiscriminada; se basa en el **Principio de Menor Privilegio**, que dicta que cada usuario debe tener únicamente los permisos mínimos necesarios para realizar su trabajo.

### 5.1. Identidad en la Base de Datos: Usuarios.

Antes de manejar cuentas de usuarios y asignar permisos, es necesario entender qué tipos de identidades existen en el SGBDR utilizado.

#### Cuentas Administrativas (Superusuarios)

Todos los SGBDR disponen de cuentas preconfiguradas con permisos totales sobre el sistema y cuyas credenciales se establecen en el proceso de instalación.

* **En MySQL (`root`):** Es el administrador único y global. Tiene control total sobre todas las bases de datos del servidor. Por seguridad, suele limitarse su acceso únicamente desde la propia máquina (`localhost`).
* **En Oracle (`SYS` y `SYSTEM`):**
* **`SYS`:** Es el usuario más poderoso; dueño del diccionario de datos (núcleo de la base de datos). Para usarlo, se requiere el privilegio **`SYSDBA`**, que permite apagar, encender y reparar la instancia.
* **`SYSTEM`:** Es el administrador de alto nivel para tareas rutinarias (crear usuarios, tablas, etc.), pero no es el dueño del núcleo del sistema como `SYS`.



#### Usuarios Comunes y Modelos de Trabajo.

Tras la instalación y configuración inicial del sistema, el Administrador de la Base de Datos (DBA) debe actuar como autoridad central para gestionar el acceso, procediendo a la creación de cuentas de usuario independientes que identifiquen de forma unívoca tanto a personas físicas como a aplicaciones externas; este proceso garantiza la trazabilidad de las operaciones y permite establecer un perímetro de seguridad basado en la autenticación antes de delegar cualquier privilegio operativo.

* **MySQL (Modelo de Conexión):** El usuario se define como `nombre@host`. La seguridad depende de *quién* es y *desde dónde* viene.
* **Oracle (Modelo de Esquema):** El usuario es un "Esquema". Al crearlo, se le asigna un **Tablespace** (espacio físico en disco) y una **Cuota**. En Oracle, el usuario es dueño de su propio espacio de la base de datos.

#### A. Creación (CREATE USER)
Para que la infraestructura de seguridad sea operativa, la simple creación de una cuenta de usuario mediante DCL es insuficiente, ya que por defecto carece de facultades de acceso; por tanto, es imperativo realizar una concesión explícita de privilegios a través de la sentencia GRANT, vinculando así la identidad del usuario con los permisos específicos (como CONNECT, SELECT o INSERT) sobre los objetos del esquema.


```sql
-- Sintaxis general
CREATE USER '<nombre_usuario>' IDENTIFIED BY '<contraseña>';

-- Ejemplo
CREATE USER 'ana_bibliotecaria' IDENTIFIED BY 'Libro.2024';

```


#### B. Modificación (ALTER USER)

Podemos modificar las propiedades de un usuario.   Generalmente se usa para cambiar contraseñas o bloquear cuentas.

```sql
-- Ejemplo: Cambio de contraseña
ALTER USER 'ana_bibliotecaria' IDENTIFIED BY 'Nueva.Clave.2025';

```


#### C. Borrado (DROP USER)

Elimina por completo la cuenta del sistema.

```sql
-- Sintaxis general
DROP USER '<nombre_usuario>';

-- Ejemplo
DROP USER 'ana_bibliotecaria';

```

Tabla resumen:  

| Operación | MySQL | Oracle |
| --- | --- | --- |
| **Crear** | `CREATE USER 'ana'@'%' IDENTIFIED BY 'Pass123';` | `CREATE USER ana IDENTIFIED BY Pass123 DEFAULT TABLESPACE users;` |
| **Cambiar Pass** | `ALTER USER 'ana'@'%' IDENTIFIED BY 'New987';` | `ALTER USER ana IDENTIFIED BY New987;` |
| **Bloquear** | `ALTER USER 'ana'@'%' ACCOUNT LOCK;` | `ALTER USER ana ACCOUNT LOCK;` |
| **Eliminar** | `DROP USER 'ana'@'%';` | `DROP USER ana CASCADE;` |

---

### 5.2. Control de Acceso: Privilegios y Roles

Un usuario creado no tiene poder alguno hasta que se le conceden privilegios. Estos se dividen en dos grandes grupos:

1. **Privilegios de Sistema:** Permisos para hacer cosas "fuera" de las tablas (ej: `CREATE TABLE`, `CREATE SESSION`, `CREATE USER`).
2. **Privilegios de Objeto:** Permisos para interactuar con los datos "dentro" de las tablas (ej: `SELECT`, `INSERT`, `UPDATE`, `DELETE`).

#### 5.2.1 Privilegios.Comandos  GRANT y REVOKE

La gestión de permisos no solo consiste en dar o quitar permisos; es un proceso dinámico que debe ser auditado constantemente.

#### A. Concesión de Privilegios (GRANT):
Permite especificar qué operaciones (`SELECT`, `INSERT`, `UPDATE`, `DELETE`) puede hacer el usuario y sobre qué objetos.

```sql
-- Sintaxis general
GRANT <privilegios> ON <objeto> TO <usuario>;
```

**MySQL** Destaca por permitir permisos a niveles muy específicos: servidor, base de datos, tabla o incluso columnas. (Granularidad)

```sql
-- Otorgar permiso de lectura sobre una tabla específica
GRANT SELECT ON tienda.productos TO 'ana'@'%';

-- Aplicar los cambios en los privilegios de forma inmediata
FLUSH PRIVILEGES;
```
Observa el uso de la sentencia `FLUSH PRIVILEGES`para aplicar los permisos de forma inmediata.

**Oracle** simplifica la entrada de nuevos usuarios mediante la utilización de roles predefinidos muy potentes. Un rol actúa como contenedor de privilegios.
```sql
-- Otorgar roles básicos para que un usuario pueda trabajar
GRANT CONNECT, RESOURCE TO ana;

```
>`CONNECT` permite iniciar sesión (`CREATE SESSION`) y `RESOURCE` permite crear sus propios objetos (tablas, secuencias) en su esquema.  

La **ampliación de permisos**
se consigue ejecutando de nuevo `GRANT`. Los permisos son aditivos.

```sql
-- Ahora Ana también puede modificar libros existentes
GRANT UPDATE ON Libros TO 'ana_bibliotecaria';

```
#### B. Retirada de privilegios (REVOKE)

Se utiliza para eliminar un permiso que fue otorgado previamente. Es vital entender que `REVOKE` no elimina al usuario, solo restringe sus acciones.  
Sintaxis básica:
```sql
-- Sintaxis general
REVOKE <privilegios> ON <objeto> FROM <usuario>;
```
Ejemplos:
```sql
-- Retira el permiso de borrar datos de una tabla
REVOKE DELETE ON tienda.pedidos FROM 'ana'@'%';
```


```sql
-- Quita todos los permisos de creación de objetos pero mantiene la conexión
REVOKE RESOURCE FROM ana;
```

```sql

--Quitamos el permiso de inserción
REVOKE INSERT ON Libros FROM 'ana_bibliotecaria';

```

#### C. Cláusulas Especiales de Delegación

Existen opciones que permiten a un usuario convertirse en "sub-administrador":

* **`WITH GRANT OPTION` (MySQL/Oracle):** Si le das un permiso a Ana con esta cláusula, Ana podrá darle ese mismo permiso a otros compañeros.
* **`WITH ADMIN OPTION` (Oracle):** Se usa para roles de sistema. Permite al usuario otorgar o revocar ese rol a otros.

### 5.3 Comprobación de Privilegios (Auditoría)

Una parte esencial del DCL es verificar qué se ha concedido. No se puede controlar lo que no se puede ver:

**En MySQL:**
```sql
SHOW GRANTS FOR 'ana'@'%';

```


**En Oracle:**
```sql
SELECT * FROM USER_SYS_PRIVS; -- Privilegios de sistema del usuario actual
SELECT * FROM USER_TAB_PRIVS; -- Privilegios sobre tablas

```
### 5.4 Seguridad Granular. Privilegios a nivel de columna.

En ocasiones, el principio de menor privilegio exige que un usuario pueda ver una tabla, pero no todos sus datos. Por ejemplo, en una tabla de `EMPLEADOS`, un administrativo de RRHH debe ver el nombre y el teléfono, pero **no el salario**.

**En MySQL**

Es uno de los sistemas que permite definir privilegios directamente sobre columnas específicas en el comando `GRANT`.

```sql
-- El usuario solo podrá ver el ID y el Nombre, pero no el sueldo
GRANT SELECT (id_empleado, nombre) ON empresa.empleados TO 'administrativo'@'%';
```

```sql
-- El usuario podrá actualizar el teléfono, pero nada más
GRANT UPDATE (telefono) ON empresa.empleados TO 'administrativo'@'%';
```


**En Oracle**

Oracle utiliza un enfoque más avanzado llamado **VPD (Virtual Private Database)** o mediante la creación de **Vistas**. 
El método estándar para lograr granularidad sin licencias adicionales es mediante vistas.

```sql
-- 1. Creamos una vista que oculte la columna sensible
CREATE VIEW vista_empleados_publica AS 
SELECT id_empleado, nombre, departamento FROM esquema_rrhh.empleados;
```

```sql
-- 2. Damos permiso sobre la vista, no sobre la tabla original
GRANT SELECT ON vista_empleados_publica TO usuario_consulta;

```

---




#### 5.2.2 Roles.

Para evitar el caos de asignar permisos uno a uno, el DCL utiliza **Roles**. Un Rol es un "contenedor" de privilegios.  

En diseños físicos complejos con cientos de usuarios, no se dan permisos uno a uno. Se crean **Roles** (perfiles), se les asignan permisos al rol, y luego se asigna el usuario al rol.  

La gestión de **Roles** es una extensión del lenguaje **DCL** que permite agrupar privilegios para facilitar la administración.

Sintaxis básica:

```sql
CREATE ROLE <nombre_del_rol>;

```
En un entorno real, primero creamos el rol, le asignamos permisos y luego vinculamos ese rol a un usuario concreto:

```sql
-- 1. Creamos el rol para el personal de inventario
CREATE ROLE rol_inventariado;

-- 2. Asignamos privilegios al rol (no al usuario directamente)
GRANT SELECT, UPDATE ON Libros TO rol_inventariado;

-- 3. Asignamos el rol al usuario específico
GRANT rol_inventariado TO 'ana_bibliotecaria';

```

Tabla resumen DCL:

| Acción | MySQL | Oracle |
| --- | --- | --- |
| **Identidad Administrativa** | `root` | `SYS` / `SYSTEM` |
| **Acceso como Admin** | Directo | Requiere `AS SYSDBA` |
| **Crear identidad** | `CREATE USER` | `CREATE USER` + `QUOTA` |
| **Otorgar acceso/rol** | `GRANT` | `GRANT` |
| **Retirar acceso/rol** | `REVOKE` | `REVOKE` |
| **Persistencia** | `FLUSH PRIVILEGES` | Automático (Inmediato) |
| **Ver permisos propios** | `SHOW GRANTS` | `SELECT * FROM USER_SYS_PRIVS` |

### 5.5 Buenas Prácticas.

1. **Nunca trabajar como administrador (`root`/`SYS`):** Estas cuentas son solo para configurar el entorno. Una vez creado el administrador secundario, las cuentas maestras deben guardarse bajo llave.
2. **Uso obligatorio de Roles:** No asignes permisos a personas, asígnalos a funciones (Roles). Si un empleado se va, solo quitas el rol.
3. **Auditoría periódica:** Revoca permisos de usuarios que ya no los necesiten (por ejemplo, tras un cambio de departamento).
4. **Cifrado en tránsito:** Asegúrate de que las contraseñas no viajen en texto plano configurando conexiones SSL/TLS.
5. **Políticas de complejidad:** Forzar el uso de mayúsculas, números y símbolos en el `IDENTIFIED BY`.



## 6. Errores comunes en el diseño del esquema físico.

El éxito de una base de datos no depende solo de que el código SQL se ejecute sin errores, sino de la eficiencia y robustez de su estructura. Un diseño físico deficiente puede derivar en problemas de rendimiento imposibles de solucionar únicamente mediante la potencia del hardware.

#### Deficiencias en la Identificación y Tipado.

El error más crítico es la **ausencia de claves primarias**, lo que impide garantizar la unicidad de los registros y obliga al SGBD a realizar costosos escaneos completos de tabla para cualquier búsqueda simple. A esto se suma la **elección de tipos de datos inadecuados** (por ejemplo, usar `VARCHAR` para almacenar fechas o importes monetarios); esto no solo incrementa el consumo de almacenamiento, sino que inhabilita las funciones nativas de cálculo y comparación temporal, además de permitir la entrada de datos basura (como "30 de febrero") que el motor no podrá validar automáticamente.

#### Falta de Rigor en la Integridad y Referencia.

La **permisividad excesiva con los valores nulos** es otra práctica de riesgo. No definir la restricción `NOT NULL` en columnas con carga semántica (como el nombre de un cliente o el código de un producto) genera inconsistencias en los informes y obliga a los desarrolladores a programar validaciones adicionales en la capa de aplicación. Asimismo, la existencia de **claves foráneas sin índices asociados** es la causa principal de la lentitud en las consultas que realizan uniones (*joins*), provocando bloqueos en el sistema cuando las tablas crecen en volumen.

#### Desequilibrio en la Optimización y Seguridad.

En el afán de optimizar, es común caer en la **creación indiscriminada de índices**. Aunque los índices aceleran las lecturas, cada índice adicional penaliza la velocidad de las operaciones `INSERT`, `UPDATE` y `DELETE`, ya que el SGBD debe actualizar todas las estructuras de índices tras cada cambio. Finalmente, en el plano de la administración, el error más grave es la **asignación de privilegios excesivos**. Ignorar el principio de "mínimo privilegio" y conceder permisos de administrador a usuarios o aplicaciones genéricas crea una vulnerabilidad crítica, permitiendo que un error de programación o un acceso no autorizado provoque la pérdida estructural de datos o la violación de la privacidad.

## 7. Herramientas.
Para que la administración de una base de datos sea eficiente y segura, es esencial conocer el software que permite ejecutar los comandos de **DCL** (Data Control Language) de forma visual. Un administrador no siempre opera desde la consola; las herramientas gráficas o **GUI** (Graphic User Interface) facilitan la gestión de usuarios, roles y privilegios mediante interfaces intuitivas de "clic y arrastrar", reduciendo el error humano en la sintaxis.


#### 1.- MySQL Workbench (MySQL)

Es la herramienta oficial desarrollada por Oracle para la gestión de MySQL. Su módulo de **"Users and Privileges"** es el estándar de la industria para este motor.

* **Gestión Visual de Host:** Permite definir de forma gráfica si un usuario se conecta desde el `localhost`, una IP específica o cualquier origen (`%`).
* **Roles Administrativos:** Incluye perfiles predefinidos como `DBManager`, `BackupAdmin` o `SecurityAdmin`. Basta con marcar una casilla para asignar un conjunto complejo de privilegios.
* **Matriz de Privilegios:** Ofrece una tabla visual donde se puede marcar exactamente qué acciones (SELECT, INSERT, etc.) tiene permitido el usuario sobre cada esquema o tabla.

#### 2.- Oracle SQL Developer (Oracle)

Es la herramienta gratuita de referencia para la administración de bases de datos Oracle.

* **Nodos de Seguridad:** Dentro del árbol de conexiones, centraliza la gestión en los nodos **"Otros usuarios"** y **"Roles"**.
* **Asistente de Esquemas:** Facilita la configuración de conceptos críticos de Oracle como los **Tablespaces** (almacenamiento) y las **Cuotas** de disco mediante pestañas, evitando errores en el comando `CREATE USER`.
* **Editor de Roles:** Permite arrastrar privilegios de sistema hacia un Rol y luego asignar dicho Rol a múltiples usuarios simultáneamente.

#### 3.- Visual Studio Code + Extensiones (Entorno Híbrido)

Aunque nació como un editor de código, su ecosistema de extensiones lo ha convertido en una herramienta de administración de bases de datos extremadamente ligera y potente.  

Algunas de las extensiones más importantes para la gestión de bases de datos permiten integrar los principales motores del mercado directamente en el flujo de trabajo del editor, clasificándose principalmente en:

**Extensiones Oficiales (Soporte Nativo):**  
  
  * **PostgreSQL:** Ofrece un explorador de objetos intuitivo, autocompletado inteligente y soporte completo para la ejecución de consultas y gestión de esquemas.
  * **Oracle SQL Developer:** La extensión oficial que traslada las funcionalidades de administración, gestión de conexiones y depuración de PL/SQL al entorno de VS Code.
  * **MySQL Shell for VS Code:** Herramienta avanzada desarrollada por el equipo de MySQL que incluye soporte para cuadernos interactivos (Notebooks) y una consola de administración optimizada.

**Extensiones Universales (Multi-motor):**  

  * **Database Client:** Una de las soluciones más completas y visuales. Destaca por su capacidad para mostrar diagramas ER, editar datos en formato tabla (estilo hoja de cálculo) y su compatibilidad con casi cualquier motor (MySQL, PostgreSQL, Redis, MongoDB, etc.).
  * **SQLTools:** Una solución versátil y ligera que, mediante el uso de *drivers* específicos, permite gestionar múltiples motores desde una única interfaz unificada, ideal para quienes buscan un entorno más minimalista.


#### Herramientas Multiplataforma. 

En empresas que manejan distintos motores de bases de datos, se suelen utilizar herramientas "todo en uno":

* **DBeaver (Open Source):** Muy popular por su versatilidad. Permite comparar visualmente cómo se aplican los permisos en un entorno MySQL frente a uno Oracle bajo una misma interfaz.
* **DataGrip (JetBrains):** Herramienta avanzada para desarrolladores que permite auditar privilegios simplemente pasando el cursor sobre el nombre de un usuario en el código SQL.
*  **Clientes web (phpmyadmin, phppgadmin):** Herraminetas web que permiten la administración de la base de datos.

### Tabla Comparativa.

| Característica | Línea de Comandos (CLI) | Herramienta Gráfica (GUI) | VS Code + Extensiones |
| --- | --- | --- | --- |
| **Velocidad** | Lenta (manual). | Muy rápida (asistentes). | Media (Snippets y plantillas). |
| **Automatización** | Ideal (Scripts repetibles). | Baja (Procesos manuales). | **Alta (Integración con Git/CI).** |
| **Visibilidad** | Requiere comandos SHOW. | Visión global jerárquica. | Explorador lateral interactivo. |
| **Riesgo de Error** | Alto (Sintaxis manual). | Bajo (Avisos visuales). | Medio (Validación en tiempo real). |
| **Uso Recomendado** | Servidores remotos. | Auditoría y diseño inicial. | **Desarrollo y Testing de scripts.** |

## 8. Actividad de clase.

**Esquema relacional**

``` sql
`
profesores(id, nombre, email)
asignaturas(id, codigo, nombre, horas)
grupos(id, nombre, id_tutor)
imparte(id, id_profesor, id_grupo, id_asignatura)
alumnos(id, nombre, nie, id_grupo)
matricula(id, id_ialumno, id_asginatura)
notas(id, id_alumno, id_asignatura, ev1,ev2,ev3,media)`
---
```


**Oracle SQL Live**

#### Bloque 1.
1. **Tabla Profesores:** Crea la tabla `PROFESORES`. El `id` debe ser clave primaria, autoincrementable y el `email` debe ser único.
2. **Tabla Asignaturas:** Crea la tabla `ASIGNATURAS`. El `id` es PK, el `codigo` debe ser clave candidata y las `horas` deben ser mayores que cero.
3. **Tabla Grupos:** Crea la tabla `GRUPOS`. El `id` es PK. Incluye la columna `id_tutor` como Clave Foránea que apunte a `PROFESORES(id)`.
4. **Tabla Alumnos:** Crea la tabla `ALUMNOS`. El `id` es PK. El `nie` debe ser único. Incluye `id_grupo` como FK que apunte a `GRUPOS(id)`.
5. **Tabla Imparte:** Crea la tabla `IMPARTE`. El `id` es PK. Define las tres Claves Foráneas: `id_profesor`, `id_grupo` e `id_asignatura`.
6. **Tabla Matrícula:** Crea la tabla `MATRICULA`. El `id` es PK. Define las Claves Foráneas `id_alumno` e `id_asignatura`.
7. **Tabla Notas:** Crea la tabla `NOTAS`. El `id` es PK. Incluye las FK de alumno y asignatura.
8. **Restricción de Notas:** En la tabla `NOTAS`, añade una restricción `CHECK` para que `ev1`, `ev2` y `ev3` estén entre 1 y 10.
9. **Valor por Defecto:** En la tabla `NOTAS`, asegúrate de que la columna `media` tenga un valor por defecto de 0.
10. **Autoincrementales:** Utiliza la cláusula `GENERATED AS IDENTITY` para que todos los `id` de las tablas se autogeneren automáticamente.

---

#### Bloque 2: 

1. **Añadir Columna:** Usa `ALTER TABLE` para añadir la columna `fecha_alta` a la tabla `ALUMNOS`.
2. **Modificar Tipo:** Cambia la longitud de la columna `nombre` en `PROFESORES` para que acepte hasta 100 caracteres.
3. **Eliminar Columna:** Elimina la columna `codigo` de la tabla `ASIGNATURAS` (suponiendo que usaremos solo el ID).
4. **Renombrar:** Cambia el nombre de la tabla `MATRICULA` a `INSCRIPCIONES`.
5. **Añadir Restricción:** Añade un `UNIQUE` a la columna `nombre` de la tabla `GRUPOS` mediante un `ALTER`.

---

#### Bloque 3: 

1. **Borrado Simple:** Borra la tabla `NOTAS` usando el comando `DROP TABLE`.
2. **Borrado con Dependencias:** Intenta borrar la tabla `PROFESORES`. Como fallará por las relaciones, usa la cláusula `CASCADE CONSTRAINTS`.
3. **Limpieza Total:** Escribe los comandos para borrar todas las tablas en el orden inverso a su creación para evitar errores de integridad.

---

#### Bloque 4:

1. **Índices:** Crea un índice para la columna `nie` de los alumnos para que las búsquedas sean instantáneas.
2. **Vistas:** Crea una vista llamada `V_DATOS_CENTRO` que muestre el nombre del alumno y el nombre de su grupo uniendo las dos tablas.

---
**MySQL**   
Utiliza Visua Studio Code.

#### Bloque 1: Definición del Esquema (DDL en MySQL)

1. **Tablas Base:** Crea la tabla `PROFESORES` (`id` INT AUTO_INCREMENT PK, `nombre` VARCHAR(100), `email` VARCHAR(50) UNIQUE).
2. **Configuración de Asignaturas:** Crea `ASIGNATURAS` estableciendo que `horas` tenga un `CHECK (horas > 0)`.
3. **Gestión de Grupos:** Crea `GRUPOS` vinculando `id_tutor` a profesores. Añade `ON DELETE SET NULL` para que el grupo no se borre si el profesor se va.
4. **Registro de Alumnos:** Crea `ALUMNOS` con la columna `nie` como `UNIQUE`. Vincula a `GRUPOS` con `ON DELETE CASCADE`.
5. **Interrelación Imparte:** Crea `IMPARTE` con una **Clave Primaria Compuesta** por los tres IDs: `id_profesor`, `id_grupo`, e `id_asignatura`.
6. **Control de Matrículas:** Crea `MATRICULA` vinculando alumnos y asignaturas.
7. **Sistema de Calificaciones:** Crea `NOTAS` con `ev1`, `ev2`, `ev3` (DECIMAL 4,2). Añade un `CHECK` para que las notas estén entre 0 y 10.
8. **Automatismo:** En la tabla `NOTAS`, define la columna `media` con un `DEFAULT 0`.
9. **Modificación de Emergencia:** Usa `ALTER TABLE` para añadir la columna `observaciones` a la tabla `NOTAS`.
10. **Índice de Rendimiento:** Crea un **INDEX** en la tabla `ALUMNOS` para la columna `nombre`.

---

#### Bloque 2:

11. **Creación de Usuario:** Crea un usuario llamado `'administrativo'@'localhost'` con la contraseña `'PassAdmin123'`.
12. **Privilegios Globales:** Otorga al usuario administrativo permiso para hacer `SELECT`, `INSERT` y `UPDATE` en **todas** las tablas de la base de datos.
13. **Privilegios de Borrado:** Asegúrate de que el administrativo **no** pueda borrar registros (`DELETE`). Prueba a intentar un borrado con ese usuario y muestra el error.
14. **Seguridad a Nivel de Columna:** Crea un usuario `'consulta_nie'@'localhost'`. Dale permiso de `SELECT` únicamente en las columnas `nombre` y `nie` de la tabla `ALUMNOS`.
15. **Creación de Roles:** Crea un `ROLE` llamado `'tutor_role'`.
16. **Asignación al Rol:** Dale al rol permisos de `UPDATE` solo en la tabla `NOTAS`.
17. **Uso del Rol:** Crea el usuario `'profesor_01'` y asígnale el `tutor_role`.
18. **Revocación:** Quita el permiso de `INSERT` al usuario `'administrativo'` que creaste en el ejercicio 11.
19. **Auditoría de Permisos:** Ejecuta el comando `SHOW GRANTS FOR 'administrativo'@'localhost';` y explica la salida en el proyector.
20. **Restricción de Acceso:** Cambia la contraseña del usuario `'profesor_01'` desde la cuenta root.
21. **Bloqueo de Cuenta:** Bloquea la cuenta del administrativo temporalmente (`ACCOUNT LOCK`).
22. **Privilegios de Sistema:** Otorga al administrativo el permiso para crear vistas (`CREATE VIEW`).
23. **Creación de Objeto Seguro:** Crea una **VISTA** que muestre solo alumnos aprobados y dale permiso de `SELECT` sobre esa vista a un usuario nuevo llamado `'invitado'`.
24. **Limpieza de Seguridad:** Borra el rol `'tutor_role'` y observa qué ocurre con los privilegios del profesor.
25. **Eliminación Total:** Borra los usuarios `'administrativo'` e `'invitado'` del sistema.

