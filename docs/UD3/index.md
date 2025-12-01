# Modelo Relacional.
## 1. Introducción.

El **modelo relacional** fue propuesto por el matemático británico **Edgar F. Codd** en 1970, en su artículo seminal *“A Relational Model of Data for Large Shared Data Banks”*.  
Esta propuesta transformó por completo el campo de las bases de datos al introducir una representación basada en **conceptos matemáticos**, especialmente la teoría de conjuntos y la lógica de predicados.

Codd estableció los principios para estructurar bases de datos de forma lógica y coherente, garantizando independencia respecto a su implementación física y proporcionando una base teórica sólida para los Sistemas Gestores de Bases de Datos (SGBD) modernos.

📚 **Recursos útiles:**  

  - [Artículo original de Codd (PDF).](https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf){target=_blank}  
  - [Biografía de Edgar F. Codd (Wikipedia).](https://es.wikipedia.org/wiki/Edgar_F._Codd){target=_blank}
  - [Explicación del modelo relacional (GeeksforGeeks).](https://www.geeksforgeeks.org/relational-model-in-dbms/){target=_blank}

El **modelo relacional** ofrece múltiples ventajas que explican su adopción como estándar en el diseño de bases de datos:

- **Proporciona un interfaz común entre usuarios y diseñadores** durante la fase de análisis, usando estructuras comprensibles y alejadas del almacenamiento físico.
- **Establece reglas y criterios sólidos para estructurar la base de datos**, favoreciendo la consistencia y reduciendo la redundancia.
- **Facilita su transferencia al nivel físico**, ya que sus conceptos (tablas, filas, columnas) se corresponden naturalmente con las estructuras utilizadas en los SGBD.
- **Permite crear definiciones de datos independientes de la implementación**, mediante lenguajes declarativos como **SQL**, estándar en la manipulación de datos.

El trabajo de Codd sigue siendo fundamental y constituye la base sobre la que se apoyan la mayoría de los sistemas de bases de datos actuales.


## 2. Estática del modelo relacional.

### 2.1 Estructura de datos relacional

La estática del modelo relacional describe **cómo se organizan los datos** dentro del sistema, estableciendo los conceptos fundamentales que permiten estructurar la información de forma lógica, independiente del almacenamiento físico.

Antes de definir formalmente el concepto de relación, analicemos algunos de los términos que aparecerán en el estudio y análisis del modelo.

- **Relación**: corresponde a lo que habitualmente llamamos *tabla*.  
  [Más información (Wikipedia).](https://en.wikipedia.org/wiki/Relation_(database)){target=_blank}
- **Tupla**: una fila de la tabla; equivalente a *registro* en los ficheros.
- **Atributo**: una columna de la tabla; equivalente a *campo*.
- **Dominio**: conjunto de valores homogéneos y atómicos (indivisibles), que puede tomar un atributo.  
  Ejemplo: `Dom_DiaSemana={lunes,martes,miercoles,jueves,viernes,sabado,domingo}`  
- **Clave candidata**: atributo(s) con valor único para cada tupla; no puede repeturse; no puede ser nulo; identifica la tupla.
- **Clave primaria (PK)**: clave candidata seleccionada para identificar de forma única cada fila.
- **Clave externa (FK)**: atributo o conjunto de atributos que referencian la clave primaria de otra tabla.

![conceptos](img/ud3_img1.png){width: "160px"; margin: 0 15px 15px 0;"}

#### Equivalencias entre terminología

| Modelo relacional | Modelo tabular | Modelo fichero |
|-------------------|----------------|----------------|
| Relación          | Tabla          | Fichero        |
| Tupla             | Fila           | Registro       |
| Atributo          | Columna        | Campo          |
| Grado             | Nº columnas    | Nº campos      |
| Cardinalidad      | Nº filas       | Nº registros   |

#### Propiedades deseables de una base de datos relacional

- **No redundancia**  
No existe información repetida en el sistema. El sistema será responsable del control de la información redundante cuando deba existir por motivos de rendimiento.
- **Consistencia**  
  Los datos deben mantener coherencia bajo las operaciones permitidas.  
  Ejemplo: el saldo de una cuenta debe coincidir en todas las consultas y debe ser el resultado de todos los movimientos.
- **Flexibilidad**  
  La estructura puede modificarse sin afectar a los datos existentes.
- **Integridad**  
  La BD debe contener la información necesaria y completa, evitando referencias a datos inexistentes o  almacenando informaciones contradictorias. 
- **Interrelación**  
  Los datos pueden relacionarse entre sí de múltiples maneras, y dichas relaciones se almacenan en el sistema.
- **Escalabilidad**  
  La base de datos debe poder expandirse a medida que se requiera almacenar nueva información, permitiendo agregar tablas y registros según las necesidades del sistema.

#### Definición formal de relación

Matemáticamente, una relación sobre los dominios *D1*,*D2*,…,*Dn* (no necesariamente distintos) es un subconjunto del producto cartesiano *D1 × D2 × … × Dn*, donde cada elemento de la relación, llamado **tupla**, es un conjunto ordenado de n valores.

Una relación R sobre un conjunto de dominios *D1*, *D2*, ... *DN* consta de:  

- **Cabecera**: Conjunto fijo de atributos, en términos más precisos de *(atributo,dominio)*:  
    *{(A1:D1), (A2:D2),(A3:D3),…(An:Dn)}*. N es el grado de la relación.  

- **Cuerpo**: Conjunto de tuplas, en la que cada tupla está formada por un conjunto de pares *(atribut,valor)*. El conjunto de tuplas varía con el tiempo.  
    *{(A1:V1), (A2:V2),…,(Am:Dm)}*. M es la cardinalidad de la relación.


#### Definiciones estructurales

- **Esquema de relación:** Constituido por el nombre de la relación y la cabecera.   
	*R{(A1:D1), (A2:D2),(A3:D3),…,(An:Dn)}*

- **Esquema relacional:** Conjunto de esquemas de relación.

- **Estado de relación:** Esquema y cuerpo de relación y al que denominaremos simplemente relación
	<esquema, cuerpo>

#### Propiedades Inherentes de las Relaciones.
Las propiedades inherentes son características fundamentales que definen cómo se estructuran y comportan las relaciones (tablas) en el modelo relacional. Estas propiedades se derivan directamente de la base matemática del modelo, específicamente de la teoría de conjuntos.  

Las 4 Propiedades Inherentes Fundamentales son:

**1. No existen tuplas repetidas**  
Una relación es un conjunto de tuplas, y por definición, los conjuntos no contienen elementos duplicados.  
Cada fila en una tabla debe ser única. No puede haber dos filas idénticas en todos sus atributos.

✅ CORRECTO: Todas las tuplas son únicas  

| ID | Nombre  | Edad | Departamento |
|----|---------|------|--------------|
| 1  | Ana     | 25   | Ventas       |
| 2  | Carlos  | 30   | IT           |
| 3  | María   | 28   | Ventas       |

❌ INCORRECTO: Tupla duplicada  

| ID | Nombre  | Edad | Departamento |
|----|---------|------|--------------|
| 1  | Ana     | 25   | Ventas       |
| 2  | Carlos  | 30   | IT           |
| 1  | Ana     | 25   | Ventas       |  -- ¡DUPLICADA!

Si necesitas almacenar información que parece repetirse, probablemente sea necesario revisar el diseño.  

**2. Las tuplas no están ordenadas.**  
Los conjuntos matemáticos son colecciones no ordenadas de elementos.  
El orden en que aparecen las filas en una tabla no tiene significado semántico. No puedes confiar en la posición física de las tuplas.

Estas dos representaciones son IDÉNTICAS en el modelo relacional:  

Versión A  

| ID | Producto   | Precio |
|----|------------|--------|
| 1  | Laptop     | 800    |
| 2  | Mouse      | 25     |
| 3  | Teclado    | 45     |

Versión B  

| ID | Producto   | Precio |
|----|------------|--------|
| 3  | Teclado    | 45     |
| 1  | Laptop     | 800    |
| 2  | Mouse      | 25     |

Podremos utilizar el lenguajde de consultas SQL para recuperar las tuplas en un orden específico.

**3. Los atributos no están ordenados.**  
Por definición, los atributos de una relación son un conjunto y no una lista ordenada.  
El orden de las columnas en una tabla no es significativo. Se accede a los atributos por nombre, no por posición.

Estas dos definiciones de tablas en SQL son EQUIVALENTES:

Definición A
```sql
CREATE TABLE Empleados (
    Id INT PRIMARY KEY,
    Nombre VARCHAR(50),
    Departamento VARCHAR(50),
    Salario DECIMAL(10,2)
);
```
Definición B
```sql
CREATE TABLE Empleados (
    Departamento VARCHAR(50),
    Salario DECIMAL(10,2),
    Nombre VARCHAR(50),
    Id INT PRIMARY KEY
);
```
En las consultas SQL, siempre debemos referenciar columnas por nombre:
```sql
SELECT Nombre, Departamento FROM Empleados;
```
**4. Todos los valores de los atributos son atómicos (1ª Forma Normal)**  
Según la definición, cada atributo contiene un valor único e indivisible del dominio correspondiente.  
El valor de un atributo en una tubla debe contener exactamente un valor simple, no conjuntos, listas o estructuras complejas.

✅ CORRECTO: Valores atómicos.  

| ID | Nombre | Teléfono    | Email               |
|----|--------|-------------|---------------------|
| 1  | María  | 555-1234    | maria@email.com     |
| 2  | Pedro  | 555-5678    | pedro@email.com     |

❌ INCORRECTO: Valores no atómicos.

| ID | Nombre | Teléfonos           | Emails                      |
|----|--------|---------------------|-----------------------------|
| 1  | María  | 555-1234, 555-5678 | maria@a.com, maria@b.

En resumen, las propiedades inherentes son el fundamento que garantiza la consistencia, predictibilidad y corrección matemática del modelo relacional, haciendo que las bases de datos sean herramientas confiables para el manejo estructurado de información.

### 2.2 Reglas de integridad relacional.

#### Concepto de Integridad
La integridad en el modelo relacional se refiere al conjunto de reglas y restricciones que garantizan que los datos almacenados en una base de datos sean correctos, consistentes y válidos en todo momento, reflejando adecuadamente la realidad que representan.
Pensemos en la integridad como las "reglas de tráfico" de la base de datos: Así como las señales de tránsito evitan accidentes y caos, las restricciones de integridad evitan datos incorrectos e inconsistentes.

**Claves Primarias y Candidatas**

Dada una relación R, un atributo o conjunto de atributos K es clave candidata si y solo si cumple las siguientes propiedades:  

  1. **Unicidad.** No existen dos tuplas en R con el mismo valor de K.  
  2. **Minimalidad.** Ningún subconjunto propio de K posee la propiedad de unicidad.  
   
La **clave primaria** es una de las claves candidatas seleccionada como identificador principal de la relación. Su elección se basa en criterios como:

- **Estabilidad**: Que no cambie frecuentemente
- **Simplicidad**: Fácil de usar y almacenar  
- **Semántica**: Que tenga significado en el dominio del problema

Es frecuente utilizar **claves artificiales** (auto-incrementales, UUIDs) en lugar de claves naturales, ya que las artificiales suelen ser más estables y predecibles que los identificadores naturales del mundo real. 

#### Regla de Integridad de las Entidades

En una relación, ningún atributo que forme parte de la clave primaria (candidata) puede contener valores nulos (NULL).  
Una clave primaria sirve para identificar unívocamente cada tupla. Un valor NULL imposibilitaría esta identificación.

**✅ CUMPLE** la Regla de Integridad de las Entidades.

| Id | Nombre | Email | Fecha Nacimiento |
|-------------|---------|-------|-----------------|
| **1001** | Ana García | ana.garcia@email.com | 2000-05-15 |
| **1002** | Carlos López | carlos.lopez@email.com | 1999-08-22 |
| **1003** | María Torres | maria.torres@email.com | 2001-03-10 |
| **1004** | Pedro Sánchez | pedro.sanchez@email.com | 2000-11-30 |
| **1005** | Laura Martínez | laura.martinez@email.com | 2001-07-18 |

🔑 **Clave primaria:** Id  
✓ Todos los valores Id son únicos y no nulos.

---

❌ **INCUMPLE** la Regla de Integridad de las Entidades.

| EstudianteID | Nombre | Email | Fecha Nacimiento |
|-------------|---------|-------|-----------------|
| **1001** | Ana García | ana.garcia@email.com | 2000-05-15 |
| **NULL** | Carlos López | carlos.lopez@email.com | 1999-08-22 |
| **1003** | María Torres | maria.torres@email.com | 2001-03-10 |
| **1004** | Pedro Sánchez | pedro.sanchez@email.com | 2000-11-30 |
| **NULL** | Laura Martínez | laura.martinez@email.com | 2001-07-18 |

🚫 Problemas:

  - Carlos López Y Laura Martínez tienen **NULL** en su clave primaria.  
  - **No se pueden identificar unívocamente** a Carlos y Laura.
  - **Imposible referenciar** sus registros desde otras tablas.  
  - **No se puede garantizar** la unicidad de sus datos.  

#### Claves foráneas.
Una clave foránea es un atributo o conjunto de atributos en una relación R1 que referencia la clave primaria de otra relación R2, es decir, atributo o conjunto de atributos en una relación, que son clave primaria en otra.

**📚 Tabla 1: ESTUDIANTES**

| EstudianteID | Nombre | Email | FechaNacimiento |
|-------------|---------|-------|-----------------|
| **1001** | Ana García | ana@universidad.edu | 2000-05-15 |
| **1002** | Carlos López | carlos@universidad.edu | 1999-08-22 |
| **1003** | María Torres | maria@universidad.edu | 2001-03-10 |
| **1004** | Pedro Sánchez | pedro@universidad.edu | 2000-11-30 |

**🔑 Clave primaria:** EstudianteID

---

**🎓 Tabla 2: MATRÍCULAS**

| MatriculaID | **EstudianteID** | Curso | Semestre | FechaMatricula |
|------------|-----------------|-------|----------|---------------|
| **M-5001** | **1001** | Matemáticas I | 2024-1 | 2024-01-15 |
| **M-5002** | **1001** | Física I | 2024-1 | 2024-01-16 |
| **M-5003** | **1002** | Programación | 2024-1 | 2024-01-17 |
| **M-5004** | **1003** | Matemáticas I | 2024-1 | 2024-01-18 |
| **M-5005** | **1003** | Química | 2024-1 | 2024-01-19 |

**🔑 Clave primaria:** MatriculaID  
**🔗 Clave foránea:** EstudianteID → REFERENCES ESTUDIANTES(EstudianteID)


#### Regla de Integridad Referencial.
Los valores de una clave foránea solo pueden ser: 

  1. Igual a algún valor de la clave primaria referenciada, O
  2. Completamente NULL (en todos sus componentes).

**Ejemplo: Protectora de Animales**

**🐕 Tabla 1: ANIMALES**

| AnimalID | Nombre | Especie | Estado |
|----------|--------|---------|--------|
| **A-100** | Luna | Perro | Disponible |
| **A-101** | Simba | Gato | Disponible |
| **A-102** | Rocky | Perro | Adoptado |

**🔑 Clave primaria:** AnimalID

---

**👥 Tabla 2: ADOPCIONES**

| AdopcionID | **AnimalID** | Adoptante | FechaAdopcion |
|------------|-------------|-----------|--------------|
| **AD-500** | **A-102** | María López | 2024-03-01 |
| **AD-501** | **A-100** | Carlos García | 2024-03-15 |

**🔑 Clave primaria:** AdopcionID  
**🔗 Clave foránea:** AnimalID → REFERENCES ANIMALES(AnimalID)

---

**✅ CUMPLE Integridad Referencial**

- **AD-500** referencia a **A-102** (Rocky) → ✅ EXISTE
- **AD-501** referencia a **A-100** (Luna) → ✅ EXISTE
- **Todos los AnimalID en ADOPCIONES existen en ANIMALES**

---

**❌ INCUMPLE Integridad Referencial**

**Si intentamos agregar:**

| AdopcionID | AnimalID | Adoptante | FechaAdopcion |
|------------|---------|-----------|--------------|
| **AD-502** | **A-999** | Ana Torres | 2024-03-20 |

La integridad referencial garantiza que solo animales existentes pueden ser adoptados. 

#### Integridad Referencial. Borrado/Modificación.

Cuando existen relaciones entre tablas mediante claves primarias y foráneas, las operaciones de **eliminación** o **actualización** de claves primarias deben gestionar las filas relacionadas para preservar la consistencia de los datos y garantizar el cumplimiento de la regla de integridad referencial. Para ello tenemos las siguientes opciones:   

**1. Restricción (RESTRICT/NO ACTION)**  

- Impide la eliminación del registro si está referenciado en otra tabla.
- Mantiene la integridad de manera estricta.
- El usuario debe eliminar manualmente primero los registros de la tabla que define la FK.
- Opera como una verificación inmediata de la restricción.
- **Caso de uso:** Datos fundamentales que no deben desaparecer del sistema bajo ninguna circunstancia.

**2. Eliminación en cascada (CASCADE)**  

- Al eliminar un registro, se eliminan automáticamente todos los registros que lo referencian en la tabla que define la FK.
- Propaga la operación de eliminación a través de las relaciones.
- Puede resultar en la eliminación masiva de datos
- Útil cuando la relación representa una composición fuerte.
- **Caso de uso:** Relaciones donde las tuplas hijo no tiene significado sin el padre.

**3. Establecer a nulo (SET NULL)**  

- Al eliminar el registro padre, los valores de la clave foránea en los registros hijos se establecen a NULL.  
- Preserva los registros hijos pero los desvincula del padre.  
- Requiere que la clave foránea permita valores nulos.  
- Los registros hijos quedan "huérfanos" pero conservados.  
- **Caso de uso:** Relaciones opcionales donde el hijo puede existir independientemente.

**4. Establecer valor por defecto. (SET DEFAULT)**  

- Al eliminar el registro padre, los valores de la clave foránea en los registros hijos se establecen a un valor predefinido por defecto.
- Similar a SET NULL pero con un valor específico en lugar de nulo
- Requiere definir un valor por defecto para la columna foránea
- Útil para redirigir relaciones a un valor "genérico" o "sin asignar"

## 3. Dinámica del modelo relaciona.
La **dinámica del modelo relacional** se refiere al conjunto de **operaciones y mecanismos** que permiten manipular y transformar los datos almacenados en las relaciones a lo largo del tiempo. A diferencia de la estructura estática definida por esquemas y restricciones, la dinámica abarca todas las **transformaciones válidas** que pueden aplicarse a las relaciones mediante operaciones del álgebra relacional y comandos de actualización, garantizando que cada transición entre estados de la base de datos preserve la **integridad** y **consistencia** definida por las restricciones del modelo. Esta capacidad de evolución controlada es lo que permite a las bases de datos relacionales reflejar cambios en el mundo real mientras mantienen la **corrección semántica** y la **fiabilidad** de la información almacenada.

### 3.1 Álgebra relacional.
El **álgebra relacional** es un **sistema formal de operadores** que permite manipular y consultar datos estructurados en forma de relaciones (tablas). Desarrollado por Edgar F. Codd en 1970, constituye la **base teórica** de los lenguajes de consulta relacionales como SQL.

#### Características Principales:
- **Lenguaje de consultas procedimental:** Especifica cómo obtener los datos.
- **Operadores cerrados:** El resultado de cada operación es otra relación.
- **Base para optimización:** Proporciona fundamentos para optimizadores de consultas.

#### Clasificación de Operadores Relacionales**

- **Grupo 1: Operadores de Conjunto Tradicionales**
*(Requieren compatibilidad de esquema)*

- **Grupo 2: Operadores Relacionales Especializados**
*(Operaciones específicas para el modelo relacional)*

**Operadores de Conjunto Tradicionales**

Dos relaciones **R** y **S** son **compatibles para unión** si y sólo si sus cabeceras son idénticas, lo cual significa:  

- Las dos relaciones tienen el mismo conjunto de atributos.
- Los atributos correspondientes se definen sobre el mismo dominio.

Diremos que dos relaciones son **compatibles respecto al producto**, si y sólo si, sus cabeceras son disjuntas, es decir, no tienen atributos en común.  

**1. Unión (UNION)**  

- **Símbolo:** ∪  
- **Definición:** Combina todas las tuplas de dos relaciones compatibles, eliminando duplicados.
- **Propiedades:**  
    - Conmutativa: R ∪ S = S ∪ R
    - Asociativa: (R ∪ S) ∪ T = R ∪ (S ∪ T)
    - Idempotente: R ∪ R = R

**Ejemplo:**
```
R = { (1, 'Ana'), (2, 'Carlos') }
S = { (2, 'Carlos'), (3, 'María') }
R ∪ S = { (1, 'Ana'), (2, 'Carlos'), (3, 'María') }
```

**2. Intersección (INTERSECT)**  

- **Símbolo:** ∩  
- **Definición:** Retorna las tuplas que existen en ambas relaciones.
- **Propiedades:**
    - Conmutativa: R ∩ S = S ∩ R
    - Asociativa: (R ∩ S) ∩ T = R ∩ (S ∩ T)
  
**Ejemplo:**
```
R = { (1, 'Ana'), (2, 'Carlos') }
S = { (2, 'Carlos'), (3, 'María') }
R ∩ S = { (2, 'Carlos') }
```

**3. Diferencia (MINUS/EXCEPT)**  

- **Símbolo:** −  
- **Definición:** Retorna las tuplas que están en la primera relación pero no en la segunda.
- **Propiedades:**
    - No conmutativa: R − S ≠ S − R (generalmente)
    - R − R = ∅ (conjunto vacío)

**Ejemplo:**
```
R = { (1, 'Ana'), (2, 'Carlos') }
S = { (2, 'Carlos'), (3, 'María') }
R − S = { (1, 'Ana') }
S − R = { (3, 'María') }
```

**4. Producto cartesiano (TIMES/CROSS JOIN)**  

- **Símbolo:** ×  
- **Requisito:** Relaciones R, S compatibles respecto al producto (cabeceras disjuntas).
- **Definición:** Combina cada tupla de R con cada tupla de S.

- **Características:**
    - Si R tiene m tuplas y S tiene n tuplas, R × S tendrá m × n tuplas.
    - Puede generar resultados muy grandes.
    - Fundamental para operaciones de reunión.

**Ejemplo:**
```
R = { (1, 'A'), (2, 'B') }
S = { ('X', 10), ('Y', 20) }
R × S = { (1, 'A', 'X', 10), (1, 'A', 'Y', 20), 
          (2, 'B', 'X', 10), (2, 'B', 'Y', 20) }
```

**Operadores Relacionales Especializados.**

**5. Selección o restricción (SELECT/WHERE)**  

- **Símbolo:** σ  
- **Definición:** Filtra tuplas basándose en una condición lógica.
- **Características:**
    - No cambia el esquema de la relación
    - Reduce el número de tuplas
    - La condición puede incluir comparaciones y operadores lógicos

**Ejemplo:**
```
 σ_{edad > 18}(Estudiantes)
```
**6. Proyección (PROJECT)**

- **Símbolo:** π  
- **Definición:** Selecciona un subconjunto de columnas (atributos).
- **Características:**
    - Puede eliminar tuplas duplicadas en el resultado
    - Cambia el esquema (reduce atributos)
    - Preserva o reduce el número de tuplas

**Ejemplo:** 
```
π_{nombre, email}(Estudiantes)
```
**7. Reunión (JOIN)**

- **Símbolo:** ⋈  
- **Definición:** Combina tuplas de dos relaciones basándose en una condición.
- **Tipos de Reunión:**
    - **Equirreunión (Equijoin):** Condición de igualdad entre atributos
    - **Reunión Natural (Natural Join):** Equirreunión que elimina un atributo duplicado
    - **Reunión Externa (Outer Join):** Incluye tuplas sin correspondencia
    - **Reunión Theta (Theta Join):** Condición general (>, <, ≥, ≤, ≠)

**Ejemplo:** 
```
Estudiantes ⋈_{Estudiantes.dept = Departamentos.id} Departamentos
```
**8. DIVISIÓN (DIVIDE BY)**  

- **Símbolo:** ÷  
- **Definición:** Toma dos relaciones, una binaria y una unaria y construye una relación formada por todos los valores de un atributo de la relación binaria que concuerdan (en el otro atributo) con todos los valores en la relación unaria.

**Ejemplo:** Encontrar los estudiantes que están matriculados en **todos** los cursos de una lista.
```
Matriculas(estudiante, curso) ÷ CursosEspecificos(curso)
```
**🔑 Herencia de Claves Primarias en Operaciones**

El álgebra relacional incluye reglas para determinar la **clave primaria resultante** de cada operación:

- **Unión/Intersección/Diferencia:** Clave primaria del primer operando.
- **Producto Cartesiano:** Combinación de ambas claves primarias.
- **Selección:** Mantiene la clave primaria original.
- **Proyección:** 
    - Si incluye la clave primaria completa → la mantiene
    - Si no incluye la clave primaria completa → requiere análisis.
- **Reunión Natural:** 
    - Si se une por clave primaria → mantiene estructura de claves
    - Caso general → necesita análisis específico
- **División:** Generalmente no preserva claves primarias simples.


El poder del álgebra relacional reside en la **composición de operadores**:

```
π_{nombre}(σ_{dept='Ventas' ∧ salario>3000}(Empleados))
```

Esta operación generaría una relación con los nombres de empleados del departamento de Ventas con salario mayor a 3000.

#### 🎯 Importancia en Sistemas de Bases de Datos**

1. **Fundamento de SQL:** Cada operador tiene su equivalente en SQL
2. **Optimización de Consultas:** Los optimizadores transforman consultas usando equivalencias algebraicas
3. **Verificación de Integridad:** Permite expresar restricciones complejas
4. **Diseño de Bases de Datos:** Útil en procesos de normalización

**📊 Resumen de Operadores**

| Operador | Símbolo | Propósito | Requisito Compatibilidad |
|----------|---------|-----------|--------------------------|
| Unión | ∪ | Combinar tuplas | Esquemas idénticos |
| Intersección | ∩ | Tuplas comunes | Esquemas idénticos |
| Diferencia | − | Tuplas exclusivas | Esquemas idénticos |
| Producto | × | Todas combinaciones | Esquemas disjuntos |
| Selección | σ | Filtrar por condición | - |
| Proyección | π | Seleccionar columnas | - |
| Reunión | ⋈ | Combinar relaciones | Atributos comunes |
| División | ÷ | "Para todos" | Relación binaria/unaria |

El álgebra relacional proporciona un **marco formal y preciso** para manipular datos relacionales, siendo fundamental para entender cómo funcionan los sistemas de bases de datos modernos a nivel conceptual.

### 3.2 Cálculo relacional
El cálculo relacional constituye el fundamento declarativo del modelo relacional, ofreciendo una aproximación matemática basada en lógica de predicados para expresar consultas sobre bases de datos. A diferencia del álgebra relacional, que es de naturaleza procedimental, el cálculo relacional permite especificar qué datos se desean recuperar sin definir explícitamente cómo obtenerlos, utilizando expresiones del tipo {t | condición(t)} donde t representa tuplas que satisfacen ciertas propiedades. Esta aproximación teórica, desarrollada inicialmente por Codd y luego refinada en variantes como el cálculo relacional de tuplas y el de dominios, demostró ser formalmente equivalente al álgebra relacional en poder expresivo, estableciendo así las bases teóricas que posteriormente inspirarían el diseño de lenguajes declarativos como SQL.
