# 2020-Stack-Overflow-Data
Project for group 19 [Cristóbal Sepúlveda Á, Fabián Jaña, Maximiliano Aguilar S, Vicente Reyes]

## Overview
El objetivo del proyecto consiste en aplicar técnicas de procesamiento masivo de datos para analizar el conjunto de datos seleccionado (Stack Overflow), el cual será descrito
más adelante, este análisis tratará de responder preguntas como:
* ¿Cuántos usuarios activos e inactivos tiene actualmente (año 2020) el sitio Stack Overflow?
* ¿Qué países tienen los usuarios con mejor reputación (Reputation)?
* ¿Cuáles son los usuarios que han escrito mayor cantidad de comentrios comenzando con "+1"?

## Data
Para el proyecto se utilizaron los datos de  [Stack Overflow](https://stackoverflow.com/), un sitio web de preguntas y respuestas para todo tipo
de programadores.
El conjunto de datos que se quería utilizar en un principio era el siguiente: [Kaggle.com - Stack Overflow Data](https://www.kaggle.com/stackoverflow/stackoverflow)
Sin embargo hubo complicaciones con la obtención de los datos mencionados anteriormente, por lo cual, se terminó por utilizar: [archive.org - Stack Exchange Data Dump](https://archive.org/details/stackexchange), los cuales tenían los mismos atributos que los datos encontrados en Kaggle.com. Por esto el análisis se realizó, en particular y debido al gran espacio en disco que utilizaban los datos de archive.org, solo con las tablas de usuarios (Users) y comentarios (Comments).

Para finalizar esta sección se entrega una breve descripción de cada tabla y sus atributos relevantes para el proyecto:

1. Users: Formato XML, alrededor de 12 millones de tuplas y ~4GB de espacio en disco.
- **Reputation:** número que asigna la reputación del usuario (ej: 59111, 15196, 101)
- **CreationDate:** Fecha en que se creó el usuario (ej: 2008-07-31T14:22:31.287)
- **LastAccessDate:** Fecha en que el usuario ingresó por última vez (ej: 2020-05-02T18:23:48.813)
- **Location:** Descripción del lugar en donde reside el usuario (ej: "Ottawa, Canada" , "Raleigh, NC, United States")
2. Comments: Formato XML, alrededor de 75.4 millones de tuplas y ~22GB de espacio en disco.
- **Text:** El texto que fue escrito por algún usuario (ej: "It will help if you give some details of which database you are using as techniques vary.")
- **UserId:** Identificación del usuario que escribió el comentario (ej: 3, 380)

## Methods
Para realizar el análisis sobre los datos se utilizó Pig: una herramienta de programación para producir jobs MapReduce en Hadoop, además para leer el formato
XML se necesitó utilizar "XPath" una funcionalidad para extraer texto de archivos XML.
La razón de elegir Pig para realizar el análisis se basó en que su sintaxis es más simple, con lo cual, fue posible centrarse más en las relaciones entre los datos.
Sin embargo, una de las consecuencias negativas del uso de Pig es que, se pueden llegar a realizar varios MapReduce sin advertirlo, y así obtener consultas
muy ineficientes lo cual tiene un impacto aún mayor con respecto a la escala de los datos utilzados (12m Tuplas y 79m tuplas). Esto, junto con lo acotado de los tiempos
desencadenó en que los objetivos del proyecto no fueran logrados del todo, tal como se verá en la siguiente sección.

## Results
Se organizarán los resultados obtenidos de acuerdo a las preguntas planteadas al inicio:
*  ¿Cuántos usuarios activos e inactivos tiene actualmente (año 2020) el sitio Stack Overflow?
Se realizó una consulta contenida en el archivo "InactiveUsersByCreationYear.pig" que obtenía los usuarios inactivos (considerados como los que no se logearon a su cuenta en lo que va de 2020)  y , para comparar, la cantidad total de usuarios, ambos resultados anteriores clasificados según el año en que los usuarios fueron creados.
Se presenta a continuación una tabla con los resultados obtenidos:

| Año de creación | Usuarios inactivos | Usuarios Registrados | Porcentaje de usuarios inactivos |
|:---------------:|:------------------:|:--------------------:|:--------------------------------:|
|       2008      |        10562       |         21653        |               48.78              |
|       2009      |        47468       |         78051        |               60.82              |
|       2010      |       149591       |        199327        |               75.05              |
|       2011      |       268884       |        358997        |               74.90              |
|       2012      |       536649       |        679458        |               78.98              |
|       2013      |       934226       |        1123534       |               83.15              |
|       2014      |       989484       |        1176302       |               84.12              |
|       2015      |       1050217      |        1254380       |               83.72              |
|       2016      |       1273565      |        1518292       |               83.88              |
|       2017      |       1446729      |        1730099       |               83.62              |
|       2018      |       1325445      |        1648472       |               80.40              |
|       2019      |       1124220      |        1724592       |               65.19              |
|       2020      |          0         |        971998        |                 0                |
|      Total      |      12485155      |        9157040       |               73.34              |

* ¿Qué países tienen los usuarios con mejor reputación (Reputation)?
Para responder a esta pregunta se realizó, nuevamente, una consulta de Pig, la cual está disponible en el archivo "TopUsersPerCountry.pig".
Se divide en dos partes:
1. Primero se agrupa a los usuarios por su atributo "Location" (descrito en la sección "Data") y se realiza un conteo para entregar usuarios clasificados por "Location"
2. Luego se Toma un "Location" específico ("United States") y se retornan todos los usuarios que pertenezcan a esta "Location" ordenados de acuerdo a su reputación en el sitio.
## Conclusion

## Appendix?

