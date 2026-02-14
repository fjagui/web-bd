# El Lenguaje SQL: Consulta de datos (DML)

## 1. Introducción a las Consultas en SQL

La consulta de información es la operación más frecuente en un SGBD. En SQL, la estructura fundamental para recuperar datos es el bloque **SELECT-FROM-WHERE**, que tiene su equivalencia directa en las operaciones del álgebra relacional:

* **SELECT:** Corresponde a la **proyección**. Lista los atributos deseados.
* **FROM:** Corresponde al **producto cartesiano**. Indica las tablas involucradas.
* **WHERE:** Corresponde a la **selección**. Filtra las filas según un predicado.

Aunque SQL se escribe de forma flexible, sigue un orden jerárquico estricto que el motor de la base de datos debe procesar.

Aquí tienes la sintaxis general de la sentencia `SELECT`, estructurada por sus **cláusulas**, siguiendo el estándar de Oracle:

#### Sintaxis general de una consulta sql.

```sql
SELECT [DISTINCT | ALL] { * | expresión [AS alias_columna], ... }
FROM tabla [alias_tabla]
[WHERE condición]
[GROUP BY columna, ...]
[HAVING condición_grupo]
[ORDER BY {columna | expresión | posición} [ASC | DESC]];

```

#### Desglose de las Cláusulas:

1. **`SELECT`**: Es la única cláusula obligatoria junto con `FROM`.
* `*`: Proyecta todas las columnas.
* `DISTINCT`: Elimina filas duplicadas del resultado.
* `AS alias_columna`: Da un nombre temporal a la columna (muy útil en expresiones calculadas).


2. **`FROM`**: Especifica la tabla o tablas de las que se van a extraer los datos (ej. `FROM employees`).
3. **`WHERE`**: Filtra los registros fila por fila antes de cualquier agrupamiento. Utiliza operadores como `=`, `<>`, `BETWEEN`, `IN`, `LIKE` e `IS NULL`.
4. `GROUP BY`: Agrupa filas que tienen los mismos valores en columnas específicas para poder aplicar funciones de agregado (`SUM`, `AVG`, `COUNT`).
5. `HAVING`: Funciona como un `WHERE`, pero se aplica exclusivamente a los grupos creados por `GROUP BY`.
6. `ORDER BY`: Define el orden de presentación final. Es la última cláusula en ejecutarse.


Para los ejemplos de esta unidad, utilizaremos el esquema **HR (Human Resources)** de Oracle, que consta de tablas como `EMPLOYEES` (empleados), `DEPARTMENTS` (departamentos), `JOBS` (puestos), `LOCATIONS` (sedes) y `COUNTRIES` (países).


Veamos con un ejemplo la potencia de la sintaxis completa en una sola sentencia:

**Consulta:** "Obtener el ID del departamento y el salario máximo de aquellos departamentos que tienen más de un empleado, pero solo para los empleados que ganan más de 3.000, ordenado de mayor a menor salario."

```sql
SELECT department_id AS "Departamento", 
       MAX(salary) AS "Salario_Maximo"      -- Proyección y Alias
FROM employees                             -- Origen
WHERE salary > 3000                        -- Filtro de filas
GROUP BY department_id                     -- Agrupamiento
HAVING COUNT(*) > 1                        -- Filtro de grupos
ORDER BY "Salario_Maximo" DESC;            -- Ordenación

```

Aunque nosotros escribimos primero el `SELECT`, el servidor de Oracle empieza procesando el `FROM`, luego el `WHERE`, después el `GROUP BY` y el `SELECT` es casi lo último que se ejecuta. ¡Por eso no puedes usar un alias de columna en la cláusula `WHERE`!


🔗[Acceder a FreeSQL](https://freesql.com/){target=blank}

---

## 2. Consultas simples.

### 2.1. La Cláusula SELECT. Proyección

La cláusula `SELECT` determina qué columnas se mostrarán en el resultado. Utiliza una lista de nombres de columnas separados por comas.

#### A. El comodín asterisco (`*`)

Se utiliza para proyectar **todas las columnas** de una tabla sin necesidad de nombrarlas una a una. Es especialmente útil en la fase de exploración para conocer la estructura y los datos contenidos.


```sql
-- Ejemplo: Consultar todos los datos de los puestos de trabajo:

SELECT * FROM jobs;

```

#### B. Alias de columna (`AS`)

Los alias permiten renombrar las columnas en el conjunto de resultados. Esto es vital por tres razones:

1. **Claridad:** Dar un nombre más intuitivo a columnas con nombres técnicos (ej. `phone_number` por "Contacto").
2. **Presentación:** Formatear los encabezados de los informes.
3. **Cálculos:** Nombrar columnas que resultan de operaciones aritméticas.

**Reglas en Oracle SQL:**

* Se usa la palabra clave `AS` (opcional, pero recomendada por legibilidad).
* Si el alias contiene espacios o caracteres especiales, debe ir entre **comillas dobles** (`" "`).

```sql
-- Ejemplo: Listado de salarios:

SELECT first_name AS "Nombre", 
       last_name AS "Apellido", 
       salary AS "Sueldo Base",
       salary * 0.15 AS "Retenciones"
FROM employees;

```

#### C. Eliminación de duplicados. `DISTINCT`

Por defecto, SQL muestra todas las filas, incluso si los valores en la cláusula `SELECT` se repiten. Para obtener una lista de valores únicos, se antepone `DISTINCT`.

```sql
-- Ejemplo: Obtener un listado único de los IDs de los puestos que existen en la empresa:

SELECT DISTINCT job_id 
FROM employees;

```

#### D. Expresiones y Funciones en la Proyección

En la cláusula `SELECT`, no solo podemos recuperar datos almacenados, sino también generar información nueva mediante el uso de expresiones y funciones.

**Expresiones Aritméticas**

Podemos utilizar los operadores `+`, `-`, `*`, `/` para realizar cálculos con columnas numéricas.

**Importante:** Si una columna contiene un valor `NULL`, el resultado de cualquier expresión aritmética sobre ella será `NULL`.

```sql
-- Ejemplo: Calcular el salario anual y un incremento hipotético del 10%:

SELECT first_name, 
       salary AS "Mensual", 
       salary * 12 AS "Anual",
       (salary * 1.10) AS "Salario con Subida"
FROM employees;

```

**Expresiones con cadenas de caracteres**

Las expresiones de cadena permiten manipular textos para cambiar su formato, extraer partes de una palabra o combinar varios campos en uno solo.

  - **Operador de concatenación (`||`)**

    En Oracle, se utiliza el símbolo de la doble barra vertical `||` para unir dos o más cadenas de texto o columnas.

```sql
-- Ejemplo: Crear un formato de envío postal o un nombre completo:

SELECT first_name || ' ' || last_name AS "Empleado",
       email || '@empresa.com' AS "Correo Corporativo"
FROM employees;
```

Los espacios en blanco y textos fijos (literales) deben ir siempre entre comillas simples (`' '`).

**Funciones de cadena:**  

  * `UPPER(col)` / `LOWER(col)`: Convierte a mayúsculas o minúsculas.
  * `CONCAT(col1, col2)` o el operador `||`: Para unir cadenas de texto.
  * `INITCAP`: Pone la primera letra en mayúscula y el resto en minúscula. Ideal para corregir nombres mal introducidos.  
  * `SUBSTR(columna, inicio, longitud)`: Extrae una parte del texto.
  * `LENGTH(columna)`: Devuelve el número de caracteres.  
  * `LPAD` / `RPAD`: Rellenan con caracteres (como asteriscos o ceros) a la izquierda o derecha hasta alcanzar una longitud.

```sql

-- Ejemplo: Crear un código de usuario con la inicial del nombre y el apellido:  

SELECT first_name, 
       last_name, 
       LOWER(SUBSTR(first_name, 1, 1) || last_name) AS "Username"
FROM employees;

```


```sql
-- Ejemplo: Poner en mayúscula las iniciales de juan pérez

SELECT INITCAP('juan pérez') FROM dual; -- Resultado: Juan Pérez

```  

La tabla DUAL es una tabla especial, de una sola columna y una sola fila, que se utiliza para realizar consultas que no requieren datos de una tabla real del usuario.

**Funciones Numéricas:**  

  * `ROUND(n, decimales)`: Redondea un número.
  * `TRUNC(n, decimales)`: Trunca un número sin redondear.


**Funciones de Fecha:**

  * `SYSDATE`: Devuelve la fecha y hora actual del sistema.
  * `MONTHS_BETWEEN(f1, f2)`: Calcula meses entre dos fechas.

**Ejemplo: Calcular los años de antigüedad de los empleados:**
```sql
SELECT last_name, 
       ROUND(MONTHS_BETWEEN(SYSDATE, hire_date) / 12) AS "Años en la Empresa"
FROM employees;
```

**Funciones de manejo de nulos (NVL):**  
  
  * `NVL(columna, valor_si_nulo)`: Permite sustituir un valor `NULL` por un valor alternativo para evitar errores en cálculos.
  

```sql
-- Ejemplo: Sumar salario y comisión (evitando que el resultado sea nulo si no hay comisión):

SELECT last_name, 
       salary + NVL(commission_pct, 0) * salary AS "Ingresos Totales"
FROM employees;

```
### 2.2. La Cláusula FROM. El origen de los datos.
La cláusula `FROM` es el punto de partida de toda consulta. Define el contexto y el conjunto de registros que estarán disponibles para el `SELECT` y el `WHERE`. Es obligatoria en toda consulta sql.

#### A.- Consulta sobre una sola tabla.

Es el escenario más sencillo. El motor de la base de datos localiza la tabla y pone a disposición del `SELECT` todas sus columnas y filas.



```sql
-- Ejemplo: Listar todos los países de la tabla `COUNTRIES`:

SELECT country_name 
FROM countries;

```

#### B.- Consultas sobre varias tablas.

Cuando necesitamos datos que están repartidos en varias tablas (por ejemplo, el nombre del empleado y el nombre de su departamento), debemos listarlas todas en el `FROM`.

Listar varias tablas en el `FROM` genera un **producto cartesiano** (todas las combinaciones posibles) entre ellas. Para que el resultado tenga sentido lógico, debemos realizar una **composición o unión (JOIN)** utilizando la cláusula `WHERE`. 

El caso más típico es la reunión natural, en el que combinamos las filas de las tablas involucradas en la consulta a través de las claves foráneas y sus respectivas claves primarias. 

Aunque es posible utilizar la palabra clave `JOIN` (estándar ANSI), aprender a combinar tablas con `WHERE` es fundamental por varias razones:

* **Comprensión lógica:** Ayuda a entender que una consulta relacional es, en esencia, un producto cartesiano filtrado.
* **Simplicidad:** Para consultas rápidas de dos tablas, es una sintaxis muy directa.
* **Lectura:** Permite ver claramente en la cláusula `WHERE` todas las condiciones de filtrado, tanto las de unión como las de restricción de datos.




```sql
-- Ejemplo: Empleados de la ciudad de 'Seattle'.
-- Necesitamos combinar `EMPLOYEES`, `DEPARTMENTS` y `LOCATIONS`.

SELECT e.first_name, d.department_name, l.city
FROM employees e, departments d, locations l
WHERE e.department_id = d.department_id 
  AND d.location_id = l.location_id     
  AND l.city = 'Seattle';               

```


#### C.- Uso de alias de tabla.

Cuando trabajamos con varias tablas (o para abreviar consultas largas), es una práctica profesional obligatoria usar **Alias de Tabla**.  
Para definirlo escribiremos el nombre de la tabla seguido de un espacio y el alias.  

Ventajas:  

  * **Simplificación:** Evita escribir nombres largos repetidamente (ej. `e` en lugar de `employees`).
  * **Resolución de Ambigüedad:** Si dos tablas tienen una columna con el mismo nombre (como `department_id` en `employees` y `departments`), el alias indica a SQL de qué tabla extraerla.




```sql
-- Ejemplo con alias:

SELECT e.first_name, e.department_id, d.department_name
FROM employees e, departments d
WHERE e.department_id = d.department_id;

```

Aquí `e` es el alias de `employees` y `d` el de `departments`.*

Técnicamente, el alias de tabla crea una **variable tupla**. Esto permite que SQL trate a la tabla como una instancia específica. Es especialmente útil en los **Self-Joins** (cuando una tabla se combina consigo misma).

**Ejemplo: ¿Quién es el jefe de cada empleado?**  

En la tabla `employees`, el jefe también es un empleado. Usamos dos alias para la misma tabla:
```sql
SELECT emp.last_name AS "Empleado", 
       mng.last_name AS "Jefe"
FROM employees emp, employees mng
WHERE emp.manager_id = mng.employee_id;

```



#### D.- Subconsultas en el FROM (Tablas Derivadas)

El estándar SQL permite que el origen de los datos no sea una tabla física, sino el resultado de otra consulta. A esto se le llama **Subconsulta en línea** o **Vista Inline**.

```sql
SELECT t.nombre, t.salario
FROM (SELECT first_name AS nombre, salary AS salario FROM employees) t
WHERE t.salario > 5000;

```


### 2.3. La Cláusula WHERE. Filtrado

Se utiliza para restringir las filas que devuelve la consulta mediante operadores tradicionales de comparación (`<`, `>`, `<=`, `>=`, `=`, `<>`) y lógicos (`AND`, `OR`, `NOT`).

**Operadores especiales**

  * **Operador BETWEEN:** Simplifica la escritura de rangos.
  
    ```sql
    SELECT last_name, salary 
    FROM employees 
    WHERE salary BETWEEN 5000 AND 10000;

    ```

* **Operadores sobre cadenas. `LIKE`:** Utiliza los caracteres comodín:
    * `%`: Representa cualquier subcadena.
    * `_`: Representa un solo carácter.

```sql
-- Ejemplo: Empleados cuyo nombre contiene una 'a' en la segunda posición 

SELECT first_name 
FROM employees 
WHERE first_name LIKE '_a%';

```

### 2.4. Cláusula ORDER BY. Orden

Se utiliza para especificar el orden en el que se mostraran los datos. Mediante el modificador `ASC` (por defecto) establecemos orden ascendente y con modificacor `DESC` el orden descendente.

```sql
-- Ejemplo: Nombre y apellidos de los empleados ordenados por fecha de contratación.

SELECT first_name, last_name, hire_date 
FROM employees 
ORDER BY hire_date DESC;

```  
## 3. Consultas avanzadas.

### 3.1. Funciones de agregación

Toman un conjunto de valores y devuelven uno solo:  

  - `AVG`: Media.
  - `MIN`: Mínimo.
  - `MAX`: Máximo.
  - `SUM`: Suma.
  - `COUNT`: Contar.

Excepto `COUNT(*)`, todas ignoran los valores nulos (`NULL`).

### 3.2. Cláusulas GROUP BY / HAVING. Agrupamiento.

La cláusula `GROUP BY` particiona los datos en grupos para aplicar funciones de agregado a cada grupo. `HAVING` es el filtro que se aplica a los grupos resultantes. No se aplica a filas individuales.

```sql
-- Ejemplo: Departamentos con más de 5 empleados y su salario medio:

SELECT department_id, COUNT(*), AVG(salary)
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;

```

### 3.3. Cláusula JOIN. Composiciones.

#### A.- Composiciones Internas. Inner Join

Combinan filas de varias tablas cuando hay una coincidencia en la columna común. Lo habitual es la coincidencia sea entre la clave foránea y la primaria que referencia.

```sql
---Ejemplo: Nombre del empleado y nombre de su departamento:

SELECT e.first_name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id;

```
El resultado de esta consulta no muestra los departamentos que no tienen empleados ni los empleados que no tienen departamento. 

#### B.- Composiciones Externas. Outer Join

Se usan cuando queremos incluir filas que **no** tienen correspondencia en la otra tabla. 
Podemos distinguir:

  * **LEFT OUTER JOIN:** Incluye todos los registros de la tabla de la izquierda.
  * **RIGHT OUTER JOIN:** Incluye todos los de la derecha.
  * **FULL OUTER JOIN:** Incluye todos los de ambas tablas.


```sql
--Ejemplo: Listar todos los departamentos, tengan o no empleados asignados:  

SELECT d.department_name, e.first_name
FROM departments d
LEFT JOIN employees e ON d.department_id = e.department_id;

```


## 4. Subconsultas y operaciones de conjuntos.

Las subconsultas permiten utilizar el resultado de una consulta como entrada para otra. Es una herramienta fundamental para resolver preguntas complejas que requieren un paso intermedio.

### 4.1. Subconsultas Anidadas

Dependiendo de lo que devuelva la consulta interna, las clasificamos en tres tipos:

#### A. Subconsultas escalares. 
El resultado es solo un valor.   
Se utilizan con operadores de comparación estándar (`=`, `>`, `<`, etc.). La subconsulta debe devolver **una única fila y una única columna**.

Suelen utilizarse para comparar un registro contra un valor calculado global como un promedio o un máximo.

```sql
--Ejemplo: Empleados que ganan más que el salario medio de la empresa.

SELECT last_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

```

#### B. Subconsultas de Lista (Operadores IN, ANY, ALL)

Se utilizan cuando la subconsulta devuelve **una columna pero varias filas**.

* `IN`: El valor debe coincidir con cualquiera de la lista.
* `ANY` / `SOME`: El valor debe ser mayor/menor que **al menos uno** de la lista.
* `ALL`: El valor debe ser mayor/menor que **todos** los de la lista.
  

```sql
--Ejemplo: Empleados que trabajan en departamentos situados en el Reino Unido (UK).

SELECT last_name 
FROM employees
WHERE department_id IN (SELECT department_id 
                        FROM departments 
                        WHERE location_id IN (SELECT location_id 
                                              FROM locations 
                                              WHERE country_id = 'UK'));

```

#### C. Subconsultas de Existencia.

* `EXISTS`: No buscan un valor específico, sino que comprueban si la subconsulta devuelve **algún registro**. Es mucho más eficiente para verificar relaciones.

```sql

-- Ejemplo: Departamentos que tienen asignado al menos un empleado.

SELECT department_name 
FROM departments d
WHERE EXISTS (SELECT 1 
              FROM employees e 
              WHERE e.department_id = d.department_id);

```

### 4.2. Operaciones de Conjuntos (Set Operators)

A diferencia de los `JOIN` que combinan tablas horizontalmente (añadiendo columnas), estos operadores las combinan **verticalmente** (añadiendo filas). Para que funcionen, ambas consultas deben tener el mismo número de columnas y tipos de datos compatibles.


- **`UNION`:** Une los resultados de ambas consultas.  Los elimina automáticamente.
- **`UNION ALL`:** Une los resultados de ambas consultas. **Mantiene** todos los duplicados (más rápido). 
- **`INTERSECT`:** Devuelve solo las filas que aparecen en ambas consultas. Solo valores comunes.
- **`MINUS`:** Devuelve las filas de la primera consulta que no están en la segunda. Exclusión.

```sql
-- Ejemplo: Obtener los IDs de todos los puestos (`job_id`) que están ocupados actualmente o que han sido ocupados en el pasado (histórico).
SELECT job_id FROM employees     -- Puestos actuales
UNION
SELECT job_id FROM job_history;  -- Puestos antiguos

```

## 5. Optimización de consultas

Para garantizar que el SGBD responda eficientemente, 
se deben seguir ciertos criterios de optimización:

  - Sustituir `SELECT *` por los nombres específicos de las columnas.
  - **Uso de Índices:** Asegurarse de que las columnas utilizadas en las cláusulas `WHERE` y `JOIN` estén indexadas.
  - **Filtrar resultados:** Colocar las condiciones más restrictivas en la cláusula `WHERE` para reducir el número de filas antes de los agrupamientos o combinaciones..
  - Evitar **expresiones con atributos** en la cĺáusula WHERE. Por ejemplo, usar `salary > 1000/12` en lugar de `salary * 12 > 1000` permite al optimizador usar índices de forma más eficaz.
  

## 6. Actividades de clase.

Utilizando el modelo **HR de Oracle** realiza las siguientes consultas sql

#### Nivel 1: Consultas Básicas (Proyección, Selección y Ordenación)

1. Listar todos los datos de la tabla de departamentos.
2. Obtener el nombre, apellido y correo electrónico de todos los empleados.
3. Mostrar el nombre y el salario de los empleados que ganen más de 12.000€.
4. Listar los apellidos de los empleados que trabajan en el departamento 50 o 80.
5. Obtener el nombre y apellido de los empleados contratados en el año 2005.
6. Mostrar el apellido y el identificador de puesto (`job_id`) de quienes no tengan comisión.
7. Listar los nombres de los empleados que contienen una 'a' y una 'e' en su nombre.
8. Mostrar el nombre de los empleados cuyo apellido empieza por 'S'.
9. Obtener el nombre y salario de los empleados con un sueldo entre 5.000€ y 10.000€, ordenados de mayor a menor salario.
10. Mostrar los diferentes IDs de jefes (`manager_id`) que hay en la tabla de empleados, evitando duplicados.

#### Nivel 2: Expresiones, Funciones y la Tabla DUAL

11. Calcular el salario anual de cada empleado (salario * 12) y mostrarlo con el alias "Salario Anual".
12. Mostrar el nombre completo de los empleados en una sola columna con el formato "Nombre Apellido".
13. Obtener el nombre del empleado en mayúsculas y su correo en minúsculas.
14. Mostrar el apellido del empleado y la longitud de dicho apellido.
15. Calcular los días que lleva trabajando cada empleado desde su fecha de contratación hasta hoy (usando `SYSDATE`).
16. Mostrar el apellido del empleado y los tres primeros caracteres de su puesto de trabajo.
17. Obtener la fecha actual del sistema con el alias "Hoy" utilizando la tabla `DUAL`.
18. Mostrar el salario de cada empleado redondeado a la unidad de millar más cercana.
19. Mostrar el apellido y la comisión del empleado, sustituyendo los valores nulos por el texto 'Sin Comisión' (usando `NVL`).
20. Calcular el salario total (salario + comisión) de los empleados. Si no tiene comisión, el total debe ser igual al salario.

#### Nivel 3: Composiciones Internas y Externas (Joins)

21. Mostrar el nombre del empleado y el nombre de su departamento (usando la cláusula `WHERE`).
22. Obtener el nombre del empleado, su puesto y el nombre del departamento para todos los empleados de la ciudad de 'Seattle'.
23. Listar el nombre de los empleados y el país donde trabajan.
24. Mostrar el nombre del departamento y la ciudad donde está ubicado (usando alias de tabla).
25. Obtener el nombre del empleado y el nombre de su jefe (Self-Join).
26. Listar todos los departamentos y, si tienen empleados, mostrar sus nombres (Outer Join).
27. Mostrar todos los empleados y el nombre de su departamento, incluyendo aquellos empleados que aún no tienen departamento asignado.
28. Obtener el nombre del empleado, su salario y el grado de su salario (si existiera una tabla de grados salariales) o compararlo con el salario máximo de su puesto.
29. Mostrar el nombre de la región, el país y la ciudad de todas las localizaciones del esquema.
30. Listar los nombres de los empleados que trabajan en el departamento de 'Sales' (Ventas).

#### Nivel 4: Funciones de Agregación y Agrupamiento

31. Calcular el salario máximo, mínimo y promedio de toda la empresa.
32. ¿Cuántos empleados hay en el departamento 90?
33. Obtener el salario promedio por cada departamento.
34. Contar el número de empleados que hay en cada puesto de trabajo (`job_id`).
35. Mostrar el ID del departamento y la suma de sus salarios, pero solo de los departamentos cuya suma supere los 20.000€.
36. Obtener el número de empleados por departamento que ganan más de 5.000€.
37. Mostrar los IDs de los jefes que tienen a su cargo a más de 5 empleados.
38. Calcular la comisión promedio de los empleados que sí tienen comisión asignada.
39. Mostrar el año de contratación y el número de empleados contratados ese año.
40. Obtener el ID de departamento con el salario más alto de entre todos los salarios medios.

#### Nivel 5: Subconsultas y Operaciones de Conjuntos

41. Mostrar los datos de los empleados que ganan más que el empleado 'Abel'.
42. Listar los nombres de los empleados que trabajan en el mismo departamento que 'Valli Iturra'.
43. Obtener los empleados cuyo salario es inferior al salario mínimo de cualquier departamento.
44. Mostrar los departamentos que no tienen ningún empleado asignado (usando `NOT IN` o `NOT EXISTS`).
45. Obtener los empleados cuyo salario es mayor que el salario promedio de su propio departamento (subconsulta correlacionada).
46. Listar los empleados que tienen el mismo puesto que el empleado con ID 141.
47. Obtener los nombres de los empleados que son jefes (su ID aparece en la columna `manager_id`).
48. Unir en una lista única los IDs de los departamentos de la tabla `DEPARTMENTS` y los de la tabla `EMPLOYEES` (usando `UNION`).
49. Mostrar los puestos (`job_id`) que existen en el departamento 10 pero no en el 30 (usando `MINUS`).
50. Obtener los IDs de empleados que han cambiado de puesto al menos una vez (que aparezcan en la tabla `JOB_HISTORY` e `INTERSECT` con la tabla `EMPLOYEES`).


