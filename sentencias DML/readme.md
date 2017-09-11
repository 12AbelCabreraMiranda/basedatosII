# **SENTENCIAS BASICAS DML**
## **USO DE FECHAS**

#### **Funciones que obtienen valores de fecha y hora del sistema**
###### Todos los valores de fecha y hora del sistema se derivan del sistema operativo del equipo en el que se ejecuta la instancia de SQL Server.

##### EJEMPLOS:
##### **SYSDATETIME (Transact-SQL)**
SYSDATETIME y SYSUTCDATETIME tienen más precisión de fracciones de segundo que GETDATE y GETUTCDATE. SYSDATETIMEOFFSET incluye el ajuste de zona horaria del sistema. SYSDATETIME, SYSUTCDATETIME y SYSDATETIMEOFFSET pueden asignarse a una variable de cualquier tipo de fecha y hora.
* ###### **A.Obtener la fecha y hora actuales del sistema**
     ###### SELECT SYSDATETIME() 
     ###### ,SYSDATETIMEOFFSET()
     ###### ,SYSUTCDATETIME()
     ###### ,CURRENT_TIMESTAMP
     ###### ,GETDATE()
     ###### ,GETUTCDATE();
###### Que al ejecutar dichas funciones nos retornaria, por ejemplo:
   ###### SYSDATETIME()      2007-04-30 13:10:02.0474381
   ###### SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00
   ###### SYSUTCDATETIME()   2007-04-30 20:10:02.0474381
   ###### CURRENT_TIMESTAMP  2007-04-30 13:10:02.047
   ###### GETDATE()          2007-04-30 13:10:02.047
   ###### GETUTCDATE()       2007-04-30 20:10:02.047

* ###### **B.Obtener la fecha actual del sistema**
     ###### SELECT CONVERT (date, SYSDATETIME())
     ###### ,CONVERT (date, SYSDATETIMEOFFSET())
     ###### ,CONVERT (date, SYSUTCDATETIME())
     ###### ,CONVERT (date, CURRENT_TIMESTAMP)
     ###### ,CONVERT (date, GETDATE())
     ###### ,CONVERT (date, GETUTCDATE());
###### Todo esto nos retornaria una fecha similar: 2007-04-30


#### **SYSDATETIMEOFFSET (Transact-SQL)**
Devuelve un valor datetimeoffset(7) que contiene la fecha y hora del equipo en el que se está ejecutando la instancia de SQL Server. El ajuste de zona horaria está incluido.
SQL Server obtiene los valores de fecha y hora utilizando la API de Windows GetSystemTimeAsFileTime(). La exactitud depende del hardware del equipo y de la versión de Windows en las que la instancia de SQL Server se esté ejecutando. La precisión de esta API se fija en 100 nanosegundos. La precisión se puede determinar mediante la API de Windows GetSystemTimeAdjustment().
* ###### **A.Mostrar los formatos que devuelven las funciones de fecha y hora**
En el ejemplo siguiente se muestran los distintos formatos que devuelven las funciones de fecha y hora.
  ###### SELECT SYSDATETIME() AS SYSDATETIME
  ###### ,SYSDATETIMEOFFSET() AS SYSDATETIMEOFFSET
  ###### ,SYSUTCDATETIME() AS SYSUTCDATETIME
  ###### ,CURRENT_TIMESTAMP AS CURRENT_TIMESTAMP
  ###### ,GETDATE() AS GETDATE
  ###### ,GETUTCDATE() AS GETUTCDATE;
###### El conjunto de resultados es el siguiente.
  ###### SYSDATETIME() 2007-04-30 13:10:02.0474381
  ###### SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00
  ###### SYSUTCDATETIME() 2007-04-30 20:10:02.0474381
  ###### CURRENT_TIMESTAMP 2007-04-30 13:10:02.047
  ###### GETDATE() 2007-04-30 13:10:02.047
  ###### GETUTCDATE() 2007-04-30 20:10:02.047

* ###### **B.Convertir valores de fecha y hora en date**
En el ejemplo siguiente se muestra cómo convertir valores de fecha y hora en date.
   ###### SELECT CONVERT (date, SYSDATETIME())
   ###### ,CONVERT (date, SYSDATETIMEOFFSET())
   ###### ,CONVERT (date, SYSUTCDATETIME())
   ###### ,CONVERT (date, CURRENT_TIMESTAMP)
   ###### ,CONVERT (date, GETDATE())
   ###### ,CONVERT (date, GETUTCDATE());
###### El conjunto de resultados es el siguiente.
  ###### 2007-04-30  
  ###### 2007-04-30  
  ###### 2007-04-30
  ###### 2007-04-30
  ###### 2007-04-30
  ###### 2007-04-30


#### **SYSUTCDATETIME (Transact-SQL)**
Devuelve un valor datetime2 que contiene la fecha y hora del equipo en el que la instancia de SQL Server se está ejecutando. La fecha y hora se devuelven como una hora universal coordinada (UTC). La especificación de precisión de fracción de segundo tiene un intervalo de 1 a 7 dígitos. La precisión predeterminada es 7 dígitos.
SYSDATETIME y SYSUTCDATE tienen más precisión de fracciones de segundo que GETDATE y GETUTCDATE. SYSDATETIMEOFFSET incluye el ajuste de zona horaria del sistema. SYSDATETIME, SYSUTCDATE y SYSDATETIMEOFFSET pueden asignarse a una variable de cualquier tipo de fecha y hora.


#### **TIMEFROMPARTS (Transact-SQL)**
Devuelve un valor time para la hora especificada y con la precisión indicada.
TIMEROMPARTS devuelve un valor de hora totalmente inicializado. Si los argumentos no son válidos, se generará un error. Si alguno de los parámetros es NULL, se devuelve NULL. Sin embargo, si el argumento precision es NULL, se generará un error.
El argumento fractions depende del argumento precision. Por ejemplo, si precision es 7, cada fracción representa 100 nanosegundos; si precision es 3, cada fracción representa un milisegundo. Si el valor de precision es cero, el valor de fractions también debe ser cero; de lo contrario, se produce un error.
Esta función se puede enviar de forma remota a servidores SQL Server 2012 y posteriores. No se puede enviar de forma remota a servidores que tengan una versión inferior a SQL Server 2012.

* ###### **A.Ejemplo simple sin fracciones de segundo**
###### SELECT TIMEFROMPARTS ( 23, 59, 59, 0, 0 ) AS Result;
###### El conjunto de resultados es el siguiente.
###### Result -------------------- 23:59:59.0000000
(1 row(s) affected)
* ###### **B.Ejemplo con fracciones de segundo**
###### En el siguiente ejemplo se muestra el uso de los parámetros fractions y precision:
###### 1.	Cuando fractions tiene el valor 5 y precision tiene un valor de 1, el valor de fractions representa 5/10 de un segundo.
###### 2.	Cuando fractions tiene el valor 50 y precision tiene un valor de 2, el valor de fractions representa 50/100 de un segundo.
###### 3.	Cuando fractions tiene el valor 500 y precision tiene un valor de 3, el valor de fractions representa 500/1000 de un segundo.
######  Transact-SQL
######  SELECT TIMEFROMPARTS ( 14, 23, 44, 5, 1 );
###### SELECT TIMEFROMPARTS ( 14, 23, 44, 50, 2 );
###### SELECT TIMEFROMPARTS ( 14, 23, 44, 500, 3 );
###### GO
###### El conjunto de resultados es el siguiente.
###### ----------------
###### 14:23:44.5
###### (1 row(s) affected)
###### ----------------
###### 14:23:44.50
###### (1 row(s) affected)
###### ----------------
###### 14:23:44.500
###### (1 row(s) affected)


#### **TODATETIMEOFFSET (Transact-SQL)**
Devuelve un valor de tipo datetimeoffset que se traduce a partir de una expresión datetime2.
La expresión no puede ser del tipo text, ntext o image, ya que estos tipos no se pueden convertir implícitamente en varchar o nvarchar.
* ###### **A.Cambiar el ajuste de zona horaria de la fecha y hora actuales**
En el ejemplo siguiente se cambia el ajuste de zona horaria de la fecha y hora actuales a la zona horaria -07:00.
###### DECLARE @todaysDateTime datetime2;
###### SET @todaysDateTime = GETDATE();
###### SELECT TODATETIMEOFFSET (@todaysDateTime, '-07:00');
###### -- RETURNS 2007-08-30 15:51:34.7030000 -07:00
* ###### **B.Cambiar el ajuste de zona horaria en minutos**
En el ejemplo siguiente se cambia la zona horaria actual a -120 minutos.
###### DECLARE @todaysDate datetime2;
###### SET @todaysDate = GETDATE();
###### SELECT TODATETIMEOFFSET (@todaysDate, -120);
###### -- RETURNS 2007-08-30 15:52:37.8770000 -02:00
* ###### **C.Agregar un ajuste de zona horaria de 13 horas**
En el ejemplo siguiente se agrega un ajuste de zona horaria de 13 horas a una fecha y hora.
###### DECLARE @dateTime datetimeoffset(7)= '2007-08-28 18:00:30';
###### SELECT TODATETIMEOFFSET (@dateTime, '+13:00');
###### -- RETURNS 2007-08-28 18:00:30.0000000 +13:00


#### **YEAR (Transact-SQL)**
Devuelve un entero que representa la parte del año de la date especificada.
YEAR devuelve el mismo valor que DATEPART (year, date).
Si date contiene únicamente una parte horaria, el valor devuelto es 1900, el año base.

###### Ejemplos
* La siguiente instrucción devuelve 2007. Este número corresponde al año.
SELECT YEAR('2007-04-30T01:01:01.1234567-07:00');
* La siguiente instrucción devuelve 1900, 1, 1. El argumento para date es el número 0. SQL Server interpreta 0 como 1 de enero de 1900.
SELECT YEAR(0), MONTH(0), DAY(0);


#### **MONTH (Transact-SQL)**
Devuelve un entero que representa el mes de date especificado.
MONTH devuelve el mismo valor que DATEPART (month, date).
Si date contiene solo una parte horaria, el valor devuelto es 1, el mes base.
###### Ejemplos
* La siguiente instrucción devuelve 4. Este número corresponde al mes.
SELECT MONTH('2007-04-30T01:01:01.1234567 -07:00');
* La siguiente instrucción devuelve 1900, 1, 1. El argumento para date es el número 0. SQL Server interpreta 0 como 1 de enero de 1900.
SELECT YEAR(0), MONTH(0), DAY(0);


#### **DAY (Transact-SQL)**
Devuelve un entero que representa el día (día del mes) de la date especificada.
DAY devuelve el mismo valor que DATEPART (day, date).
Si date contiene solo una parte horaria, el valor devuelto es 1, el día base.
###### Ejemplos
* La siguiente instrucción devuelve 30. Este número corresponde al día.
SELECT DAY('2007-04-30T01:01:01.1234567 -07:00');
* La siguiente instrucción devuelve 1900, 1, 1. El argumento para date es el número 0. SQL Server interpreta 0 como 1 de enero de 1900.
SELECT YEAR(0), MONTH(0), DAY(0);


#### **GETDATE (Transact-SQL)**
Devuelve la marca de tiempo del sistema de base de datos actual como un valor datetime sin el ajuste de zona horaria de la base de datos. Este valor se deriva del sistema operativo del equipo donde la instancia de SQL Server se está ejecutando.
SYSDATETIME y SYSUTCDATETIME tienen más precisión de fracciones de segundo que GETDATE y GETUTCDATE. SYSDATETIMEOFFSET incluye el ajuste de zona horaria del sistema. SYSDATETIME, SYSUTCDATETIME y SYSDATETIMEOFFSET pueden asignarse a una variable de cualquier tipo de fecha y hora.
###### Ejemplos
En los ejemplos siguientes se usan las seis funciones del sistema de SQL Server que devuelven la fecha y hora actuales para devolver la fecha, la hora o ambas. Los valores se devuelven en serie; por consiguiente, sus fracciones de segundo podrían ser diferentes.
* ###### **A.Obtener la fecha y hora actuales del sistema**
###### SELECT SYSDATETIME()
###### ,SYSDATETIMEOFFSET()
###### ,SYSUTCDATETIME()
###### ,CURRENT_TIMESTAMP
###### ,GETDATE()
###### ,GETUTCDATE();
###### El conjunto de resultados es el siguiente.
###### SYSDATETIME() 2007-04-30 13:10:02.0474381
###### SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00
###### SYSUTCDATETIME() 2007-04-30 20:10:02.0474381
###### CURRENT_TIMESTAMP 2007-04-30 13:10:02.047
###### GETDATE() 2007-04-30 13:10:02.047
###### GETUTCDATE() 2007-04-30 20:10:02.047

* ###### **B.Obtener la fecha actual del sistema**
###### SELECT CONVERT (date, SYSDATETIME())
###### ,CONVERT (date, SYSDATETIMEOFFSET())
###### ,CONVERT (date, SYSUTCDATETIME())
###### ,CONVERT (date, CURRENT_TIMESTAMP)
###### ,CONVERT (date, GETDATE())
###### ,CONVERT (date, GETUTCDATE());
###### El conjunto de resultados es el siguiente.
###### SYSDATETIME() 2007-05-03
###### SYSDATETIMEOFFSET() 2007-05-03
###### SYSUTCDATETIME() 2007-05-04
###### CURRENT_TIMESTAMP 2007-05-03
###### GETDATE() 2007-05-03
###### GETUTCDATE() 2007-05-04

* ###### **C.Obtener la hora actual del sistema**
###### SELECT CONVERT (time, SYSDATETIME())
###### ,CONVERT (time, SYSDATETIMEOFFSET())
###### ,CONVERT (time, SYSUTCDATETIME())
###### ,CONVERT (time, CURRENT_TIMESTAMP)
###### ,CONVERT (time, GETDATE())
###### ,CONVERT (time, GETUTCDATE());
###### El conjunto de resultados es el siguiente.
###### SYSDATETIME() 13:18:45.3490361
###### SYSDATETIMEOFFSET()13:18:45.3490361
###### SYSUTCDATETIME() 20:18:45.3490361
###### CURRENT_TIMESTAMP 13:18:45.3470000
###### GETDATE() 13:18:45.3470000
###### GETUTCDATE() 20:18:45.3470000


# Uso de Strings
## **Funciones para el manejo de cadenas**
Transact SQL proporciona una variedad bastante amplia de funciones para llevar a cabo tal tarea. Algunas de las operaciones que podemos realizar son concatenación, obtención de subcadenas, pasar a mayúsculas o minúsculas, eliminar espacios en blanco… incluso detectar la similitud entre expresiones.

#### **substring(cadena,inicio,longitud):**
Devuelve una parte de la cadena especificada como primer argumento, empezando desde la posición especificada por el segundo argumento y de tantos caracteres de longitud como indica el tercer argumento. 
_EJEMPLO_
select substring('Buenas tardes',8,6);
retorna "tardes".

#### **str(numero,longitud,cantidaddecimales):** 
Convierte números a caracteres; el primer parámetro indica el valor numérico a convertir, el segundo la longitud del resultado (debe ser mayor o igual a la parte entera del número más el signo si lo tuviese) y el tercero, la cantidad de decimales. El segundo y tercer argumento son opcionales y deben ser positivos. String significa cadena en inglés.

_EJEMPLO_ 
se convierte el valor numérico "123.456" a cadena, especificando 7 de longitud y 3 decimales:

select str(123.456,7,3);

select str(-123.456,7,3);
retorna '-123.46';

#### **stuff(cadena1,inicio,cantidad,cadena2):** 
Inserta la cadena enviada como cuarto argumento, en la posición indicada en el segundo argumento, reemplazando la cantidad de caracteres indicada por el tercer argumento en la cadena que es primer parámetro. Stuff significa rellenar en inglés. 

_EJEMPLO_
select stuff('abcde',3,2,'opqrs');
 
retorna "abopqrse". Es decir, coloca en la posición 2 la cadena "opqrs" y reemplaza 2 caracteres de la primer cadena.
Los argumentos numéricos deben ser positivos y menor o igual a la longitud de la primera cadena, caso contrario, retorna "null".
Si el tercer argumento es mayor que la primera cadena, se elimina hasta el primer carácter.

#### **len(cadena):**
Retorna la longitud de la cadena enviada como argumento. "len" viene de length, que significa longitud en inglés. 

_EJEMPLO_
select len('Hola');
devuelve 4.

#### **char(x):**
Retorna un caracter en código ASCII del entero enviado como argumento. 

_Ejemplo:_
select char(65);
retorna "A".

#### **left(cadena,longitud):** 
Retorna la cantidad (longitud) de caracteres de la cadena comenzando desde la izquierda, primer caracter. 

_Ejemplo:_

select left('buenos dias',8);
retorna "buenos d".

#### **right(cadena,longitud):**
Retorna la cantidad (longitud) de caracteres de la cadena comenzando desde la derecha, último caracter. 

_Ejemplo:_

select right('buenos dias',8);
retorna "nos dias".

#### **lower(cadena):**
Retornan la cadena con todos los caracteres en minúsculas. lower significa reducir en inglés. 

_Ejemplo:_
select lower('HOLA ESTUDIANTE');
retorna "hola estudiante".

#### **upper(cadena):**
retornan la cadena con todos los caracteres en mayúsculas. 

_Ejemplo:_
 select upper('Hola estudiante');
retorna "HOLA ESTUDIANTE".

#### **ltrim(cadena):**
Retorna la cadena con los espacios de la izquierda eliminados. Trim significa recortar. 

_Ejemplo:_

select ltrim('     Hola     ');
retorna "Hola ".

#### **rtrim(cadena):**

Retorna la cadena con los espacios de la derecha eliminados. 

_Ejemplo:_

select rtrim('   Hola   ');
retorna " Hola".

#### **replace(cadena,cadenareemplazo,cadenareemplazar):**
Retorna la cadena con todas las ocurrencias de la subcadena reemplazo por la subcadena a reemplazar. 

_Ejemplo:_
select replace('xxx.sqlserverya.com','x','w');
retorna "www.sqlserverya.com".

#### **reverse(cadena):**
Devuelve la cadena invirtiendo el order de los caracteres. 

_Ejemplo:_
select reverse('Hola');
retorna "aloH".

#### **patindex(patron,cadena):**
Devuelve la posición de comienzo (de la primera ocurrencia) del patrón especificado en la cadena enviada como segundo argumento. Si no la encuentra retorna 0. 

_Ejemplos:_

select patindex('%Luis%', 'Jorge Luis Borges');
retorna 7.

select patindex('%or%', 'Jorge Luis Borges');
retorna 2.

select patindex('%ar%', 'Jorge Luis Borges');
retorna 0.

#### **replicate(cadena,cantidad):**
Repite una cadena la cantidad de veces especificada. 

_Ejemplo:_

select replicate ('Hola',3);
retorna "HolaHolaHola";

#### **space(cantidad):**
Retorna una cadena de espacios de longitud indicada por "cantidad", que debe ser un valor positivo. 

_Ejemplo:_

 select 'Hola'+space(1)+'que tal';
retorna "Hola que tal".




# Estadística Descriptiva
