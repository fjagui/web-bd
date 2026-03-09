# El lenguaje SQL: Manipulación de Datos y Gestión de Transacciones.

## 1. Introducción al Tratamiento de Datos

El Lenguaje de Manipulación de Datos (DML) es la parte de SQL que nos permite interactuar con los datos ya existentes en la base de datos. Mientras que en unidades anteriores nos centramos en la definición de estructuras (DDL) y la consulta de información (SELECT), en esta unidad aprenderemos a alterar el estado de la base de datos de forma segura.

En un entorno profesional, la manipulación de datos no se limita a cambiar un valor; implica garantizar que la información sea siempre coherente, especialmente cuando múltiples usuarios acceden y modifican los mismos datos simultáneamente. Aquí es donde entra en juego la Gestión de Transacciones, un mecanismo que asegura que las operaciones complejas se realicen con éxito total o no se realicen en absoluto.  

Para los ejemplos de la unidad, utilizaremos un modelo simplificado basado en el funcionamiento de Bizum.

```sql
clientes (id, nombre, dni)  -- id = PK
cuentas (id, iban, saldo, banco, estado) -- id = PK
titulares_cuentas (id, id_cliente, id_cuenta, telefono)-- id = PK
transacciones_bizum (id, id_emisor_titular, id_receptor_titular, cantidad, fecha, concepto) -- id = PK
```


!!! note "🛠️ Actividad. Esquema relacional"

    Crea un script sql con el esquema relacional y desplíegalo en tu servidor de bases de datos.  

## 2. Inserción de Datos (`INSERT`)

La sentencia `INSERT` se utiliza para añadir nuevas filas a una tabla. En nuestro sistema de Bizum, es el comando que permite dar de alta a nuevos clientes, abrir sus cuentas y registrar cada movimiento de dinero.

### 2.1. Inserción de registros individuales

Se utiliza cuando conocemos los valores exactos que queremos introducir en cada columna de la tabla. Es fundamental seguir el orden de la integridad referencial (no puedes crear un titular si no existe antes el cliente y la cuenta).

**Sintaxis Genérica:**

```sql
INSERT INTO nombre_tabla (columna1, columna2, ...)
VALUES (valor1, valor2, ...);

```

**Ejemplos**. *Modelo simplificado bizum*

```sql
--Ejemplo. Insertamos un cliente.
INSERT INTO clientes (id, nombre, dni) 
VALUES (20, 'Beatriz Soler', '99887766K');

--Ejemplo. Creamos su cuenta bancaria.
INSERT INTO cuentas (id, iban, saldo, banco, estado) 
VALUES (30, 'ES4021001122334455667788', 150.00, 'Banco IT', 'ACTIVA');

--Ejemplo. Vinculamos al cliente con la cuenta para habilitar Bizum.
INSERT INTO titulares_cuentas (id, id_cliente, id_cuenta, telefono) 
VALUES (600, 20, 30, '622334455');

--Ejemplo. Registramos una transacción desde un titular existente (id: 1) a Beatriz (id: 600).
INSERT INTO transacciones_bizum (id, id_emisor_titular, id_receptor_titular, cantidad, fecha, concepto)
VALUES (2001, 1, 600, 15.50, SYSDATE, 'Cena viernes');
```

**Ejemplos**. *Modelo HR Oracle*

```sql
--Ejemplo. Insertamos un nuevo departamento de informática.
INSERT INTO DEPT (DEPTNO, DNAME, LOC) 
VALUES (50, 'INFORMATICA', 'MADRID');

--Ejemplo. Insertamos un nuevo empleado en ese departamento.
INSERT INTO EMP (EMPNO, ENAME, JOB, HIREDATE, SAL, DEPTNO) 
VALUES (8001, 'GARCIA', 'PROGRAMADOR', SYSDATE, 2500, 50);
```

### 2.2. Inserción mediante subconsultas

Permite insertar en una tabla el resultado de una consulta realizada sobre otras tablas. Esto es muy útil para procesos automáticos, como promociones o migraciones de datos.

**Sintaxis Genérica:**

```sql
INSERT INTO tabla_destino [(columna1, columna2, ...)]
SELECT columna1, columna2, ...
FROM tabla_origen
WHERE condicion;

```
**Ejemplos**. *Modelo simplificado bizum*


```sql
--Ejemplo. Queremos regalar 5€ a todos los clientes que tengan una cuenta en el 'Banco IT'.

INSERT INTO transacciones_bizum (id, id_emisor_titular, id_receptor_titular, cantidad, fecha, concepto)
SELECT 
    sec_trans.NEXTVAL, -- Genera un nuevo ID para la transacción
    1,                 -- ID del titular de la cuenta de marketing (emisor)
    tc.id,             -- ID de cada titular del Banco IT (receptor)
    5.00, 
    SYSDATE, 
    'Bonificación Banco IT'
FROM titulares_cuentas tc
JOIN cuentas c ON tc.id_cuenta = c.id
WHERE c.banco = 'Banco IT';
```

**Ejemplos**. *Modelo HR Oracle*

```sql
--Ejemplo. Supongamos que creamos una tabla llamada EMP_HISTORICO y queremos volcar allí a todos los empleados del departamento 10.

CREATE TABLE EMP_HISTORICO AS SELECT * FROM EMP WHERE 1=0;

INSERT INTO EMP_HISTORICO (EMPNO, ENAME, JOB, SAL)
SELECT EMPNO, ENAME, JOB, SAL
FROM EMP
WHERE DEPTNO = 10;

--Ejemplo. Inserción condicionada.Insertar en una tabla de AUDITORIA a todos los empleados que cobran más que la media de su departamento.
INSERT INTO AUDITORIA_SALARIOS (ID_EMP, NOMBRE, SALARIO, FECHA_REGISTRO)
SELECT EMPNO, ENAME, SAL, SYSDATE
FROM EMP e
WHERE SAL > (SELECT AVG(SAL) FROM EMP WHERE DEPTNO = e.DEPTNO);
```

## 3. Actualización de datos (`UPDATE`)

La sentencia `UPDATE` permite modificar los valores de las columnas de uno o varios registros que ya existen en una tabla. 

### 3.1. Modificación de registros individuales

Se utiliza para cambiar datos específicos de una fila. Es imprescindible usar la cláusula `WHERE` para limitar la actualización al registro deseado. Si no se usa, los cambios afectan a todos los registros de la tabla.

**Sintaxis Genérica:**

```sql
UPDATE nombre_tabla 
SET columna1 = valor1, columna2 = valor2, ...
[WHERE condicion];
```
**Importante:** Si olvidas la cláusula `WHERE` en un `UPDATE`, el SGBD aplicará el cambio a **todas** las filas de la tabla.

**Ejemplos**. *Modelo simplificado bizum*

```sql
--Ejemplo. Actualización de saldo tras un envío. Cuando un cliente realiza un Bizum, su saldo debe disminuir inmediatamente.
UPDATE cuentas 
SET saldo = saldo - 50.00 
WHERE id = 30;

--Ejemplo. Cambio de estado de una cuenta. Si un cliente pierde su teléfono, el banco puede bloquear la cuenta asociada como medida de seguridad.
UPDATE cuentas 
SET estado = 'BLOQUEADA' 
WHERE id = 30;
```

**Ejemplos**. *Modelo HR Oracle*

```SQL
--Ejemplo. Promoción de un empleado con aumento de sueldo.
UPDATE EMPLOYEES 
SET JOB_ID = 'IT_MGR', 
    SALARY = SALARY + 500
WHERE EMPLOYEE_ID = 105;
```
### 3.2. Actualización mediante subconsultas

Este método permite actualizar registros basándose en información que reside en otras tablas. Es la forma de conectar la lógica de negocio (como el DNI o el teléfono) con la tabla física que queremos modificar (cuentas).

**Sintaxis Genérica:**

```sql
UPDATE tabla_a 
SET columna = (SELECT columna FROM tabla_b WHERE condicion)
WHERE condicion_tabla_a;

```
**Ejemplos**. *Modelo simplificado bizum*
```sql
--Ejemplo: Bloqueo de cuenta por DNI de cliente.
UPDATE cuentas 
SET estado = 'BLOQUEADA' 
WHERE id IN (
    SELECT id_cuenta 
    FROM titulares_cuentas tc
    JOIN clientes c ON tc.id_cliente = c.id
    WHERE c.dni = '99887766K'
);
```

**Ejemplos**. *Modelo HR Oracle*
```sql
--Ejemplo: Queremos que todos los empleados del departamento 'IT' (ID 60) cobren, al menos, lo mismo que el empleado que menos gana en ese departamento.
UPDATE EMPLOYEES 
SET SALARY = (SELECT MIN(SALARY) FROM EMPLOYEES WHERE DEPARTMENT_ID = 60)
WHERE DEPARTMENT_ID = 60;
```

### 3.3. Actualización de múltiples filas y expresiones

`UPDATE` también permite realizar cálculos aritméticos sobre los valores actuales o modificar miles de registros a la vez si la condición del `WHERE` es amplia.

**Ejemplos**. *Modelo simplificado bizum*

```sql
--Ejemplo. El banco decide dar un incentivo del 1% de saldo adicional a todas las cuentas que tengan más de 5000€ y estén en estado 'ACTIVA'.
UPDATE cuentas 
SET saldo = saldo * 1.01 
WHERE saldo > 5000 AND estado = 'ACTIVA';
```

**Ejemplos**. *Modelo HR Oracle*
```sql
--Ejemplo. Bonus por localización.El banco decide subir un 10% el sueldo a todos los empleados que trabajan en la ciudad de 'Seattle'. 

UPDATE EMPLOYEES
SET SALARY = SALARY * 1.10
WHERE DEPARTMENT_ID IN (
    SELECT d.DEPARTMENT_ID 
    FROM DEPARTMENTS d
    JOIN LOCATIONS l ON d.LOCATION_ID = l.LOCATION_ID
    WHERE l.CITY = 'Seattle'
);
```

## 4. Borrado de datos (`DELETE`)

La sentencia `DELETE` se utiliza para eliminar una o más filas de una tabla. A diferencia del `UPDATE`, que modifica valores de columnas, el `DELETE` elimina el registro (fila) completo.

### 4.1. Borrado de registros individuales

Para eliminar un registro específico, es fundamental utilizar la cláusula `WHERE`. De lo contrario, se vaciará la tabla por completo.

**Sintaxis Genérica:**

```sql
DELETE FROM nombre_tabla 
[WHERE condicion];

```
**Importante:** Si olvidas la cláusula `WHERE` en un `DELETE`, el SGBD borrará **todas** las filas de la tabla.

**Ejemplos**. *Modelo simplificado bizum*

```sql
--Ejemplo. Baja de un teléfono en el servicio Bizum.**
DELETE FROM titulares_cuentas 
WHERE telefono = '622334455';

--Ejemplo. Eliminación de una cuenta específica.
DELETE FROM cuentas 
WHERE id = 30;
```

**Ejemplos**. *Modelo HR Oracle*
```sql
--Ejemplo. Baja de un empleado por despido o renuncia.
DELETE FROM EMPLOYEES 
WHERE EMPLOYEE_ID = 204;
```

### 4.2. Borrado mediante subconsultas

Permite eliminar registros basándose en condiciones que dependen de otras tablas.

**Sintaxis Genérica:**

```sql
DELETE FROM tabla_a
WHERE columna IN (SELECT columna FROM tabla_b WHERE condicion);

```
**Ejemplos**. *Modelo simplificado bizum*


```sql
--Ejemplo. Borrar titulares de un cliente dado de baja. Si un cliente ha solicitado la baja total del banco (y ya ha sido eliminado de la tabla `clientes`), queremos borrar todas sus vinculaciones en `titulares_cuentas`.

DELETE FROM titulares_cuentas 
WHERE id_cliente NOT IN (SELECT id FROM clientes);
```
**Ejemplos**. *Modelo HR Oracle*

```sql
--Ejemplo. Borrar empleados de una región específica. Supongamos que la empresa cierra sus oficinas en 'Canada'. Queremos borrar a todos los empleados que trabajen en departamentos ubicados en ese país.
DELETE FROM EMPLOYEES
WHERE DEPARTMENT_ID IN (
    SELECT d.DEPARTMENT_ID 
    FROM DEPARTMENTS d
    JOIN LOCATIONS l ON d.LOCATION_ID = l.LOCATION_ID
    JOIN COUNTRIES c ON l.COUNTRY_ID = c.COUNTRY_ID
    WHERE c.COUNTRY_NAME = 'Canada'
);
```

### 4.3. Integridad referencial en el borrado.

Es necesario tener en cuenta las opciones de borrado definidas en la creación del esquema, para mantener el cumplimiento de las reglas de integridad referencial.

* **RESTRICT (Restricción):** El SGBD impide borrar la fila de una tabla cuyo valor de clave primaria está referenciado por una clave foránea. Por ejmplo, impedir borrar una `cuenta` si todavía hay registros en `titulares_cuentas` que apuntan a ella.
* **CASCADE (Cascada):**. Al borrar un registro en unta tabla, el SGBD borrará todas las tuplas que referencian la clave primaria del registro borrada. Por ejemplo, si la relación se definió como *ON DELETE CASCADE*, al borrar un `cliente`, el sistema borrará automáticamente todos sus registros en `titulares_cuentas`.
* **SET NULL:** Al borrar una fila, se ponen a `NULL`(si es posible) los campos que referencian la clave primaria de la fila borrada.

**Ejemplos**. *Modelo simplificado bizum*

```sql
--Ejempñlo. Borrar un cliente titular de una cuenta.
DELETE FROM clientes WHERE id = 10;
```

**Ejemplos**. *Modelo HR Oracle*

```sql
-- Intentamos borrar un departamento que tiene empleados. Oracle lanzará un error de "Integridad Violada".
DELETE FROM DEPARTMENTS 
WHERE DEPARTMENT_ID = 60;
```

### 4.4. Borrado total de la tabla (`TRUNCATE`)

Cuando se desea vaciar una tabla completamente de forma rápida, se utiliza la sentencia `TRUNCATE`. A diferencia de `DELETE`, esta operación no se puede deshacer (no genera log de transacciones) y es mucho más rápida.

**Sintaxis Genérica:**

```sql
TRUNCATE TABLE <tabla>;
```

**Ejemplos**. *Modelo simplificado bizum*
```sql
--Ejemplo. Borrar todas las transacciones.
TRUNCATE TABLE transacciones_bizum
```

**Ejemplos**. *Modelo HR Oracle*
```sql
--Ejemplo. Si has creado una tabla para hacer pruebas, como JOB_HISTORY_COPY, y quieres eliminar todo su contenido de forma definitiva y rápida.
TRUNCATE TABLE JOB_HISTORY_COPY;
```

## 5. Herramientas y asistentes de inserción

Además de escribir código SQL manualmente, los Sistemas de Gestión de Bases de Datos (SGBD) ofrecen herramientas para facilitar el tratamiento de los datos.

* **Importación Masiva:** Permite cargar archivos CSV o Excel directamente a las tablas de la base de datos. `transacciones_bizum`.
* **Interfaces Gráficas (GUI):** Herramientas como Oracle SQL Developer o MySQL Workbench ofrecen rejillas visuales para facilitar el manejo de los datos.
* **Scripts de carga:** Archivos `.sql` permiten automatizar procesos de carga y manejo de datos. Son muy útiles para agilizar los procesos de despliegue.


## 5. Transacciones y Control de Concurrencia (ACID).
### 5.1 Transacciones. 
Una transacción es un conjunto de operaciones que el SGBD trata como una **unidad única**. Son una o más sentencias SQL que deben ejecutarse como un todo.

### 5.2 Propiedades ACID

1. **A-Atomicity** (Atomicidad)
La regla del "todo o nada". Una transacción puede tener varios pasos (ej. restar dinero de una cuenta y sumarlo en otra), pero el sistema los trata como una única unidad indivisible.  
En Bizum: Si restas 20€ del emisor pero el sistema falla antes de sumárselos al receptor, la base de datos deshace el primer paso automáticamente (Rollback). Nunca se queda el dinero "en el aire".

2. **C-Consistency** (Consistencia)
Asegura que la base de datos pase de un estado válido a otro estado válido, respetando todas las reglas y restricciones (claves foráneas, tipos de datos, checks).  
En HR: Si intentas asignar un empleado a un departamento que no existe (DEPTNO = 99), la propiedad de consistencia impedirá la transacción porque violaría la integridad referencial.

3. **I-Isolation** (Aislamiento)
Garantiza que las operaciones de una transacción sean invisibles para otras transacciones que ocurren simultáneamente hasta que la primera haya terminado.
En Bizum: Si dos personas te envían un Bizum de 10€ exactamente al mismo tiempo, el sistema las procesa de forma que una no sobrescriba a la otra. Cada una lee el saldo real en su turno.

4. **D-Durability** (Durabilidad)
Una vez que el sistema confirma que la transacción se ha completado con éxito (COMMIT), los cambios son permanentes y sobrevivirán incluso a un fallo catastrófico del sistema o un reinicio del servidor.

### 5.3 Comandos de Control de Transacciones (TCL).
Los comandos específicos de SQL para gestionar el ciclo de vida de la transacción son:  

**COMMIT** (Confirmación)
Es el comando que hace que todos los cambios realizados en la sesión actual sean permanentes y visibles para el resto de usuarios. Una vez ejecutado, se cumple la propiedad de Durabilidad.  

**Sintaxis:**
```sql
COMMIT;
```
**ROLLBACK** (Reversión)
Deshace todos los cambios realizados desde el último COMMIT. Es la herramienta principal para garantizar la Atomicidad (el "nada" del "todo o nada").
**Sintaxis:**
```sql
ROLLBACK [TO SAVEPOINT nombre_punto];
```
**SAVEPOINT** (Puntos de guardado)
Permite establecer una "bandera" o punto de retorno intermedio dentro de una transacción larga. Esto te permite deshacer solo una parte de los cambios sin perderlo todo.

**Sintaxis:**
```sql
SAVEPOINT nombre_punto;
```

**Ejemplos**. *Modelo simplificado bizum*
```sql
--Ejemplo. Veamos un ejemplo para ver cómo deshacer solo una parte de una operación compleja sin perder el trabajo anterior.
--Escenario: Ana (ID 100) envía 50€ a Pepe (ID 200)
--1. Restamos el dinero a Ana
UPDATE cuentas SET saldo = saldo - 50 WHERE id = 10;
-- 2. Creamos un punto de seguridad (El dinero ya no está en la cuenta de Ana)
SAVEPOINT tras_restar_emisor;
-- 3. Intentamos sumar el dinero a Pepe, pero nos equivocamos de ID
UPDATE cuentas SET saldo = saldo + 50 WHERE id = 999; 
-- El sistema nos dirá "0 filas actualizadas" porque el ID 999 no existe.
-- 4. En lugar de cancelar todo, volvemos al punto donde Ana ya no tiene el dinero
ROLLBACK TO SAVEPOINT tras_restar_emisor;
-- 5. Corregimos el error y sumamos al ID correcto
UPDATE cuentas SET saldo = saldo + 50 WHERE id = 20;
-- 6. Insertamos la transacción
INSERT INTO transacciones_bizum VALUES (300, 100, 200, 50, SYSDATE);
-- 7. Confirmamos todo el bloque
COMMIT;
```

**Ejemplos**. *Modelo HR Oracle*
```sql
--Ejemplo. El error del aumento masivo (Uso de ROLLBACK). Imagina que quieres subir el sueldo un 10% solo a los programadores (IT_PROG), pero te equivocas y olvidas poner el WHERE. Sin un COMMIT, puedes salvar la situación.

-- 1. Intentamos subir el sueldo (cometiendo un error a propósito)
UPDATE EMPLOYEES 
SET SALARY = SALARY * 1.10; 
-- ¡Ups! He subido el sueldo a los 107 empleados, no solo a los programadores.

-- 2. Comprobamos el desastre
SELECT SUM(SALARY) FROM EMPLOYEES;

-- 3. Aplicamos la propiedad de Atomicidad para deshacerlo
ROLLBACK;

-- 4. Comprobamos que los salarios han vuelto a su estado original
SELECT SUM(SALARY) FROM EMPLOYEES;

-- 5. Ahora lo hacemos bien y grabamos
UPDATE EMPLOYEES 
SET SALARY = SALARY * 1.10 
WHERE JOB_ID = 'IT_PROG';

COMMIT; -- Ahora el cambio es Durable (ACID)
```

### 5.4 Control de concurrencia.

El Control de Concurrencia es la parte del SGBD encargada de gestionar los accesos simultáneos de múltiples usuarios a los mismos datos. Su objetivo es que, aunque mil personas usen Bizum a la vez, cada transacción se ejecute como si fuera la única, manteniendo el Aislamiento (Isolation) de ACID.
¿Qué ocurre si Juan intenta sacar dinero de un cajero mientras su empresa le ingresa la nómina? 

Sin un control de concurrencia, ocurrirían fenómenos que corromperían la base de datos:

   - Lectura Sucia (Dirty Read): Un usuario ve datos que otro está modificando pero que aún no ha confirmado (COMMIT). Si el otro hace ROLLBACK, el primer usuario habrá leído una mentira.

   - Actualización Perdida (Lost Update): Dos usuarios leen el mismo saldo (100€), ambos restan 10€ a la vez y, al grabar, el saldo queda en 90€ en lugar de 80€ (uno pisa al otro).

   - Lectura No Repetible: Un usuario lee un dato dos veces en la misma transacción y obtiene valores distintos porque alguien lo cambió entre medias.

Para resolver estos problemas los SGBD como Oracle utilizan principalmente dos herramientas:

   - Bloqueos (Locks): Cuando una transacción modifica una fila, le pone un "candado". Otras transacciones que quieran modificar esa misma fila deben esperar a que la primera haga COMMIT o ROLLBACK.

       - Bloqueo de Fila: Solo bloquea el registro afectado (más eficiente).

       - Bloqueo de Tabla: Bloquea toda la tabla (se usa en operaciones masivas).

   - Multiversión (MVCC): Es el mecanismo utiliado por Oracle. Mientras alguien modifica un dato, el sistema guarda una "imagen" del valor anterior para que los demás puedan seguir leyendo sin esperar, pero viendo siempre datos consistentes.

No siempre se necesita el máximo rigor. SQL permite configurar el nivel de aislamiento de una transacción.  

   - Read Committed: (Por defecto en Oracle). Solo ves lo que ya ha sido confirmado por otros.

   - Serializable: El nivel más alto. Las transacciones se ejecutan una detrás de otra, evitando cualquier error de concurrencia. Esta opción es más lenta.

### 5.5 Transacciones en Oracle (Bloques PL/SQL)
Aunque en SQL estándar ejecutamos los comandos uno a uno, en entornos reales (como la aplicación de un banco) necesitamos agrupar varias operaciones en una sola unidad lógica. Para ello utilizamos PL/SQL (Procedural Language/SQL).

Un bloque PL/SQL nos permite:

- Agrupamiento Atómico: Envolvemos todas las operaciones (restar saldo, sumar saldo, insertar log) entre las palabras clave BEGIN y END.

- Control de Excepciones: Podemos programar una sección de EXCEPTION que detecte automáticamente si algo ha fallado (por ejemplo, si no hay saldo suficiente o el ID no existe).

- Gestión Automática del ROLLBACK: Si ocurre cualquier error dentro del bloque, el programa puede ejecutar un ROLLBACK de forma automática, garantizando que la base de datos nunca quede en un estado inconsistente.  


```sql

BEGIN
    -- Intentamos el proceso de Bizum
    UPDATE cuentas SET saldo = saldo - 50 WHERE id = 10;
    UPDATE cuentas SET saldo = saldo + 50 WHERE id = 20;
    INSERT INTO transacciones_bizum VALUES (1, 10, 20, 50, SYSDATE);

    -- Si todo ha ido bien hasta aquí, confirmamos
    COMMIT;

EXCEPTION
    WHEN OTHERS THEN
        -- Si ocurre CUALQUIER error, deshacemos todo automáticamente
        DBMS_OUTPUT.PUT_LINE('Error en la transacción. Deshaciendo cambios...');
        ROLLBACK;
END;
```

