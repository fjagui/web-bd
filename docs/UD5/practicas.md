# Actividades. Diseño físico e implementación de SGBDR.
En la propuesta de solución, se valorará muy positivamente la inclusión de reglas de negocio no detalladas explícitamente, pero que resulten coherentes con los escenarios (ej. validar formatos de códigos, rangos de valores realistas,coherencia entre fechas, etc.).   
Las aportaciones de este tipo deberán ser brevemente justificadas en la memoria de la actividad.

Para el desarrollo de las actividades es necesario el manejo de diferentes entornos de trabajo y el uso obligatorio de:  

- **Línea de comandos (CLI):** Uso obligatorio para las tareas de creación de estructuras básicas, asegurando el dominio de la sintaxis SQL pura.
- **Visual Studio Code:** Entorno principal para la edición de scripts complejos, aprovechando el resaltado de sintaxis y la gestión de archivos .sql.
- **Herramientas gráficas (GUI):** Uso de asistentes visuales (como pgAdmin, DBeaver o Database Client) para la gestión avanzada, visualización de esquemas y auditoría de datos.

Deberás documentar adecuadamente el uso de cada una de estas herramientas, incluyendo capturas de pantalla de los pasos críticos, donde se aprecie claramente tanto el comando ejecutado como el resultado devuelto por el servidor.

## Actividad 1. Biblioteca escolar.
#### 1. Descripción del Escenario
El Departamento de Informática del IES Gran Capitán requiere el desarrollo de una base de datos relacional para automatizar el Servicio de Biblioteca. El objetivo de este proyecto es migrar el sistema actual hacia una infraestructura más eficiente. Como técnico responsable, deberás ejecutar la fase de diseño e implementación física, asegurando el cumplimiento de las siguientes especificaciones técnicas:

* **Catálogo de Libros:** Cada libro se identifica por un ISBN único. Se debe almacenar el título, el autor, el año de publicación y la editorial. 
* **Gestión de Ejemplares:** Un mismo libro puede tener varios ejemplares. De cada ejemplar se debe conocer su ubicación en la sala, su estado (nuevo, bueno, desgastado o dañado) y un número de registro único.
* **Usuarios:** Se requiere el DNI, nombre, apellidos, dirección, teléfono y correo electrónico. El sistema debe permitir registrar una penalización acumulada.
* **Préstamos:** Se debe registrar la fecha de salida, la fecha máxima de devolución y la fecha real en que se devolvió el ejemplar.  
  
#### 2. Bloque A: Definición de Estructuras.

1. **Creación de Tablas:** Escribe las sentencias `CREATE TABLE` para las cuatro entidades, definiendo correctamente las Claves Primarias (PK) y Claves Foráneas (FK) para garantizar la integridad referencial.
2. **Tipos de Datos y Restricciones:** Selecciona los tipos de datos adecuados (VARCHAR2, NUMBER, DATE). Implementa mediante código una restricción `CHECK` para validar los estados de los ejemplares y un `UNIQUE` para el correo del usuario.
3. **Automatización:** Configura mediante comandos que la columna `fecha_salida` tome por defecto la fecha actual del sistema (`SYSDATE`).

#### 3. Bloque B: Gestión y Optimización.

1. **Modificación de Tablas:** Localiza la tabla `EJEMPLARES` en el navegador de objetos. Usa el asistente gráfico para añadir una columna llamada `observaciones` de tipo texto largo.
2. **Creación de Vistas:** Utiliza el **Constructor de Consultas (Query Builder)** para diseñar visualmente una vista llamada `V_ALERTA_DEVOLUCION`. La vista debe mostrar el nombre del usuario y el título del libro uniendo las tablas mediante sus relaciones gráficas.
3. **Indexación:** Crea un índice para la columna `TITULO` de la tabla `LIBROS` navegando por la pestaña de "Índices" de la herramienta para acelerar las búsquedas.

#### 4. Bloque C: Seguridad y Control (DCL)

1. **Administración de Usuarios:** Crea mediante comandos un usuario llamado `AUXILIAR_BIBLIO` con permisos de conexión (`CREATE SESSION`).
2. **Asignación de Privilegios:** Utiliza el editor gráfico de privilegios de la herramienta para asignar a este usuario permisos de solo lectura (`SELECT`) en el catálogo de libros, y permisos de inserción (`INSERT`) en la tabla de préstamos.

#### 5. Entrega.
La memoria final se entregará siguiendo el flujo de trabajo técnico, desde la planificación hasta la auditoría del servidor:

- **Fase de Diseño y Lógica de Negocio:** Un apartado único que incluya el boceto gráfico inicial (formato papel), la descripción de las aportaciones semánticas (reglas de negocio adicionales) y el esquema resultante de la transformación a tablas.

- **Implementación Física (Script SQL):** El código fuente íntegro (.sql) estructurado y comentado, que incluya la creación de objetos, dominios, restricciones de integridad y la configuración de seguridad.

- **Documentación de Procesos (Evidencias):** Dossier de capturas de pantalla significativas que certifiquen el uso de las herramientas obligatorias (CLI, VS Code y GUI) durante la ejecución de los bloques de trabajo.

- **Auditoría mediante Ingeniería Inversa:** Diagrama relacional generado automáticamente por la herramienta gráfica a partir de la base de datos real en el servidor, sirviendo como validación final de que el modelo físico coincide con el diseño previsto.

## Actividad 2. Hotel para mascotas.
#### 1. Descripción del Escenario
El hotel de mascotas "Pet Hotel" necesita una base de datos para gestionar el alojamiento temporal de perros y gatos. El sistema debe controlar:

* **Mascotas:** Se registra el chip (PK), nombre, especie (perro o gato), raza y posibles alergias o cuidados especiales.
* **Boxes:** El hotel dispone de recintos numerados en los que se alojan las mascotas. De cada uno se guarda el área, su capacidad, el precio por noche y si dispone de cámara web para que los dueños puedan ver al animal.
* **Clientes (Dueños):** Se requiere el DNI, nombre, apellidos, teléfono de contacto y una dirección de correo electrónico para el envío de facturas.
* **Reservas/Estancias:** Una reserva vincula a una mascota con una habitación. Se debe anotar la fecha de entrada, la fecha de salida y el estado de la reserva (confirmada, en curso, finalizada).  
  

#### 2. Bloque A: Definición de Estructura.

**Herramientas de trabajo**:  

  - Consola.
  - Lib
1. **Creación del Esquema:** Crea la base de datos `pethotel` y las tablas correspondientes asegurando que el motor sea `InnoDB` para soportar integridad referencial.
2. **Uso de Tipos de Datos:**  Usa `ENUM` para la especie de la mascota ('perro', 'gato') y el tipo de habitación.
* Define el precio de la habitación como `DECIMAL(6,2)`.
* Usa `AUTO_INCREMENT` para el identificador de la reserva.
1. **Restricciones de Integridad:** Establece que el nombre de la mascota y el DNI del cliente sean obligatorios (`NOT NULL`).
* Crea un `CHECK` (disponible en MySQL 8.0+) para asegurar que la fecha de salida sea posterior a la fecha de entrada.

#### 3. Bloque B: Gestión y optimización.

Realiza las tareas de este bloque utilizando alguna herramienta gráfica. 

1. **Modificación:** Localiza la tabla `CLIENTES`. Usa el asistente para añadir una columna `puntos_fidelidad` de tipo `INT` con un valor por defecto de 0.
2. **Índices de Búsqueda:** El hotel suele buscar a los clientes por apellidos. Crea un índice para la columna `apellidos` de la tabla `CLIENTES`.
3. **Vistas Administrativas:** Crea una vista llamada `V_OCUPACION_ACTUAL`. Utiliza el **Constructor de Consultas (Query Builder)** para unir visualmente las tablas y mostrar: *Nombre de la mascota, Número de habitación y Fecha de salida* de aquellas estancias cuyo estado sea 'en curso'.

#### 4. Bloque C: Seguridad y Usuarios (DCL)

Configura el acceso para el personal de recepción utilizando la consola y alguna herramienta gráfica.

1. **Creación de Usuario (Comandos):** Crea un usuario llamado `recepcionista_01` con acceso desde cualquier IP (`%`) y una contraseña segura.
2. **Privilegios:** Asigna al usuario permisos de consulta (`SELECT`) en todo el esquema y permisos de inserción y modificación (`INSERT`, `UPDATE`) únicamente en la tabla de Reservas.

#### 5. Entrega.
La memoria final se entregará siguiendo el flujo de trabajo técnico, desde la planificación hasta la auditoría del servidor:

- **Fase de Diseño y Lógica de Negocio:** Un apartado único que incluya el boceto gráfico inicial (formato papel), la descripción de las aportaciones semánticas (reglas de negocio adicionales) y el esquema resultante de la transformación a tablas.

- **Implementación Física (Script SQL):** El código fuente íntegro (.sql) estructurado y comentado, que incluya la creación de objetos, dominios, restricciones de integridad y la configuración de seguridad.

- **Documentación de Procesos (Evidencias):** Dossier de capturas de pantalla significativas que certifiquen el uso de las herramientas obligatorias (CLI, VS Code y GUI) durante la ejecución de los bloques de trabajo.

- **Auditoría mediante Ingeniería Inversa:** Diagrama relacional generado automáticamente por la herramienta gráfica a partir de la base de datos real en el servidor, sirviendo como validación final de que el modelo físico coincide con el diseño previsto.

## Actividad 3. Torneo escolar de fútbol sala.
#### 1. Descripción del Escenario

El IES Gran Capitán organiza anualmente un torneo de fútbol sala durante los recreos. Hasta ahora, la gestión de los encuentros dependía de una hoja de cálculo, un sistema propenso a inconsistencias como erratas en nombres, registros de goles negativos o equipos enfrentándose a sí mismos. Como técnico responsable, deberás implementar **"CapitánStats"**, una base de datos en PostgreSQL que automatice la clasificación y asegure la integridad de los datos mediante las siguientes especificaciones:

* **Jugadores:** Se identifican por un NIA único. Se debe almacenar su nombre, apellidos y el curso. Para el curso, se debe asegurar el cumplimiento del formato oficial (ej. '1DAM', '2ASIR').
* **Equipos:** Cada equipo tiene un nombre único, un grupo de referencia y un aula asignada. Además, se deben registrar sus estadísticas: partidos jugados, ganados, empatados, perdidos, y el balance de goles (favor/contra).
* **Partidos (Calendario):** Cada registro une a dos equipos (local y visitante). Se debe almacenar la fecha, el estado del encuentro ('Programado', 'Jugándose' o 'Finalizado') y el marcador final.
* **Disciplina:** Se registran las tarjetas mostradas, que solo pueden ser de tipo 'AMARILLA' o 'ROJA'.

Se valorará la implementación de **Dominios** y **Reglas de Negocio blindadas** para evitar errores lógicos (como que un equipo juegue contra sí mismo o puntos de partido distintos a 0, 1 o 3).

#### 2. Bloque A: Definición de Estructuras (DDL)

Este bloque debes realizarlo utilizando comandos SQL directamente en la consola de PostgreSQL o con Visual Studio Code.

1. **Creación de Dominios:** Antes de las tablas, crea los `DOMAIN` necesarios para validar el formato del curso, los estados del partido, los tipos de tarjeta y la puntuación permitida (0, 1, 3).
2. **Creación de Tablas:** Escribe las sentencias `CREATE TABLE` definiendo correctamente las Claves Primarias (PK) y Claves Foráneas (FK).
3. **Restricciones de Control:** Implementa mediante código una restricción `CHECK` que impida que un equipo sea local y visitante al mismo tiempo, y otra que asegure que el número de goles nunca sea inferior a cero.

#### 3. Bloque B: Gestión y Optimización

Las tareas de este bloque deberás realizarlas utilizando una herramienta visual como **pgAdmin 4** o **DBeaver**.

1. **Modificación de Tablas:** Localiza la tabla de `JUGADORES` en el navegador de objetos. Usa el asistente gráfico para añadir una columna llamada `goles_totales` de tipo entero con un valor por defecto de 0.
2. **Creación de Vistas:** Utiliza el diseñador visual de consultas para crear una vista llamada `V_CLASIFICACION`. La vista debe mostrar el nombre del equipo y sus puntos totales, ordenados de mayor a menor.
3. **Indexación:** Crea un índice para la columna `nombre_equipo` mediante la interfaz gráfica para agilizar las búsquedas en el calendario de partidos.

#### 4. Bloque C: Seguridad y Control (DCL)

Gestiona el acceso al sistema combinando comandos y la interfaz de usuario:

1. **Administración de Roles:** Crea mediante comandos un rol (usuario) llamado `ARBITRO_CENTRO` que tenga permisos de conexión.
2. **Asignación de Privilegios:** Utiliza el editor gráfico de privilegios (GRANT) para permitir que el árbitro pueda ver (`SELECT`) las tablas de equipos y jugadores, pero solo pueda insertar (`INSERT`) y actualizar (`UPDATE`) en las tablas de partidos y tarjetas.

#### 5. Entrega
La memoria final se entregará siguiendo el flujo de trabajo técnico, desde la planificación hasta la auditoría del servidor:

- **Fase de Diseño y Lógica de Negocio:** Un apartado único que incluya el boceto gráfico inicial (formato papel), la descripción de las aportaciones semánticas (reglas de negocio adicionales) y el esquema resultante de la transformación a tablas.

- **Implementación Física (Script SQL):** El código fuente íntegro (.sql) estructurado y comentado, que incluya la creación de objetos, dominios, restricciones de integridad y la configuración de seguridad.

- **Documentación de Procesos (Evidencias):** Dossier de capturas de pantalla significativas que certifiquen el uso de las herramientas obligatorias (CLI, VS Code y GUI) durante la ejecución de los bloques de trabajo.

- **Auditoría mediante Ingeniería Inversa:** Diagrama relacional generado automáticamente por la herramienta gráfica a partir de la base de datos real en el servidor, sirviendo como validación final de que el modelo físico coincide con el diseño previsto.

