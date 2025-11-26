# sentinel-doc-engine (Work in Progress)
**Mini Sistema de Gestión Documental (SGD) con Clean Architecture & Vertical Slices**

**Demostración técnica de POO avanzada, SOLID, patrones de diseño, seguridad y arquitectura moderna en C#.**

## 📘 **Descripción del Proyecto**

**AURUM** es una implementación compacta pero técnicamente profunda de un **Sistema de Gestión Documental (SGD)** diseñado especialmente como _caso de estudio profesional_.

El objetivo del proyecto es demostrar habilidades avanzadas en:

- **Programación Orientada a Objetos (POO)**    
- **Patrones de diseño (Repository, Factory, Specification opcional, Auditoría)**    
- **Principios SOLID**    
- **Arquitectura Limpia (Clean Architecture)**    
- **Vertical Slice Architecture (CQRS + MediatR)**    
- **Persistencia en EF Core**    
- **Seguridad basada en permisos granulares**    
- **Cifrado de documentos**    
- **Background Jobs**    
- **Dominio complejo modelado con DDD Lite**    

Este proyecto representa una adaptación reducida —**sin información privada ni código protegido por NDA**— de funcionalidades clave que típicamente se encuentran en sistemas documentales empresariales reales, como aquellos desarrollados en entornos corporativos.

---

## 🎯 **Objetivo del Proyecto**

Construir una aplicación pequeña pero _arquitectónicamente exigente_, donde cada funcionalidad refleja decisiones de ingeniería realmente utilizadas en sistemas de misión crítica:

- Documentos polimórficos con comportamiento especializado    
- Permisos y seguridad a nivel de documento    
- Comentarios visibles solo para usuarios autorizados    
- Cifrado AES sobre documentos sensibles    
- Ciclo de vida documental    
- Auditoría completa de cada cambio    
- Alarmas y notificaciones programadas    
- Búsquedas avanzadas con filtros combinados    

El foco es **calidad sobre cantidad**: pocas features, pero muy bien ejecutadas.

---

## 🧠 **Características Principales**

### 🟩 **1. Tipos de Documentos (Polimorfismo en acción)**

El sistema define una jerarquía basada en una clase abstracta `DocumentBase`:
- `EncryptedDocument` – contenido cifrado AES    
- `IndicacionDocument` – incluye fechas límite y alarmas    
- `NotaDocument` – permite comentarios con visibilidad restringida    

### 🟩 **2. Seguridad Granular**

Cada documento permite compartirlo con accesos:
- `SoloVer`    
- `VerYEditar`    
- `Admin`    

Cada operación valida permisos antes de ejecutarse.

### 🟩 **3. Comentarios con visibilidad controlada**

Un comentario puede tener:
- Autor    
- Contenido    
- Usuarios autorizados a leerlo    

### 🟩 **4. Auditoría Automática**

Cada modificación genera una entrada de auditoría que registra:
- Usuario responsable    
- Timestamp    
- Acción realizada    
- Cambios relevantes    

Implementado mediante **EF Core Interceptors**.

### 🟩 **5. Alarmas automáticas**

Para documentos “Indicacion”, un background job revisa fechas de vencimiento y genera alertas.

### 🟩 **6. Búsqueda avanzada**

Endpoint de búsqueda que soporta múltiples filtros combinados:
- Tipo de documento    
- Fecha    
- Estado    
- Usuario    
- Si está cifrado o no    

---
## 🏗️ **Arquitectura General**

AURUM sigue una arquitectura híbrida:

### 🟦 **Clean Architecture**

Separación estricta entre:
- Domain    
- Application    
- Infrastructure    
- API    

### 🟧 **Vertical Slice Architecture (CQRS + MediatR)**

Cada caso de uso es completamente autónomo:
- Su propio Command/Query    
- Su propio Handler    
- Su propio Validator    
- Su propia lógica    
- Su propio endpoint    

Esto facilita la mantenibilidad y escalabilidad.

---
## 🗂️ **Estructura de Carpetas (Propuesta Final)**

```
/src
 ├── Aurum.API
 │    ├── Program.cs
 │    ├── Configuration/
 │    ├── Endpoints/
 │    │     ├── Documents/
 │    │     ├── Comments/
 │    │     └── Alarms/
 │    └── Filters/
 │
 ├── Aurum.Application
 │    ├── Common/
 │    │     ├── Interfaces/
 │    │     ├── Exceptions/
 │    │     └── Behaviors/
 │    │
 │    ├── Documents/
 │    │     ├── Create/
 │    │     │     ├── CreateDocumentCommand.cs
 │    │     │     ├── CreateDocumentValidator.cs
 │    │     │     └── CreateDocumentHandler.cs
 │    │     ├── Share/
 │    │     ├── Encrypt/
 │    │     └── Search/
 │    │
 │    ├── Comments/
 │    │     └── Add/
 │    │           ├── AddCommentCommand.cs
 │    │           ├── AddCommentValidator.cs
 │    │           └── AddCommentHandler.cs
 │    │
 │    ├── Alarms/
 │    │     └── Trigger/
 │    │           ├── TriggerAlarmCommand.cs
 │    │           └── TriggerAlarmHandler.cs
 │    │
 │    └── Services/
 │          ├── Encryption/
 │          ├── Audit/
 │          └── Permissions/
 │
 ├── Aurum.Domain
 │    ├── Documents/
 │    │     ├── DocumentBase.cs
 │    │     ├── EncryptedDocument.cs
 │    │     ├── IndicacionDocument.cs
 │    │     └── NotaDocument.cs
 │    │
 │    ├── ValueObjects/
 │    ├── Entities/
 │    │     ├── Comment.cs
 │    │     ├── DocumentPermission.cs
 │    │     ├── AuditEntry.cs
 │    │     └── Alarm.cs
 │    │
 │    ├── Enums/
 │    └── Events/
 │
 └── Aurum.Infrastructure
      ├── Persistence/
      │     ├── AurumDbContext.cs
      │     ├── Configurations/
      │     ├── Repositories/
      │     └── Interceptors/
      │
      ├── Services/
      │     ├── Encryption/
      │     ├── Audit/
      │     └── BackgroundJobs/
      │
      └── DependencyInjection.cs

/tests
 └── UnitTests/
 └── IntegrationTests/
```



---
## 🧪 **Tecnologías Principalmente Utilizadas**

- **.NET 8**    
- **C# 12**    
- **EF Core 8**    
- **MediatR**    
- **FluentValidation**    
- **AES Encryption**    
- **Hosted Services / Quartz.NET**    
- **SQL Server o SQLite**    
- **xUnit / NUnit**    

---

## 🚀 **Casos de Uso Implementados**

Cada uno como vertical slice independiente:
1. **Crear Documento**    
2. **Compartir Documento con permisos**    
3. **Agregar Comentario con visibilidad por usuario**    
4. **Cifrar Documento**    
5. **Alarma automática para documentos de tipo Indicacion**    
6. **Búsqueda avanzada de documentos**

---
## 🧭 **Relación con Caso de Estudio Profesional**

Este proyecto refleja de forma reducida la experiencia adquirida desarrollando un sistema documental empresarial real:
- POO orientada al dominio    
- Seguridad basada en permisos    
- Documentos con múltiples estados y comportamientos    
- Cifrado y confidencialidad    
- Alarmas y notificaciones    
- Auditoría completa    
- Consultas avanzadas con LINQ    

Todo empaquetado en una arquitectura moderna y mantenible.

---

## 📄 **Licencia**

Este proyecto es de uso personal con fines demostrativos.
