# Guía del Proyecto — Estado del Arte (Tesis Doctoral)

**Investigador:** Juan Diego Espinosa  
**Institución:** UPV CIGIP  
**Tema de tesis:** Arquitecturas de agentes inteligentes basados en LLMs para la orquestación de procesos industriales de producción

---

## Regla de edición

> ⚠️ **Solo modificar el archivo:** `Clasificación/clasificador_estado_del_arte.html`  
> Existen copias del clasificador con distintos nombres en la misma carpeta. No tocar otros HTMLs.

---

## Fuente de datos

`Estado del Arte.csv` (mismo directorio raíz)  
- Encoding: UTF-8 | Delimitador: `;` | 434 filas de datos  
- Columnas: `Item Type; Publication Year; Author; Title; Publication Title; Url; Abstract Note; Publisher; Archive; Manual Tags`  
- Para reconstruir el array `PAPERS` en el HTML se usa un script PowerShell que lee este CSV

---

## Estructura de carpetas

```
Estado del arte/
├── PROYECTO_GUIA.md                         ← este archivo
├── Estado del Arte.csv                      ← fuente de datos (434 papers)
├── Clasificación/
│   └── clasificador_estado_del_arte.html   ← herramienta principal (ÚNICO archivo a modificar)
└── [otros archivos de revisión bibliográfica]
```

---

## Herramienta principal: `clasificador_estado_del_arte.html`

Aplicación HTML single-file que corre directamente en el navegador (sin servidor).

### Qué hace
- Carga **434 papers** académicos embebidos en el JS (`const PAPERS = [...]`)
- Llama a la API de Anthropic (`claude-sonnet-4-6`) para clasificar cada paper
- Permite clasificación manual y edición de etiquetas mediante desplegables en el modal
- Traducción de abstracts al español mediante la misma API
- Persiste todo en `localStorage` del navegador (clave: `thesis_db_v2`)
- Exporta/importa la base de datos como JSON (filtrado para evitar índices huérfanos)

### Estructura de cada paper en PAPERS
```js
{
  i:   number,      // ID único del paper (0..433, secuencial desde el CSV)
  au:  string,      // Autores
  yr:  number,      // Año de publicación
  ti:  string,      // Título completo
  url: string,      // URL de Scopus / DOI (identificador externo)
  ab:  string,      // Abstract completo en inglés (~1973 chars promedio, sin truncar)
  ap:  string,      // Approach/método (vacío; clasificado por IA o manualmente)
  sy:  string,      // Sistema
  cs:  string,      // Caso de estudio
  or:  string,      // Orquestación
  // Campos de clasificación IA (vacíos en PAPERS; se llenan en classified{} del localStorage):
  llm: string,      // LLM usado o "No"
  rea: string,      // Reasoning paradigm
  mal: string,      // Multi-algorithm
  aut: string,      // Autonomy level
  sem: string,      // Semantic interoperability
  syt: string,      // System type
  bte: string       // Base technology
}
```

### Estructura del localStorage (thesis_db_v2)
```json
{
  "classified": {
    "0": { "llm": "No", "rea": "MADRL", "mal": "Yes — PPO+MILP", "aut": "FA (Fully Autonomous)", "sem": "No", "syt": "MAS / CPS", "bte": "Deep Reinforcement Learning", "priority": 2 }
  },
  "decisions": {
    "0": { "decision": "keep", "note": "Relevante para el gap", "discardReason": "" }
  },
  "translations": {
    "0": { "abEs": "Texto del abstract traducido al español..." }
  }
}
```

### Opciones de clasificación (FIELD_OPTIONS en el JS)
| Key | Label | Opciones |
|-----|-------|----------|
| **llm** | LLM Mode | not apply · DRL · LLM General · Agents · MAS |
| **rea** | Reasoning | MADRL · DQN · PPO · MADDPG · GNN+DRL · LLM Reasoning · XAI · Lyapunov · Hybrid · Other |
| **mal** | Multi-ALGO | Supply chain · DRL · IoT · DT (Digital Twin) · HMS (Holones) · Other |
| **aut** | Autonomía | Fully autonomous · Human on loop · Asistan human · All control human · No apply |
| **sem** | Integración | H-IS (ERP/MES/WES) · V-IS (Supply Chain) · Both · No |
| **syt** | System Type | MAS · WES · ERP · Other |
| **bte** | Gestión Negociación | EAS · MAS · No aplica |
| **cas** | CAS | Yes — CAS · No |

> **CAS** (Conversational Agent Systems): campo nuevo, crucial para el enfoque de la tesis.

### Lógica de prioridad (`computePriority`)
| Prioridad | Criterio |
|-----------|----------|
| 1 — Alta  | Usa LLM + sistema MAS/ERP/CPS/Digital Twin, O usa LLM solo, O interop semántica + MAS/ERP |
| 2 — Media | MAS + multi-algoritmo, DRL + MAS, Digital Twin + semántica, o multi-algoritmo solo |
| 3 — Marginal | MAS o Digital Twin sin otras condiciones, DRL solo |
| 4 — Descarte | Todo lo demás |

### Decisiones manuales
- `keep` — conservar para la revisión
- `discard` — descartar (requiere motivo predefinido o libre)
- `pending` — sin decidir (por defecto)

### Motivos de descarte predefinidos
- Fuera del ámbito — Edge computing / telecomunicaciones puras
- Fuera del ámbito — Redes 5G / B5G sin manufactura
- Fuera del ámbito — GPU / infraestructura de cómputo
- Demasiado antiguo y superado por literatura reciente
- Sin contribución al gap de investigación
- Duplicado conceptual — cubierto por otro paper

---

## API — Soporte multi-proveedor

El header tiene un selector para elegir el proveedor. Cada uno guarda su key por separado:

| Proveedor | Modelo | localStorage key | Headers especiales |
|-----------|--------|-----------------|-------------------|
| **Anthropic (Claude)** | `claude-sonnet-4-6` | `thesis_anthropic_key` | `x-api-key`, `anthropic-version: 2023-06-01`, `anthropic-dangerous-direct-browser-access: true` |
| **OpenAI (GPT)** | `gpt-4o` | `thesis_openai_key` | `Authorization: Bearer sk-...` |

- El proveedor activo se guarda en `thesis_api_provider`
- **Clasificación:** lotes de 5 papers, max_tokens: 1400
- **Traducción:** lotes de 8 papers, max_tokens: 4000

### Formato de prompt de clasificación
Los papers se identifican con `[ID:N]` donde N es `p.i`. La respuesta JSON usa `"id": N`. Evita errores de indexación base-0/base-1.

---

## Estado actual del proyecto (abril 2026) — versión 2.0

- **434 papers cargados** (restaurados desde `Estado del Arte.csv`, abstracts completos, con URL)
- **8 criterios de clasificación** (LLM Mode, Reasoning, Multi-ALGO, Autonomía, Integración, System Type, Gestión Negociación, **CAS nuevo**)
- **Multi-API:** selector Claude (Anthropic) / GPT-4o (OpenAI) en el header
- **Clasificación IA:** ~58 clasificados con criterios antiguos; pendiente reclasificar con nuevos criterios
- **Traducción ES:** pendiente
- **Decisiones manuales:** en progreso

---

## Estado actual del proyecto (abril 2026) — actualizado

- 434 papers, abstracts completos, campo `url` incluido
- Multi-API: selector Claude / GPT-4o en el header
- 8 campos de clasificación (incluyendo CAS nuevo)
- Botón "🔄 Resetear y reclasificar" disponible en sidebar
- Filtros: LLM Mode, Autonomía, CAS, System Type

---

## Bugs corregidos (historial)

1. `window.storage` → `localStorage` (crasheaba al arrancar)
2. Headers Anthropic faltaban (`x-api-key`, `anthropic-version`)
3. `stopAll()` definida dos veces
4. Modelo obsoleto (`claude-sonnet-4-20250514` → `claude-sonnet-4-6`)
5. `prompt()` nativo en descarte masivo → modal propio con opciones predefinidas
6. Bug de indexación en lotes: el prompt usaba `[PAPER 1..N]` pero el JSON esperaba `idx: 0..N-1` → corregido usando `[ID:N]` con ID real del paper
7. Abstracts truncados a 500 chars en PAPERS → reconstruido desde CSV con abstracts completos (~1973 chars)
8. Export JSON incluía clasificaciones de índices huérfanos → filtrado por IDs válidos en PAPERS
9. 10 papers eliminados por error → restaurados todos los 434 desde el CSV
10. `.substring(0,800)` en prompt de clasificación eliminado; se envía abstract completo

## Posibles mejoras futuras
- Reintentar automáticamente papers no clasificados tras error de API
- Filtro por año
- Vista de estadísticas por categoría (gráficos)
- Exportar directamente a Zotero/Mendeley
