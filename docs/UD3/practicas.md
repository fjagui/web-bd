# Actividades. Modelo relacional.
## Actividad 1. Ejercicios de unidad.
  1. Describe las diferencias entre los términos relación y esquema de la relación.
  2. Define los términos clave primaria y clave externa.
  3. Expresa las reglas de integridad de las entidades y de integridad referencial.
  4. ¿Puede aceptar nulos una clave externa? Justifica la respuesta.
  5. ¿Qué deberá suceder si hay un intento de eliminar el objetivo de una referencia de clave externa, por ejemplo un intento de eliminar un proveedor del cual existe por lo menos un envío?
  6. ¿Qué deberá suceder si hay un intento de modificar la clave primaria del objetivo de una referencia de clave ajena, por ejemplo un intento de modificar el número de proveedor del cual existe por lo menos un envío?
  7. ¿Cuáles son las restricciones inherentes que provienen de la definición matemática de relación?
  8. Haz una clasificación de los tipos de relación.
  9. ¿Pueden estar definidas la clave externa y su correspondiente clave primaria definidas sobre dominios diferentes? Justifica la respuesta.
  10. Indica las posibles acciones que se pueden tomar ante la modificación y borrado de una clave ajena.
  11. El usuario de una base de datos debe plantearse, en el momento de realizar una consulta, el orden en que se han de realizar las distintas operaciones? Justifica la respuesta.
  12. Diseña un esquema relacional que recoja la organización de un sistema de información sobre municipios, viviendas y personas. Cada persona sólo puede habitar en una vivienda y residir en un municipio, pero puede ser propietaria de más de una vivienda. Nos interesa también la interrelación de las personas con su cabeza de familia (realiza los supuestos semánticos complementarios que consideres oportunos para justificar todas las decisiones de diseño).
  13. Diseña un esquema relacional que permita a un profesor llevar las notas de sus alumnos. Haz las suposiciones semánticas que consideres necesarias.
  14. Se desea diseñar una base de datos que contenga la información relativa a las carreteras de un determinado país. Se pide realizar el diseño en el modelo E/R, sabiendo que:
  En dicho país las carreteras se encuentran divididas en tramos. 
  Un tramo siempre pertenece a una única carretera y no puede cambiar de carretera.
  Un tramo puede pasar por varios términos municipales, siendo un dato de interés el kilómetro del tramo por el que entra en dicho término municipal y el kilómetro por el que sale.
  Existen una serie de áreas en las que se agrupan los tramos y cada uno de ellos no puede pertenecer a más de un área.

  15. Supongamos las siguientes relaciones:  
  ```
	S (s#, snombre, estado, ciudad)
	P (p#,pnombre, color, peso,ciudad)
	SP(s#,p#,cantidad)
  ```  

  Realiza las operaciones de álgebra relacional para:

  - Obtener el nombre de los proveedores que suministran la pieza con el código P2.
  - Obtener el nombre de los proveedores que suministran por lo menos una pieza roja.
  - Obtener el nombre de las piezas de color rojo suministradas por los proveedores de la ciudad de Londres.
  - Obtener el código de los proveedores que suministran alguna de las piezas que suministra el proveedor con el código S2.
  - Obtener los datos de los envíos de más de 100 unidades, mostrando también el nombre del proveedor y el de la pieza.
  - Obtener el nombre de los proveedores que suministran todas las piezas.
  - Obtener el código de los proveedores que suministran, al menos, todas las piezas suministradas por el proveedor con código S2.
  - Obtener el nombre de los proveedores que no suministran la pieza con el código P2.
  - Obtener los datos de los proveedores que sólo suministran piezas de color rojo.