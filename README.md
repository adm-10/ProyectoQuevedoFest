# QuevedoFest

- [x] 1. Introducción

   INTRODUCCIÓN
   
Se esta organizando un festival para el verano de 2021 llamado Quevedo Fest.

Tendrá lugar en la Sala Caracol de Madrid ya que consta de una gran acústica e iluminación.

En dicho festival tocaran grupos de diferentes géneros musicales de diferentes partes del mundo. Las entradas se podrán adquirir en taquilla o por internet.

Este proyecto nace de la necesidad de llevar un control sobre la venta de entradas, y de las personas que asistirán a los conciertos de este festival, poder almacenar los datos de los clientes y el tipo de entrada que comprar. 

Para ello hemos realizado un seguimiento desde que el cliente compra la entrada vendida por el festival, que organiza los conciertos en los cuales actuaran los artistas. Con la ayuda de un diagrama entidad-Relación podremos ver de manera bastante clara como es la organización de dicho festival, esto nos dará una guía para crear una base de datos y poder completarla.

Una vez la base de datos esta lista, se podrá acceder a dicha información y podremos hacer cualquier consulta que se requiera, con lo cual podremos saber en detalle todo lo acontecido en Quevedo Fest.
- [x] 2. Modelo Conceptual
   - [x] 2.1. Especificaciones


Para la creación de la base de datos primero tendremos que realizar un diagrama de entidad-relacion de acuerdo a las caracteristicas que posee el festival, este diagrama se tiene que crear de manera logica siguiendo el proceso de gestión desde que el cliente compra la entrada, hasta que asisite al concierto a ver a su grupo.
Por ello lo dividimos en varias partes:

- Cliente: Estara identificado por un identificador, tambien nos interesa saber el nombre completo del cliente, por que via a comprado la entrada, sea en taquilla o por internet y la direccion de este.

- Entradas: Tiene un  identificador,fecha en la que fue comprada,el precio, nos interesa saber si la entrada tiene algun tipo de descuento, si es normal o vip, en caso de serlo tendra un coste adicional.

- Seguro: Todas las entradas lo tiene de forma opcional, en caso que lo quieran, tendra un coste adicional, al igual que todas las partes que componen el diagrama tiene un identificador.

- Festival: Son los encargados de vender las entradas y de organizar los conciertos, el cual se compone de un identificador, fecha de inicio, fecha de fin, las visitas que tiene, su nombre, la ubicación, que se compone de ciudad y recinto.

- Concierto: Se compone de una fecha en la que se realiza, la duración en que escenario se dara, que puede ser principal o secundario y un identificador.

- Retransmisión: tendra información de las visualizaciones, el medio por el que se esta retransmitiendo, ya sea stream, televisión o radio y un identificador.

- Artistas: Nos muestra el nombre, una descrpcion y un identificador

- Categoria: los artistas podran ser de varios generos, el cual tendra un identificador,y una breve descripción.

Una vez diseñado el modelo conceptual pasamos al modelo lógico, para ello pasamos nuestro diagrama a Modelo relacional en el que se definen la columnas y se definen el valor de los atributos con lo que se facilita el establecimiento de las relaciones entre los puntos de datos.

Pasamos a la normalización con el objetivo de minimizar la redundancia de datos, facilitando su gestión posterior, y en caso que sea necesario desnormalizaremos.

El sigueinte proceso es la creacion de diagramas de clase, con el cual podremos visualizar las relaciones entre las clases que involucran el sistema, y nos facilite la creacion de tablas y objetos.

Una vez creeados las tablas pasamos a cargar los datos que requiere el sistema.
Con los datos ya cargados podremos realizar consultas y obtener la información que requerramos en ese momento.
 
   - [x] 2.2. Diagrama Entidad-Relación
   
   <img src="/images/EntidadRelacionQF.png" alt="imagen Entidad-Relación"/>


- [x] 3. Modelo Lógico 
   - [x] 3.1. Modelo Relacional

 <img src="/images/RelacionalQF.png" alt="imagen Modelo-Relacional"/>
 
   - [x] 3.2. Normalización/Desnormalización

Normalización de las tablas donde fue necesario: 

CLIENTE

1-FN:
ID | nombre | apellido | edad
--- | --- | --- | --- 

ID | ciudad	| CP
--- | --- | --- 

2-FN:
ID | nombre | apellido| edad
--- | --- | --- | --- 

ID | ciudad
--- | --- 


FESTIVAL 

1-FN:
ID | Nombre | Fecha inicio | Fecha fin
--- | --- | --- | --- 

ID | Ciudad	| Recinto
--- | --- | --- 


RETRANSMISIÓN

1-FN:
ID | Visualizaciones
--- | --- 

ID | medio
--- | --- 


ENTRADAS

1-FN:
ID  | Precio | descuento | tipo
--- | --- | --- | --- 

ID | Fecha
--- | --- 

ID | Festival id
--- | --- 
 
2-FN:
ID | Descuento | tipo
--- | --- | --- 

ID | Fecha
--- | --- 
 
ID | Festival id
--- | --- 

ID | precio
--- | --- 


3-FN:
ID | descuento | Id descuento
--- | --- | --- 

ID | Tipo | Id tipo
--- | --- | --- 

ID | Precio | Id precio
--- | --- | --- 

ID | Fecha | Id fecha
--- | --- | --- 

ID | Festival id
--- | --- 


CONCIERTO

1-FN:
ID | Festival |	fecha
--- | --- | --- 

ID | escenario
--- | --- 

ID | festival
--- | --- 


2-FN:
ID | escenario
--- | --- 

ID | fecha
--- | --- 


3-FN:
ID | Escenario | Id escenario
--- | --- | --- 

ID | Festival |	Id festival
--- | --- | --- 

ID | fecha | Id fecha
--- | --- | --- 

- [x] 4. Modelo Físico
   - [x] 4.1. Diagrama de base de datos (notación "Crow's feet" o IDEF1X)

 <img src="/images/diagramaDataBase.png" alt="imagen Diagrama Base de Datos"/>
 
   - [x] 4.2. Creación de tablas y otros objetos

```
-- Estructura de la tabla 'cliente'

CREATE TABLE cliente (
    id INT NOT NULL, 
    nombre VARCHAR(40) NOT NULL,
    apellidos VARCHAR(40) NOT NULL,
    fecha_nacimiento DATE,
    ciudad VARCHAR(30),
    calle VARCHAR(65),
    cp NUMERIC (5),
    CONSTRAINT cliente_pk PRIMARY KEY (id)
);

-- Estructura de la tabla 'seguro'

CREATE TABLE seguro (
    id VARCHAR NOT NULL, 
    precio NUMERIC(6,2) NOT NULL,
    CONSTRAINT seguro_pk PRIMARY KEY (id)
);

-- Estructura tabla `festival`

CREATE TABLE festival(
    id INT NOT NULL DEFAULT '0001', 
    nombre VARCHAR DEFAULT 'QUEVEDO FEST',
    fecha_inicio DATE,
    fecha_fin DATE,
    ciudad VARCHAR DEFAULT 'MADRID',
    recinto VARCHAR DEFAULT 'SALA CARACOL',
    CONSTRAINT festival_pk PRIMARY KEY (id)
    );
	
-- Estructura tabla 'retransmision'

CREATE TABLE retransmision(
    id VARCHAR NOT NULL, 
    visualizaciones NUMERIC,
    medio VARCHAR CHECK( medio IN ('TV','STREAMING','RADIO')),
    CONSTRAINT retransmision_pk PRIMARY KEY (id)
    );
	
-- Estructura tabla 'categorias'

CREATE TABLE categorias (
    id VARCHAR NOT NULL, 
    genero VARCHAR CHECK( genero IN ('ROCK','POP','ELECTRONICA','RAP')),
    CONSTRAINT categorias_pk PRIMARY KEY (id)
    );
	
-- Estructura tabla 'entradas'

CREATE TABLE entradas(
    id VARCHAR NOT NULL, 
    festival_id INT NOT NULL,
    precio NUMERIC(6,2) NOT NULL,
    fecha DATE,
    descuento VARCHAR CHECK( descuento IN ('SI','NO')),
    tipo VARCHAR CHECK( tipo IN ('VIP','NORMAL')),
    CONSTRAINT entradas_pk PRIMARY KEY (id),
    CONSTRAINT entradas_festival_fk FOREIGN KEY (festival_id) 
    REFERENCES festival (id)
    );
	
-- Estructura tabla 'concierto'

CREATE TABLE concierto (
    id VARCHAR NOT NULL, 
    festival_id INT NOT NULL,
    fecha DATE,
    escenario VARCHAR CHECK( escenario IN ('PRINCIPAL','SECUNDARIO')),
    CONSTRAINT concierto_pk PRIMARY KEY (id),
    CONSTRAINT concierto_festival_fk FOREIGN KEY (festival_id) 
    REFERENCES festival (id)
    );

-- Estructura tabla 'artistas'

CREATE TABLE artistas (
    id VARCHAR NOT NULL, 
    concierto_id VARCHAR NOT NULL,
    nombre VARCHAR(50),
    CONSTRAINT artistas_pk PRIMARY KEY (id),
    CONSTRAINT artistas_concierto_fk FOREIGN KEY (concierto_id) 
    REFERENCES concierto (id)
    );

-- Estructura tabla 'compra'    

CREATE TABLE compra (
    cliente_id INT NOT NULL, 
    entrada_id VARCHAR NOT NULL,
    via VARCHAR CHECK( via IN ('TAQUILLA','INTERNET')),
    CONSTRAINT compra_pk PRIMARY KEY (cliente_id, entrada_id),
    CONSTRAINT compra_cliente_fk FOREIGN KEY (cliente_id) 
     REFERENCES cliente (id),
    CONSTRAINT compra_entrada_fk FOREIGN KEY (entrada_id) 
     REFERENCES entradas (id)
    );	
	
-- Estructura tabla 'contratar'

CREATE TABLE contratar (
    entrada_id VARCHAR NOT NULL,
    seguro_id VARCHAR NOT NULL, 
    CONSTRAINT contratar_pk PRIMARY KEY (entrada_id),
    CONSTRAINT contratar_entrada_fk FOREIGN KEY (entrada_id) 
     REFERENCES entradas (id),
    CONSTRAINT contratar_seguro_fk FOREIGN KEY (seguro_id) 
     REFERENCES seguro (id)
    );

CREATE TABLE emite (
    concierto_id VARCHAR NOT NULL,
    retransmision_id VARCHAR NOT NULL, 
    CONSTRAINT emite_pk PRIMARY KEY (concierto_id, retransmision_id),
    CONSTRAINT remite_concierto_fk FOREIGN KEY (concierto_id) 
     REFERENCES concierto (id),
    CONSTRAINT cemite_retransmision_fk FOREIGN KEY (retransmision_id) 
     REFERENCES retransmision (id)
    );

CREATE TABLE categorias_artistas (
    artistas_id VARCHAR NOT NULL,
    categorias_id VARCHAR NOT NULL, 
    CONSTRAINT categorias_artistas_pk PRIMARY KEY (artistas_id, categorias_id),
    CONSTRAINT categorias_artistas_artistas_fk FOREIGN KEY (artistas_id) 
     REFERENCES artistas (id),
    CONSTRAINT categorias_artistas_categorias_fk FOREIGN KEY (categorias_id) 
     REFERENCES categorias (id)
    );	

```
   - [x] 4.3. Carga de datos de prueba


```
-- Insertando datos en cliente

INSERT INTO cliente VALUES 

  (01,'ROSA MARIA','ACIEN ZURUTA','1992-05-06','MADRID','Calle Bailen','28024'),
  (02,'DANIEL','ALBUSAC TAMARGO','1995-08-15','MADRID','Calle Batalla de Belchite','28024'),
  (03,'JOSE','GIMENO MORA','1990-12-14','BARCELONA','Paseo del Doctor Vallejo-Nájera','08020'),
  (04,'SUSANA','IGLESIAS PASTOR','1991-10-03','MADRID','Plaza de los Hermanos Falcó','28024'),
  (05,'IRENE','RODRIGUEZ GARCIA','1992-07-05','BARCELONA','Paseo de Muñoz Grandes','08020'),
  (06,'NATALIA','JALDO SUAREZ','2000-08-13','MADRID','Calle del General García','28024'),
  (07,'MONICA','MUÑOZ BENAVIDES','1992-01-07','BARCELONA','Plaza Arriba España','08020'),
  (08,'SUSANA','LOPEZ CASADO','1996-11-10','BARCELONA','Calle Caídos','08020'),
  (09,'ISABEL','NAVARRO MARTIN','1995-05-01','SANTANDER','Calle del General Asensio Cabanillas','39001'),
  (10,'MATIAS','RODRIGUEZ MARTINEZ','1997-07-02','MADRID','Calle del General Dávila','28024'),
  (11,'RAFAEL','RODRIGUEZ PARRA','1996-01-06','BARCELONA','Paseo de Marcelino Camacho','08020'),
  (12,'SANDRA','HUERTAS PEREZ','1992-02-03','MADRID',' Calle de Juan Vigón','28024'),
  (13,'HUGO','LOPEZ ROBLES','1991-03-05','BARCELONA','Calle de Max Aub','08020'),
  (14,'JESSICA','CORTES AMATE','1990-08-08','SANTANDER','Calle de Melquíades Álvarez','39001'),
  (15,'BELEN','MATIAS FERNANDEZ','1999-10-15','MADRID','Calle del Maestro Ángel Llorca','28024'),
  (16,'ALICIA','GALERA GARCIA','1996-11-11','SANTANDER','Plaza del Rastrillo','39001'),
  (17,'LUIS','GARCIA QUESADA','2000-12-16','MADRID','Calle de Guillermo Rovirosa','28024'),
  (18,'MARTA','JIMENEZ GIL','2002-11-18','MADRID','Plaza de El Pardo','28024'),
  (19,'MONICA','LOPEZ OJEDA','2005-02-20','VALENCIA','Calle de Carlos Morla Lynch','46001'),
  (20,'DAVID','MARQUEZ GAMARRA','2006-03-21','SANTANDER','Calle de Manuel Chaves','39001'),
  (21,'JORGE','SANCHEZ RAMON','2003-04-23','VALENCIA','Calle de José Rizal','46001'),
  (22,'VANESA','VAZQUEZ GONGORA','1997-05-20','VALENCIA','Avenida de Las Águilas','46001'),
  (23,'JESUS','POLO GARCIA','1993-06-03','MADRID','Calle de la Maestra Justa Freire','28012'),
  (24,'JUAN','RUIZ HERNANDEZ','1990-07-01','BARCELONA','Calle de Soledad Cazorla','08020'),
  (25,'ALVARO','VILCHEZ CARMONA ','1998-08-04','SANTANDER','Calle de Robert Capa','39001'),
  (26,'JAVIER','MAÑAS LOPEZ','1995-09-06','MADRID','Calle de Anselmo Lorenzo','28015'),
  (27,'FRANCISCO','ARCOS CARMONA','2000-05-09','MADRID',' Calle de Blas Cabrera','28016'),
  (28,'ANTONIO','BUITRAGO CONTRERAS','1996-10-13','VALENCIA','Avenida de la Memoria','46001'),
  (29,'EDUARDO','CASAS PÁEZ','1992-11-16','OVIEDO','Glorieta de Ramón Gaya','33070'),
  (30,'ALEJANDRO','URIBE ANTIA','1995-12-31','BARCELONA','Calle de Gerda Taro','08020'),
  (31,'ANDRES','JIMENEZ CORTES','1993-01-20','SANTANDER','Calle del Arquitecto Sánchez Arcas','39001'),
  (32,'HECTOR','ROMERO MONTOYA','1994-02-21','BARCELONA','Plaza de José Moreno Villa','08020'),
  (33,'MARIO','DIAZ MEJIA','1994-03-06','SANTANDER','Calle de Melchor Rodríguez','39001'),
  (34,'ENRIQUE','JIMENEZ CORTES','1993-04-09','OVIEDO','Calle de la Filósofa Simone Weil','33070'),
  (35,'JULIO','SANCHEZ PRADA','1995-05-07','BARCELONA',' Calle de la Pintora Ángeles Santos','08020'),
  (36,'ANA','GONZALEZ SUAREZ','1999-05-07','MADRID','Calle del Barco Sinaia','28025'),
  (37,'ELENA','PUERTO CASTRO','1998-06-08','VALENCIA','Plaza de Corpus Barga','46001'),
  (38,'VANESA','RODRÍGUEZ TORRES','1999-07-01','OVIEDO',' Calle de Mercedes Fórmica','33070'),
  (39,'MARIA BELEN','NOVOA GOMEZ','2000-08-02','BARCELONA','Pasaje de Enrique Ruano','08020'),
  (40,'OLGA','DEL RÍO AYERBE','2001-09-03','MADRID',' Calle del Comandante Zorita','28029'),
  (41,'MANUELA','DUEÑAS ROJAS','2003-10-04','OVIEDO','Calle del General Orgaz','33070'),
  (42,'DOMINGO','ZÚÑIGA RAMÍREZ','2004-11-16','MADRID','Calle de Fortunata y Jacinta','28031'),
  (43,'ENCARNACION','SIERRA VILLAMIL','1999-12-12','BARCELONA','Calle del General Varel','08020'),
  (44,'CRISTINA','GARCÍA FONNEGRA','1998-01-13','SANTANDER','Calle de Julián Besteiro','39001'),
  (45,'LUCIA','GARCIA RUEDA','1999-02-14','MADRID','Calle del General Yagüe','28034'),
  (46,'NATALIA','BOLÍVAR GALEANO','1993-10-16','VALENCIA','Calle de San Germán','46001'),
  (47,'ROSARIO INMACULADA','HORTA OCHOA','1991-09-15','MADRID','Calle del General Moscardó','28031'),
  (48,'MARIA ISABEL','PEREZ MORENO','1995-08-17','OVIEDO','Calle de Edgar Neville','33070'),
  (49,'DANIEL','CERVANTES LUNA','1996-07-20','MADRID','Calle y Escalinata del General Aranda','28038'),
  (50,'MARGARITA','BUITRAGO CONTRERAS','1998-06-18','BARCELONA','Calle de Manuel Sarrión','08020'),
  (51,'ANTONIO','PUENTES PERDOMO','1996-04-21','MADRID',' Calle Capitán Haya','28040'),
  (52,'NOELIA','BARRERO FORERO','1997-03-19','SANTANDER','Calle del Poeta Joan Maragall','39001'),
  (53,'JORGE','CASAS PÁEZ','2002-02-11','MADRID','Plaza de Fernández Ladreda','28042'),
  (54,'RAFAEL','OVALLE SOLANO','2007-01-13','VALENCIA','Plaza Elíptica','46001'),
  (55,'VERONICA','CASTELLANOS ROJAS','1992-7-12','MADRID','Calle de Juana Doña','28033'),
  (56,'ARACELI','ULLOA ORJUELA','1994-08-02','OVIEDO','Plaza Mayor de Barajas','33070'),
  (57,'JAVIER','URIBE ANTIA','1999-09-03','MADRID','Paseo de Marcelino Camacho','28024'),
  (58,'LUIS','ALVAREZ CASTILLO','1996-10-05','SEVILLA','Calle de la Cooperación','41001'),
  (59,'MARIA','VEGA ZAMBRANO','1995-11-08','VALENCIA','Calle de Diego Torres Villarroel','46001'),
  (60,'ISABEL','IREGUI GALEANO','2002-12-09','MADRID','Plaza de la Charca Verde','28056'),
  (61,'CARMEN','REY SANCHEZ','1993-03-13','MADRID','Plaza de José CastillejO','28023'),
  (62,'NADIA','ACERO CARO','1994-04-10','MADRID','Plaza del Baile','28045'),
  (63,'ESTHER','MAHECHA PIÑEROS','1996-06-11','BARCELONA','Calle José Rizal','08020'),
  (64,'ANTONIO','FERNÁNDEZ MARTÍNEZ','1997-05-15','SANTANDER','Calle Memorial 11 de marzo de 2004','39001'),
  (65,'RAQUEL','GOMEZ GIANINE','1998-08-16','SEVILLA','Calle Soledad Cazorla','41001'),
  (66,'JAVIER','SUARIQUE ÁVILA','1993-09-20','OVIEDO','Paseo de Muñoz Grandes','33070'),
  (67,'LUIS','DIAZ BELTRAN','1991-10-21','VALENCIA','Plaza Aunós','46001'),
  (68,'GABRIEL','FORERO PEÑA','1998-11-22','SANTANDER','Calle de Juan Vigón','39001'),
  (69,'NICOLAS','NIETO BUSTOS','1994-12-13','MADRID','Calle Melquíades Álvarez','28030'),
  (70,'JAVIER','NIETO BUSTOS','1997-01-22','BARCELONA','Calle del Cine','08020');
  
  
-- Insertando datos tabla seguro

INSERT INTO seguro VALUES
 ('SG01','10'),
 ('SG02','10'),
 ('SG03','10'),
 ('SG04','10'),
 ('SG05','10'),
 ('SG06','10'),
 ('SG07','10'),
 ('SG08','10'),
 ('SG09','10'),
 ('SG10','10'),
 ('SG11','10'),
 ('SG12','10'),
 ('SG13','10'),
 ('SG14','10'),
 ('SG15','10');
 
 
 --Insertando datos festival   

INSERT INTO festival VALUES
 (0001,'QUEVEDO FEST',TO_DATE('24/06/2021', 'DD/MM/YYYY'),TO_DATE('27/06/2021', 'DD/MM/YYYY'),'MADRID','SALA CARACOL');
 
 
 
 -- Insertando datos retransmision
 
INSERT INTO retransmision VALUES
 ('RT01','500','TV'),
 ('RT02','200','STREAMING'),
 ('RT03','50','RADIO'),
 ('RT04','400','STREAMING'),
 ('RT05','90','TV'),
 ('RT06','73','STREAMING'),
 ('RT07','98','RADIO'),
 ('RT08','800','TV');
 
 
-- Insertando datos categorias
INSERT INTO categorias VALUES
 ('CTG01','ROCK'),
 ('CTG02','POP'),
 ('CTG03','ELECTRONICA'),
 ('CTG04','RAP'); 
 
 
 -- Insertar datos entradas

INSERT INTO entradas VALUES
 ('ET01',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET02',0001,'90',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'SI','NORMAL'),
 ('ET03',0001,'110',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET04',0001,'200',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','VIP'),
 ('ET05',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET06',0001,'110',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET07',0001,'100',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET08',0001,'90',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'SI','NORMAL'),
 ('ET09',0001,'200',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','VIP'),
 ('ET10',0001,'100',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET11',0001,'110',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET12',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET13',0001,'100',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET14',0001,'90',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'SI','NORMAL'),
 ('ET15',0001,'110',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET16',0001,'200',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','VIP'),
 ('ET17',0001,'90',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'SI','NORMAL'),
 ('ET18',0001,'110',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET19',0001,'90',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'SI','NORMAL'),
 ('ET20',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET21',0001,'110',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET22',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET23',0001,'200',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','VIP'),
 ('ET24',0001,'110',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET25',0001,'110',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET26',0001,'90',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'SI','NORMAL'),
 ('ET27',0001,'110',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET28',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET29',0001,'110',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET30',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET31',0001,'200',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','VIP'),
 ('ET32',0001,'100',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET33',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET34',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET35',0001,'200',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','VIP'),
 ('ET36',0001,'100',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET37',0001,'100',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET38',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET39',0001,'90',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'SI','NORMAL'),
 ('ET40',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET41',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET42',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET43',0001,'200',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','VIP'),
 ('ET44',0001,'100',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('4ET5',0001,'200',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','VIP'),
 ('ET46',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET47',0001,'110',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET48',0001,'200',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','VIP'),
 ('ET49',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET50',0001,'90',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'SI','NORMAL'),
 ('ET51',0001,'100',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET52',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET53',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET54',0001,'100',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET55',0001,'110',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET56',0001,'200',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','VIP'),
 ('ET57',0001,'90',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'SI','NORMAL'),
 ('ET58',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET59',0001,'110',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET60',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET61',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET62',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET63',0001,'90',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'SI','NORMAL'),
 ('ET64',0001,'100',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET65',0001,'110',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET66',0001,'100',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET67',0001,'100',TO_DATE('24/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET68',0001,'110',TO_DATE('26/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET69',0001,'100',TO_DATE('27/06/2021', 'DD/MM/YYYY'),'NO','NORMAL'),
 ('ET70',0001,'100',TO_DATE('25/06/2021', 'DD/MM/YYYY'),'NO','NORMAL');
 
 
 
 -- Insertadndo datos concierto
 
INSERT INTO concierto VALUES
 ('CNT01',0001,TO_DATE('24/06/2021', 'DD/MM/YYYY'),'PRINCIPAL'),
 ('CNT02',0001,TO_DATE('24/06/2021', 'DD/MM/YYYY'),'SECUNDARIO'),
 ('CNT03',0001,TO_DATE('25/06/2021', 'DD/MM/YYYY'),'PRINCIPAL'),
 ('CNT04',0001,TO_DATE('25/06/2021', 'DD/MM/YYYY'),'SECUNDARIO'),
 ('CNT05',0001,TO_DATE('26/06/2021', 'DD/MM/YYYY'),'PRINCIPAL'),
 ('CNT06',0001,TO_DATE('26/06/2021', 'DD/MM/YYYY'),'SECUNDARIO'),
 ('CNT07',0001,TO_DATE('27/06/2021', 'DD/MM/YYYY'),'PRINCIPAL'),
 ('CNT08',0001,TO_DATE('27/06/2021', 'DD/MM/YYYY'),'SECUNDARIO');
 
 
 -- Insertando datos artistas
 
INSERT INTO artistas VALUES
 ('ART01','CNT01','GUNS AND ROSES'),
 ('ART02','CNT03','MADONA'),
 ('ART03','CNT05','DAVID GUETTA'),
 ('ART04','CNT07','EMINEM'),
 ('ART05','CNT02','AC-DC'),
 ('ART06','CNT04','BRUNO MARS'),
 ('ART07','CNT06','CALVIN HARRIS'),
 ('ART08','CNT08','DRAKE');
 
 
 -- Insertando datos compra
 
INSERT INTO compra VALUES
 (01,'ET01','TAQUILLA'),  
 (02,'ET02','INTERNET'),
 (03,'ET03','TAQUILLA'),
 (04,'ET04','TAQUILLA'),
 (05,'ET05','TAQUILLA'),
 (06,'ET06','INTERNET'),
 (07,'ET07','TAQUILLA'),
 (08,'ET08','TAQUILLA'),
 (09,'ET09','TAQUILLA'),
 (10,'ET10','TAQUILLA'),
 (11,'ET11','INTERNET'),
 (12,'ET12','TAQUILLA'),
 (13,'ET13','TAQUILLA'),
 (14,'ET14','TAQUILLA'),
 (15,'ET15','INTERNET'),
 (16,'ET16','INTERNET'),
 (17,'ET17','TAQUILLA'),
 (18,'ET18','INTERNET'),
 (19,'ET19','INTERNET'),
 (20,'ET20','INTERNET'),
 (21,'ET21','TAQUILLA'),
 (22,'ET22','TAQUILLA'),
 (23,'ET23','TAQUILLA'),
 (24,'ET24','INTERNET'),
 (25,'ET25','INTERNET'),
 (26,'ET26','TAQUILLA'),
 (27,'ET27','INTERNET'),
 (28,'ET28','INTERNET'),
 (29,'ET29','INTERNET'),
 (30,'ET30','TAQUILLA'),
 (31,'ET31','INTERNET'),
 (32,'ET32','INTERNET'),
 (33,'ET33','TAQUILLA'),
 (34,'ET34','INTERNET'),
 (35,'ET35','TAQUILLA'),
 (36,'ET36','INTERNET'),
 (37,'ET37','INTERNET'),
 (38,'ET38','TAQUILLA'),
 (39,'ET39','INTERNET'),
 (40,'ET40','TAQUILLA'),
 (41,'ET41','TAQUILLA'),
 (42,'ET42','INTERNET'),
 (43,'ET43','INTERNET'),
 (44,'ET44','INTERNET'),
 (45,'4ET5','TAQUILLA'),
 (46,'ET46','TAQUILLA'),
 (47,'ET47','TAQUILLA'),
 (48,'ET48','TAQUILLA'),
 (49,'ET49','INTERNET'),
 (50,'ET50','TAQUILLA'),
 (51,'ET51','INTERNET'),
 (52,'ET52','INTERNET'),
 (53,'ET53','TAQUILLA'),
 (54,'ET54','INTERNET'),
 (55,'ET55','INTERNET'),
 (56,'ET56','TAQUILLA'),
 (57,'ET57','INTERNET'),
 (58,'ET58','TAQUILLA'),
 (59,'ET59','INTERNET'),
 (60,'ET60','INTERNET'),
 (61,'ET61','TAQUILLA'),
 (62,'ET62','INTERNET'),
 (63,'ET63','TAQUILLA'),
 (64,'ET64','INTERNET'),
 (65,'ET65','TAQUILLA'),
 (66,'ET66','INTERNET'),
 (67,'ET67','TAQUILLA'),
 (68,'ET68','INTERNET'),
 (69,'ET69','TAQUILLA'),
 (70,'ET70','INTERNET');
 
 
 -- Insertando datos contratar
 
INSERT INTO contratar VALUES
 ('ET03','SG01'),
 ('ET06','SG02'),
 ('ET11','SG03'),
 ('ET15','SG04'),
 ('ET18','SG05'),
 ('ET21','SG06'),
 ('ET24','SG07'),
 ('ET25','SG08'),
 ('ET27','SG09'),
 ('ET29','SG10'),
 ('ET47','SG11'),
 ('ET55','SG12'),
 ('ET59','SG13'),
 ('ET65','SG14'),
 ('ET68','SG15');
 
 -- Insertar datos emite
 
INSERT INTO emite VALUES
 ('CNT01','RT01'),
 ('CNT02','RT02'),
 ('CNT03','RT03'),
 ('CNT04','RT04'),    
 ('CNT05','RT05'),
 ('CNT06','RT06'),
 ('CNT07','RT07'),
 ('CNT08','RT08');
 
 
 -- Insertar datos categorias_artistas

INSERT INTO categorias_artistas VALUES
  ('ART01','CTG01'),
  ('ART02','CTG02'),
  ('ART03','CTG03'),
  ('ART04','CTG04'),
  ('ART05','CTG01'),
  ('ART06','CTG02'),
  ('ART07','CTG03'),
  ('ART08','CTG04');

```
- [x] 5. Consultas de la base de datos
   - [x] 5.1. Consultas más frecuentes


-- cliente que contrataron seguro y el precio de este

```sql
select concat(c.nombre,' ',c.apellidos) as "nombre",
ct.entrada_id,
s.precio,
e.precio
from cliente c 
join compra cm on (c.id = cm.cliente_id)
join entradas e on (cm.entrada_id = e.id)
join contratar ct on (e.id = ct.entrada_id)
join seguro s on (ct.seguro_id = s.id)
where ct.entrada_id = e.id;
```

```
             nombre             | entrada_id | precio | precio
--------------------------------+------------+--------+--------
 JOSE GIMENO MORA               | ET03       |  10.00 | 110.00
 NATALIA JALDO SUAREZ           | ET06       |  10.00 | 110.00
 RAFAEL RODRIGUEZ PARRA         | ET11       |  10.00 | 110.00
 BELEN MATIAS FERNANDEZ         | ET15       |  10.00 | 110.00
 MARTA JIMENEZ GIL              | ET18       |  10.00 | 110.00
 JORGE SANCHEZ RAMON            | ET21       |  10.00 | 110.00
 JUAN RUIZ HERNANDEZ            | ET24       |  10.00 | 110.00
 ALVARO VILCHEZ CARMONA         | ET25       |  10.00 | 110.00
 FRANCISCO ARCOS CARMONA        | ET27       |  10.00 | 110.00
 EDUARDO CASAS PÁEZ             | ET29       |  10.00 | 110.00
 ROSARIO INMACULADA HORTA OCHOA | ET47       |  10.00 | 110.00
 VERONICA CASTELLANOS ROJAS     | ET55       |  10.00 | 110.00
 MARIA VEGA ZAMBRANO            | ET59       |  10.00 | 110.00
 RAQUEL GOMEZ GIANINE           | ET65       |  10.00 | 110.00
 GABRIEL FORERO PEÑA            | ET68       |  10.00 | 110.00
(15 rows)


```

-- nombre y apellidos de los asistentes

```sql
select concat(nombre,' ',apellidos) as "nombre y apellidos"
from cliente;
```

```

 ROSA MARIA ACIEN ZURUTA
 DANIEL ALBUSAC TAMARGO
 JOSE GIMENO MORA
 SUSANA IGLESIAS PASTOR
 IRENE RODRIGUEZ GARCIA
 NATALIA JALDO SUAREZ
 MONICA MUÑOZ BENAVIDES
 SUSANA LOPEZ CASADO
 ISABEL NAVARRO MARTIN
 MATIAS RODRIGUEZ MARTINEZ
 RAFAEL RODRIGUEZ PARRA
 SANDRA HUERTAS PEREZ
 HUGO LOPEZ ROBLES
 JESSICA CORTES AMATE
 BELEN MATIAS FERNANDEZ


```
-- Grupos que tocaron

```sql

select nombre as "nombre de artitas"
from artistas;
```

```
 nombre de artitas
-------------------
 GUNS AND ROSES
 MADONA
 DAVID GUETTA
 EMINEM
 AC-DC
 BRUNO MARS
 CALVIN HARRIS
 DRAKE
(8 rows)

```
-- Categorias de musica

```sql

select genero as "genero de música"
from categorias;
```

```

 genero de música
------------------
 ROCK
 POP
 ELECTRONICA
 RAP
(4 rows)

```

   - [x] 5.2. Consultas sencillas

-- Mostrar lista de tablas del festival 

```sql
/d
```
```

quevedofest=# \d
                 List of relations
 Schema |        Name         | Type  |   Owner
--------+---------------------+-------+------------
 public | artistas            | table | dbo_daw1_a
 public | categorias          | table | dbo_daw1_a
 public | categorias_artistas | table | dbo_daw1_a
 public | cliente             | table | dbo_daw1_a
 public | compra              | table | dbo_daw1_a
 public | concierto           | table | dbo_daw1_a
 public | contratar           | table | dbo_daw1_a
 public | emite               | table | dbo_daw1_a
 public | entradas            | table | dbo_daw1_a
 public | festival            | table | dbo_daw1_a
 public | retransmision       | table | dbo_daw1_a
 public | seguro              | table | dbo_daw1_a
(12 rows)

```
--Clientes en orden de fecha de nacimiento ascendente

```sql
select concat(nombre,' ',apellidos) as "nombre y apellidos",
fecha_nacimiento
from cliente
order by fecha_nacimiento ASC;

```

```

       nombre y apellidos       | fecha_nacimiento
--------------------------------+------------------
 JUAN RUIZ HERNANDEZ            | 1990-07-01
 JESSICA CORTES AMATE           | 1990-08-08
 JOSE GIMENO MORA               | 1990-12-14
 HUGO LOPEZ ROBLES              | 1991-03-05
 ROSARIO INMACULADA HORTA OCHOA | 1991-09-15
 SUSANA IGLESIAS PASTOR         | 1991-10-03

 ISABEL IREGUI GALEANO          | 2002-12-09
 JORGE SANCHEZ RAMON            | 2003-04-23
 MANUELA DUEÑAS ROJAS           | 2003-10-04
 DOMINGO ZÚÑIGA RAMÍREZ         | 2004-11-16
 MONICA LOPEZ OJEDA             | 2005-02-20
 DAVID MARQUEZ GAMARRA          | 2006-03-21
 RAFAEL OVALLE SOLANO           | 2007-01-13
(70 rows)

```
--Ver la edad en años del cliente mas joven

```sql

select concat(nombre,' ',apellidos) as "nombre",
 date_part('year',age(fecha_nacimiento)) as "años"
from cliente
group by fecha_nacimiento,nombre,apellidos
order by "años" asc limit 1;

```
```
        nombre        | años
----------------------+------
 RAFAEL OVALLE SOLANO |   14
(1 row)

```
-- Edad del cliente más mayor

```sql
select concat(nombre,' ',apellidos) as "nombre",
 date_part('year',age(fecha_nacimiento)) as "años"
from cliente
group by fecha_nacimiento,nombre,apellidos
order by "años" desc limit 1;
```

```
       nombre        | años
---------------------+------
 JUAN RUIZ HERNANDEZ |   30
(1 row)

```
-- Entradas de cada tipo

```sql

select tipo as "tipo entradas",
count(tipo) as "número entradas"
from entradas
group by tipo;
```

```
 tipo entradas | número entradas
---------------+-----------------
 VIP           |              10
 NORMAL        |              60
(2 rows)

```
-- Precio por tipo de entrada

```sql

select tipo, precio
from entradas
group by tipo, precio;
```

```
  tipo  | precio
--------+--------
 VIP    | 200.00
 NORMAL | 100.00
 NORMAL | 110.00
 NORMAL |  90.00
(4 rows)

```
-- Entradas con descuento

```sql

select id,precio
from entradas
where descuento = 'SI'
group by id,precio;
```

```
  id  | precio
------+--------
 ET17 |  90.00
 ET57 |  90.00
 ET63 |  90.00
 ET02 |  90.00
 ET26 |  90.00
 ET14 |  90.00
 ET39 |  90.00
 ET08 |  90.00
 ET50 |  90.00
 ET19 |  90.00
(10 rows)

```

   - [x] 5.3. Consultas de agregación y resumen

-- Cantidad total que se pagaron por los seguros

```sql
select sum(precio) as "total descontado"
from seguro;
```
```
 total descontado
------------------
           150.00
(1 row)

```
-- Candidad de entradas que se vendio por dia segun el artista

```sql
select a.nombre , 
count(e.fecha) as "numero de entrada",
c.fecha
from artistas a 
join concierto c on (a.concierto_id = c.id)
join festival f on (c.festival_id = f.id)
join entradas e on (f.id = e.festival_id)
where c.fecha = e.fecha
group by e.fecha, a.nombre,c.fecha;

```
```
     nombre     | numero de entrada |   fecha
----------------+-------------------+------------
 AC-DC          |                20 | 2021-06-24
 GUNS AND ROSES |                20 | 2021-06-24
 BRUNO MARS     |                13 | 2021-06-25
 MADONA         |                13 | 2021-06-25
 CALVIN HARRIS  |                25 | 2021-06-26
 DAVID GUETTA   |                25 | 2021-06-26
 DRAKE          |                12 | 2021-06-27
 EMINEM         |                12 | 2021-06-27
(8 rows)


```
-- Artista con menos visualizaciones

```sql
select nombre, visualizaciones
from artistas a 
join concierto c on (a.concierto_id = c.id)
join emite e on (c.id = e.concierto_id)
join retransmision r on (e.retransmision_id = r.id)
order by visualizaciones asc limit 1;
```

```

 nombre | visualizaciones
--------+-----------------
 MADONA |              50
(1 row)

```

-- Artista con más visualizaciones

```sql
select nombre, visualizaciones
from artistas a 
join concierto c on (a.concierto_id = c.id)
join emite e on (c.id = e.concierto_id)
join retransmision r on (e.retransmision_id = r.id)
order by visualizaciones desc limit 1;
```

```
 nombre | visualizaciones
--------+-----------------
 DRAKE  |             800
(1 row)

```
-- Dia con menos personas

```sql
select fecha,
count(*) as "personas"
from entradas
group by fecha
order by "personas" asc limit 1;
```

```
   fecha    | personas
------------+----------
 2021-06-27 |       12
(1 row)

```
-- Día con más personas

```sql
select fecha,
count(*) as "personas"
from entradas
group by fecha
order by "personas" desc limit 1;

```
```
   fecha    | personas
------------+----------
 2021-06-26 |       25
(1 row)

```
-- Número de entradas con descuento

```sql
select count(descuento) as "num entradas con descuento"
from entradas
where descuento = 'SI';
```

```
 num entradas con descuento
----------------------------
                         10
(1 row)

```
-- Recaudacion total de entradas

```sql

select SUM(precio) as "ganancia total venta entradas"
from entradas;
```


```
 ganancia total venta entradas
-------------------------------
                       8050.00
(1 row)

```

-- Entradas de cada tipo

```sql

select tipo as "tipo entradas",
count(tipo) as "número entradas"
from entradas
group by tipo;
```

```
 tipo entradas | número entradas
---------------+-----------------
 VIP           |              10
 NORMAL        |              60
(2 rows)

```

   - [x] 5.4. Consultas con subconsultas

--Grupos de musica y su género

```sql

select nombre ,
 genero 
 from artistas a 
 join categorias_artistas ca on (a.id = ca.artistas_id)
 join categorias c on (ca.categorias_id = c.id);
 ```

```
     nombre     |   genero
----------------+-------------
 GUNS AND ROSES | ROCK
 MADONA         | POP
 DAVID GUETTA   | ELECTRONICA
 EMINEM         | RAP
 AC-DC          | ROCK
 BRUNO MARS     | POP
 CALVIN HARRIS  | ELECTRONICA
 DRAKE          | RAP
(8 rows)

```
-- Grupo y  scenario en el que tocaron

```sql
select nombre , escenario
from artistas a 
join concierto c on (a.concierto_id = c.id);
```

```
     nombre     | escenario
----------------+------------
 GUNS AND ROSES | PRINCIPAL
 MADONA         | PRINCIPAL
 DAVID GUETTA   | PRINCIPAL
 EMINEM         | PRINCIPAL
 AC-DC          | SECUNDARIO
 BRUNO MARS     | SECUNDARIO
 CALVIN HARRIS  | SECUNDARIO
 DRAKE          | SECUNDARIO
(8 rows)

```
-- Como se retransmitio cada artista

```sql
select nombre , medio
from artistas a 
join concierto c on (a.concierto_id = c.id)
join emite e on (c.id = e.concierto_id)
join retransmision r on (e.retransmision_id = r.id);
```

```

     nombre     |   medio
----------------+-----------
 GUNS AND ROSES | TV
 MADONA         | RADIO
 DAVID GUETTA   | TV
 EMINEM         | RADIO
 AC-DC          | STREAMING
 BRUNO MARS     | STREAMING
 CALVIN HARRIS  | STREAMING
 DRAKE          | TV
(8 rows)

```
-- Artista con más visualizaciones

```sql
select nombre, visualizaciones
from artistas a 
join concierto c on (a.concierto_id = c.id)
join emite e on (c.id = e.concierto_id)
join retransmision r on (e.retransmision_id = r.id)
order by visualizaciones desc limit 1;
```

```
 nombre | visualizaciones
--------+-----------------
 DRAKE  |             800
(1 row)

```

-- Artista con menos visualizaciones

```sql
select nombre, visualizaciones
from artistas a 
join concierto c on (a.concierto_id = c.id)
join emite e on (c.id = e.concierto_id)
join retransmision r on (e.retransmision_id = r.id)
order by visualizaciones asc limit 1;
```

```

 nombre | visualizaciones
--------+-----------------
 MADONA |              50
(1 row)
```

-- Fecha en la que toca cada artista

```sql
select nombre, fecha , escenario
from artistas a 
join concierto c on (a.concierto_id = c.id);
```

```
     nombre     |   fecha    | escenario
----------------+------------+------------
 GUNS AND ROSES | 2021-06-24 | PRINCIPAL
 MADONA         | 2021-06-25 | PRINCIPAL
 DAVID GUETTA   | 2021-06-26 | PRINCIPAL
 EMINEM         | 2021-06-27 | PRINCIPAL
 AC-DC          | 2021-06-24 | SECUNDARIO
 BRUNO MARS     | 2021-06-25 | SECUNDARIO
 CALVIN HARRIS  | 2021-06-26 | SECUNDARIO
 DRAKE          | 2021-06-27 | SECUNDARIO
(8 rows)

```
-- Candidad de entrdas que se vendio por dia segun el artista

```sql
select a.nombre , 
count(e.fecha) as "numero de entrada",
c.fecha
from artistas a 
join concierto c on (a.concierto_id = c.id)
join festival f on (c.festival_id = f.id)
join entradas e on (f.id = e.festival_id)
where c.fecha = e.fecha
group by e.fecha, a.nombre,c.fecha;

```
```
     nombre     | numero de entrada |   fecha
----------------+-------------------+------------
 AC-DC          |                20 | 2021-06-24
 GUNS AND ROSES |                20 | 2021-06-24
 BRUNO MARS     |                13 | 2021-06-25
 MADONA         |                13 | 2021-06-25
 CALVIN HARRIS  |                25 | 2021-06-26
 DAVID GUETTA   |                25 | 2021-06-26
 DRAKE          |                12 | 2021-06-27
 EMINEM         |                12 | 2021-06-27
(8 rows)


```

-- cliente que contrataron seguro y el precio de este

```sql
select concat(c.nombre,' ',c.apellidos) as "nombre",
ct.entrada_id,
s.precio,
e.precio
from cliente c 
join compra cm on (c.id = cm.cliente_id)
join entradas e on (cm.entrada_id = e.id)
join contratar ct on (e.id = ct.entrada_id)
join seguro s on (ct.seguro_id = s.id)
where ct.entrada_id = e.id;
```

```
             nombre             | entrada_id | precio | precio
--------------------------------+------------+--------+--------
 JOSE GIMENO MORA               | ET03       |  10.00 | 110.00
 NATALIA JALDO SUAREZ           | ET06       |  10.00 | 110.00
 RAFAEL RODRIGUEZ PARRA         | ET11       |  10.00 | 110.00
 BELEN MATIAS FERNANDEZ         | ET15       |  10.00 | 110.00
 MARTA JIMENEZ GIL              | ET18       |  10.00 | 110.00
 JORGE SANCHEZ RAMON            | ET21       |  10.00 | 110.00
 JUAN RUIZ HERNANDEZ            | ET24       |  10.00 | 110.00
 ALVARO VILCHEZ CARMONA         | ET25       |  10.00 | 110.00
 FRANCISCO ARCOS CARMONA        | ET27       |  10.00 | 110.00
 EDUARDO CASAS PÁEZ             | ET29       |  10.00 | 110.00
 ROSARIO INMACULADA HORTA OCHOA | ET47       |  10.00 | 110.00
 VERONICA CASTELLANOS ROJAS     | ET55       |  10.00 | 110.00
 MARIA VEGA ZAMBRANO            | ET59       |  10.00 | 110.00
 RAQUEL GOMEZ GIANINE           | ET65       |  10.00 | 110.00
 GABRIEL FORERO PEÑA            | ET68       |  10.00 | 110.00
(15 rows)


```
-- Que aficonados estuvieron en el concierto de los GUNS AND ROSES

```sql
select concat(c.nombre,' ',c.apellidos) as "nombre",
a.nombre as "grupo"
from cliente c 
join compra cm on (cm.cliente_id = c.id)
join entradas e on (cm.entrada_id = e.id)
join festival f on (e.festival_id = f.id)
join concierto ct on (f.id = ct.festival_id)
join artistas a on (ct.id = a.concierto_id)
where ct.fecha= e.fecha and a.nombre = 'GUNS AND ROSES';
```

```
            nombre            |     grupo
------------------------------+----------------
 ROSA MARIA ACIEN ZURUTA      | GUNS AND ROSES
 IRENE RODRIGUEZ GARCIA       | GUNS AND ROSES
 ISABEL NAVARRO MARTIN        | GUNS AND ROSES
 LUIS GARCIA QUESADA          | GUNS AND ROSES
 ALVARO VILCHEZ CARMONA       | GUNS AND ROSES
 ANTONIO BUITRAGO CONTRERAS   | GUNS AND ROSES
 ALEJANDRO URIBE ANTIA        | GUNS AND ROSES
 MARIO DIAZ MEJIA             | GUNS AND ROSES
 JULIO SANCHEZ PRADA          | GUNS AND ROSES
 MARIA BELEN NOVOA GOMEZ      | GUNS AND ROSES
 OLGA DEL RÍO AYERBE          | GUNS AND ROSES
 MANUELA DUEÑAS ROJAS         | GUNS AND ROSES
 LUCIA GARCIA RUEDA           | GUNS AND ROSES
 MARGARITA BUITRAGO CONTRERAS | GUNS AND ROSES
 JORGE CASAS PÁEZ             | GUNS AND ROSES
 ARACELI ULLOA ORJUELA        | GUNS AND ROSES
 LUIS ALVAREZ CASTILLO        | GUNS AND ROSES
 CARMEN REY SANCHEZ           | GUNS AND ROSES
 NADIA ACERO CARO             | GUNS AND ROSES
 LUIS DIAZ BELTRAN            | GUNS AND ROSES
(20 rows)

```

-- Tres primeras letras del apellido de los Clientes qque compraron la entrada por internet

```sql
select c.apellidos,
upper (substring (c.apellidos,1,3)) as iniciales
from cliente c 
join compra cm on (c.id = cm.cliente_id)
where cm.via = 'INTERNET';
```

```
     apellidos      | iniciales
--------------------+-----------
 ALBUSAC TAMARGO    | ALB
 JALDO SUAREZ       | JAL
 RODRIGUEZ PARRA    | ROD
 MATIAS FERNANDEZ   | MAT
 GALERA GARCIA      | GAL
 JIMENEZ GIL        | JIM
 LOPEZ OJEDA        | LOP

 VEGA ZAMBRANO      | VEG
 IREGUI GALEANO     | IRE
 ACERO CARO         | ACE
 FERNÁNDEZ MARTÍNEZ | FER
 SUARIQUE ÁVILA     | SUA
 FORERO PEÑA        | FOR
 NIETO BUSTOS       | NIE
(35 rows)

```

-- Mostar los apellidos en orden inverso de clientes que compraro entrada en taquilla

```sql
select apellidos,
split_part(c.apellidos,' ',2)||' '||split_part(c.apellidos,' ',1) as " apellidos invertidos"
from cliente c 
join compra cm on (c.id = cm.cliente_id)
where cm.via = 'TAQUILLA';
```

```
     apellidos      |  apellidos invertidos
--------------------+-----------------------
 ACIEN ZURUTA       | ZURUTA ACIEN
 GIMENO MORA        | MORA GIMENO
 IGLESIAS PASTOR    | PASTOR IGLESIAS
 RODRIGUEZ GARCIA   | GARCIA RODRIGUEZ
 MUÑOZ BENAVIDES    | BENAVIDES MUÑOZ
 LOPEZ CASADO       | CASADO LOPEZ
 NAVARRO MARTIN     | MARTIN NAVARRO
 RODRIGUEZ MARTINEZ | MARTINEZ RODRIGUEZ
 HUERTAS PEREZ      | PEREZ HUERTAS
 LOPEZ ROBLES       | ROBLES LOPEZ
 CORTES AMATE       | AMATE CORTES


```


- [x] 6. Vistas, secuencias e índices

-- Precio entrada  más caras 

```sql

create view masCara as
select tipo, precio
from entradas 
group by tipo, precio
order by precio desc limit 1;

select * from masCara;
```

```
 tipo | precio
------+--------
 VIP  | 200.00
(1 row)


```

-- Clientes de Madrid

```sql
create view clientesMadrid as 
select nombre,apellidos ,ciudad
from cliente 
where ciudad= 'MADRID';

select * from  clientesMadrid;
```

```
       nombre       |     apellidos      | ciudad
--------------------+--------------------+--------
 ROSA MARIA         | ACIEN ZURUTA       | MADRID
 DANIEL             | ALBUSAC TAMARGO    | MADRID
 SUSANA             | IGLESIAS PASTOR    | MADRID
 NATALIA            | JALDO SUAREZ       | MADRID
 MATIAS             | RODRIGUEZ MARTINEZ | MADRID
 SANDRA             | HUERTAS PEREZ      | MADRID
 BELEN              | MATIAS FERNANDEZ   | MADRID

 CARMEN             | REY SANCHEZ        | MADRID
 NADIA              | ACERO CARO         | MADRID
 NICOLAS            | NIETO BUSTOS       | MADRID
(26 rows)
```

-- Ciudades diferentes


```sql
create view ciudadesClientes as 
select distinct ciudad 
from cliente;


select * from ciudadesClientes;

```

```
  ciudad
-----------
 BARCELONA
 MADRID
 OVIEDO
 SANTANDER
 SEVILLA
 VALENCIA
(6 rows)
```
-- Grupos y generos

```sql
create view grupoGenero as
select nombre , genero 
 from artistas a 
 join categorias_artistas ca on (a.id = ca.artistas_id)
 join categorias c on (ca.categorias_id = c.id);


select * from grupoGenero;
```

```
     nombre     |   genero
----------------+-------------
 GUNS AND ROSES | ROCK
 MADONA         | POP
 DAVID GUETTA   | ELECTRONICA
 EMINEM         | RAP
 AC-DC          | ROCK
 BRUNO MARS     | POP
 CALVIN HARRIS  | ELECTRONICA
 DRAKE          | RAP
(8 rows)

```


- [x] 7. Scripts en PL/pgSQL


```sql
do
$$
<<primer1>>
declare
 v_precio numeric(6)=0;
begin
 select count(*)
 into v_precio
 from entradas;
 raise notice 'El número de entradas vendidas es: %', v_precio;
end primer1;
$$

```

```
El número de entradas VIP es: 70
```



```sql

do
$$
<<primer1>>
declare
 v_personas numeric(6)=0;
begin
 select count(*)
 into v_personas
 from entradas
 where fecha= '2021-06-26';
 raise notice 'El 26/05/2021 acudieron: % personas', v_personas;
end primer1;
$$
```

```
El 26/05/2021 acudieron: 25 personas
```

```sql
do
$$
<<primer1>>
declare
 v_precio numeric(6)=0;
begin
 select count(*)
 into v_precio
 from entradas
 where descuento ='SI';
 raise notice 'num entradas con descuento: %', v_precio;
end primer1;
$$
```


```
num entradas con descuento: 10
```




- [ ] 8. Extras
   - [ ] 8.1. Cursores
   - [ ] 8.2. Prototipo de interfaz de usuario
   - [ ] 8.3. Plan de pruebas
   - [x] 8.4. Especificaciones de pruebas en [formato features Gherkin (ver ejemplo)](features/admin-carteles.feature) 
   - [ ] 8.5. Diagrama de clases
   - [ ] 8.6. Ejemplo de acceso a la base de datos con Java y JDBC
