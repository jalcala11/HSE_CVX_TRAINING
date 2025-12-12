# Documentación - Portal de Entrenamientos Demo

## 📋 Descripción General

**Demo Trainings** es un portal web de gestión de entrenamientos diseñado para mostrar y filtrar cursos de capacitación en facilidades petroleras. La aplicación implementa un modelo de **Single Page Application (SPA)** permitiendo navegación fluida entre la vista de lista de cursos y la vista de detalles sin recargar la página.

**Idioma:** Español  
**Plataforma de hosting:** Google Sites (iframe embebido)  
**Archivo principal:** `inicio.html` (513 líneas)

---

## 🏗️ Arquitectura y Estructura

### Estructura de Carpetas
```
HSE Training Portal/
├── inicio.html          # Archivo principal (HTML + CSS + JavaScript combinado)
├── Home.html            # [Deprecado - eliminado]
├── Details.html         # [Deprecado - eliminado]
└── DOCUMENTACION.md     # Este archivo
```

### Estructura del Documento HTML

```
inicio.html
├── <head>
│   ├── Meta tags (charset, viewport)
│   ├── Title: "Demo Trainings"
│   └── <style> (CSS embebido)
├── <body>
│   ├── #page-home (página de lista de cursos)
│   │   ├── .top-header
│   │   ├── .main-nav
│   │   ├── .content-wrapper
│   │   │   ├── .filters-sidebar (filtros dinámicos)
│   │   │   └── .products-container (lista de cursos)
│   │   └── <script> (JavaScript embebido)
│   └── #page-details (página de detalles del curso)
│       ├── .breadcrumb
│       ├── .course-header
│       └── .content-grid
```

---

## 🎨 Tecnología Utilizada

### Frontend Stack
- **HTML5:** Estructura semántica
- **CSS3:** Diseño responsivo (sin framework)
- **JavaScript ES6+:** Lógica de negocio y DOM manipulation
- **Métodos moderno:**
  - `Array.from()`, `Set()` para manipulación de datos
  - Template literals (backticks) para HTML dinámico
  - `classList` para gestión de clases
  - Event delegation con `onchange` y `oninput`

### Características de No-Dependencias
- ✅ Cero frameworks (vanilla JavaScript)
- ✅ Cero librerías externas
- ✅ CSS puro (sin preprocessadores)
- ✅ Archivo único autocontenido

---

## 📊 Estructura de Datos

### Objeto `coursesData`

```javascript
const coursesData = {
    'ID-del-curso': {
        id: string,                    // ID único (e.g., 'FE1-101')
        title: string,                 // Título del curso
        category: string,              // Categoría (FE, SS, HSE, Liderazgo)
        program: string,               // Programa completo
        programCode: string,           // Código del programa (e.g., 'FE1 100')
        format: string,                // Formato ('Presencial')
        duration: string,              // Duración (e.g., '8 horas')
        courseCode: string,            // Código del curso (e.g., 'FE1 101')
        overview: string[],            // Descripción general
        learnings: string[],           // Qué se aprenderá
        whoShouldAttend: string[],     // A quién está dirigido
        prerequisites: string,         // Requisitos previos
        topics: string[] | object[],   // Temas (strings simples o {day, topic})
        instructor: {
            name: string,
            bio: string
        },
        tags: string[]                 // Etiquetas
    }
}
```

### Categorías Disponibles
- **Facilidades de Superficie (FE):** FE1, FE2
- **Subsuelo (SS):** SS
- **HSE:** HSE
- **Liderazgo (LW):** LW

### Cursos Actualmente Disponibles (10 cursos)
1. FE1-101: Separadores
2. FE1-104: Válvulas de Control
3. FE1-108: Instrumentación
4. FE2-101: Bombas
5. SS-101: Sistemas de Levantamiento Artificial
6. SS-103: Cabezales de Pozo
7. HSE-101: Conceptos Básicos Excelencia Operacional
8. HSE-103: Análisis de Riesgo en el Trabajo (A.R.T)
9. HSE-105: Trabajo en Altura
10. LW-101: Leadership Workshop Phase 1

---

## 🎯 Funcionalidades Principales

### 1. **Navegación SPA (Single Page Application)**
- **`showHome()`:** Alterna a la vista de lista de cursos
- **`showDetails(courseId)`:** Alterna a la vista de detalles del curso

**Mecanismo:** Usa `classList.add('active')` y `classList.remove('active')` en elementos con ID `page-home` y `page-details`

**Ventaja:** No requiere recarga de página, funciona dentro de iframe de Google Sites

### 2. **Renderización Dinámica de Cursos**
```javascript
function renderCourses()
```
- Itera sobre `coursesData` con `Object.values()`
- Crea tarjetas (cards) dinámicamente con `createElement()`
- Asigna atributos `data-category` y `data-format` para filtrado
- Actualiza contador total de cursos

**Resultado:** 10 tarjetas mostrando título, categoría, duración y botón "Ver Detalles"

### 3. **Filtros Dinámicos**
```javascript
function renderFilters()
```
- Extrae categorías únicas con `Set()`
- Genera checkboxes dinámicamente basado en datos reales
- IDs generados automáticamente: `filter-facilidades-de-superficie`, `filter-subsuelo`, etc.
- Contadores actualizados al cargar: `(4)`, `(2)`, etc.

**Ventaja:** Si se agrega una nueva categoría en los datos, aparece automáticamente

### 4. **Sistema de Filtrado**
```javascript
function applyFilters()
```
- Captura checkboxes seleccionados en `#category-filters`
- Muestra/oculta tarjetas comparando `dataset.category`
- Actualiza contador de resultados visibles
- Lógica: Si no hay filtros seleccionados, muestra todos los cursos

### 5. **Búsqueda en Tiempo Real**
```javascript
function searchCourses()
```
- Captura entrada del usuario en `#search-input` con `oninput`
- Búsqueda case-insensitive (`toLowerCase()`)
- Busca en título del curso y metadata (categoría, duración)
- Limpia filtros automáticamente cuando hay búsqueda activa

### 6. **Vista de Detalles Dinámica**
```javascript
function showDetails(courseId)
```
- Obtiene curso desde `coursesData` por ID
- Rellena 8 secciones dinámicamente:
  1. Descripción del Curso
  2. Lo que Aprenderás
  3. A Quién Está Dirigido
  4. Prerequisitos
  5. Temas del Curso (oculta si está vacía)
  6. Información del Curso (duración, formato, código, programa)
  7. Instructor
  8. Horarios (actualmente vacío)

- Maneja dos formatos de `topics`:
  - Strings simples: `"Tema 1"` → `<li>Tema 1</li>`
  - Objetos complejos: `{day: "Día 1", topic: "..."}` → `<li><strong>Día 1:</strong> ...</li>`

### 7. **Contadores Dinámicos**
```javascript
function updateFilterCounts()
```
- Cuenta cursos por categoría desde el DOM actual
- Genera ID dinámico: `filter-` + categoría en minúsculas
- Actualiza elemento: `<span id="count-filter-facilidades-de-superficie">(4)</span>`

---

## 🎨 Diseño y Estilos (CSS)

### Paleta de Colores
| Color | Código | Uso |
|-------|--------|-----|
| Azul Oscuro | `#003366` | Headers, títulos, texto principal |
| Azul Medio | `#0066cc` | Enlaces, botones primarios |
| Azul Oscuro 2 | `#1a2c4d` | Navegación, headers |
| Gris | `#666` | Texto secundario |
| Gris Claro | `#f5f5f5` | Fondo general |
| Blanco | `#ffffff` | Tarjetas, fondos principales |
| Azul Claro Fondo | `#e6f2ff` | Etiquetas (tags) |

### Componentes CSS Principales

#### **Header**
- `.top-header`: Fondo azul oscuro, búsqueda centrada
- `.logo`: SVG con letras "DT", clickeable para volver a inicio
- `.search-box`: Campo de búsqueda responsive

#### **Navegación**
- `.main-nav`: Barra blanca con enlace "Buscar Entrenamientos"
- `.breadcrumb`: Navegación tipo migas de pan en detalles

#### **Layout Principal**
- `.content-wrapper`: Contenedor max-width 1400px con flexbox
- `.filters-sidebar`: Ancho fijo 280px, posición fija
- `.products-container`: Flex 1 para tomar espacio restante

#### **Filtros**
- `.filter-card`: Tarjeta blanca con shadow
- `.filter-checkbox`: Flex con checkbox, label y contador
- `.clear-filters`: Botón texto azul con hover

#### **Tarjetas de Cursos**
- `.product-card`: Display flex, espacio entre info y botón
- `.product-info`: Título + metadata (categoría, duración)
- `.product-meta`: Iconos y texto de metadata
- `.view-details-btn`: Botón azul con hover oscuro

#### **Vista de Detalles**
- `.course-header`: Información general del curso
- `.course-title`: Título grande (32px)
- `.course-tags`: Etiquetas pequeñas en color azul claro
- `.content-grid`: Grid 2 columnas (2fr 1fr)
- `.main-content`: Columna izquierda con secciones
- `.sidebar-info`: Columna derecha con tarjetas de info
- `.section`: Margenes y títulos con borde azul
- `.info-card`: Tarjeta lateral con información estructurada

### Responsive Design
- Viewport meta tag configurado
- Flexbox para layouts responsive
- Font sizes adaptativos
- Breakpoints implícitos (no media queries pero flexible)

---

## 🔧 Funciones Principales

### Función `renderFilters()`
**Propósito:** Generar filtros dinámicos basados en datos

**Lógica:**
```javascript
1. Extraer categorías únicas → new Set()
2. Ordenar alfabéticamente → Array.from().sort()
3. Para cada categoría:
   - Generar ID: filter-{categoria-minúsculas}
   - Crear div con checkbox + label + contador
   - Agregar al contenedor #category-filters
```

**Triggers:**
- `DOMContentLoaded` event
- Al hacer click "Buscar Entrenamientos" (showHome)

### Función `renderCourses()`
**Propósito:** Generar tarjetas de cursos dinámicamente

**Lógica:**
```javascript
1. Limpiar #courses-list
2. Para cada curso en coursesData:
   - Crear div.product-card
   - Asignar data-category y data-format
   - Generar HTML interno con title, metadata, botón
   - Agregar evento onclick al botón "Ver Detalles"
3. Actualizar contadores
4. Actualizar contador total
```

### Función `applyFilters()`
**Propósito:** Filtrar cursos según selección de checkboxes

**Lógica:**
```javascript
1. Capturar checkboxes checked en #category-filters
2. Extraer valores en array selectedCategories
3. Para cada tarjeta de curso:
   - Si selectedCategories vacío → mostrar todas
   - Si curso.category está en selectedCategories → mostrar
   - Sino → ocultar
4. Contar y actualizar resultCount
```

### Función `searchCourses()`
**Propósito:** Búsqueda en tiempo real

**Lógica:**
```javascript
1. Obtener valor de #search-input
2. Convertir a minúsculas
3. Para cada tarjeta de curso:
   - Extraer título y metadata
   - Si incluye searchTerm → mostrar
   - Sino → ocultar
4. Actualizar contador
5. Si hay búsqueda → limpiar filtros (unchecked)
```

**Nota:** La búsqueda tiene prioridad sobre filtros. Cuando escribes algo, se ignoran los checkboxes.

### Función `showDetails(courseId)`
**Propósito:** Mostrar vista de detalles del curso

**Lógica:**
```javascript
1. Buscar curso en coursesData por ID
2. Rellena 8 elementos dinámicamente:
   - Título, breadcrumb, metadata, tags
   - 5 secciones de contenido (overview, learnings, etc.)
   - Card de información
   - Card de instructor
3. Manejo especial para topics (strings vs objetos)
4. Ocultar sección topics si está vacía
5. Cambiar visibilidad: hide #page-home, show #page-details
6. Scroll a top
```

### Función `clearAllFilters()`
**Propósito:** Limpiar todos los checkboxes

**Lógica:**
```javascript
1. Desseleccionar todos los checkboxes
2. Llamar applyFilters() para refrescar vista
```

### Función `showHome()`
**Propósito:** Volver a la vista de lista

**Lógica:**
```javascript
1. Mostrar #page-home
2. Ocultar #page-details
3. Re-renderizar cursos (renderCourses)
4. Scroll a top
```

### Función `updateFilterCounts()`
**Propósito:** Actualizar contadores de filtros

**Lógica:**
```javascript
1. Obtener todas las tarjetas visibles
2. Para cada tarjeta:
   - Extraer categoría desde data-category
   - Incrementar contador para esa categoría
3. Actualizar elementos <span> con IDs count-*
```

---

## 🔄 Flujo de Ejecución

### Al Cargar la Página

```
DOMContentLoaded
├── renderFilters()
│   ├── Extraer categorías únicas de coursesData
│   └── Generar checkboxes dinámicamente
└── renderCourses()
    ├── Generar tarjetas de cursos
    ├── updateFilterCounts()
    │   └── Actualizar contadores (4), (2), (2), (2)
    └── Actualizar resultCount: "10 cursos disponibles"
```

### Al Seleccionar un Filtro

```
usuario hace click en checkbox
├── onchange="applyFilters()"
└── applyFilters()
    ├── Capturar checkbox seleccionados
    ├── Filtrar tarjetas (show/hide)
    └── Actualizar resultCount
```

### Al Buscar

```
usuario escribe en search box
├── oninput="searchCourses()"
├── searchCourses()
│   ├── Filtrar por título/metadata
│   ├── Limpiar checkboxes
│   └── Actualizar resultCount
```

### Al Hacer Clic en "Ver Detalles"

```
usuario hace click en botón
├── onclick="showDetails(courseId)"
├── showDetails(courseId)
│   ├── Buscar curso en coursesData
│   ├── Rellenar 8 secciones
│   ├── Cambiar visibilidad (SPA)
│   └── Scroll a top
```

### Al Hacer Clic en "Volver"

```
usuario hace click en link
├── onclick="showHome()"
├── showHome()
│   ├── Cambiar visibilidad (SPA)
│   ├── renderCourses()
│   └── Scroll a top
```

---

## 📱 Características de Responsividad

### Mobile-First Approach
- Viewport meta tag: `width=device-width, initial-scale=1.0`
- Flexbox para layouts adaptativos
- Font sizes escalables
- Padding/margin proporcionales

### Componentes Responsive
- **Header:** Logo + search en una fila con gaps
- **Sidebar:** Ancho fijo (280px), podría añadirse media query
- **Content:** Flex grow para ocupar espacio disponible
- **Grid:** 2 columnas en desktop, podría ser 1 en mobile con media query

**Nota:** No hay media queries implementadas actualmente. Se recomienda agregar para pantallas < 768px

---

## 🐛 Manejo de Errores

### Validación en `showDetails()`
```javascript
if (!course) { 
    console.error('Curso no encontrado'); 
    return; 
}
```

### Manejo de Topics Flexible
```javascript
course.topics && course.topics.length > 0 
    ? renderizar topics 
    : ocultar sección
```

### Renderización Segura
- Comprueba existencia de elementos antes de actualizar
- Usa `textContent` en lugar de `.innerHTML` donde es posible (seguridad XSS)

---

## 📈 Rendimiento y Optimizaciones

### ✅ Ventajas Actuales
- **Cero dependencias externas:** Tiempo de carga rápido
- **Código embebido:** Una única petición HTTP
- **SPA:** No requiere recarga de página
- **Búsqueda en tiempo real:** Respuesta inmediata sin servidor
- **Renderización dinámica:** Escalable para más cursos

### ⚠️ Áreas de Mejora
- **Media queries:** Agregar breakpoints para mobile
- **Lazy loading:** Para imágenes de instructores (si se agregan)
- **Caching:** Guardar coursesData en localStorage
- **Minificación:** Comprimir CSS y JavaScript
- **Caché de búsqueda:** Guardar resultados previos

---

## 🌐 Compatibilidad

### Navegadores Soportados
- ✅ Chrome/Edge (v90+)
- ✅ Firefox (v88+)
- ✅ Safari (v14+)
- ✅ Opera (v76+)

### Características JavaScript Utilizadas
- `Set()` - Soportado en todos los navegadores modernos
- `Array.from()` - Soportado en todos
- Template literals - Soportado en todos
- `classList` - Soportado en todos
- Spread operator - Soportado en todos

---

## 📝 Convenciones de Código

### Nomenclatura
- **IDs HTML:** Kebab-case (`#page-home`, `#courses-list`)
- **Clases CSS:** Kebab-case (`.product-card`, `.filter-checkbox`)
- **Variables JavaScript:** camelCase (`coursesData`, `categoryId`)
- **Funciones:** camelCase (`renderCourses()`, `applyFilters()`)

### Estructura de Datos
- **IDs de cursos:** `{PROGRAMA}-{NÚMERO}` (e.g., `FE1-101`)
- **IDs de filtros:** `filter-{categoria-minúsculas}` (e.g., `filter-facilidades-de-superficie`)
- **Contadores:** `count-{filtro-id}` (e.g., `count-filter-facilidades-de-superficie`)

### Eventos
- **Cambios en filtros:** `onchange="applyFilters()"`
- **Búsqueda:** `oninput="searchCourses()"`
- **Navegación:** `onclick="showHome()"` o `onclick="showDetails('courseId')"`

---

## 🚀 Próximas Mejoras Sugeridas

1. **Agregar más cursos** desde Excel/CSV
2. **Traducción multiidioma** (i18n)
3. **Guardado de preferencias** en localStorage
4. **Sistema de ratings** para cursos
5. **Calendario de horarios** interactivo
6. **Registro de usuarios** y progreso
7. **Certificados digitales**
8. **Notificaciones por email**
9. **Exportar curriculum a PDF**
10. **API backend** para persistencia de datos

---

## 📞 Contacto y Soporte

**Aplicación:** Demo Trainings Portal  
**Versión:** 1.0  
**Última actualización:** Diciembre 2025  
**Estado:** Funcional - Producción

---

## 📄 Licencia

Este proyecto es propiedad de Chevron HSE Training Portal. Uso interno únicamente.
