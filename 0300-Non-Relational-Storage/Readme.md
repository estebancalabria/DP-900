# Azure Storage y Microsoft OneLake: almacenamiento no relacional en la nube

## Introducción
Muchas aplicaciones no requieren la estructura rígida de una base de datos relacional y utilizan almacenamiento no relacional (NoSQL). **Azure Storage** y **Microsoft OneLake** ofrecen distintas opciones para almacenar este tipo de datos en la nube.

## Azure Blob Storage
Servicio para almacenar grandes volúmenes de datos no estructurados (binary large objects o *blobs*).

- Los blobs se almacenan dentro de **contenedores** (unidad de agrupación y control de acceso).
- Autenticación recomendada: **Microsoft Entra ID**, con permisos vía **Azure RBAC**.
- Dentro de un contenedor se pueden crear carpetas virtuales (uso del carácter `/` en el nombre), pero no existen operaciones a nivel carpeta.

### Tipos de blob
| Tipo | Características | Tamaño máximo |
|---|---|---|
| **Block blob** | Conjunto de bloques (hasta 4,000 MiB c/u). Ideal para objetos binarios grandes que cambian poco | 190.7 TiB |
| **Page blob** | Páginas fijas de 512 bytes, optimizado para lectura/escritura aleatoria (discos de VM) | 8 TiB |
| **Append blob** | Solo permite agregar bloques al final (no editar/eliminar) | ~195 GiB |

### Niveles de acceso (access tiers)
| Nivel | Uso | Retención mínima |
|---|---|---|
| **Hot** (por defecto) | Acceso frecuente | — |
| **Cool** | Acceso infrecuente | 30 días |
| **Cold** | Acceso raro pero con recuperación rápida | 90 días |
| **Archive** | Datos históricos, acceso muy raro (rehidratación hasta 15 h) | 180 días |

- Las **políticas de ciclo de vida** mueven automáticamente los blobs entre niveles (Hot → Cool → Cold → Archive) y pueden eliminar blobs obsoletos.

### Redundancia
- **LRS**: 3 copias en un mismo datacenter.
- **ZRS**: copias en 3 zonas de disponibilidad de la región primaria.
- **GRS / GZRS**: replicación asíncrona a una región secundaria.
- **RA-GRS / RA-GZRS**: permiten lectura desde la región secundaria incluso antes de un failover.

## Azure Data Lake Storage Gen2
- Data lake a escala de nube construido sobre Azure Blob Storage.
- Combina escalabilidad/costo de Blob Storage con un **sistema de archivos jerárquico** compatible con sistemas de análisis (p. ej. Azure Databricks).
- Soporta **ACLs estilo POSIX** para permisos granulares, independientes del RBAC.
- Requiere habilitar el **namespace jerárquico** (proceso irreversible una vez activado).
- **OneLake** (Microsoft Fabric) está construido sobre ADLS Gen2.

## Microsoft OneLake (Fabric)
Data lake único, lógico y unificado para toda la organización, provisto automáticamente en cada tenant de Fabric.

**Beneficios clave:**
- **Data lake organizacional único**: reemplaza la proliferación de lagos de datos por área.
- **Propiedad distribuida y colaboración**: mediante *workspaces* por equipo/área.
- **Abierto y compatible**: almacena tablas en formato **Delta Parquet** o **Iceberg**; compatible con APIs/SDKs de ADLS Gen2.
- **Fácil de navegar**: acceso desde Windows vía OneLake File Explorer.

## Azure Files
Shares de archivos en la nube (equivalente a los file shares on-premises), eliminando costos de hardware y mantenimiento.

- Capacidad: hasta **256 TiB** (cuentas SSD, modelo de facturación *provisioned v2*) o **5 PiB** (cuentas HDD *pay-as-you-go*).
- Tamaño máximo por archivo: **4 TiB**.
- Hasta **2,000 handles concurrentes** por archivo/directorio.
- Niveles de medio: **HDD** y **SSD** (mayor throughput, mayor costo).
- Protocolos soportados:
  - **SMB**: multiplataforma (Windows, Linux, macOS).
  - **NFS**: solo clientes Linux; requiere nivel SSD y acceso vía endpoint privado/de servicio en una VNet.
- Sincronización con **Azure File Sync**; carga vía portal o **AzCopy**.

## Azure Table Storage
Almacenamiento NoSQL de tipo clave/valor (cada fila = una entidad).

- No es una tabla relacional: no hay foreign keys, stored procedures ni vistas.
- Cada fila tiene una clave única compuesta por **PartitionKey** + **RowKey**, más una columna de **timestamp** automática.
- Datos típicamente **desnormalizados** (una fila contiene toda la información de una entidad lógica, con número variable de campos).
- **Particionamiento**: agrupa filas por PartitionKey, lo que mejora escalabilidad y rendimiento (menos I/O en las búsquedas).
- Dentro de una partición, las filas se almacenan ordenadas por RowKey, permitiendo *point queries* y *range queries* eficientes.
- Alternativa más moderna: **Azure Cosmos DB for Table** (mismo modelo clave/valor, con mayor rendimiento, disponibilidad global y distribución automática, índices secundarios).

---
*Fuente: módulo de Microsoft Learn sobre Azure Storage y Microsoft OneLake.*
