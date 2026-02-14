# El Lenguaje SQL: Consulta de datos (DML). Actividades

## 📝 Actividad 1. Gestión de pedidos en Oracle.

### Objetivos de la Práctica

1. Desplegar un entorno de base de datos profesional mediante **Docker**.
2. Realizar ingeniería inversa para comprender el modelo de datos.
3. Casrgar y manipular datos mediante scripts SQL.
4. Resolver consultas de negocio de complejidad creciente.


### 📥 Paso 1: Descarga el esquema de trabajo.

Para esta práctica utilizaremos el esquema Order Entry de Oracle. Debes descargar el modelo desde el repositorio oficial de ejemplos de Oracle:

> **🔗 Enlace de Descarga:** [Ejemplos oficiales de ORACLE (GitHub)](https://github.com/oracle-samples/db-sample-schemas/tree/main){targert=blank}



### 🏗️ Paso 2: Preparación del Entorno (Docker)

Debes tener el contenedor de Oracle 21c en ejecución.

1. Conectarse como `SYSDBA` desde VS Code o terminal.
2. Asegurarse de estar en la PDB correcta: `ALTER SESSION SET CONTAINER = XEPDB1;`.
3. Sigue las instrucciones para la instalación de los esquemas de ejemplo.

### 🎨 Paso 3: Ingeniería Inversa (SQL Developer)

Una vez cargados los datos, utiliza **SQL Developer** para entender cómo se relacionan las tablas y generar el diagrama entidad/relación del modelo

### 🔍 Paso 4: Consultas SQL (VS Code)

Realiza en visual studio las siguientes consultas SQL.

1. **Catálogo de Productos:** Listar el nombre, descripción y precio de lista de todos los productos, ordenados del más caro al más barato.
2. **Filtro de Categorías:** Mostrar los nombres de los productos que pertenezcan a la categoría número 10 o 11.
3. **Precios Mínimos:** Listar los productos cuyo `min_price` (precio mínimo de venta) sea superior a 1000€.
4. **Estado de Pedidos:** Mostrar los ID de los pedidos (`order_id`) que tengan un estado (`order_status`) superior a 5.
5. **Búsqueda de Texto:** Listar los productos cuyo nombre contenga la palabra 'Monitor' o 'Laptop'.
6. **Rango de Ventas:** Mostrar los pedidos realizados cuyo `order_total` esté entre 5.000 y 15.000€.
7. **Formato de Fechas:** Mostrar el `order_id` y el mes en el que se realizó el pedido utilizando la columna `order_date`.
8. **Vendedores Activos:** Listar los IDs de los representantes de ventas (`sales_rep_id`) que han gestionado algún pedido, sin repetir valores.
9. **Detalle de Factura:** Mostrar el ID del pedido, el nombre del producto y la cantidad pedida (`quantity`) para cada línea de pedido.
10. **Precios Reales:** Listar el nombre del producto junto con su `list_price` y el `unit_price` real al que se vendió en los pedidos (unión entre `product_information` y `order_items`).
11. **Pedidos por Cliente:** Mostrar el nombre de los productos comprados por un `customer_id` específico (por ejemplo, el 101).
12. **Localización de Stock:** Listar los productos y las cantidades disponibles en cada almacén (`inventories` joined with `warehouses`).
13. **Ventas por Representante:** Mostrar el nombre de los productos vendidos por el representante con ID 150.
14. **Categorías Detalladas:** Mostrar el nombre del producto y el nombre de su categoría (unión con la tabla `categories_tab`).
15. **Pedidos Completos:** Listar el `order_id`, la fecha y el nombre de la ciudad del almacén desde donde se despachó el pedido.
16. **Volumen de Ventas:** ¿Cuál es el importe total de todos los pedidos registrados en la tabla `orders`?
17. **Rendimiento de Productos:** Calcular el precio medio de lista de todos los productos de la empresa.
18. **Ventas por Estado:** Mostrar el número de pedidos que hay en cada estado (`order_status`) diferente.
19. **Ranking de Almacenes:** Mostrar el ID de cada almacén y la cantidad total de productos que tiene en stock.
20. **Facturación por Pedido:** Mostrar el `order_id` y la suma total de sus líneas de pedido (multiplicando `unit_price` * `quantity`).
21. **Categorías Populares:** Listar las categorías que tienen más de 5 productos diferentes registrados.
22. **Productos Estrella:** Mostrar los nombres de los productos cuyo precio de lista es superior al precio medio de todos los productos de la empresa.
23. **Clientes VIP:** Listar los IDs de los clientes que han realizado pedidos por un total acumulado superior a 50.000€.
24. **Stock Crítico:** Mostrar los nombres de los productos que tienen una cantidad en inventario inferior a la cantidad media de stock global.
25. **Peores Vendedores:** Identificar a los representantes de ventas cuyo total de ventas gestionadas es inferior al promedio de ventas de todos los representantes.

### ✅ Entrega.

1. Documentación técnica del proceso de instalación de los ejemplos.
2. Diagrama entidad/relación.
3. Pantallazos con las consultas y resultados obtenidos.



## ✈️ Actividad 2. Gestión de Vuelos en MySQL

### 🎨 Paso 1. Interpretación del Modelo.

Accede a la documentación del modelo y analiza en detalla el diseño lógico. 

[Acceso al modelo Airport DB](https://dev.mysql.com/doc/airportdb/en/airportdb-structure.html){target=blank}


### 🏗️ Paso 2. Añade la base de datos a tu servidor MySql local.

1. **Descarga:** Descarga los archivos del modelo.
2. **Configuración SQL:** 
   1. Crea la base de datos.
   2. Crea y configura un usuario para el acceso a la base de datos.
3. **Carga de Datos:** Importa el archivo principal.

## 🔍 Paso 3. Consultas SQL

Resuelve las siguientes consultas SQL

### Bloque 1: Filtros, Ordenación y Fechas (Nivel 1-2)

1. **Flota disponible:** Listar todos los modelos de aviones y su capacidad de pasajeros, ordenados por capacidad de mayor a menor.
2. **Aeropuertos elevados:** Mostrar el nombre, la ciudad y la elevación de los aeropuertos que estén por encima de los 4000 pies.
3. **Vuelos críticos:** Listar los IDs de vuelos que tengan actualmente un estado de 'Delayed' (Retrasado).
4. **Grandes aeronaves:** Mostrar los aviones cuya capacidad sea superior a 300 asientos.
5. **Búsqueda nominal:** Listar los aeropuertos cuyo nombre contenga la palabra 'International'.
6. **Rango de rutas:** Mostrar los vuelos que cubren una distancia entre 1000 y 2500 km.
7. **Calendario de salidas:** Mostrar el ID del vuelo y la fecha de salida, pero solo para aquellos vuelos programados después del mediodía (12:00:00).
8. **Fabricantes preferidos:** Listar los aviones cuyo fabricante sea 'Airbus' o 'Boeing'.

### Bloque 2: Joins y Relaciones de Datos (Nivel 3)

9. **Rutas con nombre:** Mostrar el ID del vuelo junto con el nombre completo del aeropuerto de origen y el nombre del aeropuerto de destino.
10. **Detalle de Reservas:** Listar el ID de reserva (`booking_id`) y el nombre del pasajero asociado (unión entre `booking` y `passenger`).
11. **Asignación de Equipos:** Mostrar el ID de cada vuelo y el modelo del avión asignado para realizarlo.
12. **Ingresos por Ticket:** Mostrar el nombre del pasajero y el precio pagado por su ticket en una sola lista.
13. **Vuelos desde Centros Logísticos:** Listar todos los vuelos (ID y fecha) que salen desde la ciudad de 'London'.
14. **Puertas de Embarque:** Mostrar el ID del vuelo, la hora de salida y el código de la puerta (`gate`) en el aeropuerto de origen.
15. **Localización Geográfica:** Listar los vuelos indicando el nombre del país del aeropuerto de destino.

### Bloque 3: Agregación y Estadísticas (Nivel 4)

16. **Volumen de Ventas:** ¿Cuántas reservas (bookings) se han realizado en total en la historia de la aerolínea?
17. **Promedio de Altitud:** Calcular la elevación media de todos los aeropuertos del sistema.
18. **Tráfico por Aeropuerto:** Mostrar el nombre de cada aeropuerto y el número total de vuelos que han salido de él.
19. **Recaudación por Vuelo:** Calcular la suma total de dinero recaudado por tickets para el vuelo con ID 1024.
20. **Capacidad por Fabricante:** Mostrar la capacidad máxima de pasajeros agrupada por cada fabricante de aviones.
21. **Ciudades Hub:** Listar las ciudades que tienen registrados más de 3 aeropuertos diferentes.

### Bloque 4: Lógica Avanzada y Subconsultas (Nivel 5)

22. **Vuelos de Larga Distancia:** Mostrar los vuelos cuya distancia es superior a la distancia media de todos los vuelos de la base de datos.
23. **Aviones de Alto Rendimiento:** Listar los modelos de avión que han realizado más de 50 vuelos con éxito.
24. **Destino Más Rentable:** Identificar el nombre del aeropuerto de destino que ha generado la mayor cifra de ventas totales.
25. **Pasajeros Frecuentes:** Listar los nombres de los pasajeros que han realizado más de 10 reservas en vuelos distintos.


### ✅ Entrega.

1. Documentación técnica del proceso de instalación de los ejemplos.
2. Diagrama entidad/relación.
3. Pantallazos con las consultas y resultados obtenidos.
