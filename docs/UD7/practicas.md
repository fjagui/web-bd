# 💾 El lenguaje SQL: Manipulación de Datos y Gestión de Transacciones

## 📝 Actividad 1. Ejercicios de DML y Control TCL

### 🏗️ Paso 1: Espejo de Datos

Ejecuta esto en tu instancia Oracle (Docker/local) para no dañar los datos maestros:

```sql
CREATE TABLE productos_u4 AS SELECT * FROM OE.PRODUCT_INFORMATION;
CREATE TABLE inventario_u4 AS SELECT * FROM OE.INVENTORIES;

```

### 🔍 Paso 2: Bloque de Inserción (INSERT)

1. **Simple:** Inserta un producto con ID `7000`, nombre 'Cable HDMI 2.1' y precio `25`.
2. **Columnas específicas:** Inserta el producto `7001` llamado 'Hub USB-C' solo con el ID, nombre y `category_id = 10`.
3. **Uso de Nulos:** Inserta el producto `7002` con todos los campos pero deja `warranty_period` como `NULL`.
4. **Sintaxis de Fecha:** Inserta un pedido en una tabla de log (puedes crearla) usando `SYSDATE`.
5. **Copia de fila:** Inserta un nuevo producto cuyos datos sean idénticos al producto `1797` pero con ID `7003`.
6. **Inserción Masiva:** Inserta en `productos_u4` todos los productos de la tabla original que tengan `list_price > 5000`.
7. **Subconsulta con Filtro:** Inserta productos de la categoría 11 que no existan actualmente en tu tabla `productos_u4`.
8. **Carga Parcial:** Inserta solo el `product_id` y `product_name` de los productos que tienen stock en el almacén 1.
9. **Cálculo en Inserción:** Inserta el producto `7005` con un `list_price` que sea el doble del precio medio de la categoría 10.
10. **Multitabla (Teórico/Práctico):** Inserta en una tabla `precios_altos` todos los productos cuyo precio sea > 1000 usando un `SELECT`.

### 🔄 Paso 3: Bloque de Modificación (UPDATE)

11. **Directo:** Cambia el `product_status` a 'obsolete' para el producto `1797`.
12. **Múltiple:** Cambia el `min_price` a `50` y el `list_price` a `80` del producto `7000`.
13. **Filtro Simple:** Incrementa en `10` el precio de todos los productos de la categoría `12`.
14. **Uso de LIKE:** Pon en 'discontinued' todos los productos cuyo nombre empiece por 'Software%'.
15. **Basado en NULL:** Asigna un `min_price` de `5` a todos los productos que tengan ese campo como nulo.
16. **Cálculo Porcentual:** Rebaja un `20%` el precio de los productos con `weight_class = 5`.
17. **Subconsulta Simple:** Sube el precio `100€` a todos los productos que pertenezcan a la categoría llamada 'Software/Other' (buscando el ID en `categories_tab`).
18. **Update Correlacionado:** Actualiza el `min_price` de `productos_u4` para que sea igual al precio más bajo registrado para ese producto en la tabla `order_items`.
19. **Condición de Existencia:** Cambia el estado a 'available' solo de aquellos productos que tengan al menos 1 unidad en el `inventario_u4`.
20. **Lógica Compleja:** Si un producto tiene un `list_price` superior a la media global, redúcelo un `5%`.

### 🗑️ Paso 4: Bloque de Borrado (DELETE)

21. **ID Específico:** Borra el producto `7000`.
22. **Filtro de Texto:** Borra todos los productos que contengan la palabra 'Test' en su descripción.
23. **Rango Numérico:** Borra los productos con `list_price` entre `0` y `1`.
24. **Estado y Categoría:** Borra productos de la categoría `10` que estén 'under development'.
25. **Sin Inventario:** Borra de `productos_u4` aquellos que no tengan ninguna entrada en `inventario_u4`.
26. **Subconsulta de Agregación:** Borra los productos cuyo `min_price` sea el más bajo de toda la tabla.
27. **Relacional:** Borra los productos que nunca hayan sido vendidos (que no aparezcan en `order_items`).
28. **Basado en Almacén:** Borra del inventario todos los registros de productos que pertenezcan a almacenes situados en 'Japan' (requiere join con `warehouses` y `locations`).
29. **Doble Condición Subquery:** Borra productos cuya categoría tenga menos de 5 productos registrados.
30. **Limpieza Total:** Borra todos los registros de `productos_u4` que insertaste en el paso 2 (IDs entre 7000 y 8000).

### ⚡ Paso 5: Transacciones y Concurrencia.

Para estos ejercicios, utilizaremos una tabla limpia. Ejecuta esto antes de empezar:

```sql
CREATE TABLE cuenta_bancaria (
    id NUMBER PRIMARY KEY,
    titular VARCHAR2(50),
    saldo NUMBER(10,2)
);

INSERT INTO cuenta_bancaria VALUES (1, 'Usuario A', 1000);
INSERT INTO cuenta_bancaria VALUES (2, 'Usuario B', 2000);
COMMIT;

```

#### Escenario 1: El Principio de Atomicidad (All-or-Nothing)

Simularemos una transferencia bancaria que falla a mitad de proceso.

1. Crea un script que:
      * Reste 500€ de la cuenta 1.
      * Intente sumar 500€ a una cuenta que **no existe** (ID 99).
2. Ejecuta el script. Oracle lanzará un error en el segundo paso.
3. **Verificación:** Ejecuta `SELECT * FROM cuenta_bancaria;`.
4. Si ves que la cuenta 1 tiene 500€, la transacción ha quedado "a medias". Ejecuta `ROLLBACK` para restaurar la integridad.
5. Explica por qué es vital que estas dos operaciones vayan en un bloque que termine en `ROLLBACK` si algo falla.

#### Escenario 2: Puntos de Guardado y Deshacer Parcial

1. Sube el saldo de todos un 10%. Crea `SAVEPOINT sp_subida;`.
2. Inserta un nuevo titular: `INSERT INTO cuenta_bancaria VALUES (3, 'Usuario C', 500);`. Crea `SAVEPOINT sp_nuevo_usuario;`.
3. Borra accidentalmente a todos los usuarios: `DELETE FROM cuenta_bancaria;`.
4. **Recuperación:** Ejecuta `ROLLBACK TO SAVEPOINT sp_nuevo_usuario;`.

#### Escenario 3: Bloqueos y Tiempo de Espera 

Para este ejercicio, abre **dos terminales (T1 y T2)**.

1. Ejecuta `UPDATE cuenta_bancaria SET saldo = 0 WHERE id = 1;` (No hagas COMMIT).
2. Ejecuta el mismo comando: `UPDATE cuenta_bancaria SET saldo = 5000 WHERE id = 1;`.
3. **Observación:** Verás que la **T2** se queda bloqueada.
4. **T1:** Ejecuta `COMMIT;`.
5. Mira qué ocurre en la **T2**.

#### Escenario 4: El "Commit Fantasma" (DDL)

1. Borra al Usuario 2: `DELETE FROM cuenta_bancaria WHERE id = 2;`.
2. Sin hacer commit, crea una tabla de log: `CREATE TABLE log_errores (msg VARCHAR2(100));`.
3. Intenta hacer `ROLLBACK;`.
4. **Verificación:** Haz un `SELECT * FROM cuenta_bancaria;`.
5. ¿Ha vuelto el Usuario 2? Describe el comportamiento de Oracle ante sentencias DDL dentro de una transacción DML abierta.
### ✅ Entrega

* Memoria en markdown del desarrollo de las prácticas.
* Fichero sql con las consultas planteadas.
* Carpeta con las capturas de pantalla.