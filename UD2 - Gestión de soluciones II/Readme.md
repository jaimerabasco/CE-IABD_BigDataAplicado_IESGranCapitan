# UD 2 - Gestión de Soluciones II

En este capítulo se profundizará en los almacenes de datos o Data Warehouses (DW) como solución propia de la inteligencia de negocio o Business Intelligence (BI). Esta solución aparece como respuesta al crecimiento de datos digitales almacenados por las empresas y la necesidad de extraer información y generar conocimiento a partir de ellos para optimizar el proceso de toma de decisiones en cada uno de los departamentos o divisiones de la empresa. Para ello, en este capítulo se aborda el concepto de almacén de datos, su arquitectura así como su diseño e implementación.

## 2.1. Sistemas de ayuda a la decisión

En una empresa u organización, los datos generados a diario son, principalmente, aquellos derivados de las operaciones rutinarias de la empresa. Estos datos, tradicionalmente, se almacenaban en **bases de datos relacionales** y su manipulación se correspondía con **transacciones** realizadas sobre la base de datos. Sin embargo, el objetivo de cualquier organización es seleccionar esos datos para realizar estudios y análisis que permitan generar informes que, a su vez, permitan a la empresa **extraer información para tomar decisiones estratégicas** que conduzcan a la organización al éxito.


El crecimiento exponencial de los datos manejados por una organización ha hecho que los computadores sean las únicas herramientas capaces de procesar estos datos para obtener información y ofrecer ayuda en la toma de decisiones. En este contexto, aparecen los **sistemas de ayuda a la decisión** o _Decision Support Systems (DSS)_ que ayudan a quienes ocupan puestos de gestión a tomar decisiones o elegir entre diferentes alternativas. 

>📄 **Sistema de ayuda a la decisión**: Conjunto de técnicas y herramientas tecnológicas desarrolladas para procesar y analizar datos para ofrecer soporte en la toma decisiones a quienes ocupan puestos de gestión o dirección en una organización. Para ello, el sistema combina los recursos de los gestores junto con los recursos computacionales para optimizar el proceso de toma de decisiones.

Mientras que las bases de datos relacionales han sido tradicionalmente el componente del _back-end_ en el diseño de sistemas de ayuda a la decisión, los almacenes de datos se han convertido en una opción mucho más competitiva como elemento _back-end_ al mejorar el rendimiento de éstas.

Los **campos de aplicación** de los almacenes de datos no se reducen únicamente al ámbito empresarial, sino que cubren multitud de dominios como las **ciencias naturales, demografía, epidemiología o educación, entre otros muchos**. La propiedad común a todos estos campos y que hace de los almacenes de datos una adecuada solución en estos ámbitos **es la necesidad de almacenamiento y herramientas de análisis que permitan obtener en tiempos razonables información y conocimiento útiles para mejorar el proceso de toma de decisiones**.

_[ver presentación BDA2.1](https://moodle.iesgrancapitan.org/pluginfile.php/58143/mod_folder/content/0/BDA_UD2_01.pdf?forcedownload=1)_

## 2.2. Almacenes de datos: Concepto

La aparición de los **almacenes de datos** está ligada, principalmente, a una serie de retos que es necesario abordar para **convertir los datos transaccionales** con los que trabaja una base de datos relacional en **información para generar conocimiento y dar soporte al proceso de toma de decisiones**

   - **Accesibilidad**: Desde cualquier dispositivo, a cualquier tipo de usuario y a gran cantidad de información que no puede ser almacenada de forma centralizada. La accesibilidad, en este sentido, debe hacer frente al problema de la **escalabilidad** del sistema y de los datos que este maneja.
  
   - **Integración**: Referente a la gestión de datos heterogéneos, con distintos formatos, y provenientes de distintos ámbitos de la organización. Una correcta integración debe garantizar a su vez la corrección y completitud de los datos integrados.
  
   - **Consultas mejoradas**: Permitiendo incluir operadores avanzados y dar soporte a herramientas y procedimientos que posibiliten obtener el máximo partido de los datos existentes. De este modo, será posible obtener i**nformación precisa para realizar un análisis eficiente**.
  
   - **Representación multidimensional**: Proporciona herramientas para analizar de forma multi-dimensional los datos del sistema, incluyendo datos de **diferentes unidades** de la organización con el objetivo de proporcionar herramientas de **análisis y visualización multi-dimensional** para mejorar el proceso de toma de decisiones.

A continuación, se muestra una **definición de almacén de datos** muy extendida, dada por W. Inmon, quien es conocido por ser el “padre” del concepto de almacén de datos.


>📄 **Almacén de datos (Data Warehouse)**: Colección de datos orientados a temas, integrados, variante en el tiempo y no volátil que da soporte al proceso de toma de decisiones de la dirección.

Para entender correctamente esta definición, es necesario ahondar en las características que incluye la misma.

- **Orientados a temas**: Es decir, no orientado a procesos (transacciones), sino a entidades de mayor nivel de abstracción como “artículo” o “pedido”.

- **Integrados**: Almacenados en un formato uniforme y consistente, lo que implica depurar o limpiar los datos para poder integrarlos.

- **Variante en el tiempo**: Asociados a un instante de tiempo (mes, trimestre, año...)

- **No volátiles**: Se trata de datos persistentes que no cambian una vez se incluyen en el almacén de datos.

El diseño y funcionamiento de los almacenes de datos se basa en el sistema de procesamiento analítico en-línea, **OLAP**. Este sistema se encarga del análisis, interpretación y toma de decisiones acerca del negocio, en contraposición a los sistemas de procesamiento de transacciones en línea, **OLTP**.

Así pues, los sistemas **OLTP están dirigidos por la tecnología y orientados a automatizar las operaciones del día a día** de la organización, mientras que los sistemas **OLAP están dirigidos por el negocio y proporcionan herramientas para tomar decisiones a largo plazo**, mejorando la estrategia y la competitividad de la organización. La tabla 2.1 muestra una comparativa entre las principales características de las bases de datos operacionales (OLTP) y los almacenes de datos (OLAP).


| Característica | **BBDD Operacionales(OLTP)** | **Almacén Datos(OLAP)** |
| -- | -- | -- |
| **Objetivo** | Depende de la aplicación | Toma de decisiones |
| **Usuarios** | Miles | Cientos |
| **Trabajo con...** | Transacciones predefinidas | Consultas y análisis específicos |
| **Acceso** | Lectura y escritura a cientos de registros | Principalmente lecutra. Miles de registros |
| **Datos** | Detallados, numéricos y alfanuméricos | Agregados, principalmente numéricos |
| **Integración** | En función de la aplicación | Basados en temas, con mayor nivel de abstracción |
| **Calidad** | Medida en términos de integridad | Medida en términos de consistencia |
| **Temporalidad Datos** | Solo datos actuales | Datos actuales e históricos |
| **Actualizaciones** | Continuas | Periódicas |
| **Modelo** | Normalizado | Desnormalizado, multidimensional |
| **Optimización** | Para acceso OLTP a parte de la BBDD | Para acceso OLAP a gran parte de la BBDD |

_Tabla 2.1. Diferencias entre BBDD Operacionales y Almacenes de Datos_


## 2.3 Almacenes de datos: Arquitectura

Las arquitecturas disponibles para el **diseño de almacenes de datos** se basan, principalmente, en garantizar que el sistema cumpla una serie de propiedades esenciales para su óptimo funcionamiento

- **Separación**: De los datos transaccionales y los datos estratégicos que sirven como punto de partida a la toma de de decisiones.

- **Escalabilidad**: A nivel hardware y software, para actualizarse y garantizar el correcto funcionamiento del sistema a medida que el número de datos y usuarios aumenta.

- **Extensiones**: Permitiendo integrar e incluir nuevas aplicaciones sin necesidad de rediseñar el sistema completo.

- **Seguridad**: Monitorizando el acceso a los datos estratégicos guardados en el almacén de datos.

>📄 Las arquitecturas de **almacenes de datos** se clasifican, fundamentalmente, en dos tipos: arquitecturas orientadas a la estructura y arquitecturas orientadas a la empresa.

### 2.3.1 Arquitecturas orientadas a la estructura

Las arquitecturas orientadas a la estructura reciben su nombre debido a que están diseñadas poniendo especial énfasis en el **número de capas y elementos que componen la arquitectura del sistema de almacén de datos**. De acuerdo con este criterio, es posible distinguir las siguientes arquitecturas.

***Arquitectura de una capa***

El objetivo principal de esta arquitectura, poco utilizada en la práctica, es **minimizar la cantidad de datos almacenados eliminando para ello los datos redundantes**. La [figura2.1][figura2.1] muestra un esquema de este tipo de arquitectura. En ella, el almacén de datos creado es virtual, existiendo un middleware que interpreta los datos operacionales y ofrece una vista multidimensional de ellos.

**El principal inconveniente** de esta arquitectura es que su simplicidad hace que el sistema **no cumpla la propiedad de separación**, ya que los procesos de análisis se realizan sobre los datos operacionales.

![figura2.1][figura2.1]

_Figura 2.1. Almacén de datos. Arquitectura de una capa._

***Arquitectura de dos capas***

Fue diseñada con el objetivo de solucionar el problema de la separación que presentaba la arquitectura de una capa. Este esquema consigue **subrayar la separación entre los datos disponibles y el almacén de datos** a través de los siguientes componentes (ver [figura2.2][figura2.2):

- **Capa de origen (fuente)**: Se corresponde con los orígenes y fuentes de los datos heterogéneos que se pretenden incorporar al almacén de datos.

- **Puesta a punto**: Proceso por el cual se utilizan herramientas de Extracción, Transformación y Carga (ETL) para extraer, limpiar, filtrar, validar y cargar datos en el almacén de datos.

- **Capa de almacén de datos**: Almacenamiento centralizado de la información en el almacén de datos, el cual puede ser utilizado para crear _data marts_ o repositorios de metadatos.

- **Análisis**: Conjunto de procesos a partir de los cuales los datos son eficientemente y flexiblemente analizados, generando informes y simulando escenarios hipotéticos para dar soporte a la toma de decisiones.

>📋 **Data mart** es un subconjunto o agregación de los datos almacenados en un almacén de datos primario que incluye información relevante sobre un área específica del negocio.


![figura2.2][figura2.2]

_Figura 2.2. Almacén de datos. Arquitectura de dos capas._

***Arquitectura de tres capas***

Este tercer tipo de arquitectura incluye una capa llamada de **datos reconciliados** o almacén de datos operativos. Con esta capa, los datos operativos obtenidos tras la limpieza y depuración son integrados y validados, proporcionando un modelo de datos de referencia para toda la organización.

De este modo, **el almacén de datos no se nutre de los datos de origen directamente, sino de los datos reconciliados generados**, los cuales también son utilizados para realizar de forma más eficiente tareas operativas, como la realización de informes o la alimentación de datos a procesos operativos.

Esta capa de datos reconciliados también puede implementarse de forma virtual en una arquitectura de dos capas, ya que se define como una vista integrada y coherente de los datos de origen. La [figura2.3][figura2.3] muestra de forma gráfica este tipo de arquitectura

![figura2.3][figura2.3]

_Figura 2.3. Almacén de datos. Arquitectura de tres capas._

### 2.3.2. Arquitecturas orientadas a la empresa

Esta clasificación distingue **cinco tipos de arquitecturas** que combinan las capas mencionadas en la primera clasificación para diseñar almacenes de datos.


***1. Arquitectura de data marts independientes***

Arquitectura preliminar en la que **los distintos data marts son diseñados de forma independiente y construidos de forma no integrada**. Suele utilizarse en los inicios de implementación de proyectos de almacenes de datos y reemplazada a medida que el proyecto va creciendo.


***2. Arquitectura en bus***

Similar a la anterior, **asegura la integración lógica de los data marts creados**, ofreciendo una visión amplia de los datos de la empresa y permitiendo realizar análisis rigurosos de los procesos que en ella se llevan a cabo.


***3. Arquitectura hub-and-spoke (centro y radio)***

Esta arquitectura es **muy utilizada en almacenes de datos de tamaños medio y grande**. Su diseño pone especial énfasis en garantizar la escalabilidad del sistema y permitir añadir extensiones al mismo.

Para ello, **los datos se almacenan de forma atómica y normalizada en una capa de datos reconciliados que alimenta a los data marts** construidos que contienen, a su vez, los datos agregados de forma multidimensional. Los usuarios acceden a los data marts, si bien es cierto que también pueden hacer consultas directamente sobre los datos reconciliados.


***4. Arquitectura centralizada***

Se trata de un caso particular de la arquitectura hub-and-spoke. En ella, **la capa de datos reconciliados y los data marts se almacenan en un único repositorio físico**.


***5. Arquitectura federada***

Se trata de un tipo de arquitectura **muy utilizada en entornos dinámicos, cuando se pretende integrar almacenes de datos o data marts existentes con otros para ofrecer un entorno único e integrado de soporte a la toma de decisiones**. De esta forma, cada almacén de datos y cada data mart es integrado virtual o físicamente con lo demás. Para ello, se utilizan una serie de técnicas y herramientas avanzadas como son las ontologías, consultas distribuidas e interoperabilidad de metadatos, entre otras.

### 2.4. Almacenes de datos: Diseño e implementación

En esta sección se profundizará en cada una de las **etapas necesarias para diseñar e implementar un almacén de datos**. Para ello, en primer lugar se describirá el proceso de **Extracción, Transformación y Carga (ETL)**, fundamental para construir y alimentar un almacén de datos. Después, se desarrollarán dos de los diseños más extendidos de almacén de datos: el **diseño en estrella** y el **diseño en copo de nieve**.

_[ver presentación BDA2.2](https://moodle.iesgrancapitan.org/pluginfile.php/58143/mod_folder/content/0/BDA_UD2_02.pdf?forcedownload=1)_

### 2.4.1 El proceso de Extracción, Transformación y Carga (ETL)

El proceso ETL es el encargado de extraer, limpiar e integrar los datos provenientes de las fuentes de datos para alimentar el almacén de datos. Este proceso también es el encargado de alimentar la capa de datos reconciliados en la arquitectura de tres capas. El proceso ETL tiene lugar cuando se puebla el almacén de datos y se lleva a cabo cada vez que el almacén de datos se actualiza. A continuación, se describen detalladamente cada una de las fases de las que consta este proceso


**⬆️ Extracción**

Etapa que consiste en la **lectura de los datos de las distintas fuentes de las que provienen**. Cuando un almacén de datos se rellena por primera vez, se suele utilizar la técnica de **extracción estática**, la cual consiste en extraer una instantánea de los datos operacionales. A partir de entonces, se utiliza la **extracción incremental** para actualizar periódicamente los datos del almacén de datos, recogiendo los cambios aplicados desde la última extracción. Para ello, se utiliza el registro mantenido por el SGBD que, por ejemplo, asocia una **marca de tiempo (timestamp)** a los datos operacionales para registrar cuando fueron modificados y agilizar el proceso de extracción.

En la actualidad, existe una gran cantidad de conjuntos de datos o _data sets_ públicos, conocidos bajo el nombre de Open Data, que abarcan una gran cantidad de dominios y con los que es posible trabajar para construir soluciones big data.  

>📋 **Open Data**: Se trata de datos que han sido generados por una fuente en particular, que abarcan un dominio temático o disciplinar y tienen atributos, dentro de los cuales está la frecuencia de actualización. Además, cuentan con una licencia específica que indica las condiciones de reutilización de los mismos.

**La fuente de los datos es en muchos de los casos el estado** nacional, provincial, municipal u organizaciones comerciales. En otras ocasiones, **la fuente de los datos es fruto del estudio o medición por parte de particulares**. Los **atributos** de los conjuntos de datos deben especificar cómo fueron obtenidos, incluyendo fechas de obtención, actualización y validez, así como el público involucrado, la metodología de recogida o muestreo, etc.

Algunas de las fuentes más utilizadas en la actualidad para la obtención de datos abiertos provienen de **los centros nacionales e internacionales de estadísticas**, como son el Instituto Nacional de Estadística de España (INE) [^1] , eurostat [^2] , la oficina europea de estadísticas, la Organización Mundial de la Salud (OMS) [^3] ... Por otra parte, existen **repositorios públicos de datos abiertos y a disposición de los usuarios como UCI4 [^4] o Kaggle [^5]** , que es una comunidad de científicos de datos donde empresas y organizaciones suben sus datos y plantean problemas que son resueltos por los miembros de la comunidad. La figura 2.4 muestra las fuentes de datos que se acaban de mencionar.

![figura2.4][figura2.4]

_Figura 2.4: Fuentes para la obtención de datos abiertos_

[^1]: [https://www.ine.es](https://www.ine.es)
[^2]: [https://ec.europa.eu/eurostat](https://ec.europa.eu/eurostat)
[^3]: [https://www.who.int/es/data/gho/publications/world-health-statistics](https://www.who.int/es/data/gho/publications/world-health-statistics)
[^4]: [https://archive.ics.uci.edu/ml/index.php](https://archive.ics.uci.edu/ml/index.php)
[^5]: [https://www.kaggle.com](https://www.kaggle.com)

_[ver presentación BDA2.3](https://moodle.iesgrancapitan.org/pluginfile.php/58143/mod_folder/content/0/BDA_UD2_03.pdf?forcedownload=1)_

**🔄 Transformación**

La etapa de transformación es la fase clave para **transformar los datos operativos en datos con un formato específico para alimentar un almacén de datos**. En esta etapa, **los datos se limpian y se transforman, añadiéndoles contexto y significado**. En caso de implementar un almacén de datos siguiendo una arquitectura de tres capas, **el proceso de transformación es el encargado de obtener la capa de datos reconciliados**. Si bien es cierto que algunos autores separan la limpieza y la transformación de los datos en dos etapas distintas, en este capítulo se considerarán ambas dentro de la fase de transformación.

> 📄 La etapa de **transformación** engloba todos los procesos de limpieza y manipulación de los datos, con el objetivo de transformar los datos operativos propios de sistemas relacionales (OLTP) en datos preparados para ser incluidos dentro del almacén de datos (OLAP).

La **limpieza de los datos** o _data cleaning_ engloba todos aquellos procedimientos necesarios para detectar y resolver situaciones problemáticas con los datos de partida que pudieran suponer problemas potenciales a la hora de analizarlos. Así pues, los datos de partida pueden ser **incompletos**, es decir, pueden contener atributos sin valor o valores agregados, **incorrectos** que incluyan errores o valores sin ningún significado, lo cual es común cuando los datos se introducen manualmente en el sistema, o inconsistentes cuando los cambios no son propagados a todos los módulos del sistema, los rangos de un determinado atributo son cambiantes, existen datos duplicados...


Otra tarea muy importante que se debe abordar en la fase de limpieza es la gestión de los **datos perdidos**. Se trata de **datos que no están disponibles para algunas de las variables en alguna instancia**. Esto puede ser debido, por ejemplo, a que no se registraran las modificaciones sufridas por los datos, se produjera un mal funcionamiento del equipo, los datos nunca fueron rellenados o no existían en el momento en que fueron completados, se eliminaron por ser inconsistentes con la información registrada...

En general, se distinguen **dos tipos de situaciones cuando existen valores perdidos: datos perdidos completamente aleatorios y datos perdidos de forma no completamente aleatoria**. En el segundo caso, puede ser interesante intentar analizar la razón de la pérdida de los datos, la cual puede ser indicativa. En muchas ocasiones, los valores perdidos tienen relación con un subconjunto de variables predictoras y no se encuentran aleatoriamente distribuidos por todas ellas. Por todo ello, las aproximaciones más comunes a la hora de gestionar datos perdidos son:

- **Eliminación de instancias que contengan valores perdidos**: consiste en establecer un porcentaje umbral de tal forma que, si una instancia contiene un número de valores perdidos que supera este porcentaje, la instancia se descarta. Se trata de una técnica útil cuando el conjunto de datos es grande y el número de instancias con valores perdidos no es muy elevado. No obstante, esta estrategia se debe utilizar con precaución, ya que puede suponer una excesiva pérdida de información si el conjunto de datos no es muy grande, el número de instancias con valores perdidos es alto o el porcentaje umbral es muy pequeño.

- **Asignación de valores fijos**: consiste en asignar un valor fijo a todos los valores perdidos de todas las variables. Este valor puede ser el número 0 o incluso un valor desconocido _Unknown_ o no numérico (NaN, Not a Number) en función del lenguaje de programación utilizado.

- **Asignación de valores de referencia**: asigna un valor de referencia a los valores perdidos para cada variable. Estos valores de referencia suelen ser medidas de centralización como la media o la mediana de los valores de cada variable.

- **Imputación de valores perdidos**: consiste en la aplicación de técnicas más sofisticadas, como pueden ser técnicas estadísticas o de aprendizaje automático para predecir o averiguar los valores que se han perdido.

Finalmente, la etapa de limpieza de datos también se encarga de la **detección de valores anómalos o outliers**. Se trata de valores que se han introducido de forma errónea o bien a una deformación en la distribución de valores. El proceso de detección de anomalías consiste, fundamentalmente, en dos etapas: en primer lugar, **asumir que existe un modelo generador de datos**, como podría ser una distribución de probabilidad. En segundo lugar, considerar que **las anomalías representan un modelo generador distinto**, que no coincide con el original. Existen multitud de técnicas para detectar y descartar o imputar valores anómalos, como lo son técnicas estadísticas basadas en la desviación y el rango intercuartílico o técnicas de aprendizaje automático.

_[ver presentación BDA2.4](https://moodle.iesgrancapitan.org/pluginfile.php/58143/mod_folder/content/0/BDA_UD2_04.pdf?forcedownload=1)_

Después de la etapa de limpieza, comienza la etapa de **transformación/manipulación**. En ella, los datos se preparan para su carga en el almacén de datos. Para ello, se transformarán los datos provenientes de sistemas transaccionales OLTP en datos preparados para su análisis OLAP. A continuación, se muestran algunos de los procesos de transformación más comunes:

- **Estandarización de códigos y formatos de representación**: incluye tareas como transformar la información EBCDIC a formato ASCII o Unicode, convertir números cardinales en ordinales o viceversa, homogeneización y tratamiento de fechas, expandir codificaciones añadiendo textos descriptivos, unificación de códigos (sexo, estado civil, etc) y estándares (medidas, monedas, etc). 

- **Conversiones y combinaciones de campos**: realización de agregaciones y cálculos simples (importe neto, beneficio, realización de cuentas y promedios), cálculos derivados con valores numéricos y textuales... 

- **Correcciones**: en caso de que no se pudiera hacer en el origen o en la etapa de limpieza, se deben corregir errores tipográficos, datos sin sentido ni significado, resolver conflictos de dominio de variables, aclaración de datos ambiguos y asignación de valores a datos nulos.

- **Integración de varias fuentes**: implica la resolución de claves y utilización de diccionarios de datos y repositorios de metadatos para el diseño del almacén de datos.

- **Eliminación de datos y/o registros duplicados**: frecuente cuando se trabaja con distintas fuentes de datos provenientes de diferentes dimensiones o departamentos de la organización. Para ello, es muy útil el uso de funciones de búsqueda y agrupamiento aproximado.

- **Escalado y centrado**: para reducir los efectos adversos de contar con datos dispersos, es muy útil utilizar técnicas para escalar y centrar los datos. Una posible transformación consiste en restar a cada valor de cada instancia el valor medio de la variable correspondiente y después dividir los valores por la desviación estándar. De esta forma, los valores se escalan y se centran entorno al cero. Esta transformación también puede hacerse entorno al valor de la media o la mediana. También es posible escalar los valores en el intervalo [0−1] o aplicar el logaritmo, para reducir la dispersión de los datos y mejorar la visualización.

- **Discretización**: la aplicación de muchas técnicas de aprendizaje automático requiere que los valores de las variables sean discreto, en lugar de continuos. La discretización es el proceso por el cual se divide un intervalo de valores continuos en tantos fragmentos como etiquetas o valores discretos se quieran asignar, sustituyendo los valores originales por la etiqueta o valor discreto correspondiente.

- **Selección de características**: permite reducir el coste computacional de trabajar con todas las variables del conjunto de datos cuando, en muchas ocasiones, existen variables que no aportan información para el aprendizaje. Además de reducir el coste computacional, se reduce el número de dimensiones del conjunto de datos, mejorando la visualización y mejorando el rendimiento de los modelos no solo en términos de tiempo, sino también de rendimiento, puesto que existen métodos de aprendizaje cuyo rendimiento es peor cuando se ejecutan utilizando variables no significativas.

_[ver presentación BDA2.5](https://moodle.iesgrancapitan.org/pluginfile.php/58143/mod_folder/content/0/BDA_UD2_05.pdf?forcedownload=1)_

**🔀 Carga**

Se trata de la última fase de cara a incluir datos en el almacén de datos. **La carga inicial de los datos puede requerir bastante tiempo al cargar de forma progresiva todos los datos históricos**, por lo que es normal realizarla en horas de baja carga de los sistemas. Una vez que el almacén de datos ha sido inicialmente cargado, las sucesivas cargas de datos se pueden realizar de dos formas:

- **Refresco**: El almacén de datos se reescribe completamente, de forma que se reemplazan los datos antiguos. El refresco **es utilizado habitualmente junto con la extracción estática** y suele ser una estrategia muy utilizada para **la carga inicial** del almacén de datos, aunque puede también realizarse a posteriori.

- **Actualización**: **Se añaden al almacén de datos solamente aquellos datos nuevos que se pretenden incluir, sin eliminar ni modificar los datos ya existentes**. Esta técnica es **utilizada frecuentemente junto con la extracción incremental** para actualizar regularmente el almacén de datos. Se trata de una estrategia de carga **compleja de diseñar**, ya que requiere que sea eficiente computacionalmente. Para ello, se requieren mecanismos de detección del cambio como aquellos basados en la marca de tiempo (timestamp) o la utilización de tablas de log, entre otros.

_[ver presentación BDA2.6](https://moodle.iesgrancapitan.org/pluginfile.php/58143/mod_folder/content/0/BDA_UD2_06.pdf?forcedownload=1)_
 
**⬆️🔄🔀 Frameworks ETL**

Recientemente, han aparecido múltiples _frameworks_ que ofrecen herramientas tecnológicas para dar un soporte integrado al proceso ETL. A continuación, se describen algunos de los más utilizados:

- **Bubbles [http://bubbles.databrewery.org](http://bubbles.databrewery.org)**: Se trata de un framework ETL escrito en Python, aunque es posible utilizarlo desde otros lenguajes. Ofrece un marco de trabajo sencillo, donde el proceso ETL se describe como una secuencia de nodos conectados que representan los datos y las distintas operaciones que se pueden realizar con ellos. De esta forma, es posible construir un grafo que implemente el proceso ETL completo para un conjunto de datos.

- **Apache Camel [http://bubbles.databrewery.org](https://camel.apache.org)**: Este framework escrito en lenguaje Java y de acceso abierto se enfoca en facilitar la integración entre distintas fuentes de datos, haciendo el proceso más accesible a los desarrolladores, ofreciendo disitntas herramientas para dar soporte al proceso ETL.

- **Keetle [https://help.hitachivantara.com/Documentation/Pentaho/9.3/Products/Pentaho_Data_Integration](https://help.hitachivantara.com/Documentation/Pentaho/9.3/Products/Pentaho_Data_Integration)**: Es el framework de Pentaho para dar soporte al proceso ETL. Pentaho es una suite o conjunto de programas para construir soluciones de inteligencia de negocio y Keetle es su entorno de trabajo ETL. Similar a Bubbles, ofrece un entorno de trabajo sencillo para implementar un proceso ETL a través de nodos y conexiones entre ellos. Además, Keetle es de acceso abierto.

### 2.4.2 Diseño en Estrella

A la hora de diseñar un almacén de datos, existen dos alternativas ampliamente utilizadas: el **diseño en estrella**, que promueve el diseño directo de estructuras lógicas sobre el modelo relacional, y el **diseño en copo de nieve**, como variante del diseño en estrella.

El proceso de desarrollo de un almacén de datos siguiendo el diseño en estrella puede estructurarse según las siguientes fases:

1. **Elegir un proceso de negocio a modelar**: cualquier proceso de negocio puede ser modelado como un almacén de datos. No obstante, serán de especial interés aquellos procesos **necesarios para la toma de decisiones y que involucran a diferentes unidades de la organización**.

2. **Escoger la granularidad del proceso de negocio**: Los datos almacenados en el almacén de datos **deben expresarse siempre al mismo nivel de detalle**. Por este motivo es necesario escoger un nivel de granularidad al comienzo del diseño del almacén de datos.

3. **Selección de medidas/hechos**: indican **qué se necesita medir o evaluar enel proceso de negocio** con el fin de dar respuesta a las necesidades de información y toma de decisiones por las cuales se pretende diseñar el almacénde datos. Por ejemplo, en el ámbito logístico las medidas podrían ser las unidades aceptadas odevueltas, mientras que en el ámbito del control de calidad podrían ser lasunidades producidas, defectuosas, costo de la producción.

4. **Elegir las dimensiones que se aplicarán a cada medida o hecho**: las dimensiones especifican **las distintas propiedades de las medidas o hechos que se pretenden almacenar e integrar dentro del almacén de datos**. Por ejemplo, si las medidas seleccionadas están relacionadas con las ventas de una determinada empresa, las dimensiones podrían ser: fecha de la venta, vendedor ,cliente, producto...

A la hora de diseñar el almacén de datos siguiendo el diseño en estrella, se crea una **tabla central, también llamada tabla de hechos, tabla factual o fact table**. Esta tabla es la que posee los datos (medidas o hechos) sobre las diferentes combinaciones de las dimensiones. En algunas ocasiones, un diseño en estrella presenta más de una tabla de hechos. Los hechos introducidos pueden ser de distintos tipos: **completamente aditivos** (total de ventas de un departamento), **no aditivos** (márgen de beneficios expresado como porcentaje) o **semiaditivos** (como datos intermedios que pueden agregarse con datos de otras dimensiones).

Rodeando a la tabla de hechos, se encuentra u**na tabla por cada una de las dimensiones definidas**. La clave primaria de la tabla de hechos se crea combinando las claves primarias de sus dimensiones relacionadas, de forma que está compuesta por las claves ajenas de las tablas de dimensiones que relaciona. De esta forma, la tabla de hechos presenta una relación del tipo “muchos a muchos” entre todas las tablas de dimensiones que relaciona, a la vez que una relación “muchos a uno” con cada tabla de dimensión por separado. Este esquema con forma de estrella da nombre a este tipo de diseño.

**Ejemplo**

Sea un almacén de datos en el que se pretende almacenar **las ventas llevadas a cabo en una organización**. La tabla central de hechos representa los datos de cada venta, mientras que las dimensiones que rodean a la tabla central recogen datos sobre los productos vendidos, la fecha de la venta, el canal de distribución y el lugar de entrega. La [figura2.5][figura2.5] muestra un ejemplo de este esquema de almacén de datos.

![figura2.5][figura2.5]

_Figura 2.5: Ejemplo de almacén de datos diseñado en estrella._

**Ventajas e incovenientes**

Entre las **ventajas del diseño en estrella**, destaca ser un diseño **fácil de modificar que proporciona tiempos rápidos de respuesta**, simplificando la navegación de los medatadatos. Además, este diseño **permite simular vistas de los datos que obtendrán los usuarios finales y facilita la interacción con otras herramientas**.

Sin embargo, el diseño en estrella presenta algunos **inconvenientes** que surgen, por ejemplo, cuando **se combinan dimensiones con distintos niveles de detalle**. Además, cuando se pretende limitar los niveles de una dimensión se debe añadir un campo de nivel o utilizar un modelo de tipo constelación, donde se incluyen diferentes estrellas que almacenan tablas de hechos agregadas, lo cual **añade complejidad al esquema**. 

**Resumen**

📄 El **diseño en estrella** permite crear un almacén de datos con una estructura centralizada. El diseño se compone de una tabla de hechos central, que recoge los valores de las medidas del proceso de negocio que se pretende modelar. Rodeando a la tabla de hechos, se incluyen tantas tablas como dimensiones se hayan especificado en el modelo, las cuales se relacionan entre sí a través de la tabla de hechos.

_[ver presentación BDA2.7](https://moodle.iesgrancapitan.org/pluginfile.php/58143/mod_folder/content/0/BDA_UD2_07.pdf?forcedownload=1)_

### 2.4.3 Diseño en Copo de Nieve

Otro de los diseños más extendidos a la hora de implementar un almacén de datos es el **diseño en copo de nieve**. De hecho, el diseño de copo de nieve es considerado una variante o derivado del diseño en estrella y, por tanto, puede construirse a partir de este siguiendo las mismas etapas que se describieron en la sección anterior. 

Este diseño es muy utilizado cuando se dispone de **tablas de dimensión con una gran cantidad de atributos**. En esta circunstancia, el diseño de copo de nieve permite **normalizar las tablas de dimensión**, ofreciendo un mejor rendimiento cuando las consultas realizadas sobre el almacén de datos requieren del uso de operadores de agregación. De esta forma, la tabla de hechos deja de ser la única tabla que presenta relaciones con las demás, apareciendo nuevas relaciones que se dan entre las tablas normalizadas que componen las tablas de dimensiones. Este esquema, que incluye ramificaciones en cada dimensión que se corresponden con las tablas necesarias para su normalización, guarda un gran parecido con la estructura de un copo de nieve, lo cual da nombre a este diseño.

**La diferencia entre los diseños en estrella y copo de nieve radica fundamentalmente en la modelización de las tablas de dimensiones**. Para obtener un diseño en copo de nieve, basta con partir de un diseño en estrella en el que, tras un proceso de normalización, se obtienen varias tablas de dimensión a partir de una tabla desnormalizada. Dentro del diseño de copo de nieve, es posible distinguir entre **copo de nieve parcial**, en el que solo algunas de las dimensiones son normalizadas y **copo de nieve completo**, en el que todas las dimensiones del esquema son normalizadas.

Siguiendo con el ejemplo anterior, en el que se mostraba un almacén de datos para representar las ventas realizadas por una organización, la [figura2.6][figura2.6] muestra un diseño para un almacén de datos siguiendo un esquema de copo de nieve parcial, en el que las dimensiones geográfica y producto han sido normalizadas. 

![figura2.6][figura2.6]

_Figura 2.6: Ejemplo de almacén de datos diseñado en copo de nieve._

**Ventajas e Inconvenientes**

La normalización de las tablas de dimensiones proporciona al diseño de copo de nieve la **mejora de la eficiencia en la realización de consultas complejas que requieren el uso de operadores de agregación**. Sin embargo, este diseño es conceptualmente más difícil de implementar, ya que **incluye un mayor número de tablas** y requiere de realizar muchos joins entre ellas, lo que **incrementa los tiempos de consulta**.


En ocasiones, se requiere diseñar un almacén de datos que contenga más de una tabla de hechos. Para ello, el **esquema o diseño en constelación** permite crear un almacén de datos de forma similar al diseño en estrella, con la diferencia de que este modelo incluye más de una tabla de hechos. Además, cada tabla de hechos puede vincularse con todas o solo algunas tablas de dimensiones. Este esquema contribuye a la reutilización de las tablas de dimensiones, las cuales se pueden relacionar con más de una tabla de hechos.

**Resumen**

📄 El **diseño en copo de nieve** permite crear un almacén de datos a partir de un diseño en estrella. Para ello, las tablas de dimensiones se normalizan, generando más de una tabla para cada una de las dimensiones, mejorando la eficiencia de consultas complejas que requieren el uso de operadores avanzados.

_[ver presentación BDA2.8](https://moodle.iesgrancapitan.org/pluginfile.php/58143/mod_folder/content/0/BDA_UD2_08.pdf?forcedownload=1)_




[figura2.1]: images/Figura2.1_Almacén%20de%20datos.%20Arquitectura%20de%20una%20capa.png "Figura 2.1: Almacén de datos. Arquitectura de una capa."
[figura2.2]: images/Figura2.2_Almacén%20de%20datos.%20Arquitectura%20de%20dos%20capas.png "Figura 2.1: Almacén de datos. Arquitectura de una capa."
[figura2.3]: images/Figura2.3_Almacén%20de%20datos.%20Arquitectura%20de%20tres%20capas.png "Figura 2.1: Almacén de datos. Arquitectura de una capa."
[figura2.4]: images/Figura2.4_Fuentes%20para%20la%20obtención%20de%20datos%20abiertos.jpg "Fuentes para la obtención de datos abiertos.jpg"
[figura2.5]: images/Figura2.5_Ejemplo%20de%20almacén%20de%20datos%20diseñado%20en%20estrella.jpg "Figura2.5_Ejemplo de almacén de datos diseñado en estrella.jpg"
[figura2.6]: images/Figura2.6_Ejemplo%20de%20almacén%20de%20datos%20diseñado%20en%20copo%20de%20nieve.jpg "Figura2.6_Ejemplo de almacén de datos diseñado en copo de nieve.jpg"





