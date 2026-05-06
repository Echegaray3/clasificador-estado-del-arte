# Clasificador Estado del Arte

**Aplicación web interactiva para clasificación y gestión de papers académicos.**

## 📋 Descripción

Herramienta HTML standalone para clasificar, filtrar y analizar papers del estado del arte. Integra inteligencia artificial (OpenAI y Anthropic) para procesamiento automático de datos académicos.

---

## 🌐 Acceso

**URL pública:** https://echegaray3.github.io/clasificador-estado-del-arte/

**Repositorio GitHub:** https://github.com/Echegaray3/clasificador-estado-del-arte

---

## 🔧 Características Técnicas

### Especificaciones
- **Tipo:** Aplicación web (HTML5 + CSS + JavaScript vanilla)
- **Tamaño:** 2,022 líneas de código
- **Estructura:** Archivo único (`index.html`) — standalone y portátil

### Dependencias Externas
- **XLSX Library v0.18.5** (CDN: cdnjs.cloudflare.com) — exportación/importación de Excel
- **OpenAI API** — procesamiento inteligente de papers
- **Anthropic API** — análisis alternativo de contenido académico

### Características
- 🎨 Interfaz responsive con tema oscuro
- 📊 Gestión de dataset embebido (dataset de papers en memoria)
- 🔍 Filtrado avanzado de papers
- 📥 Importación desde Excel (XLSX)
- 📤 Exportación a Excel
- 🤖 Integración con APIs de IA para análisis automático
- 💾 Almacenamiento local (localStorage) para credenciales de API

---

## 🚀 Instalación y Despliegue

### Despliegue Automático (GitHub Pages)
El proyecto está automáticamente desplegado en GitHub Pages. Los cambios en la rama `master` se publican automáticamente.

**Pasos para actualizar:**
```bash
# Editar index.html en tu máquina
# Luego:
git add index.html
git commit -m "Descripción del cambio"
git push origin master
```

El sitio se actualizará en ~1-2 minutos.

### Uso Local
1. Descarga `index.html`
2. Abre el archivo en tu navegador
3. ¡Listo! No requiere servidor web

---

## 🔐 Seguridad

⚠️ **Las claves de API se almacenan en `localStorage` del navegador:**
- No se envían al repositorio
- Solo se guardan localmente en tu máquina
- Cada usuario introduce sus propias credenciales

**Nunca compartir claves de API** en el código o repositorio.

---

## 📁 Estructura

```
clasificador-estado-del-arte/
├── index.html          # Aplicación completa (2,022 líneas)
├── .git/               # Repositorio git
├── .gitignore          # (opcional) Archivos a ignorar
└── README.md           # Esta documentación
```

---

## 🛠️ Stack Tecnológico

| Componente | Tecnología |
|-----------|-----------|
| Frontend | HTML5, CSS3, JavaScript vanilla |
| Almacenamiento | localStorage (navegador) |
| Export/Import | XLSX.js v0.18.5 |
| IA | OpenAI API, Anthropic API |
| Hosting | GitHub Pages |
| Control de versiones | Git + GitHub |

---

## 📝 Historial de Versiones

### v1.0 (2026-05-06)
- ✅ Inicialización del repositorio
- ✅ Despliegue en GitHub Pages
- ✅ Documentación del proyecto

---

## 🎯 Próximos Pasos

Posibles mejoras futuras:
- [ ] Agregar README con instrucciones de uso
- [ ] Crear CHANGELOG.md
- [ ] Documentar funciones principales
- [ ] Agregar ejemplos de uso
- [ ] Optimizar rendimiento con bundling (opcional)

---

## 📧 Contacto

**Autor:** Estado del Arte  
**Email:** naviechegaray3@gmail.com

---

## 📜 Notas

Este proyecto fue desplegado automáticamente utilizando:
- **GitHub CLI v2.92.0** para crear y configurar el repositorio
- **Git** para control de versiones
- **GitHub Pages** para hosting público gratuito

Fecha de creación: **2026-05-06**
