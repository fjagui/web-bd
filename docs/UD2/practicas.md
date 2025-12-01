# Actividades. Modelo entidad/interrelación.
## Actividad 1. Ejercicios de unidad.

1. ¿En qué fase del diseño de bases de datos se utiliza el modelo entidad-interrelación?
2. ¿Es cierto que una entidad puede tener más de un atributo identificador principal?
3. ¿Pueden las interrelaciones contener atributos? Pon un ejemplo.
4. Pon un ejemplo de jerarquía parcial y exclusiva.
5. ¿Es cierto que toda entidad débil necesita el atributo identificador principal de la entidad de la que depende, para su identificación?
6. Clasifica los tipos de correspondencia en las interrelaciones. Pon ejemplos .
7. ¿Qué son las entidades débiles? Pon un ejemplo.
8. ¿Puede un mismo elemento desempeñar dos funciones diferentes en un modelo? Pon un ejemplo. 
9. Indicar cuál de las frases siguientes define mejor el concepto de entidad.
    a. Entidad es una propiedad asociada a un atributo.
    b. Entidad es un objeto que presenta interés para una organización y acerca del cual se recoge información.
    c. La entidad es cualquier objeto del mundo real.
    d. Ninguna de las anteriores.
10. Responder V/F a las siguientes afirmaciones:
    a. Las ocurrencias de un tipo de entidad suelen tener distintos atributos.
    b. Las existencias de un tipo de entidad fuerte depende de otro tipo de entidad.
    c. No existen tipos de interrelación que asocien más de tres tipos de entidad.
    d. Un tipo de entidad débil depende en existencia de otro tipo de entidad también débil.

## Actividad 2. Modalo entidad/interrelación.

Realiza el diseño conceptual de los siguientes sistemas de información.

**Ejercicio1. Campeonato de ajedrez.** 

El club de Ajedrez de Villatortas de Arriba, ha sido encargado por la Federación Internacional de Ajedrez de la organización de los próximos campeonatos mundiales que se celebrarán en la mencionada localidad. Por este motivo, desea llevar una base de datos con toda la gestión relativa a participantes, alojamientos y partidas. Teniendo en cuenta que:  

En el campeonato participan jugadores y árbitros; de ambos se requiere conocer el número de asociado, nombre, dirección, teléfono de contacto y campeonatos en los que ha participado (como jugador o como árbitro). De los jugadores se precisa además el nivel de juego en una escala de 1 a 10.  

Ningún árbitro puede participar como jugador.  

Los países envían al campeonato un conjunto de jugadores y árbitros, aunque no todos los países envían participantes. Todo jugador y árbitro es enviado por un único país. Un país puede ser representado por otro país.  

Cada país se identifica por un número correlativo según su orden alfabético e interesa conocer además de su nombre, el número de clubes de ajedrez existentes en el mismo.  

Cada partida se identifica por un número correlativo (cod_p), la juegan dos jugadores y la arbitra un árbitro. Interesa registrar las partidas que juegan cada jugador y el color (blancas o negras) con el que juega. Ha de tenerse en cuenta que un árbitro no puede arbitrar a jugadores enviados por el mismo país que le ha enviado a él.  

Todo participante participa al menos en una partida.  

Tanto jugadores como árbitros se alojan en uno de los hoteles en los que se desarrollan las partidas, se desea conocer en qué hotel y en qué fechas se ha alojado cada uno de los participantes. Los participantes pueden no permanecer en Villatortas durante todo el campeonato, sino acudir cuando tienen que jugar alguna partida alojándose en el mismo o distinto hotel. De cada hotel, se desea conocer el nombre, la dirección y el número de teléfono.  

El campeonato se desarrolla a lo largo de una serie de jornadas (año, mes, día) y cada partida tiene lugar en una de las jornadas aunque no tengan lugar partidas todas las jornadas.  

Cada partida se celebra en una de las salas de las que pueden disponer los hoteles, se desea conocer el número de entradas vendidas en la sala para cada partida. De cada sala, se desea conocer la capacidad y medios de que dispone (radio, televisión, vídeo…) para facilitar la retransmisión de los encuentros. Una sala puede disponer de varios medios distintos.  

De cada partida se pretende registrar todos los movimientos que la componen, la identificación de movimiento se establece en base a un número de orden dentro de cada partida: para cada movimiento se guardan la jugada (5 posiciones) y un breve comentario realizado por un experto.  

- - -

**Ejercicio 2. Asociaciones no gubernamentales.**

La coordinadora nacional de Organizaciones No Gubernamentales desea mantener una base de datos de las asociaciones de este tipo que existen en nuestro país. Para ello necesita almacenar información sobre cada asociación, los socios que la componen, los proyectos que realizan y los trabajadores de las mismas.  

De las asociaciones se desea almacenar su CIF, denominación, dirección y provincia, su tipo (ecologista, integración, desarrollo…) así como si esta declara de utilidad pública por el Ministerio del Interior.  

Cada asociación está formada por socios de los que se precisa conocer su DNI, nombre, dirección, provincia, fecha de alta en la asociación, la cuota mensual con que colabora y la aportación anual que realizan (que se obtendrá multiplicando la cuota mensual por los meses del año).  

Los trabajadores de estas organizaciones pueden ser de dos tipos: asalariados y voluntarios.  

Los asalariados son trabajadores que cobran un sueldo y ocupan cierto cargo en la asociación. Se desea almacenar la cantidad que éstos pagan a la seguridad social y tanto por ciento de IRPF que se le descuenta.  

Los voluntarios trabajan en la organización desinteresadamente, siendo preciso conocer su edad, profesión y las horas que dedican a la asociación a efectos de cálculo de estadísticas.  

Cada trabajador se identifica por su DNI, tiene un nombre y una fecha de ingreso.  

Un socio no puede ser trabajador de la asociación.  

Las asociaciones llevan a cabo proyectos. De cada proyecto se desea almacenar su número de identificación dentro de la asociación, en qué país se lleva a cabo y en qué zona de éste, así como el objetivo que persigue y el número de beneficiarios a los que afecta. Un proyecto se compone a su vez de subproyectos (que tienen entidad de proyecto).  

- - -

**Ejercicio 3. Cursos de formación.**

El departamento de formación de una empresa desea construir una base de datos para planificar y gestionar la formación de sus empleados.  

La empresa organiza cursos internos de formación de los que se desea conocer el código de curso, el nombre, una descripción, el número de horas de duración y el coste del curso.  

Un curso puede tener como prerrequisito haber realizado otro(s) previamente, y, a su vez la realización de un curso puede ser prerrequisito de otros. Un curso que es un prerrequisito de otro puede serlo de forma obligatoria o sólo recomendable.  

Un mismo curso tiene diferentes ediciones, es decir, se imparte en diferentes lugares, fechas y con diferentes horarios (intensivo, de mañana o de tarde). En una misma fecha de inicio sólo puede impartirse una edición de un curso.  

Los cursos se imparten por personal de la propia empresa.  

De los empleados se desea almacenar su código de empleado, nombre y apellidos, dirección, teléfono, NIF, fecha de nacimiento, nacionalidad, sexo, firma y salario, así como si está o no capacitado para impartir cursos.  

Un mismo empleado puede ser docente en una edición de un curso y alumno en otra edición, pero nunca puede ser ambas cosas a la vez (en una misma edición de curso o lo imparte o lo recibe).  

- - -

**Ejercicio 4. Energía Eléctrica.**

Se pretende llevar a cabo un control sobre la energía eléctrica que se produce y consume en un determinado país. Se parte de las siguientes hipótesis.  

Existen productores básicos de electricidad que se identifican por un nombre, de los cuales interesa su producción media, producción máxima y fecha de entrada en funcionamiento. Estos productores básicos pertenecen a algunas de las siguientes categorías: Central Hidroeléctrica, Central Solar, Central Nuclear o Central Térmica. De una central hidroeléctrica o presa interesa saber su ocupación, capacidad máxima y número de turbinas. De una central solar interesa saber la superficie total de paneles solares, la media anual de horas de sol y el tipo (fotovoltaica o termodinámica). De una central nuclear, interesa saber el número de reactores que posee, el volumen de plutonio consumido y el de residuos nucleares que produce. De una central térmica, interesa saber el número de hornos que posee, el volumen de carbón consumido y el volumen de su emisión de gases.  

Por motivos de seguridad nacional interesa controlar el plutonio de que se provee una central nuclear. Este control se refiere a la cantidad de plutonio que compra a cada uno de sus posibles suministradores (nombre y país) y que porta un determinado transportista (nombre y matrícula). Ha de tenerse en cuenta que un mismo suministrador puede vender plutonio a distintas centrales nucleares y que cada porte (un único porte por compra) puede realizarlo un transportista diferente.  

Cada día, los productores entregan la energía producida a una o varias estaciones primarias, las cuales pueden recibir diariamente una cantidad distinta de energía de cada uno de esos productores. Los productores entregan siempre el total de su producción. Las estaciones primarias se identifican por su nombre y tienen un número de transformadores de baja a alta tensión y son cabecera de una o varias redes de distribución. 

Una red de distribución se identifica por un número de red y sólo puede tener una estación primaria como cabecera. La propiedad de una red puede ser compartida por varias compañías eléctricas. A cada compañía eléctrica se le identifica por su nombre.  

La energía sobrante en una de las redes puede enviarse a otra red. Se registra el volumen total de energía intercambiada entre dos redes.  

Una red está compuesta por una seria de líneas, cada línea se identifica por un número secuencial dentro del número de red y tiene una determinada longitud. La menor de las líneas posibles abastecerá al menos a dos subestaciones.  

Una subestación es abastecida sólo por una línea y distribuye a una o varias zonas de servicio. A estos efectos, las provincias (código y nombre), se encuentran divididas en tales zonas de servicio, aunque no puede haber zonas de servicio que pertenezcan a más de una provincia. Cada zona de servicio puede ser atendida por más de una subestación.

En cada zona de servicio se desea registrar el consumo medio y el número de consumidores finales de cada una de las siguientes categorías: particulares, empresas e instituciones.  

---

**Ejercicio 5. Conflictos Bélicos.**

Una Organización Internacional pretende realizar un seguimiento de los conflictos bélicos que se producen en todo el mundo. Para ello creará una base de datos que responderá al siguiente análisis:   

Se entiende por conflicto cualquier lucha armada que afecte a uno o varios países y en el cual se produzcan muertos y/o heridos. Todo conflicto se identificará por un nombre que habitualmente hará referencia a la zona o causa que provoca el conflicto, aunque dado que ese nombre puede cambiar con el
paso del tiempo, dentro de la base de datos cada conflicto se identificará mediante un código numérico sin significado alguno. Para cada conflicto se desea recoger los países a que afecta, así como el número de muertos y heridos contabilizados hasta el momento.  

Los conflictos pueden ser de distintos tipos según la causa que lo ha originado, clasificándose, a lo sumo, en cuatro grupos: territoriales, religiosos, económicos o raciales. En cada grupo se recogerán diversos datos. En los conflictos territoriales se recogerán las regiones afectadas, en los religiosos las religiones afectadas, en los económicos las materias primas disputadas y en los raciales las etnias enfrentadas.  

En los conflictos intervienen diversos grupos armados (al menos dos) y diversas organizaciones mediadoras (podría no haber ninguna). Los mismos grupos armados y organizaciones mediadoras pueden intervenir en diferentes conflictos. Tanto los grupos armados como las organizaciones mediadoras podrán entrar y salir del conflicto. En ambos casos se recogerá tanto la fecha de incorporación como la fecha de salida. Temporalmente, tanto un grupo armado como una organización mediadora podrían no intervenir en conflicto alguno.   

De cada grupo armado se recoge el código que se le asigna y un nombre. Cada grupo armado dispone de al menos una división y es liderado por al menos un líder político. Las divisiones de que dispone un grupo armado se numeran consecutivamente y se registra el número de barcos, tanques, aviones y hombre de que dispone. Asimismo, se recoge el número de bajas que ha tenido. Para los grupos armados se recoge el número de bajas como la suma de las bajas producidas en todas sus divisiones.  

Los traficantes de armas suministran diferentes tipos de arma a los grupos armados. De cada tipo de armas se recoge un nombre y un indicador de su capacidad destructiva. De cada traficante se recoge un nombre, los diferentes tipos de arma que puede suministrar y la cantidad de armas de cada uno de los tipos de arma que podría suministrar. Se mantiene el número total de armas de cada uno de los diferentes tipos de armas suministrados por cada traficante a cada grupo armado.  

Los líderes políticos se identifican por su nombre y por el código de grupo armado que lideran. Además se recoge una descripción textual de los apodos que éste posee.  

Cada división la pueden dirigir conjuntamente un máximo de tres jefes militares, aunque cada jefe militar no dirige más de una división. A cada jefe militar se le identifica por un código. Además, se recoge el rango que éste posee y dado que un jefe militar no actúa por iniciativa propia sino que siempre  obedece las órdenes de un único líder político de entre aquellos que lideran al grupo armado al que el jefe pertenece, se registrará el líder político al que obedece.  

De las organizaciones mediadoras se recogerá su código, su nombre, su tipo (gubernamental, no gubernamental o internacional), la organización de qué depende (una cómo máximo), el número de personas que mantiene desplegadas en cada conflicto y el tipo de ayuda que presta en cada conflicto que será de uno y sólo uno de los tres tipos siguientes: médica, diplomática o presencial.   

Con diversos fines, los líderes políticos dialogan con las organizaciones; se desea recoger explícitamente esta información. Así para cada líder se recogerán aquellas organizaciones con que dialoga y viceversa.    

- - -

**Ejercicio 6. Gestión de nóminas.**

Una Empresa decide informar su gestión de nóminas. Del resultado del análisis realizado, se obtienen las siguientes informaciones:   

A cada empleado se le entregan múltiples justificantes de nómina a lo largo de su vida laboral en la empresa y al menos uno mensualmente. A cada empleado se le asigna un número de matrícula en el momento de su incorporación a la empresa, y éste es el número usado a efectos internos de identificación. Además, se registran el Número de Identificación Fiscal del empleado, nombre, número de hijos, porcentaje de retención para Hacienda, datos de cuenta corriente en la que se le ingresa el dinero (banco, sucursal y número de cuenta) y departamentos en los que trabaja. Un empleado puede trabajar en varios departamentos y en cada uno de ellos trabajará con un función distinta.  

De un departamento se mantiene el nombre y cada una de sus posibles sedes.  

Son datos propios de un justificante de nómina el ingreso total percibido por el empleado y el descuento total aplicado. La distinción entre dos justificantes de nómina se hará, además de mediante el número de matrícula del empleado, mediante el ejercicio fiscal y número de mes al que pertenece y con un número de orden en el caso de varios justificantes de nómina recibidos el mismo mes.   

Cada justificante de nómina consta de varias líneas (al menos una de ingresos) y cada línea se identifica por un número de línea del correspondiente justificante. Una línea puede corresponder a un ingreso o a un descuento. En ambos casos, se recoge la cantidad que corresponde a la línea (en positivo si se trata de un ingreso o en negativo si se trata de un descuento); en el caso de los descuentos, se recoge la base sobre la cual se aplica y el porcentaje que se aplica para el cálculo de éstos.   

Toda línea de ingreso de un justificante de nómina responde a un único concepto retributivo. En un mismo justificante, puede haber varias líneas que respondan al mismo concepto retributivo. De los conceptos retributivos se mantiene un código y una descripción.   

De cara a la contabilidad de la empresa, cada línea de un justificante de nómina se imputa al menos a un elemento de coste. Al mismo elemento de coste pueden imputársele varias líneas. Para cada elemento de coste, se recoge un código, una descripción y un saldo.  

Entre los elementos de coste se establece una jerarquía, en el sentido de que un elemento de coste puede contener a otros elementos de coste, pero un elemento de coste sólo puede estar contenido en, a lo sumo, otro elemento de coste.   

En determinadas fechas, que se deben recoger, cada elemento de coste se liquida con cargo a varios apuntes contables (código y cantidad) y a una o varias transferencias bancarias, de las que se recogen los datos de cuenta corriente (banco, sucursal y número de cuenta) y la cantidad. Por cada apunte contable y transferencia bancaria se pueden liquidar varios elementos de coste.  

- - -

**Ejercicio 7. Entorno de ejecución.**

Una empresa decide crear un único entorno de ejecución que controle la seguridad de acceso para todas sus aplicaciones informáticas. Para ello considera conveniente dividir sus aplicaciones en subsistemas funcionales especializados y establecer el control de acceso al nivel de estos subsistemas. Se desarrollará un motor de ejecución que, tomando como parámetros los contenidos de la BD, controlará la ejecución de los subsistemas y el acceso a los mismos. Este motor se hará cargo también de la navegación dentro de los subsistemas. Profundizando en este enfoque, se establecen los siguientes requisitos:

La unidad básica de acceso a los subsistemas es el denominado perfil de acceso. Un usuario tendrá acceso a todos los subsistemas a los que permiten acceder los distintos perfiles de que disfruta (al menos uno). Un perfil permite el acceso de al menos un subsistema y para cualquier subsistema habrá siempre un perfil que permita acceder al mismo.

De cada usuario se mantiene el DNI, nombre, teléfono y terminales en que trabaja.

De los perfiles de acceso, lo mismo que de los subsistemas, se mantiene un código y una descripción. De los subsistemas, se mantiene, además, la ventana en la que arranca.

Las ventanas están compuestas por controles; toda ventana tendrá un control que permita cerrarla. Todo control ha de emplearse en alguna ventana y el mismo control puede ser empleado en distintas ventas. De las ventanas y controles se mantiene también un código y una descripción.  

Los controles pueden ser de dos tipos; botones o ítems de menú. Para soportar la estructura jerárquica de menús, de un ítem de menú pueden depender otros ítems, pero no puede darse la situación de que el mismo ítem dependa de varios ítems. En los ítems de menú se ha de mantener forzosamente el texto que se visualizará en pantalla. De los controles de tipo botó se mantiene el nombre del icono que opcionalmente se visualizará.  

La activación de un control tiene como consecuencia la ejecución de una única acción (todo control ejecutará una acción al menos). Una acción requiere siempre un control que pueda ejecutarla. De las acciones se mantiene el código y la descripción.  

Las acciones pueden ser de dos tipos, de función y de llamada. Las acciones de función ejecutan una función interna del propio entorno (de la que se ha de guardar el nombre). Las acciones de llamada invocan una única ventana.  

---

**Ejercicio 8. Administración de fincas.**

Una firma de abogados dedicada a la administración de fincas desea tener una base de datos para facilitar la gestión de la información de sus clientes, es decir, de las distintas comunidades de vecinos que administra. La información que debe contener la base de datos concierne a los aspectos que se describen a continuación. 

La firma tiene varios abogados y cada uno de ellos ejerce de administrador de una o más comunidades de vecinos, por lo que cobra a cada una de ellas unos honorarios anuales. Una comunidad de vecinos es gestionada por un único administrador (Nombre, Documento Nacional de Identidad y Número de Colegiado). Las funciones de un administrador, sobre las que en este caso interesa guardar información, consisten en llevar la contabilidad de la comunidad, gestionando los recibos que pagan los vecinos mensualmente, así como los pagos a las distintas compañías que proporcionan algún servicio a la comunidad (limpieza, ascensores, seguridad, luz, etc.).   

De las empresas que tienen contratadas las distintas comunidades de vecinos se guarda su nombre, Código de Identificación Fiscal, dirección, teléfono y una persona de contacto. Además, interesa tener estas compañías agrupadas en diferentes sectores (luz, seguridad, ascensores, etc.).  

De cada comunidad de vecinos gestionada por la firma de abogados interesa almacenar un código identificador, su nombre, calle, código postal y población. Cada comunidad consta de una serie de propiedades que pueden ser de tres tipos (vivienda particular, local comercial y oficina). Cada propiedad se caracteriza por un número de portal, planta y letra, un nombre y apellidos del propietario con su dirección completa (que puede ser ésta u otra) y un teléfono de contacto, un porcentaje de participación en los gastos de la comunidad así como los datos de la cuenta bancaria en la que el propietario desea se le domicilie el pago de los recibos.  

Si el propietario no habita en su propiedad entonces se necesitan sus datos (nombre, apellidos, dirección y teléfono de contacto) así como los del inquilino que la habita (nombre, apellidos y teléfono de contacto), en caso de que esté habitada la propiedad. Si el propietario habita en la propiedad sólo son necesarios sus datos (nombre, apellidos, teléfono de contacto). 

Si la vivienda es particular se guardará el número de habitaciones de que dispone; si es un local comercial se almacenará el tipo de comercio que se desarrolla en él y el horario (en caso de que esté en uso); si es una oficina se guardará la actividad a la que se destina.  

Cada comunidad de vecinos tiene además un presidente y varios vocales (nombre, apellidos y propiedad de la que son dueños) elegidos entre todos los propietarios, que se encargan de tratar directamente con el administrador los distintos problemas que pudieran surgir.  

En cuanto a la contabilidad, cada comunidad de vecinos tiene una cuenta en un banco. De los distintos bancos se almacena el código de banco, el nombre y una persona de contacto, mientras que para una cuenta bancaria se guarda un código de cuenta (que costa de un código de sucursal, dos dígitos de control y un número de cuenta) y un saldo. Para identificar una cuenta es necesario añadir al código de cuenta el código del banco en el que se encuentra.  

Es necesario almacenar dos tipos de apuntes (ingresos y gastos) para la contabilidad de cada comunidad de vecinos.  
 
Por un lado, aunque es el banco el que emite los recibos de las cuotas de comunidad a los distintos propietarios, el administrador guarda información sobre dichos recibos que se ingresan en las cuentas bancarios de las comunidades, es decir, el  número de recibo, fecha, importe y si se ha podido cobrar o no. Esta última información es importante para realizar a final de cada trimestre una relación de impagados.  

En cuanto a los apuntes relativos a los gastos se tienen los importes que cobran las empresas contratadas por cada comunidad de vecinos. Las compañías cobran sus recibos (número de recibo, fecha e importe) cargándolos en la cuenta de cada comunidad.  

---

**Ejercicio 9. Gestión de hospitales.**

Una compañía aseguradora de tipo sanitario, desea diseñar una BBDD para informatizar parte de su gestión hospitalaria. En una primera fase sólo quiere contemplar los siguientes supuestos semánticos:  

Los hospitales de su red pueden ser propios o concertados; además de unos datos comunes a todos ellos como son el código de hospital, nombre, número de camas, etc., cuando el hospital es propio se tienen otros específicos como el presupuesto, tipo de servicio, etc.  

Una póliza que se identifica por un número de póliza tiene varios atributos que en un principio no interesa especificar y que se agrupan bajo el nombre de datos de póliza. Una póliza cubre a varios aseguradores, los cuales se identifican por un numero correlativo, añadido al código de la póliza y tienen un nombre, fecha de nacimiento, etc.  

Los aseguradores cubiertos por una misma póliza pueden tener distintas categorías. Mientras los aseguradores de primera categoría, pueden sr hospitalizados en cualquier hospital, los de segunda categoría sólo pueden ser hospitalizados en hospitales propios. Aunque las otras categorías no tienen derecho a hospitalización, en la BD se guardan todos los asegurados sea cual sea su categoría.  

Interesa saber en qué hospitales han estado (o están) hospitalizados los asegurados, el medico que prescribió la hospitalización, así como las fechas de inicio y de fin de la misma.  

Existen áreas,  identificadas por un código y con datos sobre su superficie, número de habitantes, etc. Los hospitales concertados tienen que estar asignados a una única área que no puede cambiar, mientras que los propios no están asignados a áreas.  

Los médicos que se identifican por un código, tienen un nombre, teléfonos de contacto, etc.  Interesa conocer las áreas a las que está adscrito un médico. Existe dependencia jerárquica entre médicos de forma que un medico tiene un único jefe.  

---

**Ejercicio 10. Olimpiadas de invierno.**

Como parte de la organización de las próximas olimpiadas de invierno, se decide la creación de un sistema de información para realizar la gestión de las pruebas de esquí. Del análisis realizado se obtiene la siguiente información:  

Los juegos se componen de una serie de pruebas, en cada una de las cuales intervienen una serie de participantes. Cada participante en una prueba puede intervenir a título individual (esquiador individual) o bien formando parte de un equipo, en cuyo caso el participante será el equipo (no el esquiador). De cada esquiador (individual o de equipo) se desea tener el DNI, el nombre y la edad. A cada participante en una prueba (esquiador individual o equipo participante) se le asigna un código de participación dentro de la prueba (nombre de la prueba y un número secuencial). De cada equipo se mantiene un nombre, un entrenador, los esquiadores que lo componen y el número de éstos. El que un equipo participe en una prueba no significa que todos los esquiadores que lo componen intervengan en la misma. Un esquiador que forma parte de un equipo, no podrá cambiarse a otro ni actuar a título individual mientras duran los Juegos. Tampoco un esquiador individual podrá pasar a formar parte de un equipo.  

Existen una serie de federaciones de esquí, cada una de las cuales tiene un nombre y un número de federados (en las federaciones se federan los esquiadores a título individual). Por un acuerdo existente entre las distintas federaciones, no se permite que ningún esquiador se federe en dos federaciones distintas. Tampoco se admite que participen esquiadores (ni a título individual ni formando parte de un equipo) que no estén federados.  

Cada federación puede administrar una serie de estaciones de esquí, y toda estación se administrará al menos por una federación, aun cuando puede haber estaciones de esquí administradas conjuntamente por varias federaciones. Una estación de esquí se identifica por un código, tiene un nombre, unas personas de contacto, una dirección, un teléfono y un número total de kilómetros esquiables, así como las pistas de las que dispone.  

Dentro del sistema, cada pista se identifica a partir del código de la estación de esquí y un número secuencial. Se consideran también como pistas (para la realización de pruebas de largo recorrido) a varias de estas pistas (siempre de la misma estación) que por sus características físicas pudieran enlazarse. Así, por ejemplo, la pista diez estaría compuesta por las pistas dos y cuatro. Se requiere, para poder planificar las pruebas, mantener esta utilización combinada de las pistas. Para cada pista se mantiene también su longitud en kilómetros y su grado de dificultad (en la escala azul, verde, roja y negra).  

La realización de cada prueba se desarrollará a lo largo de varias jornadas en una serie de pistas de una única estación. Los equipos o esquiadores individuales podrán competir en diferentes pruebas y en distintas pistas. Para cada participante en una prueba (equipo o esquiador individual) se registrará la fecha o fechas en que participa, el tiempo empleado y la posición obtenida; en el caso de equipos, estos datos se obtienen de los correspondientes a cada uno de los esquiadores del equipo que han intervenido en la prueba.   

Cada prueba se identifica por un nombre, será de un tipo (fondo, slalom, salto...) tendrá unas fechas previstas de realización y se registrará el participante vencedor y el tiempo empleado por éste.  

---

**Ejercicio 11. Alquiler de automóviles.**

Se desea diseñar una base de datos sobre la información de las reservas de una empresa dedicada al alquiler de automóviles teniendo en cuenta que:  

Un determinado cliente puede tener en un momento dado hechas varias reservas.  

De cada cliente se desea almacenar su DNI, nombre, dirección y teléfono. Además dos clientes se diferencian por un código único.  

Cada cliente puede ser avalado por otro cliente de la empresa.  

Es importante registrar la fecha de inicio y final de la reserva, el precio del alquiler de cada uno de los coches, los litros de gasolina en el depósito en el momento de realizar la reserva, el precio total de la reserva y un identificador de si el coche o los coches han sido entregados.  

No se mantienen los datos de reservas anteriores.  

Todo coche tiene siempre asignado un determinado garaje que no puede cambiar.   

De cada coche se requiere la matrícula, el modelo, el color y la marca.  

Cada reserva se realiza en una determinada agencia.  

---

**Ejercicio 12. Tarjetas de crédito.**

Realiza el diseño conceptual para un sistema que contiene los siguientes elementos de información: TARJETAS DE CRÉDITO (identificadas por un número y que pueden ser de diferente tipo), PERSONAS PROPIETARIAS de esas tarjetas (de las que conocemos DNI, domicilio y teléfono), CUENTAS CORRIENTES (con un número, un saldo y una fecha de apertura). Las siguientes restricciones semánticas han de satisfacerse:  
Cada persona puede tener más de una tarjeta.  
Cada tarjeta pertenece a una persona.  
Cada tarjeta lleva asociada una única cuenta.  
Podemos cargar mas de una tarjeta a un cuenta determinada.  
Cada cuenta pertenece a una sola persona.  
Una persona puede tener más de una cuenta.  

Se desea también llevar el control de las operaciones realizadas con las tarjetas.  

**Ejercicio 13. Reservas de vuelos.**  

Se trata de organizar la información relativa a la gestión de reservas para vuelos. Debemos poder gestionar los datos que figuran en una tarjeta de embarque: Fecha y hora de emisión, a qué asiento corresponde, de qué avión, a qué vuelo corresponde, su fecha y hora de salida y a qué trayecto (ciudad de salida y ciudad de destino) de línea aérea pertenece ese vuelo. Se consideran, además, las siguientes restricciones semánticas mínimas:  
Tenemos diferentes aviones cuyos números de asiento pueden coincidir.  
Una tarjeta de embarque se corresponde con un asiento concreto de un avión concreto en un vuelo concreto.  
Un avión puede participar en diferentes vuelos.  
Un trayecto aéreo está identificado por un número y puede incluir varios vuelos con posible cambio de avión.  
Cada una de estos vuelos está caracterizado por una fecha y hora de partida.  
Puede existir más de una tarjeta de embarque por cada vuelo.  
Cada avión tiene una capacidad máxima.  

---

**Ejercicio 14. Gestión proyectos.**

Una empresa almacena datos referentes a :
	Departamentos: Depto#, Nom Dpto.  
	Empleados: DNI, Nombre, DNI Cónyuge.  
	Proyectos: Proy#, Nombre.  
	Proveedores: Prov#, Nombre, Teléfono, Dirección.  
	Productos: Prod#, Nombre, Precio.  

Las restricciones semánticas mínimas a cumplir son:
	Cada empleado trabaja en un departamento.  
	Un empleado puede trabajar en varios proyectos.  
	Existe un empleado que dirige cada proyecto.  
	Los proyectos usan productos.  
	Los precios de los productos pueden variar de un proveedor a otro.  
	Algunos productos tienen componentes que son, a su vez, productos.  

---

**Ejercicio 15. Gestión hospital.**

Realizar el diagrama E/R del sistema de información de un hospital en el que se maneja información de: MEDICOS, PLANTAS, HABITACIONES,  PACIENTES, ENFERMEROS, ENFERMEDADES, etc. Suponer las siguientes consideraciones:  

Médicos, enfermeros y pacientes tendrán los datos habituales de personas, nombre edad, etc. además de datos específicos de cada uno de ellos.  

Un enfermo puede tener varios ingresos con una fecha de entrada, otra de alta y su causa, la descripción de la causa, el médico responsable.  

Una planta estará compuesta de habitaciones, y tendrá una especialización clínica y un médico director.  

Las habitaciones tendrán, número de camas, características especiales, etc. Existirá información acerca de la ocupación de una habitación.  

Los enfermeros están asignados a una planta y tendrán un conjunto de habitaciones asignado.  

---

**Ejercicio 16. Parques naturales.**

El Ministerio de Medio Ambiente ha decidido crear un sistema de información sobre los parques naturales gestionados por cada comunidad autónoma. Después de realizar un detallado análisis, se ha llegado a las siguientes conclusiones:  

Una comunidad autónoma (CA) puede tener varios parques naturales. En toda comunidad autónoma existe uno y sólo un organismo responsable de los parques. Un parque puede estar compartido por más de una comunidad.  

Un parque natural se identifica por un nombre, fue declarado en una fecha, se compone de varias áreas identificadas por un nombre y caracterizadas por una determinada extensión. Por motivos de eficiencia se desea favorecer las consultas referentes al número de parques existentes en cada comunidad y la superficie total declarada parque natural en cada CA.  

En cada área forzosamente residen especie que pueden ser de tres tipos: vegetales, animales y minerales. Cada especie tiene una denominación científica, un nombre vulgar y número inventariado de individuos por área. De las especies vegetales se desea saber si tienen floración y en qué periodo se produce ésta; de las animales se desea saber su tipo de alimentación (herbívora, carnívora u omnívora) y sus periodos de celo; de las minerales se desea saber si se trata de cristales o de rocas. Además, interesa registrar qué especies sirven de alimento a otras especies, teniendo en cuenta que ninguna especie mineral se considera alimento de cualquier otra especie y que una especie vegetal no se alimenta de ninguna otra especie  

Del personal del parque se guarda el DNI, número de seguridad social, nombre, dirección, teléfonos (domicilio, móvil) y sueldo. Se distinguen los siguientes tipos de personal:  

- Personal de gestión: registra los datos de los visitantes del parque y están destinados en una entrada del parque (las entradas se identifican por un número).  
- Personal de vigilancia: vigila un área determinada del parque que recorre en un vehículo (tipo y  matrícula).  
- Personal de conservación: mantiene y conserva un área determinada del parque. Cada uno lo realiza en una especialidad determinada (limpieza, caminos, …)  
- Personal investigador: Tienen una titulación que ha de recogerse y pueden realizar (incluso conjuntamente) proyectos de investigación sobre una determinada especie.  

Un proyecto de investigación tiene un presupuesto y un periodo de realización.  

Un visitante (DNI, nombre, domicilio y profesión) debe alojarse dentro de los alojamientos de que dispone el parque; éstos tienen una capacidad limitada y tienen una determinada categoría.  

Los alojamientos organizan excursiones al parque, en vehículos o a pie, en determinado días de la semana y a una hora determinada. A estas excursiones puede acudir cualquier visitante del parque.


## Práctica: Diseño conceptual utilizando una herramienta de diseño y administración de bases de datos.

Diseño e implementación de un sistemas de bases de datos para la Gestión de la Liga de Fútbol Profesional.

#### Objetivo de la Práctica.

Desplegar un sistema de base de datos para la gestión de una liga de fútbol profesional, utilizando MySQL y MySQL Workbench, aplicando las fases de diseño conceptual, lógico y físico. 

#### Instrucciones.

**Fase 1: Instalación y Configuración**  

Tarea 1.1: Instalación de herramientas.  

  - Descargar e instalar MySQL Server.  
  - Descargar e instalar MySQL Workbench.  
  - Verificar la conexión entre ambas herramientas.  
  - Crear una nueva conexión en MySQL Workbench.    

Tarea 1.2: Preparación del Entorno

  - Crear una base de datos llamada liga_futbol. 
  - Documentar el proceso de instalación con capturas de pantalla.

**Fase 2: Diseño Conceptual**

Tarea 2.1: Diagrama entidad/interrelación utilizando una herramienta gráfica. Determina las entidades, atributos, relaciones, etc.
  
Tarea 2.2: Diagrama E-R en Workbench

  - Crear un nuevo modelo EER en MySQL Workbench.
  - Diseñar el diagrama Entidad-Relación con todas las entidades identificadas.
  - Establecer las relaciones entre entidades (1:N, N:M).
  - Definir los atributos principales de cada entidad.

**Fase 3: Diseño Lógico**

Tarea 3.1: Esquema relacional en Workbench.

  - Generar el esquema relacional directamente con Workbench.
  - Establecer restricciones de integridad.

Tarea 3.2: Diccionario de Datos

  - Crear un documento que incluya:
      - Nombre de cada tabla.
      - Descripción de su propósito.
      - Listado de campos con tipo de datos y descripción.
      - Relaciones entre tablas.

**Fase 4: Diseño Físico**

Tarea 4.1: Creación del esquema realacional.

  - Ejecutar en MySQL Workbench el script SQL para crear las siguientes tablas:

Tarea 4.2: Inserción de Datos de Prueba

  - Insertar un conjunto adecuado de datos de ejemplo.

**Fase 5: Consultas SQL**

Tarea 5.1: Consultas Básicas

  - Listar todos los equipos de la liga.
  - Mostrar jugadores de un equipo específico ordenados por dorsal.
  - Consultar partidos jugados por un equipo.
  - Proponer y realizar cinco consultas de interés.

#### Entrega.

Documentación técnica que incluya.
  - Documentación con pantallazos personalizados del proceso de instalación.
  - Diagrama E/R utilizando la notación inicial propuesta por Peter Chen para el modelo.
  - Diagrama E-R en formato PNG/PDF desde MySQL Workbench.
  - Script SQL completo con creación de tablas e inserciones.
  - Diccionario de datos en formato tabla.
  - Consultas SQL ejecutadas y resultados obtenidos

#### Criterios de Evaluación

  - Correcto diseño del modelo E-R.
  - Funcionamiento correcto de las consultas SQL
  - Documentación clara y completa
  - Cumplimiento de todos los requisitos