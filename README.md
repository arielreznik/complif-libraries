# Repositorio de Templates - Complif Platform

## Descripción

Este repositorio centraliza la gestión de templates utilizados en la plataforma Complif. Los templates se organizan por categorías y se mantienen como archivos individuales YAML que reflejan la estructura de las tablas de base de datos.

**Estado del Proyecto**: ✅ **Completamente migrado y estructurado** - 64 templates organizados en 4 categorías principales.

## Estructura del Repositorio

```
├── cases/                          # 17 Templates de casos
│   ├── *.yaml                     # Archivos individuales por caso
│   └── cases_templates_*.csv      # Datos de respaldo CSV
├── emails/                         # 9 Templates de emails  
│   ├── *.yaml                     # Archivos individuales por email
│   └── email_templates_*.csv      # Datos de respaldo CSV
├── pdfs/                          # 26 Templates de PDFs
│   ├── *.yaml                     # Archivos individuales por PDF
│   └── pdf_templates_*.csv        # Datos de respaldo CSV
├── requirements/                   # 12 Templates de requerimientos
│   ├── *.yaml                     # Archivos individuales por requirement
│   └── requirement_templates_*.csv # Datos de respaldo CSV
├── scripts/                       # Scripts de utilidad (vacío - tareas completadas)
├── docs/                          # Documentación adicional
├── env.example                    # Ejemplo de variables de entorno
└── README.md                      # Este archivo
```

## Tipos de Templates

### Cases (17 Templates)
Templates para la creación de casos en el sistema:
- **Coincidencias de monitoreo**: Lista negra, PEP, sanciones
- **Vencimientos**: Documentos, cuentas, legajos
- **Gestión de legajos**: Datos faltantes, actualizaciones
- **Operaciones**: Inusuales, grandes volúmenes

**Estructura**: `name`, `code`, `config`, `description`, `entity`, `metadata`

### Emails (9 Templates)
Templates de emails para comunicaciones automatizadas:
- **Invitaciones**: Asesores, magic links
- **Notificaciones**: Sistema, recordatorios
- **Comunicaciones**: Clientes, bienvenidas

**Estructura**: `name`, `code`, `config`, `description`, `entity`, `footer`, `header`, `html`
- HTML formateado con BeautifulSoup para mejor legibilidad

### PDFs (26 Templates)
Templates para generación de documentos PDF:
- **Reportes**: Análisis, estadísticas
- **Formularios**: Legales, administrativos
- **Certificaciones**: Documentos oficiales

**Estructura**: `name`, `code`, `config`, `description`, `entity`, `footer`, `header`, `html`
- HTML complejo para renderizado de PDFs

### Requirements (12 Templates)
Templates para solicitudes de información:
- **FORM_LEGAL_TEXT** (7): Formularios con schemas de validación
- **FILE_UPLOAD** (5): Solicitudes de documentación
- **LEGAL_TEXT** (1): Textos legales con variables dinámicas

**Estructura**: `name`, `type`, `subtitle`, `video_link`, `advisor_action`, `autocomplete_with_actual_data`, `automatic_approval`, `category`, `code`, `config`, `description`, `expiration_days`, `instructions`

**Configs especiales**:
- JSON indentado dentro de YAML para fácil lectura
- Schemas de validación JSON Schema
- UI Schemas para formularios
- Sistemas de scoring complejos
- Variables dinámicas: `{{person.name}}`, `{{accounts.0.number}}`

## Configuración de Templates

### Requirements - Configuraciones Avanzadas

#### FORM_LEGAL_TEXT
```yaml
config: |
  {
    "legal_text": "Texto con variables {{person.name}}",
    "schema": {
      "type": "object",
      "properties": { ... },
      "required": [...]
    },
    "ui_schema": {
      "type": "VerticalLayout",
      "elements": [...]
    }
  }
```

#### Encuesta Perfil de Inversor
Sistema complejo de scoring con matrices de evaluación:
```yaml
config: |
  {
    "matriz": {
      "respuestas": { ... },
      "resultados": [
        {"perfil": "CONSERVADOR", "puntaje": 19},
        {"perfil": "MODERADO", "puntaje": 49},
        {"perfil": "ARRIESGADO", "puntaje": 1000}
      ]
    },
    "schema": { ... },
    "ui_schema": { ... }
  }
```

## Configuración Inicial

### 1. Instalar Dependencias

```bash
pip install -r requirements.txt
```

### 2. Configurar Variables de Entorno

```bash
cp env.example .env
# Editar .env con las credenciales reales de la base de datos
```

Variables requeridas:
- `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`

## Características del Proyecto

### ✅ Migración Completa
- **64 templates** migrados exitosamente desde CSV a YAML
- **Estructura unificada** para todas las categorías
- **Validación completa** de datos y formatos
- **Encoding UTF-8** para caracteres especiales

### ✅ Formato Optimizado
- **HTML formateado** con BeautifulSoup para emails y PDFs
- **JSON indentado** en configs de requirements para legibilidad
- **Estructura consistente** en todos los templates
- **Variables dinámicas** preservadas

### ✅ Datos de Respaldo
- **Archivos CSV originales** mantenidos en cada directorio
- **Historial completo** de migración documentado
- **Validación cruzada** entre YAML y CSV

## Flujo de Trabajo

### Para Desarrolladores
1. **Consultar**: Revisar archivos YAML para estructura
2. **Modificar**: Editar templates manteniendo formato
3. **Validar**: Verificar sintaxis YAML
4. **Documentar**: Actualizar cambios realizados

### Para Product Managers
1. **Actualizar contenido**: Modificar textos en archivos YAML
2. **Nuevos templates**: Seguir estructura establecida
3. **Variables dinámicas**: Usar formato `{{variable.field}}`
4. **Configs complejos**: Mantener formato JSON indentado

## Ejemplos de Uso

### Template de Email
```yaml
name: "Invitación Asesor - Onboarding"
code: COMPLIF_ADVISOR_INVITATION_ONBOARDING_ES
html: |
  <div style="font-family: Arial, sans-serif;">
    <h1>Bienvenido {{advisor.name}}</h1>
    <p>Has sido invitado a la plataforma Complif.</p>
  </div>
```

### Template de Requirement
```yaml
name: "Actualización datos de contacto"
type: FORM_LEGAL_TEXT
config: |
  {
    "legal_text": "Email: **{{submission.data.form.email}}**\nTeléfono: **{{submission.data.form.telefono}}**",
    "schema": {
      "properties": {
        "email": {"type": "string", "format": "email"},
        "telefono": {"type": "string"}
      }
    }
  }
```

## Consideraciones Técnicas

### Formato YAML
- **Indentación**: 2 espacios
- **Strings**: Comillas dobles para textos con caracteres especiales
- **Multiline**: Usar `|` para contenido HTML/JSON

### Variables Dinámicas
- `{{person.name}}` - Nombre de la persona
- `{{accounts.0.number}}` - Número de cuenta
- `{{submission.data.form.field}}` - Datos del formulario
- `{{current_date}}` - Fecha actual

### JSON Configs
- **Indentación**: 2 espacios dentro de YAML
- **Encoding**: UTF-8 sin escape ASCII
- **Validación**: JSON Schema para formularios

## Estado del Proyecto

| Categoría | Templates | Estado | Características |
|-----------|-----------|--------|-----------------|
| Cases | 17 | ✅ Completo | Configs estructurados, metadatos |
| Emails | 9 | ✅ Completo | HTML formateado, headers/footers |
| PDFs | 26 | ✅ Completo | HTML complejo, renderizado |
| Requirements | 12 | ✅ Completo | JSON indentado, schemas complejos |

## Contacto

Para consultas técnicas o de producto, contactar al equipo de desarrollo de Complif.

---

**Última actualización**: 2025-06-18  
**Versión**: 3.0.0  
**Estado**: ✅ Proyecto completado - 64 templates migrados y estructurados 