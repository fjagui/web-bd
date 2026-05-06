# Programación de Bases de Datos. PL/SQL.
## 1. Introducción y entorno de desarrollo.
Una vez definida la estructura física de la base de datos, el siguiente paso para convertirla en un sistema más avanzado consiste en incorporar lógica procedimental. Los sistemas gestores de bases de datos (SGBD) actuales no funcionan únicamente como almacenes de datos, sino que permiten ejecutar programas dentro del propio motor, lo que mejora la eficiencia, la seguridad y la integridad de la información.

La programación en bases de datos permite ampliar las capacidades de SQL mediante distintos elementos:

* **Lógica de control:** uso de variables y estructuras condicionales para crear scripts dinámicos.
* **Encapsulamiento:** creación de procedimientos y funciones para reutilizar código y centralizar reglas de negocio.
* **Procesamiento de filas:** uso de cursores para recorrer conjuntos de resultados de forma individual.
* **Gestión de errores:** control de excepciones para asegurar el correcto funcionamiento del sistema ante fallos.
* **Automatización:** uso de triggers y eventos que reaccionan automáticamente ante cambios en los datos o en el tiempo.

En este tema trabajaremos principalmente con **PL/SQL (Oracle)**, que es uno de los lenguajes procedimentales más utilizados en entornos empresariales. También se utilizará **MySQL/MariaDB (SQL/PSM)** para comparar distintas implementaciones de programación en bases de datos.

### 1.1. Entorno de desarrollo y herramientas.

Para programar en bases de datos no basta con ejecutar consultas sueltas. Es necesario utilizar herramientas que permitan escribir, organizar, depurar y ejecutar bloques de código complejos almacenados en archivos `.sql`.

#### Herramientas principales.

**Oracle (PL/SQL):**

* Oracle SQL Developer: herramienta oficial con interfaz gráfica y depuración paso a paso.
* SQL*Plus: herramienta de línea de comandos para ejecución de scripts.
* Visual Studio Code: alternativa ligera mediante extensiones.

**MySQL/MariaDB:**

* MySQL Workbench: entorno gráfico con editor de rutinas almacenadas.
* Visual Studio Code: alternativa con extensiones para bases de datos.

### 1.2. Delimitadores y ejecución de scripts.

Al trabajar con programación procedimental aparece un problema importante: el uso del delimitador de sentencias SQL (`;`) dentro de los bloques de código. Para evitar errores en la interpretación de las sentencias, cada sistema gestiona la ejecución de forma distinta:

* **MySQL/MariaDB:** se utiliza `DELIMITER //` para cambiar temporalmente el final de sentencia y permitir el uso de `;` dentro del bloque.
* **Oracle:** no cambia el delimitador, pero utiliza `/` en una línea independiente para ejecutar el bloque completo.

#### Ejecución de scripts.

Un script es un archivo `.sql` que contiene instrucciones completas para crear o modificar objetos en la base de datos:

* **Oracle:**

```sql
@C:\scripts\archivo.sql
```

* **MySQL:**

```sql
source /ruta/archivo.sql;
```

En Oracle es habitual activar la salida de mensajes en pantalla con:

```sql
SET SERVEROUTPUT ON;
```

### 1.3. Unidades de programación en PL/SQL.

Con PL/SQL se pueden crear distintos tipos de objetos programables dentro de Oracle:

* Scripts
* Procedimientos almacenados
* Funciones
* Triggers

Además, PL/SQL puede integrarse con herramientas del ecosistema Oracle como:

* Oracle Forms
* Oracle Reports
* Oracle Graphics
* Oracle Application Server

!!! note "🛠️ Actividad: Mi primer bloque de código"

    Vamos a crear un bloque anónimo (un código que se ejecuta una vez y no se guarda) en ambos sistemas. El objetivo es simplemente mostrar un mensaje por pantalla.

    #### 1. En Oracle (PL/SQL)
    Oracle requiere activar el servidor de salida para poder ver mensajes de texto.

    ```sql
    -- 1. Activamos la salida de consola
    SET SERVEROUTPUT ON;

    -- 2. Bloque anónimo
    BEGIN
        DBMS_OUTPUT.PUT_LINE('Hola mundo desde PL/SQL');
    END;
    /
    ```
    #### 2. En MySQL
    En MySQL, para mostrar un mensaje simple en un bloque, solemos usar una sentencia `SELECT`.

    ```sql
    DELIMITER //

    BEGIN
        -- En MySQL no existe DBMS_OUTPUT, usamos SELECT para "imprimir"
        SELECT 'Hola mundo desde MySQL' AS Mensaje;
    END//

    DELIMITER ;
    ```

#### 📝 Actividades Propuestas.  

1.  **Configuración del Entorno:** Abre tu cliente de base de datos (Workbench o SQL Developer) y asegúrate de que puedes ejecutar un comando básico de sistema (como `SELECT NOW();` en MySQL o `SELECT SYSDATE FROM DUAL;` en Oracle).
2.  **El Delimitador en MySQL:** Crea un script llamado `prueba_delimitador.sql`. Cambia el delimitador a `$$`, realiza un `SELECT 'Test';` y cierra el bloque con `$$`. Vuelve a poner el delimitador en `;`. Ejecútalo usando el comando `source`.
3.  **Investigación PL/SQL:** ¿Qué ocurre en Oracle si olvidas poner la barra inclinada `/` al final de un bloque `BEGIN...END;`? Pruébalo en el editor y documenta el comportamiento.
4.  **Script de Limpieza:** Crea un archivo `.sql` que contenga las sentencias necesarias para borrar un procedimiento llamado `test_p` si existe, y ejecútalo desde la línea de comandos de tu SGBD.


## 2. Fundamentos del Lenguaje.
### 2.1. Unidades léxicas.

PL/SQL no es *case-sensitive* en palabras clave ni en identificadores no entrecomillados, es decir, no distingue entre mayúsculas y minúsculas como ocurre en otros lenguajes de programación como C o Java. Sin embargo, Oracle sí es *case-sensitive* en el contenido de las cadenas de texto, por lo que las comparaciones entre valores de tipo carácter pueden distinguir entre mayúsculas y minúsculas si no se utilizan funciones de conversión como `UPPER()` o `LOWER()`.

Una línea en PL/SQL está compuesta por unidades básicas de análisis llamadas unidades léxicas, las cuales pueden clasificarse en diferentes categorías.

**Delimitadores:** son símbolos simples o compuestos que tienen una función estructural dentro del lenguaje. Su finalidad es organizar y dar forma al código. Entre ellos se encuentran el punto y coma (`;`) utilizado para finalizar sentencias, la coma (`,`) para separar elementos, los paréntesis (`()`) para agrupar expresiones y el punto (`.`) para acceder a atributos o componentes de objetos.

**Identificadores:** son los nombres que se utilizan para representar objetos y elementos dentro de un programa PL/SQL. Estos pueden corresponder a variables, constantes, cursores, subprogramas, excepciones o paquetes, y sirven para referenciar dichos elementos dentro del código.

**Literales:** representan valores explícitos que no están asociados a un identificador. Pueden ser valores numéricos como `100`, cadenas de texto como `'Hola'`, valores de fecha como `DATE '2025-04-13'` o valores lógicos como `TRUE` y `FALSE`.

**Comentarios:** son anotaciones incluidas en el código con el objetivo de mejorar su comprensión y no son ejecutados por el motor de base de datos. En PL/SQL pueden escribirse como comentarios de una sola línea utilizando `--`, o comentarios de varias líneas delimitados por `/* ... */`.


### 2.2. Tipos de datos. Variables, constantes y operadores.

#### Tipos de Datos en PL/SQL

Los tipos de datos determinan el formato de almacenamiento, las restricciones y el rango de valores válidos que puede tomar una variable. PL/SQL proporciona una amplia variedad de tipos de datos predefinidos, los cuales en su mayoría son compatibles con los tipos de datos de SQL.

A continuación, se describen los principales tipos de datos en PL/SQL:

* **NUMBER**: Se utiliza para almacenar valores numéricos, ya sean enteros o con decimales, con prácticamente cualquier longitud. Opcionalmente, se puede especificar la precisión (número total de dígitos) y la escala (número de decimales). Su sintaxis es `NUMBER(precision, escala)`. Por ejemplo, `saldo NUMBER(16,2)` permite almacenar un valor numérico de hasta 16 dígitos, de los cuales 2 corresponden a la parte decimal y 14 a la parte entera.

* **CHAR**: Se utiliza para almacenar cadenas de caracteres de longitud fija. Puede almacenar hasta 32767 caracteres, aunque su longitud por defecto es 1 si no se especifica. Su sintaxis es `CHAR(longitud)`. Por ejemplo, `nombre CHAR(20)` almacena una cadena de 20 caracteres, rellenando con espacios si el contenido es menor.

* **VARCHAR2**: Permite almacenar cadenas de caracteres de longitud variable, utilizando únicamente el espacio necesario según el contenido almacenado. Su sintaxis es `VARCHAR2(longitud)`. Por ejemplo, `nombre VARCHAR2(20)` puede almacenar hasta 20 caracteres sin rellenar con espacios adicionales cuando la longitud es menor.

* **BOOLEAN**: Se emplea para almacenar valores lógicos, que pueden ser `TRUE`, `FALSE` o `NULL`.

* **DATE**: Se utiliza para almacenar fechas y horas. Internamente, las fechas se representan como valores numéricos, lo que permite realizar operaciones aritméticas con ellas, como sumas o restas de días.

Además de estos tipos básicos, PL/SQL incorpora atributos de tipo que permiten obtener información de objetos de la base de datos. El **atributo `%TYPE`** permite declarar una variable con el mismo tipo de dato que una columna, variable o constante existente. Por su parte, **`%ROWTYPE`** permite definir una variable que representa una fila completa de una tabla, vista o cursor, heredando automáticamente el tipo de cada columna.

Finalmente, PL/SQL permite la creación de tipos de datos personalizados, como registros, así como estructuras de tipo colección.

#### Variables.

En programación de bases de datos, las **variables** son estructuras que permiten almacenar valores de los distintos tipos datos de forma temporal durante la ejecución de un bloque o programa.

A diferencia de los datos almacenados en tablas, estos valores:

* Se guardan en memoria (RAM)
* Solo existen durante la ejecución
* No persisten tras finalizar el bloque (salvo variables de sesión)

Según su ámbito podemos clasificarlas en:

**Variables locales.**

* Se declaran dentro de un bloque PL/SQL (`DECLARE ... BEGIN ... END`)
* Solo son accesibles dentro de ese bloque
* Se destruyen al finalizar la ejecución

*Ejemplo.*

```sql
DECLARE
  v_nombre VARCHAR2(50);
BEGIN
  v_nombre := 'Ana';
END;
```

**Variables de sesión.**

* Persisten mientras la sesión del usuario esté activa
* Se utilizan principalmente en herramientas como SQL*Plus o SQL Developer
* Suelen identificarse con `DEFINE` o `ACCEPT` (según entorno)

*Ejemplo.*

```sql
DEFINE v_usuario = 'ADMIN';
```

**Variables de sistema.**

* Controlan el comportamiento del SGBD (Oracle)
* No se declaran directamente en PL/SQL

*Ejemplos.*  

  * Zona horaria
  * Parámetros de memoria
  * Configuración del entorno


**Declaración y asignación.**

| Concepto    | Oracle (PL/SQL)            | MySQL                                |
| ----------- | -------------------------- | ------------------------------------ |
| Declaración | `DECLARE`                  | `DECLARE` dentro de `BEGIN`          |
| Sintaxis    | `nombre tipo := valor;`    | `DECLARE nombre tipo DEFAULT valor;` |
| Asignación  | `:=`                       | `SET` o `:=`                         |
| SELECT INTO | `SELECT ... INTO variable` | `SELECT ... INTO variable`           |


#### Constantes.

Las **constantes** son identificadores cuyo valor no puede modificarse durante la ejecución de un bloque de código.

* Se declaran con la palabra clave `CONSTANT`
* Deben inicializarse obligatoriamente
* Su valor permanece fijo durante todo el bloque

*Ejemplo.*

```sql
c_pi CONSTANT NUMBER := 3.1416;
```

*Ejemplo en Oracle:*

```sql
SET SERVEROUTPUT ON;

DECLARE
    -- Variable anclada al tipo de una columna
    v_nombre    ALUMNOS.nombre%TYPE;

    -- Variable numérica
    v_edad      NUMBER(2) := 20;

    -- Constante
    c_pi        CONSTANT NUMBER := 3.1416;

BEGIN
    v_nombre := 'Juan Pérez';

    DBMS_OUTPUT.PUT_LINE('Nombre: ' || v_nombre);
    DBMS_OUTPUT.PUT_LINE('Edad: ' || v_edad);
    DBMS_OUTPUT.PUT_LINE('PI: ' || c_pi);
END;
/
```

*Ejemplo en MySQL*

```sql
SET @iva = 0.21;
```

#### Operadores.

| Tipo de operador | Descripción | Operadores |
|------------------|-------------|------------|
| **Asignación** | Se utiliza para asignar valores a variables | `:=` |
| **Aritméticos** | Operaciones matemáticas básicas | `+` (suma)<br>`-` (resta)<br>`*` (multiplicación)<br>`/` (división)<br>`**` (potenciación) |
| **Relacionales / Comparación** | Comparan valores y devuelven resultados lógicos | `=` (igual a)<br>`<>` (distinto de)<br>`<` (menor que)<br>`>` (mayor que)<br>`<=` (menor o igual)<br>`>=` (mayor o igual) |
| **Lógicos** | Combinan condiciones lógicas | `AND` (y lógico)<br>`OR` (o lógico)<br>`NOT` (negación lógica) |
| **Concatenación** | Une cadenas de texto | `||` |


#### 📝 Actividades propuestas.

1.  **Cálculo de Áreas:** Crea un bloque de código en **MySQL** que declare una variable para el `radio`, una variable de usuario `@pi` con el valor 3.1416, y calcule el área de un círculo. Muestra el resultado con un `SELECT`.
2.  **Uso de %TYPE (Oracle):** Imagina que tienes una tabla `PRODUCTOS` con una columna `precio` de tipo `NUMBER(10,2)`. Escribe el código PL/SQL necesario para declarar una variable que herede ese tipo de dato y asígnale el valor `50.25`.
3.  **Variables de Sesión:** En MySQL, asigna mediante un `SELECT` el nombre de un alumno de tu base de datos a una variable de usuario llamada `@alumno_favorito`. Después, realiza una consulta a la tabla `NOTAS` filtrando por esa variable.
4.  **Investigación de tipos:** Busca en la documentación oficial de MySQL si existe alguna forma de implementar algo parecido al `%TYPE` de Oracle. ¿Qué ocurre si cambiamos un `VARCHAR(50)` a `VARCHAR(100)` en una tabla respecto a las variables que lo usan?


## 3. Estructuras de Control y Lógica
### 3.1. Condicionales
**IF / ELSIF** 

La estructura `IF` permite ejecutar instrucciones dependiendo de si una condición es verdadera o falsa.

```sql
IF (expresion) THEN
   -- Instrucciones
ELSIF (expresion) THEN
   -- Instrucciones
ELSE
   -- Instrucciones
END IF;
```

⚠️ **Importante:** En PL/SQL se usa `ELSIF` y en MYSQL `ELSEIF`.

*Ejemplo.*

```sql
DECLARE
  nota NUMBER := 7;
BEGIN
  IF (nota >= 9) THEN
    dbms_output.put_line('Sobresaliente');
  ELSIF (nota >= 5) THEN
    dbms_output.put_line('Aprobado');
  ELSE
    dbms_output.put_line('Suspenso');
  END IF;
END;
```
 
**CASE**

La estructura `CASE` se utiliza para evaluar múltiples condiciones de forma más clara y ordenada que varios `IF`.

```sql
CASE [expresion]
  WHEN valor1 THEN
    bloque_instrucciones_1
  WHEN valor2 THEN
    bloque_instrucciones_2
  ...
  ELSE
    bloque_instrucciones_por_defecto
END CASE;
```

*Ejemplo.*

```sql
DECLARE
  dia NUMBER := 3;
BEGIN
  CASE dia
    WHEN 1 THEN dbms_output.put_line('Lunes');
    WHEN 2 THEN dbms_output.put_line('Martes');
    WHEN 3 THEN dbms_output.put_line('Miercoles');
    WHEN 4 THEN dbms_output.put_line('Jueves');
    WHEN 5 THEN dbms_output.put_line('Viernes');
    ELSE dbms_output.put_line('Fin de semana');
  END CASE;
END;
```

#### `IF` vs `CASE`

*Ejemplo con `IF`:*

```sql
DECLARE
  numero NUMBER := 2;
BEGIN
  IF numero = 1 THEN
    dbms_output.put_line('Uno');
  ELSIF numero = 2 THEN
    dbms_output.put_line('Dos');
  ELSIF numero = 3 THEN
    dbms_output.put_line('Tres');
  ELSE
    dbms_output.put_line('Otro numero');
  END IF;
END;
```

*Ejemplo con `CASE`:*

```sql
DECLARE
  numero NUMBER := 2;
BEGIN
  CASE numero
    WHEN 1 THEN dbms_output.put_line('Uno');
    WHEN 2 THEN dbms_output.put_line('Dos');
    WHEN 3 THEN dbms_output.put_line('Tres');
    ELSE dbms_output.put_line('Otro numero');
  END CASE;
END;
```

* Usa `IF` cuando las condiciones son complejas (comparaciones, rangos, etc.).
* Usa `CASE` cuando comparas una misma variable con varios valores posibles.

**GOTO**

La sentencia `GOTO` permite saltar a una parte específica del código identificada por una etiqueta.

Las etiquetas se definen con `<< >>`.

*Ejemplo.*

```sql
DECLARE
  flag NUMBER := 1;
BEGIN
  IF (flag = 1) THEN
     GOTO paso2;
  END IF;

<<paso1>>
  dbms_output.put_line('Ejecucion de paso 1');

<<paso2>>
  dbms_output.put_line('Ejecucion de paso 2');
END;
```

En este caso, se salta directamente a `paso2`, ignorando `paso1`.

### 3.2. Estructuras de Repetición. Bucles.

En PL/SQL existen tres tipos principales de bucles:

* `LOOP`
* `WHILE`
* `FOR`

Cada uno se utiliza según la situación y el tipo de control que necesites sobre la iteración.

**LOOP**

El bucle `LOOP` es el más básico. Se repite indefinidamente hasta que se fuerza su salida mediante la instrucción `EXIT`.

```sql
LOOP
   -- Instrucciones
   IF (expresion) THEN
      EXIT;
   END IF;
END LOOP;
```

*Ejemplo (modelo HR):* mostrar empleados hasta encontrar uno con salario > 10000.

```sql
DECLARE
  v_id employees.employee_id%TYPE := 100;
  v_sal employees.salary%TYPE;
BEGIN
  LOOP
    SELECT salary INTO v_sal
    FROM employees
    WHERE employee_id = v_id;

    dbms_output.put_line('Empleado ' || v_id || ' salario: ' || v_sal);

    IF v_sal > 10000 THEN
      EXIT;
    END IF;

    v_id := v_id + 1;
  END LOOP;
END;
```

Se usa cuando no sabemos exactamente cuántas iteraciones habrá.

**WHILE**

El bucle `WHILE` se ejecuta **mientras se cumpla una condición**.

```sql
WHILE (expresion) LOOP
   -- Instrucciones
END LOOP;
```

*Ejemplo (modelo HR):* listar empleados con salario menor a 5000.

```sql
DECLARE
  v_id employees.employee_id%TYPE := 100;
  v_sal employees.salary%TYPE;
BEGIN
  WHILE v_id <= 110 LOOP
    SELECT salary INTO v_sal
    FROM employees
    WHERE employee_id = v_id;

    IF v_sal < 5000 THEN
      dbms_output.put_line('Empleado ' || v_id || ' tiene salario bajo');
    END IF;

    v_id := v_id + 1;
  END LOOP;
END;
```

Este bucle está indicado cuando la condición se evalúa antes de cada iteración.

**FOR**

El bucle `FOR` se utiliza cuando conocemos el número de iteraciones.

```sql
FOR contador IN [REVERSE] inicio..final LOOP
   -- Instrucciones
END LOOP;
```

`REVERSE` permite recorrer el rango en orden descendente.


*Ejemplo (modelo HR):* mostrar IDs de empleados.

```sql
BEGIN
  FOR i IN 100..105 LOOP
    dbms_output.put_line('Empleado ID: ' || i);
  END LOOP;
END;
```

*Ejemplo con `REVERSE`.*

```sql
BEGIN
  FOR i IN REVERSE 100..105 LOOP
    dbms_output.put_line('Empleado ID: ' || i);
  END LOOP;
END;
```

Salida en orden inverso: 105 → 100


*Ejemplo con `FOR` y modelo HR.*

```sql
BEGIN
  FOR emp IN (
    SELECT first_name, salary
    FROM employees
    WHERE department_id = 50
  ) LOOP
    dbms_output.put_line(emp.first_name || ' gana ' || emp.salary);
  END LOOP;
END;
```

Este tipo de bucle (`FOR`) es muy usado porque recorre directamente resultados de consultas.

A modo de resumen:

* `LOOP` → cuando no sabes cuántas veces se repetirá (control manual con `EXIT`)
* `WHILE` → cuando depende de una condición lógica
* `FOR` → cuando sabes el número de iteraciones o recorres datos

!!! note "🛠️ Actividad: Lógica de bonificaciones"

    Supongamos que queremos calcular una bonificación según los años de experiencia de un empleado.

    #### 1. En Oracle (Uso de `IF` y `ELSIF`)
    ```sql
    SET SERVEROUTPUT ON;

    DECLARE
        v_años_exp NUMBER := 6;
        v_bono     NUMBER;
    BEGIN
        IF v_años_exp < 2 THEN
            v_bono := 0;
        ELSIF v_años_exp BETWEEN 2 AND 5 THEN
            v_bono := 500;
        ELSE
            v_bono := 1200;
        END IF;
        
        DBMS_OUTPUT.PUT_LINE('La bonificación es de: ' || v_bono || '€');
    END;
    /
    ```

    #### 2. En MySQL (Uso de `WHILE`)
    Vamos a crear un procedimiento que sume los números del 1 al N.

    ```sql
    DELIMITER //

    CREATE PROCEDURE suma_hasta_n(IN p_n INT)
    BEGIN
        DECLARE v_contador INT DEFAULT 1;
        DECLARE v_suma     INT DEFAULT 0;
        
        WHILE v_contador <= p_n DO
            SET v_suma = v_suma + v_contador;
            SET v_contador = v_contador + 1;
        END WHILE;
        
        SELECT v_suma AS 'Resultado de la suma';
    END//

    DELIMITER ;

    -- Para probarlo:
    CALL suma_hasta_n(10);
    ```

#### 📝 Actividades Propuestas

1.  **Selector de Mensajes (CASE):** Crea un bloque en **Oracle** que declare una variable `v_dia` (número del 1 al 7). Utiliza una estructura `CASE` para imprimir el nombre del día correspondiente (1 -> 'Lunes', etc.).
2.  **Control de Stock (IF):** En **MySQL**, escribe un procedimiento que reciba el `id_producto` y su `cantidad_actual`. Si la cantidad es menor a 10, debe mostrar un mensaje "Realizar pedido urgente", si está entre 10 y 50 "Stock normal", y si es mayor a 50 "Stock excedido".
3.  **Bucle de Pares:** Desarrolla un script en **MySQL** que utilice un bucle `WHILE` para mostrar por pantalla todos los números pares entre el 1 y el 20.
4.  **Investigación - El bucle FOR en Oracle:** Investiga cómo funciona el `FOR` en PL/SQL para recorrer un rango numérico (ej. `FOR i IN 1..10`). ¿Es necesario declarar la variable índice `i` en la sección `DECLARE`? Intenta replicar el ejercicio de la suma (ejercicio 2 de MySQL) en Oracle usando un `FOR`.


## 4. Bloques de Código y Excepciones

Un programa en PL/SQL está formado por **bloques**.
Todo programa tiene al menos un bloque.

### 4.1 Tipos de bloques

* **Bloques anónimos** → No tienen nombre, se ejecutan directamente.
* **Subprogramas** → Bloques con nombre que pueden ser invocados explícitamente; incluyen procedimientos y funciones.
* **Disparadores (triggers)** → Bloques de código almacenados que se ejecutan automáticamente en respuesta a eventos de la base de datos (DML, DDL o de sistema).  

### 4.2 Estructura de un bloque PL/SQL

Un bloque PL/SQL se divide en tres partes:

1. **Sección declarativa** → Variables, constantes, cursores…
2. **Sección de ejecución** → Código que se ejecuta
3. **Sección de excepciones** → Manejo de errores

```sql
[DECLARE | IS | AS]
   -- Declaraciones
BEGIN
   -- Ejecución
[EXCEPTION]
   -- Manejo de errores
END;
```

La única parte obligatoria es la sección de ejecución (`BEGIN ... END`).

*Ejemplo de bloque anónimo:*

```sql
DECLARE
  v_fecha DATE;
BEGIN
  SELECT SYSDATE
  INTO v_fecha
  FROM DUAL;

  dbms_output.put_line('Fecha actual: ' || v_fecha);

EXCEPTION
  WHEN OTHERS THEN
    dbms_output.put_line('Se ha producido un error');
END;
```

*Ejemplo práctico (modelo HR). Calcular el salario anual de un empleado:*

```sql
DECLARE
  v_salario employees.salary%TYPE;
  v_anual   NUMBER;
BEGIN
  SELECT salary
  INTO v_salario
  FROM employees
  WHERE employee_id = 100;

  v_anual := v_salario * 12;

  dbms_output.put_line('Salario anual: ' || v_anual);
END;
```

**Sección declarativa.**

En esta sección se definen:

* Variables
* Constantes
* Cursores
* Excepciones

*Sintaxis general:*

```sql
nombre_variable [CONSTANT] tipo_dato [NOT NULL] [:= valor_inicial];
```

Las variables no inicializadas toman valor `NULL`.

*Ejemplos de declaraciones.*

```sql
DECLARE
  -- Variable con valor inicial
  v_ciudad VARCHAR2(20) := 'Granada';

  -- Constante
  c_pi CONSTANT NUMBER := 3.1416;

  -- Variable obligatoria (no admite NULL)
  v_edad NUMBER NOT NULL := 18;

BEGIN
  dbms_output.put_line(v_ciudad);
END;
```


```sql
DECLARE
  v_nombre employees.first_name%TYPE;
BEGIN
  SELECT first_name
  INTO v_nombre
  FROM employees
  WHERE employee_id = 100;

  dbms_output.put_line(v_nombre);
END;
```

```sql
DECLARE
  v_emp employees%ROWTYPE;
BEGIN
  SELECT *
  INTO v_emp
  FROM employees
  WHERE employee_id = 100;

  dbms_output.put_line(v_emp.first_name || ' - ' || v_emp.salary);
END;
```
**Sección de ejecución.**

La ejecución de un bloque PL/SQL se estructura siempre en torno a las palabras clave BEGIN y END, que delimitan la sección de ejecución principal del programa. Todo el código que contiene las sentencias SQL y las instrucciones de control de flujo se escribe dentro de este bloque, y se ejecuta de forma secuencial cuando el motor de Oracle procesa el script.   

Opcionalmente, puede existir una sección DECLARE previa para la definición de variables y una sección EXCEPTION posterior para el tratamiento de errores. En herramientas como SQL*Plus o SQL Developer, el bloque completo se ejecuta como una única unidad lógica, y suele finalizar con el carácter / para indicar su ejecución.

**Excepciones en PL/SQL.**

Las **excepciones** gestionan errores en tiempo de ejecución.

*Estructura.*

```sql
BEGIN
  -- código
EXCEPTION
  WHEN condicion THEN
    -- manejo
END;
```

*Ejemplo.*

```sql
DECLARE
  v_result NUMBER;
BEGIN
  SELECT 1/0 INTO v_result FROM dual;

EXCEPTION
  WHEN ZERO_DIVIDE THEN
    dbms_output.put_line('División por cero');
  WHEN OTHERS THEN
    dbms_output.put_line('Error general');
END;
```

*Excepciones predefinidas importantes.*

* `NO_DATA_FOUND`
* `TOO_MANY_ROWS`
* `ZERO_DIVIDE`
* `VALUE_ERROR`

*Excepciones definidas por el usuario.*

```sql
DECLARE
  ex_negativo EXCEPTION;
  v_num NUMBER := -1;
BEGIN
  IF v_num < 0 THEN
    RAISE ex_negativo;
  END IF;

EXCEPTION
  WHEN ex_negativo THEN
    dbms_output.put_line('Número negativo no permitido');
END;
```

*SQLCODE y SQLERRM*

SQLCODE y SQLERRM son funciones predefinidas de PL/SQL que se utilizan dentro del bloque de excepciones para obtener información detallada sobre los errores ocurridos durante la ejecución. SQLCODE devuelve el código numérico del error generado por Oracle, mientras que SQLERRM devuelve el mensaje descriptivo asociado a dicho error. Ambas son especialmente útiles en el manejo de la excepción WHEN OTHERS, ya que permiten identificar y registrar de forma precisa la causa del fallo para su depuración o registro en logs.

```sql
WHEN OTHERS THEN
  dbms_output.put_line(SQLCODE);
  dbms_output.put_line(SQLERRM);
```

*RAISE_APPLICATION_ERROR*

`RAISE_APPLICATION_ERROR` es una instrucción de PL/SQL que permite generar errores personalizados en tiempo de ejecución. Se utiliza dentro de bloques o subprogramas para interrumpir el flujo normal del programa y devolver un código de error definido por el desarrollador, junto con un mensaje explicativo. El número de error debe estar en el rango de -20001 a -20999, lo que facilita identificar y gestionar situaciones específicas del negocio de forma controlada desde el bloque de excepciones.

```sql
RAISE_APPLICATION_ERROR(-20001, 'Error personalizado');
```


*Ejemplo*

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_salary       HR.EMPLOYEES.SALARY%TYPE;
    v_avg_salary   NUMBER;
    v_emp_id       HR.EMPLOYEES.EMPLOYEE_ID%TYPE := 100; -- prueba cambiarlo

BEGIN
    -- 1. SELECT INTO (puede fallar con NO_DATA_FOUND o TOO_MANY_ROWS)
    SELECT salary
    INTO v_salary
    FROM HR.EMPLOYEES
    WHERE employee_id = v_emp_id;

    DBMS_OUTPUT.PUT_LINE('Salario del empleado: ' || v_salary);

    -- 2. Cálculo con riesgo de ZERO_DIVIDE
    SELECT AVG(salary)
    INTO v_avg_salary
    FROM HR.EMPLOYEES;

    DBMS_OUTPUT.PUT_LINE('Media salarial: ' || v_avg_salary);

    DBMS_OUTPUT.PUT_LINE('Ratio salario / media: ' || (v_salary / v_avg_salary));

EXCEPTION
    -- No se encontró el empleado
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('❌ No existe el empleado con ID: ' || v_emp_id);

    -- Más de una fila (ej: si se cambia la condición a no única)
    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('❌ La consulta devolvió más de un empleado');

    -- División por cero
    WHEN ZERO_DIVIDE THEN
        DBMS_OUTPUT.PUT_LINE('❌ Error: división por cero en cálculo de media');

    -- Captura genérica con diagnóstico completo
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('❌ Error inesperado');
        DBMS_OUTPUT.PUT_LINE('Código: ' || SQLCODE);
        DBMS_OUTPUT.PUT_LINE('Mensaje: ' || SQLERRM);

        -- Ejemplo de error controlado para el sistema
        RAISE_APPLICATION_ERROR(
            -20001,
            'Error en el cálculo de salario para el empleado ' || v_emp_id
        );
END;
/
```

**Ejercicio propuesto:** Crear un bloque anónimo que calcule la edad de un empleado a partir de su fecha de nacimiento (puedes añadir una columna ficticia o usar una variable).

💡 Pista:

* Usa `SYSDATE`
* Usa funciones de fecha como `MONTHS_BETWEEN`

## 5. Subprogramas Almacenados
Son bloques con nombre que se guardan en el servidor para ser reutilizados.

### 5.1. Procedimientos (`PROCEDURE`)

**Ejemplo.**

```sql
CREATE OR REPLACE PROCEDURE subir_salario(p_id NUMBER) IS
BEGIN
  UPDATE employees
  SET salary = salary * 1.1
  WHERE employee_id = p_id;
END;
```

**Ejecución.**

```sql
BEGIN
  subir_salario(100);
END;
```

**Ejemplo.**

```sql
CREATE OR REPLACE PROCEDURE ejemplo_procedimiento IS
  v_ciudad VARCHAR2(20) := 'Granada';
  c_pi CONSTANT NUMBER := 3.1416;
  v_nombre employees.first_name%TYPE;
BEGIN
  SELECT first_name
  INTO v_nombre
  FROM employees
  WHERE employee_id = 100;

  dbms_output.put_line(v_nombre || ' vive en ' || v_ciudad);

EXCEPTION
  WHEN OTHERS THEN
    dbms_output.put_line('Error en el procedimiento');
END;
```

Un procedimiento almacenado es un conjunto de instrucciones SQL y estructuras de control que se compilan y guardan en el servidor. A diferencia de una consulta simple, un procedimiento puede recibir parámetros, realizar múltiples operaciones sobre diferentes tablas y devolver resultados al usuario o aplicación.
Se invocan con `CALL` o `EXEC`

**Estructura de un procedimiento almacenado.**

Independientemente del SGBD, un procedimiento consta de:  

1.  **Cabecera:** Nombre del procedimiento y definición de parámetros (entrada/salida).
2.  **Bloque de código PL/SQL:** El bloque de código responsable de la lógica de negocio, habitualmente entre palabras clave `BEGIN` y `END`.

**Ventajas de su uso.**

* **Reducción del tráfico de red:** En lugar de enviar 50 líneas de código SQL desde una aplicación Java/Python, solo enviamos una instrucción: `CALL mi_procedimiento()`.
* **Seguridad mejorada:** El administrador puede quitar permisos de `DELETE` o `UPDATE` sobre las tablas a los usuarios y, en su lugar, darles permiso de `EXECUTE` sobre un procedimiento que realiza la operación de forma controlada.
* **Mantenimiento centralizado:** Si una regla de negocio cambia (por ejemplo, el cálculo de una comisión), solo modificamos el procedimiento en la base de datos y todas las aplicaciones que lo usan se actualizan automáticamente.

**Parámetros.**

Es fundamental entender cómo fluye la información entre el SGBD y el procedimiento. En la programación de bases de datos, los parámetros se definen en la cabecera del procedimiento. Cada parámetro debe tener un **nombre**, un **tipo de dato** y un **modo**.

Existen tres formas de gestionar el paso de información, conocidas como modos:

1.  **`IN` (Entrada):**
    * Es el modo por defecto si no se especifica ninguno.
    * El valor se pasa desde el exterior hacia el procedimiento.
    * Dentro del procedimiento, el parámetro actúa como una **constante de solo lectura**: no se puede modificar su valor para que el cambio se refleje fuera.
2.  **`OUT` (Salida):**
    * Se utiliza para devolver datos al programa que realizó la llamada.
    * Al entrar al procedimiento, su valor es inicialmente `NULL`.
    * Cualquier cambio realizado en este parámetro dentro del código se asignará a la variable exterior al terminar la ejecución.
3.  **`INOUT` (Entrada/Salida):**
    * Combina ambos comportamientos.
    * El procedimiento recibe un valor inicial, puede procesarlo o modificarlo, y el resultado final se devuelve al exterior.

| Característica | MySQL | Oracle (PL/SQL) |
| :--- | :--- | :--- |
| **Sintaxis** | `(IN p1 tipo, OUT p2 tipo)` | `(p1 IN tipo, p2 OUT tipo)` |
| **Valor por defecto** | Solo aplicable a parámetros `IN`. | Muy flexible (ej. `p1 IN tipo := valor`). |
| **Llamada (Variables)** | Requiere variables de usuario (`@var`). | Requiere variables declaradas en bloque. |


!!! note "🛠️ Actividad: El proceso de un pedido"

    Vamos a crear un escenario donde recibimos un precio (`IN`), le aplicamos un descuento (`INOUT`) y devolvemos el total de impuestos acumulados (`OUT`).

    #### 1. En MySQL (Uso de los tres modos)
    ```sql
    DELIMITER //

    CREATE PROCEDURE procesar_precio(
        IN    p_precio_base DECIMAL(10,2),
        INOUT p_descuento   DECIMAL(10,2), -- Entra como porcentaje, sale como ahorro
        OUT   p_impuestos   DECIMAL(10,2)
    )
    BEGIN
        -- Calculamos cuánto se ahorra el cliente
        DECLARE v_ahorro DECIMAL(10,2);
        SET v_ahorro = p_precio_base * (p_descuento / 100);
        
        -- Calculamos impuestos sobre el precio final (ej. 21%)
        SET p_impuestos = (p_precio_base - v_ahorro) * 0.21;
        
        -- Devolvemos el ahorro en el parámetro INOUT
        SET p_descuento = v_ahorro;
    END //

    DELIMITER ;

    -- Ejecución en MySQL:
    SET @mi_descuento = 15; -- Queremos un 15% de descuento
    CALL procesar_precio(100.00, @mi_descuento, @mis_impuestos);

    SELECT @mi_descuento AS Ahorro, @mis_impuestos AS IVA;
    ```

    #### 2. En Oracle (PL/SQL)
    Oracle permite definir valores por defecto en la cabecera, lo que hace que algunos parámetros sean opcionales.

    ```sql
    CREATE OR REPLACE PROCEDURE calcular_puntos(
        p_euros   IN     NUMBER,
        p_multi   IN     NUMBER DEFAULT 1, -- Parámetro opcional
        p_puntos  OUT    NUMBER
    ) IS
    BEGIN
        p_puntos := p_euros * p_multi;
    END;
    /

    -- Ejecución en un bloque anónimo:
    DECLARE
        v_res NUMBER;
    BEGIN
        -- Llamada usando el valor por defecto de p_multi
        calcular_puntos(50, p_puntos => v_res); 
        DBMS_OUTPUT.PUT_LINE('Puntos obtenidos: ' || v_res);
    END;
    /
    ```

#### 📝 Actividades Propuestas

1.  **Intercambio de Valores:** Crea un procedimiento en **MySQL** llamado `swap` que reciba dos parámetros `INOUT` (a y b) e intercambie sus valores. Pruébalo con dos variables de usuario.
2.  **Contador de Caracteres:** Crea un procedimiento en **Oracle** que reciba una cadena (`IN`) y devuelva mediante un parámetro `OUT` la longitud de dicha cadena.
3.  **Cajero Automático:** Diseña un procedimiento `retirar_efectivo` que reciba el `saldo_actual` como `INOUT` y la `cantidad_a_retirar` como `IN`. 
    * Si hay saldo suficiente, resta la cantidad.
    * Si no lo hay, no hagas nada y devuelve el saldo original.
4.  **Investigación - Paso por Referencia vs Valor:** Investiga qué significa el modificador `NOCOPY` en los parámetros de **Oracle**. ¿En qué casos mejora el rendimiento del procedimiento?
5.  **Refactorización:** Toma la función `calcular_iva` y conviértela en un procedimiento que utilice un parámetro `OUT` para devolver el resultado. Discute con tus compañeros: ¿Qué formato es más cómodo para el programador?

!!! note "🛠️ Actividad: Gestión de stock crítico"

    Vamos a crear un procedimiento que gestione una venta y, si el stock baja de un límite, nos avise a través de un parámetro de salida.

    #### 1. En MySQL (Implementación con lógica de negocio)
    ```sql
    DELIMITER //

    CREATE PROCEDURE realizar_venta(
        IN p_producto_id INT,
        IN p_cantidad INT,
        OUT p_estado VARCHAR(50)
    )
    BEGIN
        DECLARE v_stock_actual INT;

        -- Obtener stock actual
        SELECT stock INTO v_stock_actual 
        FROM productos 
        WHERE id = p_producto_id;

        -- Comprobar si hay suficiente
        IF v_stock_actual >= p_cantidad THEN
            UPDATE productos 
            SET stock = stock - p_cantidad 
            WHERE id = p_producto_id;
            
            SET p_estado = 'Venta realizada';
        ELSE
            SET p_estado = 'Error: Stock insuficiente';
        END IF;
    END //

    DELIMITER ;
    ```

En Oracle es posible utilizar `IN OUT` para variables que queremos transformar.

*Ejemplo* 

```sql
CREATE OR REPLACE PROCEDURE aplicar_formato_precio(
    p_precio IN OUT NUMBER,
    p_divisa  IN     VARCHAR2
) IS
BEGIN
    -- Añadimos un cargo de gestión y redondeamos
    p_precio := ROUND(p_precio * 1.05, 2);
    
    DBMS_OUTPUT.PUT_LINE('Precio ajustado para divisa: ' || p_divisa);
END;
/
``` 

```sql
-- Para probarlo necesitamos un bloque anónimo:
DECLARE
    v_mi_precio NUMBER := 100;
BEGIN
    aplicar_formato_precio(v_mi_precio, 'EUR');
    DBMS_OUTPUT.PUT_LINE('El nuevo precio es: ' || v_mi_precio);
END;
/
```

#### 📝 Actividades Propuestas

1.  **Procedimiento de Alta de Alumnos:** Crea un procedimiento en **MySQL** que reciba nombre, apellidos y email. El procedimiento debe insertar el registro en la tabla `ALUMNOS` y devolver el `ID` generado automáticamente mediante un parámetro `OUT`. (Pista: usa `LAST_INSERT_ID()`).
2.  **Seguridad y Definición:** Investiga la cláusula `SQL SECURITY` en MySQL. ¿Qué diferencia hay entre un procedimiento creado con `SQL SECURITY DEFINER` y uno con `SQL SECURITY INVOKER`?
3.  **Transferencia de Fondos:** Diseña un procedimiento llamado `transferencia` que reciba `cuenta_origen`, `cuenta_destino` y `cantidad`. Debe restar el dinero de una cuenta y sumarlo en la otra.
4.  **Gestión de Catálogo:** En **Oracle**, crea un procedimiento que reciba el nombre de una tabla y nos imprima cuántos registros tiene utilizando SQL dinámico (`EXECUTE IMMEDIATE`). *Nota: Este es un ejercicio de ampliación para investigar cómo manejar nombres de tablas como variables.*



### 5.2. Funciones (`FUNCTION`)

```sql
CREATE OR REPLACE FUNCTION salario_anual(p_id NUMBER)
RETURN NUMBER IS
  v_salario NUMBER;
BEGIN
  SELECT salary INTO v_salario
  FROM employees
  WHERE employee_id = p_id;

  RETURN v_salario * 12;
END;
```
Uso:

```sql
SELECT salario_anual(100) FROM dual;
```

Una función es un objeto programable que, tras recibir cero o más parámetros, realiza un procesamiento y **debe devolver obligatoriamente un único valor** de un tipo de dato simple (escalar).

#### A. Diferencias clave con los procedimientos

| Característica | Procedimiento (`PROCEDURE`) | Función (`FUNCTION`) |
| :--- | :--- | :--- |
| **Retorno** | Puede devolver varios valores (`OUT`) o ninguno. | Debe devolver **siempre** un valor (`RETURN`). |
| **Invocación** | Se llama con la sentencia `CALL` o `EXEC`. | Se usa dentro de expresiones SQL (`SELECT`, `WHERE`). |
| **Uso en SQL** | No se puede usar dentro de un `SELECT`. | Se integra como una función más del sistema. |
| **Propósito** | Ejecutar lógica de negocio y acciones DML. | Realizar cálculos y transformaciones de datos. |

#### B. Restricciones y determinismo.

A diferencia de los procedimientos, las funciones suelen tener restricciones más estrictas para evitar efectos secundarios inesperados en la base de datos:

* **Efectos secundarios:** En muchos SGBD (como Oracle), no se permiten sentencias `INSERT`, `UPDATE` o `DELETE` dentro de una función si esta se va a usar en una consulta `SELECT`.
* **Determinismo:** Se dice que una función es **determinista** si para los mismos valores de entrada siempre devuelve el mismo resultado (ej. una función que calcula el IVA). Si la función depende de datos que cambian (como la hora del sistema), es **no determinista**.

!!! note "🛠️ Actividad: Transformación y cálculo"

    Vamos a crear una función que determine si un alumno ha promocionado y otra que limpie formatos de texto.

    #### 1. En MySQL (Cálculo de estado académico)
    En MySQL, para crear funciones es necesario especificar si afectan a los datos (`READS SQL DATA`) o si son deterministas.

    ```sql
    DELIMITER //

    CREATE FUNCTION estado_nota(p_nota DECIMAL(4,2)) 
    RETURNS VARCHAR(20)
    DETERMINISTIC
    BEGIN
        DECLARE v_resultado VARCHAR(20);
        
        IF p_nota >= 5 THEN
            SET v_resultado = 'APROBADO';
        ELSE
            SET v_resultado = 'SUSPENSO';
        END IF;
        
        RETURN v_resultado;
    END //

    DELIMITER ;

    -- Uso en una consulta:
    SELECT nombre, nota, estado_nota(nota) AS estado FROM calificaciones;
    ```

    #### 2. En Oracle (Formateo de NIE/DNI)
    PL/SQL es muy eficiente manejando cadenas. Aquí no necesitamos indicar si es determinista para un uso básico.

    ```sql
    CREATE OR REPLACE FUNCTION formatear_id(p_identificador VARCHAR2) 
    RETURN VARCHAR2 
    IS
    BEGIN
        -- Pasamos a mayúsculas y quitamos espacios en blanco
        RETURN UPPER(TRIM(p_identificador));
    END;
    /

    -- Uso en una consulta:
    SELECT formatear_id(nie), apellidos FROM alumnos;
    ```

#### 📝 Actividades Propuestas

1.  **Conversor de Divisas:** Crea una función en **MySQL** llamada `convertir_a_dolares` que reciba una cantidad en euros y devuelva la conversión (usa un tipo de cambio fijo de 1.08).
2.  **Validación de Edad:** Crea una función en **Oracle** que reciba una fecha de nacimiento y devuelva un valor booleano (o un número 0/1) indicando si la persona es mayor de edad en la fecha actual.
3.  **Funciones y Agregación:** Utiliza la función `estado_nota` creada anteriormente en una consulta que cuente cuántos alumnos han aprobado ('APROBADO') y cuántos han suspendido ('SUSPENSO'). Pista: usa `GROUP BY` sobre la función).
4.  **Investigación - Limitaciones:** Intenta crear una función en MySQL que realice un `UPDATE` en una tabla. ¿Qué ocurre cuando intentas usar esa función dentro de un `SELECT`? Documenta el error obtenido.
5.  **Anidación:** Crea una función que calcule el área de un círculo llamando internamente a otra función que devuelva el valor de PI.




## 6. Tratamiento de Datos: Cursores.

Los **cursores** en PL/SQL son estructuras que permiten **procesar el resultado de una consulta SQL**. Oracle reserva un área de trabajo en memoria para poder gestionar el resultado de la consulta.

### 6.1. Cursores implícitos.

Son creados automáticamente por Oracle cuando se ejecutan sentencias SQL como:

* `INSERT`
* `UPDATE`
* `DELETE`
* `SELECT INTO`

Solo devuelven **una fila** y **no necesitan declaración** por parte del programador.

*Ejemplo.* 

```sql
BEGIN
  UPDATE employees
  SET salary = salary * 1.1
  WHERE department_id = 10;

  DBMS_OUTPUT.PUT_LINE('Filas modificadas: ' || SQL%ROWCOUNT);
END;
```
Oracle gestiona el cursor internamente y se puede consultar su estado mediante atributos como `SQL%ROWCOUNT`.

*Ejemplo.*

```sql
DECLARE
  v_salario employees.salary%TYPE;
BEGIN
  SELECT salary
  INTO v_salario
  FROM employees
  WHERE employee_id = 100;

  dbms_output.put_line('Salario: ' || v_salario);
END;
```

### 6.2. Cursores explícitos.
Se utilizan cuando en el procesamiento es necesario el recorrido de los datos. Son definidos por el programador según las necesitades de gestión y permiten recorrer **múltiples filas**, ofreciendo mayor control y rendimiento.  
Pueden lanzar excepciones:

  * `NO_DATA_FOUND`
  * `TOO_MANY_ROWS`

La estructura de un cursor explícito incluye:

1. **Declaración**
2. **Apertura**
3. **Recuperación de datos (FETCH)**
4. **Cierre**

*Ejemplo.* 

```sql
DECLARE
  CURSOR c_empleados IS
    SELECT employee_id, first_name, salary
    FROM employees;

  v_id employees.employee_id%TYPE;
  v_nombre employees.first_name%TYPE;
  v_salario employees.salary%TYPE;

BEGIN
  OPEN c_empleados;
  LOOP
    FETCH c_empleados INTO v_id, v_nombre, v_salario;
    EXIT WHEN c_empleados%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE(v_nombre || ' cobra ' || v_salario);
  END LOOP;
  CLOSE c_empleados;
END;
```
*Ejemplo.*

```sql
DECLARE
  CURSOR c_emp IS
  SELECT first_name, salary FROM employees;
  v_nombre employees.first_name%TYPE;
  v_salario employees.salary%TYPE;
BEGIN
  OPEN c_emp;
  LOOP
    FETCH c_emp INTO v_nombre, v_salario;
    EXIT WHEN c_emp%NOTFOUND;

    dbms_output.put_line(v_nombre || ' - ' || v_salario);
  END LOOP;
  CLOSE c_emp;
END;
```
#### *Atributos*.

Los cursores disponen de **atributos** que permiten controlar su estado:

| Atributo    | Descripción                                |
| :---------- | :----------------------------------------- |
| `%FOUND`    | Devuelve TRUE si se ha recuperado una fila |
| `%NOTFOUND` | Devuelve TRUE si no hay más filas          |
| `%ROWCOUNT` | Número de filas procesadas                 |
| `%ISOPEN`   | Indica si el cursor está abierto           |

*Ejemplo.*
```sql
DECLARE
  CURSOR c_emp IS
    SELECT employee_id, first_name, salary
    FROM employees
    WHERE department_id = 50;

  v_emp c_emp%ROWTYPE;

BEGIN
  -- Comprobamos si el cursor está abierto
  IF NOT c_emp%ISOPEN THEN
    OPEN c_emp;
  END IF;

  LOOP
    FETCH c_emp INTO v_emp;

    -- Si no hay más filas, salimos
    EXIT WHEN c_emp%NOTFOUND;

    -- Mostramos información del empleado
    DBMS_OUTPUT.PUT_LINE(
      'Empleado: ' || v_emp.first_name ||
      ' | Salario: ' || v_emp.salary
    );

    -- Mostramos cuántas filas llevamos procesadas
    DBMS_OUTPUT.PUT_LINE(
      'Filas procesadas hasta ahora: ' || c_emp%ROWCOUNT
    );

  END LOOP;

  -- Verificamos si el cursor sigue abierto antes de cerrarlo
  IF c_emp%ISOPEN THEN
    CLOSE c_emp;
  END IF;

  DBMS_OUTPUT.PUT_LINE('Proceso finalizado');

END;

```
#### *Parámetros.*

Los cursores pueden recibir **parámetros** para filtrar datos de manera dinámica.

*Ejemplo.*

```sql
DECLARE
  CURSOR c_empleados(p_dept NUMBER) IS
    SELECT first_name, salary
    FROM employees
    WHERE department_id = p_dept;

BEGIN
  FOR reg IN c_empleados(10) LOOP
    DBMS_OUTPUT.PUT_LINE(reg.first_name || ' - ' || reg.salary);
  END LOOP;
END;
```
```sql
DECLARE
  CURSOR c_emp (p_dept NUMBER) IS
    SELECT first_name, salary
    FROM employees
    WHERE department_id = p_dept;

  v_emp c_emp%ROWTYPE;
BEGIN
  OPEN c_emp(50);

  LOOP
    FETCH c_emp INTO v_emp;
    EXIT WHEN c_emp%NOTFOUND;

    dbms_output.put_line(v_emp.first_name);
  END LOOP;

  CLOSE c_emp;
END;
```


#### *Cursor FOR LOOP (implícito simplificado)*

Oracle permite una forma más sencilla de recorrer cursores sin `OPEN`, `FETCH` ni `CLOSE`.

*Ejemplo*

```sql
BEGIN
  FOR reg IN (SELECT first_name, salary FROM employees) LOOP
    DBMS_OUTPUT.PUT_LINE(reg.first_name || ' - ' || reg.salary);
  END LOOP;
END;
```

En este caso:

* Oracle abre el cursor automáticamente
* Realiza el `FETCH`
* Y lo cierra al finalizar

### 6.3. Ventajas e inconvenientes.

#### *Ventajas.*

* Permiten procesar datos fila a fila.
* Facilitan lógica compleja sobre resultados SQL.
* Se integran directamente con estructuras PL/SQL.
* Son útiles cuando no se puede resolver con SQL puro.

#### *Inconvenientes.*

* Menor rendimiento frente a operaciones en conjunto (SQL set-based).
* Mayor consumo de recursos si no se usan correctamente.
* Código más extenso y complejo que un `SELECT` simple.


!!! note "🛠️ Actividad: Aumento de salarios por departamento"

    ```sql 
    DECLARE
      CURSOR c_emp IS
        SELECT employee_id, salary
        FROM employees
        WHERE department_id = 20;

    BEGIN
      FOR reg IN c_emp LOOP
        UPDATE employees
        SET salary = reg.salary * 1.05
        WHERE employee_id = reg.employee_id;
      END LOOP;
    END;
    ```

#### 📝 Actividades Propuestas

1. **Listado de alumnos:**
   Crea un cursor que recorra la tabla `ALUMNOS` e imprima nombre y nota media.
2. **Incremento salarial condicionado:**
   Diseña un cursor que aumente un 10% el salario solo a empleados con salario inferior a 2000.
3. **Conteo de registros:**
   Utiliza un cursor y el atributo `%ROWCOUNT` para contar cuántos empleados hay en un departamento concreto.
4. **Cursor con parámetro:**
   Crea un cursor que reciba el nombre de una ciudad y liste los empleados que trabajan en ella.
5. **Reflexión:**
   Compara el uso de un cursor con una sentencia SQL equivalente (set-based). ¿Cuál es más eficiente y por qué?





## 7. Automatización. Triggers y eventos
### 7.1. Triggers (Disparadores)

*Ejemplo.*

```sql
CREATE OR REPLACE TRIGGER trg_salario
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  dbms_output.put_line('Salario actualizado');
END;
```

Los **disparadores o triggers** son bloques de código PL/SQL almacenados en la base de datos que **se ejecutan automáticamente cuando ocurre un evento específico sobre una tabla, vista o esquema**.

A diferencia de los procedimientos y funciones, los triggers **no se invocan manualmente**, sino que se ejecutan en respuesta a acciones del sistema.

Estos eventos suelen ser operaciones de manipulación de datos (**DML**), como `INSERT`, `UPDATE`, `DELETE`. 

#### *Estructura.*

Un **trigger** se compone de:  

1. **Momento de ejecución.**
    
      * `BEFORE`: antes del evento
      * `AFTER`: después del evento  
  
2. **Evento.**

      * `INSERT`
      * `UPDATE`
      * `DELETE`
  
3. **Nivel de ejecución.**

       * Nivel de fila. Se ejecuta por cada fila afectada. `FOR EACH ROW`
       * Nivel de sentencia. Se ejecuta una sola vez por sentencia. Es la opción sin la cláusula `FOR EACH ROW`
   
4. **Bloque PL/SQL**. Contiene la lógica que se ejecuta automáticamente.


#### *Pseudoregistros :OLD y :NEW*

En los triggers de nivel fila se utilizan dos pseudoregistros (:OLD y :NEW) que permiten acceder a los valores antiguos y nuevos de cada fila afectada durante la ejecución del trigger.

| Pseudoregistro | Significado                                    |
| :------------- | :--------------------------------------------- |
| `:OLD`         | Valores antiguos de la fila (antes del cambio) |
| `:NEW`         | Valores nuevos de la fila (después del cambio) |

* En `UPDATE`: existen ambos
* En `INSERT`: solo existe `:NEW`
* En `DELETE`: solo existe `:OLD`


*Ejemplo.*

```sql
CREATE OR REPLACE TRIGGER trg_actualizar_fecha
BEFORE UPDATE ON employees
FOR EACH ROW
BEGIN
  NEW.last_update := SYSDATE;:
END;
```


*Ejemplo.*

```sql
CREATE OR REPLACE TRIGGER trg_auditar_borrado
AFTER DELETE ON employees
FOR EACH ROW
BEGIN
  INSERT INTO auditoria_empleados (employee_id, fecha_borrado)
  VALUES (:OLD.employee_id, SYSDATE);
END;
```


#### *Tipos.*

Según el momento de ejecución:

* **BEFORE trigger**

      * Se ejecuta antes de la operación
      * Se usa para validaciones o modificaciones previas

* **AFTER trigger**

    * Se ejecuta después de la operación
    * Se usa para auditoría o acciones derivadas

Según el nivel:

* **Row-level (FOR EACH ROW)**

    * Se ejecuta por cada fila afectada  
  
* **Statement-level**

    * Se ejecuta una sola vez por sentencia

### 7.2 Ventajas e inconvenientes.
#### *Ventajas.*
* **Automatización de tareas**. Por ejemplo, auditoría automática de cambios.
* **Integridad de datos**. Validaciones que no dependen de la aplicación.
* **Centralización de reglas de negocio**. Se aplican independientemente del programa cliente.
* **Transparencia para el usuario**. El usuario no necesita ejecutar nada adicional.

#### *Inconvenientes.*

* Pueden hacer que la lógica de negocio sea menos visible
* Dificultan la depuración si se abusa de ellos
* Pueden afectar al rendimiento si son complejos o se ejecutan por fila
* No siempre son portables entre SGBD


!!! note "🛠️ Actividad: Control de stock automático."

    Vamos a crear un trigger que impida vender más productos de los disponibles.  

    ```sql 
    CREATE OR REPLACE TRIGGER trg_control_stock
    BEFORE INSERT ON ventas
    FOR EACH ROW
    DECLARE
        v_stock NUMBER;
      BEGIN
        SELECT stock INTO v_stock
        FROM productos
        WHERE id = :NEW.producto_id;

        IF v_stock < :NEW.cantidad THEN
          RAISE_APPLICATION_ERROR(-20001, 'Stock insuficiente');
        END IF;
      END;
    ```

!!! note "🛠️ Actividad: Auditoría de cambios de precio"

    ```sql
    CREATE OR REPLACE TRIGGER trg_auditar_precio
    AFTER UPDATE OF precio ON productos
    FOR EACH ROW
    BEGIN
      INSERT INTO log_precios (producto_id, precio_antiguo, precio_nuevo, fecha)
      VALUES (:OLD.id, :OLD.precio, :NEW.precio, SYSDATE);
    END;
    ```

#### 📝 Actividades Propuestas

1. **Auditoría de inserciones:**
   Crea un trigger que registre en una tabla `LOG_ALUMNOS` cada vez que se inserte un nuevo alumno.

2. **Control de salario mínimo:**
   Diseña un trigger que impida que el salario de un empleado sea inferior a 1.000 €.

3. **Actualización automática:**
   Crea un trigger que actualice el campo `updated_at` cada vez que se modifique un registro en una tabla.

4. **Investigación - mutating table:**
   Investiga el error *“mutating table”* en Oracle. ¿Cuándo ocurre y cómo se puede evitar?

5. **Comparativa:**
   Explica las diferencias entre implementar una regla de negocio con:  

      * Trigger
      * Procedimiento almacenado
      * Código en la aplicación

### 7.2. Eventos.

Los **eventos (Events)** en MySQL son objetos programables que permiten ejecutar automáticamente sentencias SQL en momentos concretos o de forma periódica.  

A diferencia de los *triggers*, que reaccionan a cambios en los datos (INSERT, UPDATE, DELETE), los eventos reaccionan al **tiempo**, no a los datos. Esto significa que forman parte del sistema de **automatización temporal** de la base de datos.

Los eventos son útiles para tareas automáticas que deben ejecutarse de forma programada, como por ejemplo:

* Limpieza de registros antiguos (logs, auditorías, temporales)
* Generación de informes diarios o mensuales
* Actualización periódica de datos
* Mantenimiento automático de tablas
* Archivado de información

Un evento se define indicando:

* **Cuándo empieza a ejecutarse**
* **Cada cuánto tiempo se repite (opcional)
* **Qué instrucciones SQL debe ejecutar**

Para que funcionen, el **planificador de eventos debe estar activado** en MySQL:

```sql
SET GLOBAL event_scheduler = ON;
```

#### *Sintaxis básica.*

```sql 
CREATE EVENT nombre_evento
ON SCHEDULE EVERY intervalo
DO
    instrucción_sql;
```

*Ejemplo: limpieza de logs antiguos.*

```sql 
CREATE EVENT limpiar_logs
ON SCHEDULE EVERY 1 DAY
DO
    DELETE FROM logs
    WHERE fecha < NOW() - INTERVAL 30 DAY;
```

👉 Este evento se ejecuta una vez al día y elimina registros con más de 30 días de antigüedad.


*Ejemplo: ejecución única (evento puntual)*

```sql 
CREATE EVENT generar_reporte_inicial
ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 1 HOUR
DO
    CALL generar_reporte();
```

👉 Se ejecuta una sola vez una hora después de su creación.


####  *Gestión de eventos.*

```sql 
--Activar evento
ALTER EVENT nombre_evento ENABLE;
```

```sql 
--Desactivar evento
ALTER EVENT nombre_evento DISABLE;
```

```sql 
--Eliminar evento
DROP EVENT nombre_evento;
```

En Oracle, la automatización temporal se realiza principalmente con **DBMS_SCHEDULER** 

Es el mecanismo oficial y más potente para programar tareas.

Permite ejecutar:

* Procedimientos
* Bloques PL/SQL
* Scripts
* Jobs periódicos o puntuales

#### *Ejemplo: tarea diaria (equivalente a un evento MySQL)*

```sql 
BEGIN
  DBMS_SCHEDULER.create_job (
    job_name        => 'limpiar_logs',
    job_type        => 'PLSQL_BLOCK',
    job_action      => 'DELETE FROM logs WHERE fecha < SYSDATE - 30',
    start_date      => SYSTIMESTAMP,
    repeat_interval => 'FREQ=DAILY;INTERVAL=1',
    enabled         => TRUE
  );
END;
/
```

👉 Esto ejecuta una limpieza diaria automática.

## 8. Objetos Avanzados
### 8.1 Packages (Oracle)

En Oracle, los *packages* (paquetes) son estructuras que permiten agrupar lógicamente elementos relacionados de PL/SQL, como procedimientos, funciones, variables, cursores y excepciones. Su principal objetivo es mejorar la organización, reutilización y mantenimiento del código dentro de la base de datos.

Un paquete se compone de dos partes principales:

**1. Especificación del paquete (Package Specification)**
Es la interfaz pública del paquete. En ella se declaran los elementos que serán accesibles desde fuera del paquete, como procedimientos, funciones, tipos de datos y constantes. Todo lo declarado en esta sección puede ser utilizado por otros programas.

**2. Cuerpo del paquete (Package Body)**
Contiene la implementación de los elementos definidos en la especificación, así como elementos privados que no son visibles desde el exterior. Aquí se escribe la lógica de los procedimientos y funciones.

#### *Ventajas de los paquetes en Oracle.*

* **Modularidad:** Permiten dividir el código en unidades lógicas más manejables.
* **Encapsulación:** Ocultan la implementación interna y exponen solo lo necesario.
* **Reutilización:** Facilitan el uso de código común en diferentes aplicaciones.
* **Mejor rendimiento:** Oracle carga los paquetes en memoria, lo que reduce el tiempo de acceso.
* **Mantenimiento sencillo:** Los cambios en el cuerpo del paquete no afectan a la interfaz pública.

*Ejemplo básico.*

```sql
CREATE OR REPLACE PACKAGE ejemplo_pkg AS
   PROCEDURE mostrar_mensaje;
   FUNCTION sumar(a NUMBER, b NUMBER) RETURN NUMBER;
END ejemplo_pkg;
/

CREATE OR REPLACE PACKAGE BODY ejemplo_pkg AS

   PROCEDURE mostrar_mensaje IS
   BEGIN
      DBMS_OUTPUT.PUT_LINE('Hola desde el paquete');
   END;

   FUNCTION sumar(a NUMBER, b NUMBER) RETURN NUMBER IS
   BEGIN
      RETURN a + b;
   END;

END ejemplo_pkg;
/
```

En este ejemplo, la especificación define los elementos disponibles, mientras que el cuerpo contiene su implementación.

En resumen, los paquetes en Oracle son una herramienta fundamental para estructurar aplicaciones PL/SQL de forma eficiente, segura y escalable.

### 8.2 **SQL Dinámico:**

El **SQL dinámico** en Oracle es una técnica de PL/SQL que permite construir y ejecutar sentencias SQL en tiempo de ejecución, en lugar de definirlas de forma fija en tiempo de compilación. 

Se emplea cuando:

* La sentencia SQL depende de valores introducidos por el usuario.
* El nombre de tablas o columnas es variable.
* Se necesitan consultas flexibles o generadas en tiempo de ejecución.
* Se trabaja con DDL (CREATE, ALTER, DROP), que no se permite directamente en PL/SQL estático.
  
La forma recomendada por su simplicidad y rendimiento es utilizando `EXECUTE IMMEDIATE`.

```sql
BEGIN
   EXECUTE IMMEDIATE 'CREATE TABLE prueba (id NUMBER, nombre VARCHAR2(50))';
END;
/
```

*Ejemplo con variables.*

```sql
DECLARE
   v_sql VARCHAR2(200);
   v_nombre VARCHAR2(50) := 'Juan';
BEGIN
   v_sql := 'INSERT INTO empleados (nombre) VALUES (:1)';
   EXECUTE IMMEDIATE v_sql USING v_nombre;
END;
/
```
*Ejemplo de SQL dinámico con retorno de valores.*

```sql
DECLARE
   v_sql VARCHAR2(200);
   v_total NUMBER;
BEGIN
   v_sql := 'SELECT COUNT(*) FROM empleados';
   EXECUTE IMMEDIATE v_sql INTO v_total;

   DBMS_OUTPUT.PUT_LINE('Total empleados: ' || v_total);
END;
/
```

### 8.3 **Tipos avanzados (Oracle PL/SQL)**

En Oracle, los **tipos avanzados de datos** permiten trabajar con estructuras más complejas que los tipos básicos (NUMBER, VARCHAR2, DATE). Se utilizan para modelar información más rica, mejorar la organización del código y facilitar el manejo de datos en PL/SQL.

Estos tipos son fundamentales en el desarrollo de aplicaciones más grandes y profesionales dentro de Oracle.

***Registros (RECORD*)**

Un **registro** es una estructura que agrupa varios campos de distintos tipos bajo un mismo nombre, similar a una fila de una tabla.

```sql
DECLARE
   TYPE t_empleado IS RECORD (
      id NUMBER,
      nombre VARCHAR2(50),
      salario NUMBER
   );

   v_emp t_empleado;
BEGIN
   v_emp.id := 1;
   v_emp.nombre := 'Ana';
   v_emp.salario := 2000;

   DBMS_OUTPUT.PUT_LINE(v_emp.nombre);
END;
/
```

***Tipos basados en tablas (ROWTYPE)***

Permite crear variables que representan automáticamente la estructura de una tabla.

```sql
DECLARE
   v_emp empleados%ROWTYPE;
BEGIN
   SELECT * INTO v_emp
   FROM empleados
   WHERE id = 1;

   DBMS_OUTPUT.PUT_LINE(v_emp.nombre);
END;
/
```

Se adaptan automáticamente si la tabla cambia.

***Colecciones (Collections)***

Permiten almacenar múltiples valores en una sola variable. Hay tres tipos principales:

**VARRAY.** Arrays de tamaño fijo.

Tienen un tamaño máximo definido.

```sql
DECLARE
   TYPE t_numeros IS VARRAY(5) OF NUMBER;
   v_lista t_numeros := t_numeros(10, 20, 30);
BEGIN
   DBMS_OUTPUT.PUT_LINE(v_lista(1));
END;
/
```
**TABLE.** Tablas indexadas o asociativas.

Son dinámicas y permiten usar índices no consecutivos.

```sql 
DECLARE
   TYPE t_tabla IS TABLE OF VARCHAR2(50) INDEX BY PLS_INTEGER;
   v_nombres t_tabla;
BEGIN
   v_nombres(1) := 'Luis';
   v_nombres(5) := 'Marta';

   DBMS_OUTPUT.PUT_LINE(v_nombres(5));
END;
/
```

**NESTED TABLE.** Tablas anidadas.

Son más flexibles que VARRAY y pueden crecer dinámicamente.

```sql 
DECLARE
   TYPE t_lista IS TABLE OF NUMBER;
   v_nums t_lista := t_lista(1, 2, 3);
BEGIN
   v_nums.EXTEND;
   v_nums(4) := 4;

   DBMS_OUTPUT.PUT_LINE(v_nums.COUNT);
END;
/
```

***Tipos de objeto (OBJECT TYPE)***

Permiten crear estructuras más avanzadas similares a clases en programación orientada a objetos.

```sql 
CREATE OR REPLACE TYPE t_persona AS OBJECT (
   id NUMBER,
   nombre VARCHAR2(50)
);
/
```

Uso en PL/SQL:

```sql 
DECLARE
   v_persona t_persona;
BEGIN
   v_persona := t_persona(1, 'Carlos');

   DBMS_OUTPUT.PUT_LINE(v_persona.nombre);
END;
/
```


### 8.4 **BULK COLLECT (recolección masiva)**

En Oracle PL/SQL, **BULK COLLECT** es una técnica que permite recuperar múltiples filas de una consulta SQL en una sola operación y almacenarlas en una colección (como un *array*, *tabla asociativa* o *nested table*). Su objetivo principal es mejorar el rendimiento al reducir el número de cambios entre el motor SQL y el motor PL/SQL.

Normalmente, un `SELECT INTO` solo permite obtener una fila. Si se necesitan muchas filas, se usan cursores con bucles, lo que puede ser más lento.

**BULK COLLECT soluciona esto** al traer todos los registros de una vez.

*Ejemplo básico.*

```sql 
DECLARE
   TYPE t_nombres IS TABLE OF empleados.nombre%TYPE;
   v_nombres t_nombres;
BEGIN
   SELECT nombre
   BULK COLLECT INTO v_nombres
   FROM empleados;

   DBMS_OUTPUT.PUT_LINE(v_nombres.COUNT);
END;
/
```

En este ejemplo se cargan todos los nombres de la tabla en una colección.

*Ejemplo de BULK COLLECT con múltiples columnas (ROWTYPE)*

```sql 
DECLARE
   TYPE t_empleados IS TABLE OF empleados%ROWTYPE;
   v_emps t_empleados;
BEGIN
   SELECT *
   BULK COLLECT INTO v_emps
   FROM empleados;

   DBMS_OUTPUT.PUT_LINE(v_emps(1).nombre);
END;
/
```
*Ejemplo de BULK COLLECT con LIMIT (uso con cursores)*


```sql
DECLARE
   CURSOR c_emp IS SELECT * FROM empleados;
   TYPE t_emp IS TABLE OF empleados%ROWTYPE;
   v_emp t_emp;
BEGIN
   OPEN c_emp;

   LOOP
      FETCH c_emp BULK COLLECT INTO v_emp LIMIT 100;
      EXIT WHEN v_emp.COUNT = 0;

      DBMS_OUTPUT.PUT_LINE('Bloque cargado: ' || v_emp.COUNT);
   END LOOP;

   CLOSE c_emp;
END;
/
```
Cuando hay muchos datos, se puede usar `LIMIT` para evitar consumir demasiada memoria.En el ejemplo procesamos los datos por bloques de 100 filas.

