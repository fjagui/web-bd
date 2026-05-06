# Programación de Bases de Datos. PL/SQL.
## Actividad 1.
Actividades propuestas de unidad.

## Actividad 2.
### 🐾 Práctica: Sistema de Gestión "Huellas Seguras"

#### 1. 🎯 Objetivos.
Desarrollar un sistema profesional de gestión para una protectora de animales. El foco no es solo la funcionalidad, sino el **proceso de ingeniería**: desde la especificación técnica hasta la implementación de lógica de negocio robusta en el servidor.

**Stack Tecnológico Obligatorio:**
* **Base de Datos:** Oracle Database (SQL + PL/SQL).
* **Interfaz:** Oracle APEX.
* **Diseño:** Documentación Markdown (OpenSpec).
* **IA (Opcional):** Herramienta de apoyo con registro de prompts.

#### 2. 🧠 Metodología: El "Contrato" de Diseño
Antes de tocar el código, es obligatorio definir la **Especificación de Diseño** en formato Markdown. Este documento será el "contrato" que guiará el desarrollo y la IA. Debe incluir:
1.  **Diccionario de Datos:** Nombre de tablas, columnas, tipos de datos y restricciones (PK, FK, Not Null).
2.  **Lógica de Negocio:** Descripción textual de qué debe ocurrir (ej: "Al adoptar, el animal deja de estar disponible").
3.  **Mapa de Navegación:** Qué páginas tendrá la app en APEX.

#### 3. 🗂️ Requisitos del Sistema
El sistema debe gestionar, como mínimo, las siguientes entidades:

* **Animales:** ID, Nombre, Especie, Raza, Fecha de entrada, Estado (Disponible / En tratamiento / Adoptado).
* **Adoptantes:** ID, Nombre, DNI, Email, Teléfono.
* **Adopciones:** ID, ID_Animal (FK), ID_Adoptante (FK), Fecha, Observaciones.
* **Historial Médico (Mejora):** ID, ID_Animal, Fecha, Descripción, Coste.

#### 4. 🧱 Requisitos Técnicos Obligatorios

#### A. Base de Datos (SQL)
* Scripts de creación de tablas con integridad referencial completa.
* Uso de secuencias o identidad para las claves primarias.

#### B. Lógica de Negocio (PL/SQL - El Corazón del Proyecto)
Es **obligatorio** centralizar la lógica en un **Package de Oracle** (ej. `PKG_PROTECTORA`).
1.  **Validaciones:** Función que verifique si un animal es apto para adopción (ej. no puede estar "En tratamiento").
2.  **Procedimiento de Adopción:** Un proceso único que inserte la adopción y actualice el estado del animal en una misma transacción (`COMMIT`/`ROLLBACK`).
3.  **Trigger de Auditoría:** Implementar al menos un disparador que registre en una tabla de logs cualquier cambio de estado de un animal (quién, cuándo y qué cambió).

#### C. Interfaz (Oracle APEX)
* **CRUDs:** Formularios de mantenimiento para animales y adoptantes.
* **Proceso de Adopción:** Una página específica donde se invoque el procedimiento del paquete PL/SQL.
* **Informes:** Un Dashboard o Reporte Interactivo que muestre estadísticas (ej. animales por especie).

#### 5. 🤖 Uso Ético y Crítico de la IA
Se permite el uso de IA (ChatGPT, Claude, Roo Code, etc.) bajo estas condiciones:
* **Documentación de Prompts:** Se debe entregar un anexo con los prompts principales utilizados.
* **Validación Humana:** El alumno debe explicar y justificar cada bloque de código generado. *Si no sabes explicarlo, no puedes entregarlo.*
* **Refactorización:** La IA suele cometer errores en Oracle (ej. usar `VARCHAR` en vez de `VARCHAR2`). Corregir estos matices será evaluable.

#### 6. 📄 Entregables
1.  **📘 Especificación:** Documento Markdown con el diseño previo.
2.  **🗄️ Scripts SQL:** Archivo con el DDL completo (Tablas, Constraints, Triggers).
3.  **⚙️ Código PL/SQL:** Archivo con la especificación y el cuerpo del Package.
4.  **🖥️ App APEX:** Exportación `.sql` de la aplicación funcional.
5.  **🧠 Diario de IA:** Breve informe de prompts y reflexión sobre los cambios realizados al código sugerido por la IA.
