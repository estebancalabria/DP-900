# Fundamentos de datos

## Introducción

Los datos son un recurso fundamental para las organizaciones. Los sistemas, aplicaciones y dispositivos generan grandes cantidades de información que puede almacenarse, procesarse y analizarse para obtener conocimiento y apoyar la toma de decisiones.

Las soluciones de datos permiten:

- Recopilar datos.
- Almacenar grandes volúmenes de información.
- Procesar y transformar datos.
- Analizar información.
- Generar informes y visualizaciones.
- Obtener información útil para la toma de decisiones.

---

# Tipos de datos

Los datos representan normalmente **entidades** importantes para una organización, como clientes, productos, pedidos o transacciones.

Cada entidad posee **atributos** que describen sus características.

Los datos pueden clasificarse en tres categorías principales:

- **Estructurados**
- **Semiestructurados**
- **No estructurados**

## Datos estructurados

Los **datos estructurados** siguen un esquema fijo.

Generalmente se representan mediante tablas:

- Las **filas** representan registros.
- Las **columnas** representan atributos.

Ejemplo:

| CustomerID | Name | City |
|---|---|---|
| 1 | Joe Jones | New York |
| 2 | Samir Nadoy | Seattle |

Este modelo es característico de las **bases de datos relacionales**.

## Datos semiestructurados

Los **datos semiestructurados** poseen cierta estructura, pero no requieren que todos los registros tengan exactamente los mismos campos.

Uno de los formatos más utilizados es **JSON (JavaScript Object Notation)**.

```json
{
  "firstName": "Joe",
  "lastName": "Jones",
  "contact": [
    {
      "type": "email",
      "address": "joe@litware.com"
    }
  ]
}
```

JSON permite representar estructuras flexibles y jerárquicas.

## Datos no estructurados

Los **datos no estructurados** no siguen necesariamente un esquema definido.

Ejemplos:

- Documentos
- Imágenes
- Audio
- Vídeo
- Archivos binarios

También existen los **datos vectoriales o embeddings**, que representan contenido, como texto o imágenes, mediante vectores numéricos.

Los sistemas de inteligencia artificial pueden utilizar embeddings para realizar **búsquedas por similitud semántica**.

---

# Almacenamiento de datos

Existen dos categorías generales de almacenamiento:

1. **Almacenes de archivos**
2. **Bases de datos**

La elección depende del tipo de datos, las aplicaciones que necesitan utilizarlos y los requisitos de procesamiento.

---

# Almacenamiento basado en archivos

Los archivos pueden almacenarse localmente, en sistemas compartidos o en servicios de almacenamiento en la nube.

El formato utilizado depende de:

- Tipo de datos.
- Aplicaciones que necesitan procesarlos.
- Necesidades de interoperabilidad.
- Volumen de información.
- Rendimiento.
- Compresión.
- Eficiencia de almacenamiento.

## CSV

**CSV (Comma-Separated Values)** almacena datos tabulares mediante texto separado por comas.

```text
FirstName,LastName,Email
Joe,Jones,joe@litware.com
Samir,Nadoy,samir@northwind.com
```

Es sencillo, legible y ampliamente compatible.

También existen formatos similares como:

- TSV
- Valores separados por espacios
- Archivos de ancho fijo

## JSON

JSON representa información mediante una estructura jerárquica basada en objetos y colecciones.

Características principales:

- `{ }` representan objetos.
- `[ ]` representan colecciones.
- Los atributos se representan mediante pares `nombre: valor`.

Es ampliamente utilizado para datos estructurados y semiestructurados.

## XML

**XML (Extensible Markup Language)** utiliza etiquetas para representar elementos y atributos.

```xml
<Customers>
  <Customer name="Joe" lastName="Jones">
    <Contact type="email" address="joe@litware.com"/>
  </Customer>
</Customers>
```

XML fue especialmente popular durante las décadas de 1990 y 2000. Actualmente JSON suele utilizarse como una alternativa más compacta, aunque muchos sistemas continúan utilizando XML.

## BLOB

**BLOB (Binary Large Object)** hace referencia a información almacenada en formato binario.

Es habitual para almacenar:

- Imágenes
- Vídeos
- Audio
- Documentos
- Otros archivos binarios

---

# Formatos optimizados para datos

Para escenarios de análisis y grandes volúmenes de información existen formatos diseñados para mejorar el almacenamiento y el procesamiento.

## Apache Parquet

**Parquet** es un formato de almacenamiento orientado a columnas.

Características:

- Almacenamiento columnar.
- Compresión eficiente.
- Optimizado para consultas analíticas.
- Soporte para estructuras de datos anidadas.
- Muy utilizado en Data Lakes y Lakehouses.

Es especialmente eficiente cuando una consulta necesita leer solo algunas columnas de conjuntos de datos muy grandes.

## Apache Avro

**Avro** es un formato orientado a filas.

Características:

- Los datos se almacenan en formato binario.
- El esquema se almacena en el encabezado utilizando JSON.
- Buena capacidad de compresión.
- Reduce los requisitos de almacenamiento y transferencia.

## Delta Lake

**Delta Lake** es un formato de almacenamiento de código abierto basado en Parquet.

Agrega un **registro de transacciones** que proporciona capacidades como:

- Transacciones ACID.
- Versionado de datos.
- Actualizaciones confiables.
- Mayor consistencia sobre archivos almacenados en un Data Lake.

Conceptualmente:

```text
Delta Lake = Parquet + Transaction Log
```

---

# Bases de datos

Una base de datos proporciona un sistema especializado para almacenar, administrar y consultar registros.

Existen dos grandes categorías:

- **Bases de datos relacionales**
- **Bases de datos no relacionales**

---

# Bases de datos relacionales

Las **bases de datos relacionales** almacenan información estructurada mediante tablas.

Las tablas representan entidades como:

- Clientes
- Productos
- Pedidos
- Transacciones

Cada registro puede identificarse mediante una **clave primaria (Primary Key)**.

Las relaciones entre tablas se establecen mediante claves.

Ejemplo conceptual:

```text
Customer -> Order -> Product
```

La **normalización** permite reducir la duplicación de información almacenando cada dato en un lugar apropiado.

Las bases de datos relacionales utilizan normalmente **SQL (Structured Query Language)** para consultar y modificar los datos.

---

# Bases de datos NoSQL

Las bases de datos **NoSQL** no requieren necesariamente un modelo relacional tradicional.

Existen cuatro tipos principales.

## Key-Value

Cada registro posee una clave única asociada a un valor.

```text
Clave -> Valor
```

## Documentos

Almacenan documentos, normalmente JSON.

```text
Clave -> Documento JSON
```

## Familias de columnas

Organizan los datos mediante filas y columnas, pero permiten agrupar columnas relacionadas en **familias de columnas**.

## Grafos

Representan la información utilizando nodos y relaciones.

```text
Nodo -> Relación -> Nodo
```

Son especialmente útiles cuando las relaciones entre entidades constituyen una parte fundamental del problema.

---

# Procesamiento transaccional

Los sistemas transaccionales procesan eventos específicos de una organización.

Ejemplos:

- Transferencias bancarias.
- Compras.
- Pagos.
- Reservas.
- Pedidos.

Este tipo de procesamiento se conoce como:

**OLTP (Online Transaction Processing)**

Los sistemas OLTP suelen procesar grandes cantidades de pequeñas operaciones.

Las operaciones habituales se conocen como **CRUD**:

- **Create**
- **Read**
- **Update**
- **Delete**

---

# Transacciones ACID

Las bases de datos transaccionales utilizan propiedades **ACID** para garantizar la integridad de los datos.

## Atomicidad

Una transacción se ejecuta completamente o no se ejecuta.

Por ejemplo, en una transferencia bancaria:

```text
Cuenta A -> Cuenta B
```

El débito y el crédito deben completarse juntos.

## Consistencia

Una transacción debe llevar la base de datos de un estado válido a otro estado válido.

## Aislamiento

Las transacciones simultáneas no deben interferir entre sí produciendo resultados inconsistentes.

## Durabilidad

Una vez confirmada una transacción, los cambios deben conservarse incluso si el sistema se reinicia posteriormente.

---

# Procesamiento analítico

El procesamiento analítico trabaja principalmente con grandes volúmenes de información histórica.

Estas cargas suelen ser principalmente de lectura y permiten:

- Analizar tendencias.
- Crear métricas.
- Generar informes.
- Crear visualizaciones.
- Construir modelos analíticos.
- Apoyar decisiones empresariales.

---

# ETL y ELT

Los datos provenientes de sistemas operativos deben integrarse en plataformas analíticas.

Existen dos patrones principales.

## ETL

**Extract -> Transform -> Load**

1. Se extraen los datos.
2. Se transforman.
3. Se cargan en el sistema destino.

## ELT

**Extract -> Load -> Transform**

1. Se extraen los datos.
2. Se cargan en el sistema destino.
3. Se transforman dentro de la plataforma.

ELT es especialmente común en arquitecturas modernas basadas en **Data Lakes y Lakehouses**.

---

# Data Warehouse

Un **Data Warehouse** almacena datos estructurados y optimizados para análisis.

Características:

- Modelo relacional.
- Optimización para consultas.
- Integración de múltiples fuentes.
- Datos históricos.
- Uso intensivo de SQL.

Se utiliza principalmente para:

- Business Intelligence.
- Reporting.
- Dashboards.
- Análisis empresarial.

---

# Data Lake

Un **Data Lake** permite almacenar grandes cantidades de datos en archivos.

Puede contener:

- Datos estructurados.
- Datos semiestructurados.
- Datos no estructurados.

Normalmente conserva grandes volúmenes de información para procesamiento y análisis posterior.

---

# Data Lakehouse

Un **Data Lakehouse** combina características de un Data Lake y un Data Warehouse.

```text
Data Lake + Data Warehouse = Data Lakehouse
```

Busca proporcionar:

- Escalabilidad de un Data Lake.
- Almacenamiento basado en archivos.
- Flexibilidad de formatos.
- Consultas estructuradas mediante tablas.
- Capacidades transaccionales.
- Procesamiento analítico eficiente.

Tecnologías como **Delta Lake** permiten implementar muchas de estas capacidades.

---

# OLAP y modelos semánticos

**OLAP (Online Analytical Processing)** está diseñado para cargas de trabajo analíticas.

Actualmente, estas estructuras suelen implementarse mediante **modelos semánticos**.

Los datos pueden organizarse utilizando:

- Tablas de hechos.
- Tablas de dimensiones.
- Medidas.
- Jerarquías.
- Agregaciones.

Por ejemplo:

```text
Ventas -> Fecha -> Región -> Ciudad -> Producto
```

Los datos pueden preagregarse para acelerar las consultas analíticas.

Los **modelos semánticos de Power BI** son un ejemplo de este enfoque.

---

# Arquitectura analítica

Una arquitectura analítica moderna puede representarse conceptualmente como:

```text
Fuentes de datos
      |
   ETL / ELT
      |
Data Lake / Lakehouse
      |
Data Warehouse
      |
Modelo semántico
      |
Informes / Dashboards / Analytics
```

Diferentes perfiles pueden trabajar en distintas capas:

- **Data Scientists:** exploran datos y construyen modelos.
- **Data Engineers:** desarrollan procesos de ingesta y transformación.
- **Data Analysts:** crean consultas, modelos e informes.
- **Business Users:** consumen dashboards, métricas e información.

---

# Arquitectura Medallion

Una arquitectura común para organizar datos dentro de un Lakehouse es la **Medallion Architecture**.

Utiliza tres capas principales:

```text
Bronze -> Silver -> Gold
```

## Bronze

Contiene los datos originales tal como fueron recibidos desde los sistemas de origen.

Características:

- Datos sin procesar.
- Sin transformaciones significativas.
- Conservación del dato original.
- Permite reprocesar información posteriormente.

## Silver

Contiene datos limpios y estandarizados.

Procesos habituales:

- Eliminación de duplicados.
- Corrección de errores.
- Conversión de tipos.
- Normalización.
- Integración de fuentes.

## Gold

Contiene datos preparados para consumo empresarial.

Puede incluir:

- Agregaciones.
- Métricas.
- KPIs.
- Tablas analíticas.
- Datos preparados para reporting.

---

# Plataformas modernas de análisis de datos

## Microsoft Fabric

**Microsoft Fabric** es una plataforma SaaS unificada para análisis de datos.

Integra capacidades relacionadas con:

- Ingeniería de datos.
- Integración de datos.
- Data Warehouse.
- Data Science.
- Real-Time Intelligence.
- Power BI.
- OneLake.

También incorpora **Copilot** para trabajar con datos utilizando lenguaje natural.

## Azure Databricks

**Azure Databricks** es una plataforma de análisis de datos orientada a escenarios como:

- Ingeniería de datos.
- Big Data.
- Data Science.
- Machine Learning.
- Inteligencia Artificial.

Utiliza ampliamente **Delta Lake** para implementar arquitecturas Lakehouse.

---

# Gobierno de datos con Microsoft Purview

**Microsoft Purview** proporciona capacidades para descubrir, clasificar, proteger y gobernar datos distribuidos entre diferentes sistemas.

Permite abordar aspectos como:

- Gobierno de datos.
- Clasificación de información.
- Seguridad de datos.
- Cumplimiento.
- Descubrimiento de activos de datos.
- Administración del ciclo de vida de la información.

---

# Resumen

## Tipos de datos

```text
Estructurados | Semiestructurados | No estructurados
```

## Almacenamiento

```text
Archivos | Bases de datos
```

## Formatos

```text
CSV | JSON | XML | BLOB | Parquet | Avro | Delta Lake
```

## Bases de datos

```text
Relacionales | NoSQL
```

## Procesamiento

```text
OLTP | OLAP
```

## Integración

```text
ETL | ELT
```

## Plataformas analíticas

```text
Data Lake | Data Warehouse | Data Lakehouse
```

## Arquitectura Medallion

```text
Bronze -> Silver -> Gold
```

## Ecosistema Microsoft

```text
Microsoft Fabric | Azure Databricks | Microsoft Purview | Power BI
```

---

# Idea clave

> Los sistemas **OLTP** están optimizados para registrar y modificar las operaciones del negocio, mientras que los sistemas **OLAP** están optimizados para analizar grandes volúmenes de información y ayudar a tomar decisiones.

Una arquitectura moderna de datos puede integrar ambos mundos:

```text
Aplicaciones / OLTP
        |
     ETL / ELT
        |
Data Lake / Lakehouse
        |
Data Warehouse / Modelo semántico
        |
Power BI / Analytics / AI
```
