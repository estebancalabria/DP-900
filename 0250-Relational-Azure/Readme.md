# Azure Relational Database Services

Azure ofrece múltiples servicios de bases de datos relacionales para ejecutar motores como **SQL Server, MySQL y PostgreSQL** en la nube.

La mayoría de estos servicios son **PaaS (Platform as a Service)** y ofrecen capacidades administradas como:

- Alta disponibilidad.
- Escalabilidad.
- Backups automáticos.
- Actualizaciones y parches.
- Monitoreo.
- Seguridad y detección de amenazas.
- Recuperación ante fallos.

---

# Azure SQL

**Azure SQL** es el nombre de la familia de servicios de Azure basados en Microsoft SQL Server.

Las principales opciones son:

1. **SQL Server on Azure Virtual Machines**
2. **Azure SQL Managed Instance**
3. **Azure SQL Database**

## Comparación

| Característica | SQL Server on Azure VM | Azure SQL Managed Instance | Azure SQL Database |
|---|---|---|---|
| Modelo | IaaS | PaaS | PaaS |
| Compatibilidad con SQL Server | Completa | Casi 100% | Capacidades principales |
| Administración del SO | Cliente | Microsoft | Microsoft |
| Administración de SQL Server | Cliente | Microsoft | Microsoft |
| Backups y actualizaciones | Cliente | Automáticos | Automáticos |
| Varias bases por instancia | ✅ | ✅ | Se administran como bases individuales o pools |
| Caso típico | Lift-and-shift con control total | Migración con pocos cambios | Nuevas aplicaciones cloud |

Una forma sencilla de recordarlo:

**Más control**

`SQL Server on Azure VM → SQL Managed Instance → SQL Database`

**Más administración por parte de Azure**

`SQL Server on Azure VM ← SQL Managed Instance ← SQL Database`

---

# SQL Server on Azure Virtual Machines

Permite ejecutar una instalación completa de **SQL Server dentro de una máquina virtual de Azure**.

Es una solución **IaaS**.

La arquitectura es similar a ejecutar SQL Server en un servidor físico o virtual on-premises:

```text
Azure Virtual Machine
├── Operating System
└── SQL Server
    ├── Database 1
    ├── Database 2
    └── Database 3
```

El cliente mantiene control administrativo sobre:

- Sistema operativo.
- SQL Server.
- Configuración.
- Actualizaciones.
- Backups.
- Seguridad.
- Recursos de la VM.

## Cuándo utilizarlo

Es especialmente útil para:

- Migraciones **lift-and-shift**.
- Aplicaciones que requieren compatibilidad completa con SQL Server.
- Sistemas que necesitan acceso al sistema operativo.
- Aplicaciones que necesitan características que no están disponibles en servicios PaaS.
- Implementaciones híbridas entre on-premises y Azure.
- Entornos de desarrollo y testing.

### Ventaja

Máximo control y compatibilidad.

### Desventaja

Mayor carga administrativa.

---

# Azure SQL Managed Instance

**Azure SQL Managed Instance** es un servicio **PaaS** que proporciona una instancia administrada de SQL Server.

Ofrece una experiencia muy similar a una instancia tradicional de SQL Server, pero Azure administra gran parte de la infraestructura.

Una instancia puede contener múltiples bases de datos:

```text
Azure SQL Managed Instance
├── Database 1
├── Database 2
└── Database 3
```

Microsoft administra tareas como:

- Instalación.
- Patching.
- Backups.
- Alta disponibilidad.
- Monitoreo.
- Recuperación.
- Infraestructura subyacente.

El cliente continúa controlando aspectos como:

- Bases de datos.
- Seguridad.
- Logins.
- Asignación de recursos.

## Compatibilidad

Ofrece **casi 100% de compatibilidad con SQL Server Enterprise Edition**.

Por este motivo es especialmente útil para migraciones de SQL Server existentes.

## Cuándo utilizarlo

Es una buena opción cuando:

- Se quiere migrar una instancia completa de SQL Server.
- Existen múltiples bases de datos relacionadas.
- La aplicación utiliza características a nivel de instancia.
- Se quieren minimizar los cambios en aplicaciones existentes.
- Se quiere evitar administrar una máquina virtual.

### Ventaja

Alta compatibilidad con SQL Server con mucha menos administración.

---

# Azure SQL Database

**Azure SQL Database** es un servicio completamente administrado **PaaS**, diseñado específicamente para aplicaciones cloud.

Microsoft administra:

- Infraestructura.
- Sistema operativo.
- SQL Server.
- Patching.
- Backups.
- Alta disponibilidad.
- Escalabilidad.

El usuario se concentra principalmente en:

```text
Database
├── Tables
├── Views
├── Stored Procedures
├── Users
└── Data
```

Es especialmente adecuado para **nuevas aplicaciones cloud**.

---

# Logical Server

En Azure SQL Database, el servidor es un **servidor lógico**, no una máquina virtual ni una instancia tradicional de SQL Server.

Funciona como punto administrativo para:

- Bases de datos.
- Logins.
- Firewall rules.
- Auditing.
- Threat detection.
- Failover groups.

---

# Single Database

Permite crear una base de datos independiente con recursos propios.

```text
Azure SQL Logical Server
└── Database
```

Los recursos pueden escalar de acuerdo con las necesidades de la aplicación.

También puede utilizarse el modelo **serverless**.

En serverless, Azure puede ajustar automáticamente los recursos de cómputo según la demanda y, dependiendo de la configuración y el nivel de servicio, pausar la base cuando permanece inactiva.

Es útil para workloads con uso variable o intermitente.

---

# Elastic Pool

Un **Elastic Pool** permite que múltiples Azure SQL Databases compartan un conjunto de recursos.

```text
Elastic Pool
├── Database A
├── Database B
├── Database C
└── Shared Resources
```

Es especialmente útil cuando las bases de datos tienen picos de consumo en diferentes momentos.

Ejemplo:

```text
Payroll DB
    ↓
Alta utilización a fin de mes

Reporting DB
    ↓
Alta utilización a mitad de mes
```

Al compartir recursos mediante un Elastic Pool se puede mejorar su utilización y reducir costos.

---

# Hyperscale

**Hyperscale** es un nivel de servicio de Azure SQL Database diseñado para bases de datos muy grandes.

Características:

- Bases de datos de hasta **128 TB**.
- Escalabilidad rápida.
- Backups rápidos.
- Restauraciones rápidas independientemente del tamaño de los datos.

---

# Seguridad en Azure SQL Database

Azure SQL Database proporciona múltiples mecanismos de seguridad.

## Encryption

Protección de datos:

- **At rest**: datos almacenados.
- **In motion**: datos transmitidos por la red.

## Microsoft Defender for SQL

Proporciona capacidades como:

- Vulnerability Assessment.
- Advanced Threat Protection.
- Detección de comportamientos anómalos.
- Detección de posibles ataques SQL Injection.
- Alertas de seguridad.

## Auditing

Permite registrar eventos de la base de datos para:

- Compliance.
- Investigación de incidentes.
- Monitoreo.
- Detección de actividad sospechosa.

## Point-in-Time Restore

Permite restaurar una base de datos a un estado anterior dentro del período de retención disponible.

---

# SQL Server migration: ¿qué servicio elegir?

Una regla práctica:

### Necesito control total

```text
SQL Server on Azure VM
```

### Necesito alta compatibilidad pero quiero evitar administrar el servidor

```text
Azure SQL Managed Instance
```

### Estoy desarrollando una nueva aplicación cloud

```text
Azure SQL Database
```

---

# Azure Database for MySQL

**Azure Database for MySQL** es una implementación **PaaS** de MySQL basada en **MySQL Community Edition**.

Microsoft administra la infraestructura subyacente.

Características:

- Alta disponibilidad.
- Failover automático.
- Escalabilidad.
- Backups automáticos.
- Point-in-time restore.
- Seguridad.
- Firewall.
- TLS.
- Monitoring.
- Métricas y logs.
- Pago según uso.

Los backups permiten **point-in-time restore de hasta 35 días**, con **7 días por defecto**.

---

# Azure Database for MySQL Flexible Server

**Flexible Server** proporciona mayor control sobre la configuración del servidor manteniendo el modelo administrado.

Está diseñado para ofrecer:

- Mayor flexibilidad.
- Configuración granular.
- Alta disponibilidad.
- Optimización de costos.
- Escalabilidad.

---

# Azure Database for PostgreSQL

**Azure Database for PostgreSQL** es una implementación **PaaS** administrada de PostgreSQL.

Ofrece características similares al servicio de MySQL:

- Alta disponibilidad.
- Failover.
- Escalabilidad.
- Seguridad.
- Backups.
- Monitoring.
- Administración de infraestructura por parte de Microsoft.

Azure admite un conjunto de extensiones de PostgreSQL, aunque no todas las extensiones disponibles en instalaciones on-premises están soportadas.

---

# Azure Database for PostgreSQL Flexible Server

**Flexible Server** proporciona PostgreSQL como servicio completamente administrado con mayor control sobre:

- Configuración.
- Recursos.
- Rendimiento.
- Alta disponibilidad.
- Costos.

---

# pgAdmin

Los administradores de PostgreSQL pueden continuar utilizando **pgAdmin** para conectarse y administrar Azure Database for PostgreSQL.

Sin embargo, algunas operaciones relacionadas con la administración del servidor subyacente no están disponibles porque esa infraestructura es administrada por Microsoft.

---

# Query Store en PostgreSQL

Azure Database for PostgreSQL puede registrar información sobre las consultas ejecutadas utilizando **Query Store**.

La información se almacena en la base:

`azure_sys`

y puede consultarse mediante:

`query_store.qs_view`

Esto permite analizar las consultas y detectar oportunidades de optimización de rendimiento.

---

# Comparación general

| Servicio | Modelo | Motor | Caso de uso principal |
|---|---|---|---|
| SQL Server on Azure VM | IaaS | SQL Server | Lift-and-shift y control total |
| Azure SQL Managed Instance | PaaS | SQL Server | Migraciones con alta compatibilidad |
| Azure SQL Database | PaaS | SQL Server | Aplicaciones cloud modernas |
| Azure Database for MySQL | PaaS | MySQL | Aplicaciones basadas en MySQL |
| Azure Database for PostgreSQL | PaaS | PostgreSQL | Aplicaciones basadas en PostgreSQL |

---

# Idea clave

Azure permite elegir cuánto de la infraestructura queremos administrar.

```text
Más control
    ↓
SQL Server on Azure VM
    ↓
Azure SQL Managed Instance
    ↓
Azure SQL Database
    ↓
Más administración automática
```

En general:

- **VM** → máximo control.
- **Managed Instance** → máxima compatibilidad con SQL Server dentro de PaaS.
- **SQL Database** → simplicidad y aplicaciones cloud.
- **MySQL Flexible Server** → MySQL administrado.
- **PostgreSQL Flexible Server** → PostgreSQL administrado.

La elección depende principalmente de la **compatibilidad requerida, el nivel de control necesario y cuánto trabajo administrativo se quiere delegar a Azure**.
