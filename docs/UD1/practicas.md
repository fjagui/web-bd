# Actividades. Introducción a las bases de datos.
## Actividad 1. Ejercicios de unidad.
1. Define las funciones del administrador de la base de datos.  
2. Indica las diferencias existentes entre las funciones de manipulación y de descripción.  
3. ¿Qué tipos de usuarios interaccionan con una base de datos?  
4. Indica qué es un lenguaje huésped y un lenguaje anfitrión.  
5. La gestión del espacio de almacenamiento, ¿a qué nivel de la arquitectura ANSI/SPARC pertenece? ¿Qué es la arquitectura ANSI/SPARC?
6. Dibuja un diagrama de la arquitectura de sistemas de bases de datos (ANSI/SPARC).  
7. Indica las principales funciones realizadas por el SGBD.  
8. Explica la diferencia entre la independencia física y lógica de los datos.  
9.  ¿Qué es el diccionario de datos?  
10. Diferencias entre el LDD y LMD de un sistema gestor de base de datos.  
11. Indica los componentes principales de un sistema gestor de base de datos.  
12. ¿Qué es un modelo de datos?  
13. ¿Qué son los lenguajes de cuarta generación? Pon ejemplos.  
14. Indica las principales ventajas de un sistema de bases de datos. ¿Existen algunas desventajas?
15. Dado el siguiente listado, clasifica cada elemento según corresponda a sistemas de ficheros o sistemas de bases de datos:  
    - Almacenamiento en archivos CSV
    - Oracle Database
    - MongoDB con documentos JSON
    - MySQL con tablas relacionales
    - Archivos de texto plano con datos de clientes

17. Relaciona cada tipo de base de datos con su descripción correspondiente:
    
    - Base de datos relacional  
    - Base de datos documental  
    - Base de datos de grafos  
    - Base de datos clave-valor  
    ...
    - Almacena datos como pares clave-valor, ideal para cachés  
    - Organiza datos en tablas con relaciones entre ellas  
    - Representa datos como nodos y relaciones entre ellos  
    - Almacena datos en documentos flexibles (JSON/BSON)  

18. Describe brevemente las principales diferencias entre bases de datos centralizadas y distribuidas, mencionando dos ventajas y dos desventajas de cada una.

19. Enumera cinco funciones principales de un Sistema Gestor de Bases de Datos y explique brevemente en qué consiste cada una.

20. Clasifica los siguientes SGBD según su tipo (relacional, documental, clave-valor, grafos) y su licencia (open source, comercial):

    - MySQL
    - Oracle Database
    - MongoDB
    - Redis
    - PostgreSQL
    - Neo4j

21. Analiza un caso práctico donde sería recomendable utilizar una base de datos distribuida en lugar de una centralizada, justificando su respuesta con al menos tres argumentos basados en las ventajas de este tipo de bases de datos.

22. Explica el Teorema CAP y cómo afecta a las bases de datos distribuidas, describiendo qué significan cada una de sus letras y qué dos propiedades suelen elegir la mayoría de sistemas distribuidos modernos.

23. Dada la siguiente tabla de EMPLEADOS (id, nombre, apellido, departamento, salario, fecha_contratación), propon:

    - a) Una fragmentación horizontal para distribuir los datos por departamento
    - b) Una fragmentación vertical para separar información personal de información salarial
    - c) Una fragmentación mixta que combine ambos criterios

24. Completa la siguiente tabla comparativa:  
    
   | Característica          | Sistema de Ficheros | Sistema de Bases de Datos|
   |-------------------------|---------------------|--------------------------|
   | Redundancia de datos    |                     |                          |
   | Control de concurrencia |                     |                          |
   | Independencia de datos  |                     |                          |
   | Seguridad               |                     |                          |  



## Actividad 2. Análisis de un Sistema de Información.
#### Objetivo
Analizar un sistema de información real para comprender cómo se gestionan los datos, los usuarios y los procesos en la práctica.
#### Instrucciones
1. Selecciona un **sistema de información actual** que te sea familiar. Puede ser de una empresa conocida, una aplicación que uses regularmente o un servicio relacionado con tus intereses personales o profesionales.
2. Realiza un análisis detallado del sistema, abordando los siguientes aspectos:  
      - **Usuarios:** Identifica quiénes interactúan con el sistema y qué funciones desempeñan.  
      - **Datos:** Describe qué información se recoge, almacena y gestiona.  
      - **Procesos:** Explica cómo se procesan los datos y qué operaciones realiza el sistema.  
      - **Tecnología:** Señala las herramientas, plataformas o infraestructura tecnológica que soportan el sistema.  
      - **Propósito:** Explica la finalidad del sistema y qué necesidades satisface.
3. Presenta en tu porfolio personal el trabajo en un formato claro y estructurado, ya sea como **informe escrito, presentación o documento digital**.

#### Criterios de evaluación
- Claridad y organización del análisis.  
- Precisión en la descripción de usuarios, datos, procesos y tecnología.  
- Capacidad para relacionar conceptos teóricos con un ejemplo real.  
- Originalidad y reflexión crítica sobre el sistema.
  
## Actividad 3: Presentación y debate.
#### Objetivo
Desarrollar habilidades de exposición, argumentación y pensamiento crítico. Los estudiantes deberán demostrar dominio de los contenidos al exponer cualquier parte de la presentación y formular preguntas relevantes para profundizar en el aprendizaje de los contenidos vistos en la unidad.
#### Instrucciones
1. **Presentación de la unidad**

      - Elabora una presentación de la unidad.  
      - La presentación debe incluir los apartados de la unidad y cualquier otro punto que se considere de interés.
  
2. **Preguntas individuales**  
   
      - Cada estudiante debe preparar **al menos 3 preguntas** relacionadas con el tema.
      - Las preguntas deben ser claras, relevantes y fomentar el análisis de los contenidos.
  
3. **Dinámica la actividad** 
    
      - De manera **aleatoria** se seleccionarán los estudiantes que realizarán la exposición.
      - Tras cada exposición se realizarán algunas de las preguntas preparadas.
      - Se evaluará tanto la exposición como la calidad de las preguntas y respuestas. 
   
#### Criterios de evaluación
- Claridad y calidad de la exposición.  
- Dominio del tema (capacidad de exponer cualquier parte de la presentación).  
- Relevancia y pertinencia de las preguntas planteadas.  
- Participación activa.  

---
## 🐾. Práctica: Ejemplo básico de instalación y uso de un SGBDR ligero.
Como cierre de la primera unidad de introducción a las bases de datos, realizaremos una práctica sencilla para instalar y utilizar un sistema de gestión de bases de datos ligero: SQLite. 
El objetivo de la práctica es crear y gestionar una base de datos muy simple sobre una protectora de animales y ver de forma práctica su funcionamiento.
Deberás documentar utilizando pantallazos personalizados el desarrollo de la práctica.
------------------------------------------------------------
#### 1. Sistemas de ficheros.
1. Crea un fichero con la información de 5 mascotas.
2. Analiza los problemas que pueden aparecer en la gestión de la información al trabajar con ficheros. 
   
#### 2. Instalación de SQLite

**En Windows**

  1. Descarga SQLite desde la página oficial: https://www.sqlite.org/download.html
  2. Descarga el archivo sqlite-tools para Windows.
  3. Descomprime la carpeta en C:\sqlite.
  4. Abre la terminal CMD o PowerShell.
  5. Agrega la ruta C:\sqlite a las variables de entorno del sistema.

**En Linux**

``` bash
sudo apt update
sudo apt install sqlite3
```
Comprobar instalación:

``` bash 
sqlite3 --version

``` 

#### 3. Creación de la base de datos

Abrir terminal y ejecutar:  

``` bash
sqlite3 mascotas.db
```
Aparecerá el prompt de SQLite:  
``` bash
sqlite>
```
#### 4. Creación de tablas

``` sql
CREATE TABLE mascotas (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT NOT NULL,
    especie TEXT NOT NULL,
    edad INTEGER,
    dueño TEXT
);

``` 
Comprobar que la tabla existe:

``` sql
.tables

``` 
#### 4. Inserción de datos
``` sql
INSERT INTO mascotas (nombre, especie, edad, dueño) VALUES ('Luna', 'Perro', 3, 'María');
INSERT INTO mascotas (nombre, especie, edad, dueño) VALUES ('Michi', 'Gato', 2, 'Carlos');
INSERT INTO mascotas (nombre, especie, edad, dueño) VALUES ('Nemo', 'Pez', 1, 'Lucía');
``` 
------------------------------------------------------------

#### 5. Consultas básicas

Ver todos los registros:
``` sql
SELECT * FROM mascotas;
``` 
Ver solo los gatos:
``` 
SELECT nombre, dueño FROM mascotas WHERE especie = 'Gato';
``` 
Contar cuántas mascotas hay:
``` sql
SELECT COUNT(*) FROM mascotas;
``` 
------------------------------------------------------------

#### 6. Actualización y eliminación
 
Actualizar la edad de "Luna":
``` sql
UPDATE mascotas SET edad = 4 WHERE nombre = 'Luna';
``` 
Eliminar al pez "Nemo":
``` sql
DELETE FROM mascotas WHERE nombre = 'Nemo';
``` 
------------------------------------------------------------

#### 7. Salir y volver a entrar

Salir de SQLite:
``` sql
.exit
``` 
Volver a abrir la base de datos:
``` sql
sqlite3 mascotas.db
```

#### 8. Tareas.

1. Inserta al menos 3 mascotas más de distintas especies.
2. Crea una consulta que muestre solo los nombres y edades de los perros.
3. Modifica la tabla para añadir un nuevo campo fecha_registro de tipo DATE.
4. Inserta la fecha de registro en las nuevas mascotas.
5. Elimina todas las mascotas con edad menor a 2 años.
