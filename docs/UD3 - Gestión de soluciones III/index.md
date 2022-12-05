# UD 3 - Gestión de Soluciones III

Este capítulo profundiza en dos soluciones de almacenamiento y gestión de datos propias de las bases de datos NoSQL: **las bases de datos XML y las bases de datos documentales**.

## 3.1 Bases de datos NoSQL

El término **NoSQL** proviene del inglés y está formado por las palabras "No" y "SQL". El significado literal del término no debe confundir al lector ni alejarlo de su correcto significado. NoSQL no engloba solamente un conjunto de bases de datos y sistemas de almacenamiento que no utilizan SQL para su gestión. El significado de NoSQL amplía lo anterior, siendo el acrónimo de "***Not only SQL***". Por tanto, NoSQL es una categoría general de sistemas de bases de datos que, además de SQL, utilizan otra serie de tecnologías para almacenar y gestionar los datos. El término NoSQL fue acuñado en 1998 por Carlo Strozzy y retomado en el año 2009 por Eric Evans, quien aprovechó este término para referirse a esta nueva generación de familias de bases de datos como "**Big Data**".


Las transacciones realizadas en los **sistemas de bases de datos relacionales** garantizan la consistencia y escalabilidad de las operaciones a través de una serie de características especificadas en el **modelo ACID**.

- **Atomicidad (A)**: Garantiza que una transacción se ha realizado completamente o no, evitando que algunas operaciones se ejecuten a medias.

- **Consistencia (C)**: También conocida como **integridad**, asegura que cualquier transacción realizada desde un estado seguro en la base de datos conducirá a esta a otro estado también seguro o válido.

- **Aislamiento (I: Isolation)**: Permite que el resultado en el estado del sistema sea el mismo, independientemente del modo de ejecución de las transacciones (secuencial o concurrente).

- **Durabilidad (D)**: O **persistencia**, garantiza que a pesar de que se produzca un fallo en el sistema, los cambios realizados por una transacción persistirán.

Alternativamente, las bases de datos **NoSQL** siguen el modelo conocido como **BASE**, más flexible que ACID en aquellos sistemas de bases de datos que no se adhieren estrictamente al modelo relacional.

- **Disponibilidad básica (Basic Availability)**: El sistema siempre ofrece una respuesta a una petición de datos, aunque los datos sean inconsistentes, estén en fase de cambio o incluso se produzca un fallo.

- **Soft-state (S)**: La consistencia de este tipo de bases de datos es eventual, por lo que el estado del sistema cambia a lo largo del tiempo, aunque no haya habido entradas de datos.

- **Eventual Consistency (E)**: Cuando se dejan de recibir datos, el sistema se vuelve consistente. Así pues, los datos se propagan a todo el sistema, pero este continúa recibiendo datos sin evaluar la consistencia de los mismos para cada transacción antes de avanzar a la siguiente.

En la actualidad, las aplicaciones web modernas, el auge de la computación ubicua y el Big Data, presentan desafíos muy diferentes a los que presentan los sistemas de información tradicionales, implementados por medio de sistemas de bases de datos relacionales. Algunos **desafíos** son: el procesamiento masivo de datos, la alta frecuencia de lecturas y escrituras, los cambios dinámicos y frecuentes en el esquema de datos, la escalabilidad a costes razonables, gestión de datos temporales...En este contexto, en los últimos años han aparecido multitud de enfoques, modelos y tecnologías para aportar soluciones a los desafíos mencionados anteriormente. En este capítulo, se profundiza en **las bases de datos XML y las bases de datos documentales**.

!!! info "Base de datos NoSQL"

    📄 **Base de datos NoSQL**: Sistema de bases de datos no relacional que utiliza distintas tecnologías para la gestión y almacenamiento distribuido de datos masivos sin un esquema estricto subyacente. Esta familia de base de datos permite incrementar la disponibilidad y escalabilidad del sistema, mejorando su rendimiento.

## 3.2 Bases de datos XML

El lenguaje **XML** (eXtensible Markup Language) es un lenguaje de marcas extensible derivado del lenguaje **SGML** (Standard Generalized Markup Language). Se trata de un lenguaje ideado para la definición y gestión de documentos que permite representar datos estructurados y semi-estructurados. En la actualidad, XML se ha convertido en un formato estándar para la comunicación entre aplicaciones, facilitando su integración. Entre las principales ventajas de XML destacan:

- **Datos autodocumentados**: Gracias a la posibilidad de definir cualquier tipo de etiquetas, no es necesario conocer el esquema para entender el significado de los datos. 

- **Extensión**: Permite adaptar el lenguaje fácilmente a cualquier dominio. Anidamiento: La definición de estructuras anidadas dota a los documentos de gran flexibilidad para transferir y consultar información. 

- **Flexibilidad**: Los documentos XML no tienen un formato estricto ni rígido, permitiendo añadir o ignorar información en ellos. 

**Estructura de un documento XML**

La estructura de un documento XML está compuesta de elementos. Un **elemento** es una porción de documento delimitado por un par de etiquetas _< >_ (apertura) y _< />_ (cierre) entre las cuales se incluye un texto llamado **contexto** del elemento. Un elemento sin contexto puede abreviarse mediante el uso de una única etiquete _<nombre_elemento/>_. La estructura de un XML se forma **anidando** elementos para especificar la estructura del documento, formando una estructura de árbol. El fragmento de código 3.1 representa la estructura básica de un documento XML que representa un listado de asignaturas.

```xml
<Asignaturas_Primero>
    <Asignatura>
        <titulo> Cálculo </titulo>
        <tipo> Anual </tipo>
        <profesor>
            <nombre> Ana </nombre>
            <apellido> Sanz </apellido>
        </profesor>
        <profesor>
            <nombre> Roberto </nombre>
            <apellido> Hernández </apellido>
        </profesor>
    </Asignatura>
    <Asignatura>
        <titulo> Física </titulo>
        <tipo> Semestral </tipo>
        <profesor>
            <nombre> Carmelo </nombre>
            <apellido> Moya </apellido>
        </profesor>
    </Asignatura>
</Asignaturas_Primero>
```
_Listado 3.1: Listado de Asignaturas_

Los elementos de un documento pueden contener atributos. Los atributos se incluyen en la definición del elemento, dentro de la etiqueta de apertura. Sintácticamente, se representan mediante un par _nombre = valor_. A diferencia de un subelemento, **un atributo solo puede aparecer una vez en una etiqueta**. El uso de atributos y su diferencia con respecto a los elementos no siempre es evidente. A **nivel de documento**, los atributos se introducen para añadir texto que no se visualizará en el documento, mientras que los subelementos formarán parte del contenido del documento. A **nivel de datos**, la diferencia es insustancial, puesto que la misma información se puede representar utilizando atributos o subelementos. Por ejemplo, el subelemento _< tipo >_ de una asignatura, podría representarse como un atributo del elemento asignatura. El listado 3.2 representa la asignatura de Cálculo del ejemplo anterior, incluyendo el título de la asignatura como atributo de la misma.


```xml
<Asignaturas_Primero>
    <Asignatura>
        <titulo> Cálculo tipo = 'anual' </titulo>
        <tipo> Anual </tipo>
        <profesor>
            <nombre> Ana </nombre>
            <apellido> Sanz </apellido>
        </profesor>
        <profesor>
            <nombre> Roberto </nombre>
            <apellido> Hernández </apellido>
        </profesor>
    </Asignatura>
</Asignaturas_Primero>
```
_Listado 3.2: Uso de atributos para la definición de asignaturas_

Dado que una de las principales aplicaciones de XML es el intercambio de información entre organizaciones, y ante la flexibilidad del lenguaje que permite definir cualquier nombre para una etiqueta, es posible que **existan conflictos entre los nombres de éstas**. Para ello, se recurre a los **espacios de nombres**, los cuales permiten a las organizaciones especificar nombres de etiquetas únicos. De esta forma, se añade al nombre de la etiqueta o atributo un identificador de recursos universal. El listado muestra de forma resumida un ejemplo de utilización del espacio de nombres para la definición de asignaturas.

```xml
<Asignatura xmlns:AS="http://www.listado-asignaturas.com">
    <AS:titulo> Cálculo tipo = 'anual' </AS:titulo>
    <AS:tipo> Anual </AS:tipo>
    <AS:profesor>
        <AS:nombre> Ana </AS:nombre>
        <AS:apellido> Sanz </AS:apellido>
    </AS:profesor>
    <AS:profesor>
        <AS:nombre> Roberto </AS:nombre>
        <AS:apellido> Hernández </AS:apellido>
    </AS:profesor>
</Asignatura>
```
_Listado 3.3: Uso de un espacio de nombres_

Al igual que ocurre en las bases de datos relacionales, las bases de datos XML presentan una serie de **lenguajes de definición de tipos de documentos XML**. Concretamente, a continuación se profundizaran en **DTD y XML Schema**. Seguidamente, se estudiarán los lenguajes **XPath, XSLT y XQuery**, **como lenguajes de manipulación de datos XML** para la definición y ejecución de consultas.

_[ver presentación BDA3.1](https://moodle.iesgrancapitan.org/pluginfile.php/58312/mod_folder/content/0/BDA_UD3_01.pdf)_

### 3.2.1 Definition Type Document (DTD)

En XML, se dice que un documento está **bien formado** si cumple con la sintaxis del lenguaje: correcta definición de elementos y atributos, correcto anidamiento...Como se puede apreciar, este concepto alude a la **dimensión sintáctica de los documentos**.

Por otra parte, se dice que un documento XML **es válido** cuando **está bien formado y cumple con la estructura de documento que ha sido dada a través de un lenguaje de definición de datos**. Esta definición de la estructura de los documentos XML es **opcional**, si bien es cierto que es recomendable, especialmente cuando se trabaja con documentos complejos y con un volumen grande de ellos. La definición de tipos de documento o **DTD permite definir la estructura de un documento XML**. Para ello, la DTD especifica la estructura de los elementos y atributos de un documento: qué elementos pueden aparecer, qué atributos, qué subelementos, multiplicidad de ellos... Existen multitud de validadores on-line de documentos XML y sus correspondientes DTDs y/o XML Schemas[^1].

La **declaración de una DTD** dentro de un documento XML puede implementarse de dos formas: en primer lugar, de manera **interna**. En este caso, se añade la DTD después del preámbulo XML y justo antes del documento propiamente dicho. El listado 3.4 muestra un ejemplo de declaración interna de una DTD.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
<!DOCTYPE Elemento_Raiz [Aquí la DTD]>
<!-------Aquí va el documento XML----->
```
_Listado 3.4: Declaración Interna DTD_

[^1]: [https://www.liquid-technologies.com/online-xsd-validator](http://xmlvalidator.new-studio.org) y [http://xmlvalidator.new-studio.org](http://xmlvalidator.new-studio.org)

En segundo lugar, la **DTD puede declararse de forma externa**, definiendo la misma en un fichero .DTD aparte que después es enlazado en el documento. El listado 3.5 muestra un ejemplo de declaración externa de una DTD.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
<!DOCTYPE Elemento_Raiz SYSTEM "Mi_DTD.dtd">
<!-------Aquí va el documento XML-----
```
_Listado 3.5: Declaración Externa DTD_

Siguiendo con el ejemplo del listado 3.1, el listado 3.6 muestra el archivo .dtd que validaría la estructura del documento de asignaturas.

```xml
<¡ DOCTYPE Listado_Asignaturas[
<! ELEMENT Asignaturas_Primero (Asignatura+)>
<! ELEMENT Asignatura (titulo, tipo, profesor+)>
<! ELEMENT titulo (#PCDATA)>
<! ELEMENT tipo (#PCDATA)>
<! ELEMENT profesor (nombre, apellido)>
<! ELEMENT nombre (#PCDATA)>
<! ELEMENT apellido (#PCDATA)>
```
_Listado 3.6: DTD Listado de Asignaturas_

En el listado anterior, se define en primer lugar el tipo de documento "Listado_Asignaturas". A continuación, y especificados entre corchetes, se describen los elementos que contendrá el documento, comenzando por el elemento raíz "Asignaturas_Primero". Este elemento estará formado por uno o más elementos "Asignatura" (especificado mediante el operador +). Seguidamente, el elemento "Asignatura" viene dado por un conjunto de tres subelementos: título, tipo y profesor. Además, cada asignatura puede tener uno o más profesores. Los elementos título y tipo vienen dados por cadenas de caracteres (denotadas con la instrucción #PCDATA) mientras que el elemento profesor estará formado por dos subelementos llamados "nombre" y "apellido" ambos definidos mediante cadenas de caracteres.

La definición de atributos para un elemento se hace a continuación de la definición del mismo utilizando _<!ATTLIST >_. El listado 3.7 muestra la DTD anterior en caso de que el tipo de asignatura sea considerado un atributo. 

```xml
<¡ DOCTYPE Listado_Asignaturas[
<! ELEMENT Asignaturas_Primero (Asignatura+)>
<! ELEMENT Asignatura (titulo, profesor+)>
<! ATTLIST tipo id #CDATA REQUIRED>
<! ELEMENT titulo (#PCDATA)>
<! ELEMENT tipo (#PCDATA)>
<! ELEMENT profesor (nombre, apellido)>
<! ELEMENT nombre (#PCDATA)>

```
_Listado 3.7: DTD Listado de Asignaturas con atributos_

Finalmente, la tabla 3.1a muestra un resumen de definición de elementos y atributos en una DTD.

| **Tipos** | **ELEMENTO** |
| :--: | :--: |
| #PCDATA | Cadena de caracteres |
| EMPTY | Elemento sin contexto |
| ANY | Cualquier tipo |
| **Operadores** | **ELEMENTO** |
| \| | Disyunción |
| + | Uno o más |
| * | Cero o más |
| ? | Cero o uno |

_Tabla 3.1a: Descripción DTD de elementos y atributos XML_

Y la tabla 3.1b muestra un resumen de definición de atributos en una DTD.

| **Tipos** | **ATRIBUTOS** |
| :--: | :--: |
| CDATA | Cadena de caracteres |
| ID | Identificador |
| IDREF |Referencia a un ID |
| **Operadores** | **ELEMENTO** |
| #REQUIRED | Obligatorio |
| #IMPLIED | Opcional |
| #FIXED | Valor fijo |
| #DEFAULT | Valor por defecto |

_Tabla 3.1a: Descripción DTD de atributos XML_

_[ver presentación BDA3.2](https://moodle.iesgrancapitan.org/pluginfile.php/58312/mod_folder/content/0/BDA_UD3_02.pdf)_

### 3.2.2 XML Schema

Se trata de un **lenguaje de definición de la estructura de documentos XML más sofisticado que DTD**. Aunque es **más complejo, soluciona muchos de los inconvenientes de las DTD**. De hecho, XML Schema es un superconjunto del lenguaje DTD especificado mediante la sintaxis de XML. Entre otros aspectos, XML Schema soporta no solo el tipo cadena de caracteres (soportado por DTD), sino también tipos numéricos, tipos complejos como listas y además permite al usuario definir sus propios tipos así como extender tipos de datos complejos mediante un mecanismo similar a la herencia. Además, XML Schema soporta el concepto de espacios de nombre, permitiendo reutilizar elementos de un esquema en otros.

Un XML Schema se especifica en un documento aparte del documento XML con extensión **.xsd**. Dentro de este archivo, que sigue las reglas sintácticas de cualquier documento XML, se especifican las definiciones de cada elemento del esquema, qué atributos incluye así como sus tipos de datos válidos asociados tanto a los elementos como a los atributos. De esta forma, **XML Schema permite definir un esquema para validar documentos XML, de forma más sofisticada a como lo hace DTD**. El listado 3.8 muestra un XML Schema para el ejemplo del listado de asignaturas.

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:element name="Asignaturas_Primero">
        <xs:complexType>
            <xs:sequence>
                <xs:element maxOccurs="unbounded" name="Asignatura">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="titulo" type="xs:string" />
                            <xs:element name="tipo" type="xs:string" />
                            <xs:element maxOccurs="unbounded" name="profesor">
                                <xs:complexType>
                                    <xs:sequence>
                                        <xs:element name="nombre" type="xs:string" />
                                        <xs:element name="apellido" type="xs:string" />
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```
_Listado 3.8: XML Schema: Listado de asignaturas_

El listado anterior comienza definiendo un XML Schema en la segunda línea de código. Cualquier XML Schema deberá incluir esta línea de código. El atributo _attributeFormDefault = "unqualified"_ indica que no es necesario utilizar el prefijo del espacio de nombres para definir los atributos del esquema, mientras que el atributo _elementFormDefault = "qualified"_ indica que no es necesario utilizar el prefijo del espacio de nombre para definir los elementos del esquema.

A continuación, se define el elemento raíz "Asignaturas_Primero" el cual se trata de un tipo complejo, puesto que no se define con un tipo de datos básico como entero, carácter, etc. Este elemento está formado por una secuencia de elementos del tipo "Asignatura" (el número máximo de elementos Asignatura no está limitado ya que _maxOccurs = "unbounded"_. Los elementos Asignatura, a su vez, están formados de una secuencia de elementos "nombre", "tipo" (ambos de tipo cadena de caracteres) y un conjunto de elementos de tipo "profesor" no acotados, el cual se compone a su vez de los elementos "nombre" y "apellido", ambos de tipo cadena de caracteres.

XML Schema permite representar la cardinalidad de un elemento, definiendo el número mínimo y máximo de ocurrencias mediante los atributos _minOccurs_ y _maxOccurs_. Además, también es posible añadir restricciones de forma que se limiten los valores de un elemento o atributo, se permita elegir entre una lista de posibles...Finalmente, XML Schema dispone de los contenedores _sequence_ para definir una secuencia ordenada de subelementos, _chocie_ para definir la elección entre posibles grupos de elementos o elementos y _all_ para definir un conjunto no ordenado de elementos. Al igual que sucedía con DTD, **existen distintas herramientas on-line que no solo permiten validar un documento XML a partir de su esquema XSD, sino que también permiten generar el esquema XSD a partir del archivo XML**[^2].

[^2]: [https://www.freeformatter.com](https://www.freeformatter.com), [https://www.liquid-technologies.com/online-xsd-validator](https://www.liquid-technologies.com/online-xsd-validator) y [https://extendsclass.com/xml-schema-validator.html](https://extendsclass.com/xml-schema-validator.html)

### 3.2.2 XPath

En cualquier sistema de bases de datos, además de los lenguajes de definición de datos, los **lenguajes de manipulación de datos** son esenciales para poder realizar consultas y recuperar datos, realizar actualizaciones, etc.

**XPath es un lenguaje de manipulación de datos XML**. La principal característica de este lenguaje es que **interpreta un documento XML como una estructura de datos de tipo árbol**, donde los nodos y sus respectivas hojas se corresponden con los elementos y subelementos del documento XML. De esta forma, y utilizando comandos que permiten recorrer el documento de forma análoga a los comandos que se utilizan en una terminal de comandos para recorrer el sistema de archivos de un computador, es posible realizar consultas. La tabla 3.2 muestra un listado de los principales operadores utilizados en XPath.

| **Operador** | **Significado** |
| :-- | :-- |
| / | Navegación entre nodos del documento. Selecciona los nodos del nivel inferior. |
| // | Navegación entre nodos del documento. Selecciona los nodos del nivel inferior de cualquier nodo especificado a continuación |
| . | Selecciona el nodo actual |
| .. | Selecciona el nodo padre o nodo de nivel superior |
| \| | Expresa alternativas |
| @ | Selecciona un atributo de un nodo |
| * | Selecciona todos los nodos |
| [] | Agrupa otros operadores |

_Tabla 3.2: XPath: Operadores básicos_

Con estos operadores, veamos cómo realizar algunas consultas de ejemplo sobre el fragmento XML que especifica un listado de asignaturas.

??? question "**Consulta 1**. Recuperar los nombres de todas las asignaturas."

    /Asignaturas_Primero/Asignatura/titulo/text()

??? question "**Consulta 2**. ¿Cuántas asignaturas son impartidas por más de un profesor?"

    /Asignaturas_Primero/Asignatura[count(profesor)>1]

??? question "**Consulta 3**. ¿Quiénes son los profesores que imparten la asignatura de física?"

    /Asignaturas_Primero/Asignatura[titulo="Fisica"]/profesor/nombre

**Es posible utilizar XPath de forma on-line a través de visualizadores[^3] o, por ejemplo, instalando el plug-in XPatherizer de Notepad++, en caso de utilizar este último editor de código XML.**

[^3]: [https://download.cnet.com/XPath-Visualizer/3000-7241_4-75804649.html](https://download.cnet.com/XPath-Visualizer/3000-7241_4-75804649.html), [https://www.freeformatter.com/xpath-tester.html](https://www.freeformatter.com/xpath-tester.html)

_[ver presentación BDA3.3](https://moodle.iesgrancapitan.org/pluginfile.php/58312/mod_folder/content/0/BDA_UD3_03.pdf)_

### 3.2.4 XSLT

Otro lenguaje de manipulación de datos XML es el lenguaje XSLT. **El lenguaje XSLT (XML StylesSheet Transformations) proviene del lenguaje XSL (XML StylesSheet)**. Este último permite especificar las opciones de formato de un documento XML en un archivo separado, aislando así la especificación del contenido y la presentación de un archivo XML. Podría decirse que XSL es a XML lo que CSS es a HTML. **El lenguaje XSLT permite especificar las opciones e formato de un documento XML, pudiendo transformar este documento en otros formatos como HTML, PDF, etc**. Dada la potencia de este lenguaje, que es capaz de transformar un archivo XML en otro que también pudiera ser XML, **se utiliza frecuentemente como lenguaje de consulta**.

Las transformaciones XSLT se definen por medio de **plantillas**, que permiten a su vez recuperar contenido del documento XML a través de la utilización de expresiones XPath. La principal característica de XSLT es que la aplicación de plantillas se realiza de forma recursiva, lo cual recibe el nombre de **recursividad estructural**. El listado 3.9 presenta un ejemplo de aplicación de una transformación XSLT por medio de una plantilla.

```xml
<xsl: template match = /Asignaturas_Primero/Asignatura >
    <titulo_asignatura>
        <xsl:value-of select = titulo/>
    </titulo_asignatura>
</xsl:template>
```
_Listado 3.9: XSLT: Ejemplo de plantilla_

Tal y como se puede observar en el fragmento anterior, una transformación o consulta XSLT se define mediante dos órdenes básicas: _match_ que especifica una expresión XPath para seleccionar uno o más nodos y _value − of select_ que devuelve los valores especificados de los nodos que se han obtenido como resultado de la expresión XPath. Merece la pena destacar que **cualquier texto o etiqueta del archivo XSLT que no esté en el espacio de nombre se copia a la salida sin cambios**. Esto quiere decir, que el resultado del fragmento anterior no son únicamente los títulos de las asignaturas, sino estos mismos delimitados por una etiqueta de apertura y cierre llamada _< titulo_asignatura >_. De esta forma, a partir de un documento XML se ha generado otro con el resultado de la consulta. A modo de resumen, la tabla 3.3 define los principales constructores XSLT.

| **Constructor XSLT** | **Significado** |
| :-- | :-- |
| _< xsl:template match = "expresion XPath" >_ | Selecciona nodos a devolver |
| _<xsl:value − of select = "valor">_ | Devuelve todos los nodos que no coinciden con alguna otra plantilla |
| _<xsl:template ><xsl:template match = "∗"/>_ | Selecciona el nodo actual |
| _<xsl:attribute nombre = "nombre atributo">_ | Añade un atributo al elemento precedente |
| _<xsl:apply − templates/>_ | Aplica la recursividad estructural |
| _<xsl:keyname = "nombre clave" match = "elementos aplicar" use = "valor clave"/>_ | Define una clave |
| _<xsl:value − of select = "key("nombre clave" "valor")"/>_ | Devuelve los nodos que coincidan con el valor especificado. Especifica
operaciones de combinación (JOINS) |
| _<xsl:sort select = "elemento o atributo"/>_ | Devuelve los nodos resultados, ordenados por un elemento o atributo |

_Tabla 3.3: Principales Constructores XSLT_

Para trabajar con XSLT, se aconseja la descarga del software XML Copy Editor [https://xml-copy-editor.sourceforge.io](https://xml-copy-editor.sourceforge.io).

_[ver presentación BDA3.4](https://moodle.iesgrancapitan.org/pluginfile.php/58312/mod_folder/content/0/BDA_UD3_04.pdf)_

### 3.2.5 XQuery

Es un **lenguaje de consulta de propósito general sobre datos XML estandarizado por el consorcio W3C**. XQuery procede del lenguaje de programación Quilt y, por este motivo, toma prestadas muchas características de otros lenguajes como XPath o SQL. De hecho, la sintaxis de XQuery es mucho más similar a la de SQL, al contrario que ocurría con lenguajes como XPath o XSLT. 

Para definir una consulta en XQuery se construye una expresión, denominada **"FLWR"(flower, flor en inglés)**. A continuación, se describen cada una de las sentencias de las que se compone la expresión:

- **FOR**: Obtiene una serie de variables cuyos valores son el resultado de una expresión XPath. Es el equivalente a la cláusula FROM del lenguaje SQL.

- **LET**: Define y renombra expresiones complejas para ser utilizadas en el resto de la consulta, reduciendo la complejidad y aliviando la sintaxis. Es una cláusula opcional.

- **WHERE**: En esta cláusula, llamada igualmente en SQL, se especifican condiciones sobre los resultados obtenidos en la cláusula FOR.

- **RETURN**: Especifica y define el resultado que se va a obtener de la consulta. Se trata del equivalente a la cláusula **SELECT** en SQL.

Siguiendo la sintaxis anteriormente mencionada, el listado 3.10 muestra un ejemplo para obtener un listado con las asignaturas anuales.

```xquery
for $x in /Asignaturas_Primero/Asignatura
let $nombre_asignatura:= $x/titulo
where $x/tipo='anual'
return <asignatura_anual> $nombre_asignatura </asignatura_anual>
```
_Listado 3.10: XQuery: Ejemplo de consulta_

En el listado anterior,la cláusula _for_ permitirá recorrer todos los elementos asignatura. Por su parte, let define una expresión en la que se asigna a _nombre_asignatura_ el título de la asignatura que se está recorriendo en cada momento. La cláusula _where_ impone el criterio de que el tipo de asignatura sea anual y, finalmente, _result_ define como resultado un fragmento de código XML donde el nombre de la asignatura anual (según la expresión indicada en let) aparecerá como resultado encerrado entre dos etiquetas XML llamadas _asignatura_anual_. Tal y como se puede apreciar, XQuery también permite generar nuevos documetnos XML a partir de consultas.

Al igual que SQL, XQuery dispone de multitud de funciones para la consulta de datos. Así, es posible utilizar la función _distinct()_ para eliminar duplicados en los resultados así como funciones de agregación como _sum()_ o _count()_ además de poder especificar funciones definidas por el usuario. XQuery también permite utilizar "joins" incluyendo en la cláusula where el operador de combinación al igual que en SQL. Por otra parte, si bien es cierto que XQuery no posee el operador _group by_ de SQL, este puede implementarse mediante la creación de consultas anidadas, las cuales sí que son soportadas por el lenguaje.

## 3.3 Bases de datos documentales: MongoDB

Los sistemas de bases de datos documentales u orientados a documentos **son sistemas de bases de datos NoSQL que almacenan datos, los cuales se estructuran en forma de documentos**. En los últimos años, han proliferado una gran variedad de sistemas de este tipo como pueden ser **Cassandra[^4], CouchDB[^5], Riak[^6] o MongoDB[^7], sobre el cual trata esta sección**.


[^4]: [https://cassandra.apache.org/](https://cassandra.apache.org/)
[^5]: [http://couchdb.apache.org](http://couchdb.apache.org)
[^6]: [https://riak.com](https://riak.com)
[^7]: [https://www.mongodb.com/es](https://www.mongodb.com/es)

El nombre **MongoDB** proviene de la palabra inglesa **"homongous"** que significa enorme. **MongoDB es, por tanto, un sistema de base de datos NoSQL, open source, orientado a documentos y escrito en lenguaje C++**. Este sistema de bases de datos está disponible no solo para múltiples plataformas y sistemas operativos (Windows, Linux, OS X) sino también como servicio empresarial en la nube y se puede integrar con otros servicios como, por ejemplo, Amazon Web Services (AWS).

### 3.3.1 MongoDB: Características y aplicaciones

Tal y como se ha presentado anteriormente, MongoDB es un sistema de bases de datos documental u orientado a documentos. Esta orientación hace que los datos se almacenen de forma estructurada en forma de documentos, los cuales se acoplan sin problema en los tipos de datos utilizados por los lenguajes de programación. Además, esta concepción hace que una base de datos MongoDB disponga de un esquema dinámico y fácilmente modificable.

En este apartado se pretende poner de relieve **las principales características de MongoDB** que hacen que sea en la actualidad uno de los sistemas de bases de datos NoSQL más utilizados tanto en el ámbito académico como en el profesional. 

- **Alto rendimiento**: Gracias a la definición de los documentos y la creación de índices que hace que las lecturas y escrituras se realicen de forma más rápida.

- **Alta disponibilidad**: MongoDB dispone de servidores replicados con restablecimiento automático maestro.

- **Fácil escalabilidad**: Permitiendo distribuir colecciones de documentos entre diferentes máquinas de forma muy sencilla.

- **Indexación**: Similar al concepto de índice en una base de datos relacional, permite crear índices e índices secundarios para mejorar el rendimiento de las consultas.

- **Consultas ad-hoc**: Al igual que en una base de datos relacional, MongoDB da soporte a la búsqueda por campos, consultas de rangos, uso de expresiones regulares... A todo lo anterior, se añade la posibilidad de ejecutar y devolver una función JavaScript definida por el programador.

- **Replicación**: Siguiendo el modelo maestro-esclavo. El maestro puede ejecutar comandos de lectura y escritura mientras que el esclavo solo tiene acceso de lectura y la posibilidad de realizar copias de seguridad. En caso de que el maestro caiga, el esclavo puede elegir un nuevo maestro para mantener el servicio de replicación.

- **Balanceo de carga**: MongoDB es capaz de ejecutarse en múltiples servidores, pudiendo balancear la carga y/o duplicar los datos para mantener el correcto funcionamiento del sistema aunque se produzca un fallo hardware.

- **Almacenamiento de archivos**: Todo lo anterior, facilita que este sistema de base de datos pueda ser usado con un sistema de archivos GridFS con balanceo de carga y replicación.

- **Agregación**: Proporciona operadores de agregación y la posibilidad de utilizar funciones MapReduce para el procesamiento de datos por lotes.

- **Ejecución de Javscript del lado del servidor**: Es posible realizar consultas utilizando JavaScript, de forma que éstas son enviadas directamente a la base de datos para ser ejecutadas.

Todas estas características hacen que, en la actualidad, MongoDB sea uno de los principales sistemas de bases de datos NoSQL elegidos en multitud de aplicaciones como: **almacenamiento y registro de eventos, comercio electrónico, juegos, aplicaciones móviles, almacenes de datos, gestión de estadísticas en tiempo real y cualquier aplicación que requiera llevar a cabo analíticas sobre grandes volúmenes de datos**.

### 3.3.2 MongoDB: Conceptos básicos, utilidades y herramientas

***[Getting Started MongoDB](https://www.mongodb.com/docs/manual/tutorial/getting-started/)***

Aunque MongoDB es un sistema de bases de datos NoSQL con todo lo que ello implica, también ofrece toda la funcionalidad de la que disponen las bases de datos relacionales. Sin embargo, **la estructura de una base de datos MongoDB difiere de la de una base de datos relacional**.

En un servidor MongoDB es posible crear tantas bases de datos como se desee. El concepto de **base de datos** es en este caso equivalente al de **base de dato**s en los sistemas relacionales. Una vez creada, la base de datos estará compuesta por una o más **colecciones**. El término colección en el ámbito NoSQL es el equivalente al concepto de **tabla** en los sistemas relacionales. Cada colección, por tanto, estará formada por un conjunto de **documentos** (o incluso ninguno, en cuyo caso la colección estaría vacía). El concepto de documento en NoSQL se corresponde con el concepto de **registro** en una base de datos relacional. Un documento estará compuesto por una serie de **campos**, al igual que lo están los registros de una base de datos relacional. En el ámbito NoSQL, y más concretamente en MongoDB, cada documento viene dado por un archivo **JSON** (http://www.json.org/) en el cual, siguiendo una estructura clave-valor se especifican las características de cada documento. El listado 3.11 muestra un ejemplo de documento que define un barco.

```json
{
    name: 'USS Enterprise-D',
    operator: 'Starfleet',
    type: 'Explorer',
    class: 'Galaxy',
    crew: 750,
    codes: [10,11,12]
}
```
_Listado 3.11: Definición json de un documento barco_

Para la administración del sistema de bases de datos, MongoDB pone a disposición de los usuarios las siguientes utilidades:

- **mongo**: Se trata del shell interactivo de MongoDB que permite insertar, eliminar, actualizar datos y realizar consultas, además de replicar la información, apagar servidores y ejecutar código JavaScript.

- **mongostat**: Herramienta de línea de comandos que muestra las estadísticas de una instancia en ejecución de MongoDB

- **mongotop**: Herramienta de línea de comandos que muestra la cantidad de tiempo empleado en la lectura y escritura de datos por parte de la instancia en ejecución

- **mongosniff**: Herramienta de línea de comandos que permite hacer un rastreo del tráfico de la red que va desde y hacia MongoDB.

- **mongoimport/mongoexport**: Herramienta de línea de comandos para importar y exportar contenido en o desde .json, .csv o .tsv entre otros.

- **mongodump/mongorestore**: Herramienta de línea de comandos para la creación de una exportación binaria del contenido de la base de datos.

Aunque MongoDB tiene una interfaz administrativa accesible desde http://localhost:28017/ (siempre que se haya iniciado con la opción mongod − −rest, existen algunas herramientas gráficas para la administración y uso de este sistema de base de datos como **UMongo** (https://github.com/agirbal/umongo) , el cual es una aplicación open source de sobremesa para navegar y administrar un clúster MongoDB o **Robomongo** (https://robomongo.org), que también es una herramienta gráfica open source que incluye una terminal de comandos completamente compatible con el shell de mongo.

_[ver presentación BDA3.5](https://moodle.iesgrancapitan.org/pluginfile.php/58312/mod_folder/content/0/BDA_UD3_05.pdf)_

### 3.3.3 MongoDB: Operaciones CRUD

***[MongoDB CRUD Operations](https://www.mongodb.com/docs/manual/crud/#mongodb-crud-operations)***

Cualquier sistema de datos permite definir operaciones básicas como son la creación de tablas e inserción de registros, lectura de datos, actualización y eliminación de registros. Estas operaciones básicas se conocen con el nombre de **CRUD (Create, Read, Update, Delete)**. A continuación, se muestran distintos ejemplos para ilustrar la implementación de estas operaciones en MongoDB.

Una vez iniciada una sesión del Shell de Mongo, es posible crear una base de datos utilizando el siguiente comando.

```
test>use DB_Ejemplo
switched to db DB_Ejemplo
```

Para crear una colección de documentos "Barco" dentro de la base de datos
DB_Ejemplo se puede escribir.
```
DB_Ejemplo>db.createCollection("ships")
{ ok: 1 }
```

Para visualizar las bases de datos almacenadas en el servidor se puede utilizar el comando _show dbs_ mientras que para obtener un listado de las colecciones de la base de datos sobre la que se está trabajando, es posible utilizar el comando _show collections_. Por otra parte, para insertar un documento dentro de la colección, como el mostrado en el listado 3.11, se ejecuta el siguiente comando.

```
db.ships.insertOne({name:'USS-Enterprise-D',operator:'Starfleet',type:'Explorer',class:'Galaxy',crew:750,codes:[10,11,12]})
```
_[ver presentación BDA3.6](https://moodle.iesgrancapitan.org/pluginfile.php/58312/mod_folder/content/0/BDA_UD3_06.pdf)_

Para **actualizar** el nombre del barco "USS-Enterprise-D" por "USS Something" es posible escribir el comando.

***[MongoDB Update Operations](https://www.mongodb.com/docs/manual/crud/#update-operations)***

```
db.ships.updateOne({name : {$eq: 'USS-Enterprise-D'}}, {$set : {name: 'USS Something'}})
```

Mientras que si se pretenden establecer o cambiar varios atributos de un mismo documento, como por ejemplo el operador y la clase, se puede utilizar el comando update de la siguiente forma.

```
db.ships.updateOne({name : {$eq: 'USS Something'}}, {$set : {name: 'USS Prometheus', class: 'Prometheus'}})
```

Finalmente, la eliminación de algún atributo de un documento también es una operación de actualización que puede realizarse de la siguiente forma.

```
db.ships.updateOne({name : {$eq: 'USS Prometheus'}}, {$unset : {operator: 1}})
```

***[MongoDB Delete Operations](https://www.mongodb.com/docs/manual/crud/#delete-operations)***

La **eliminación** de documentos de una colección puede realizarse de forma directa o bien utilizando expresiones regulares. A continuación se muestran dos comandos para ilustrar ambas formas de eliminación. El segundo de ellos elimina aquellos documentos que comienzan por "a".

```
db.ships.deleteOne({name : 'USS Prometheus'})
db.ships.deleteOne({name:{$regex:'^A*'}})
```

Para **leer y mostrar** documentos, se utiliza el comando find(). A continuación se muestra un ejemplo en el que, el primer comando muestra un documento al azar de los existentes, el segundo muestra todos los documentos y lo hace de forma indexada en lugar de como texto seguido, el tercero muestra solo los nombres de los barcos y el último, encuentra un documento cuyo nombre sea "USS Defiant".

***[MongoDB Read Operations](https://www.mongodb.com/docs/manual/crud/#read-operations)***

```
db.ships.findOne()
db.ships.find().pretty()
db.ships.find({}, {name:true})
db.ships.findOne({'name':'USS Defiant'})
```
_[ver presentación BDA3.7](https://moodle.iesgrancapitan.org/pluginfile.php/58312/mod_folder/content/0/BDA_UD3_07.pdf)_

### 3.3.4 MongoDB Consultas y Agregación

Al igual que cualquier lenguaje de manipulación de datos, MongoDB dispone de las herramientas y operadores necesarios para realizar consultas sobre los documentos almacenados en las colecciones. A continuación, se muestran ejemplos de uso de los principales operadores.

Los operadores relaciones mayor que, menor que, mayor o igual que y menor o igual que se corresponden, respectivamente, con los operadores $gt, $lt, $gte, $lte. Además, también es posible utilizar el operador $regex para recuperar elementos que cumplan una expresión regular. A continuación, se muestran dos consultas que recuperan aquellos barcos que permitan subir a bordo a más de cien pasajeros y otra consulta para recuperar aquellos que dejen subir tan solo a 1" pasajeros o menos.

```
db.ships.find({crew:{$gt:100}})
db.ships.find({crew:{$lte:100}})
```

El operador \$exists permite devolver aquellos documentos para los que existe o no un determinado atributo. El siguiente comando permite encontrar aquellos barcos para los cuales existe el campo "class".

```
db.ships.find({class:{$exists:true}})
```

***[MongoDB Aggregation Operations](https://www.mongodb.com/docs/manual/aggregation/#aggregation-operations)***

Las funciones de **agregación** son especialmente útiles en los lenguajes de consulta y manipulación de datos. De esta forma, $sum permite agregar mediante la operación suma una serie de valores, $avg permite obtener la media, $mnin y $max encontrar el valor máximo y mínimo de un atributo para el conjunto de elementos, $push introduce en un array los resultados de la consulta que se ha realizado, $addToSet es similar al anterior solo que sin incluir valores duplicados y, por último, $first y $last permiten obtener el primer y último documento en una consulta. A continuación, se muestra un ejemplo del uso de cada uno de estos operadores de agregación.

```
db.ships.aggregate([{$group:{_id:"$operator",num_ships:{$sum:"$crew"}}}])
db.ships.aggregate([{$group : {_id : "$operator", num_ships : {$avg : "$crew"}}}])
db.ships.aggregate([{$group : {_id : "$operator", num_ships : {$min : "$crew"}}}])
db.ships.aggregate([{$group : {_id : "$operator", classes : {$push: "$class"}}}])
db.ships.aggregate([{$group : {_id : "$operator", classes : {$addToSet : "$class"}}}])
db.ships.aggregate([{$group:{_id:"$operator",last_class:{$last: "$class"}}}])
```

Finalmente, MongoDB pone a disposición de los usuarios multitud de funciones útiles en la realización de consultas. La tabla 3.4 muestra algunas de las más utilizadas.

| **Función** | **Significado** |
| -- | -- |
| $project | Cambia el conjunto de documentos modificando sus claves y valores |
| $match | Operación de filtrado para reducir el número de elementos recuperados |
| $group | Operador de agregación para agrupar resultados |
| $sort | Ordena los documentos |
| $skip | Recupera los documentos a partir de un numero especificado por el usuario |
| $limit | Limita los resultados de la consulta al valor pasado como parámetro a la función |
| $unwind | Utilizado como equivalente al join de SQL |

_Tabla 3.4: Funciones útiles en MongoDB_

_[ver presentación BDA3.8](https://moodle.iesgrancapitan.org/pluginfile.php/58312/mod_folder/content/0/BDA_UD3_08.pdf)_










