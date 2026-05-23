# 🚀 Claude Playground Project

Este proyecto tiene como objetivo servir como un entorno de práctica y experimentación para aprender y probar distintas funcionalidades de Claude y flujos modernos de desarrollo asistidos por IA.

La intención principal del repositorio es aprender, iterar y construir buenas prácticas reales utilizando Claude Code, GitHub, MCPs y herramientas modernas de desarrollo.

---

# 📌 Reglas importantes

- Usar el sistema de diseño definido en `DESIGN.md` al generar componentes visuales.
- Priorizar siempre código legible, mantenible y escalable.
- Evitar soluciones innecesariamente complejas.
- Mantener una estructura consistente en todo el proyecto.
- Explicar decisiones importantes cuando aporten contexto útil.

---

# 🎯 Objetivos del proyecto

## Aprender a utilizar correctamente

- `CLAUDE.md`
- instrucciones globales y contextuales
- skills personalizadas
- workflows iterativos con IA
- agentes y automatizaciones
- MCPs (Model Context Protocol)
- integración entre herramientas IA + GitHub
- automatización de flujos de desarrollo

---

## Experimentar con

- generación y refactor de código
- documentación automática
- análisis de arquitectura
- generación automática de Pull Requests
- integración con GitHub
- herramientas externas conectadas mediante MCP
- workflows multiagente
- automatizaciones de desarrollo
- CI/CD
- revisión automática de código

---

# 🔌 MCP (Model Context Protocol)

Uno de los principales objetivos del proyecto será aprender a utilizar MCPs para extender las capacidades de Claude mediante herramientas externas.

Se busca experimentar con:

- conexión de herramientas externas
- automatización de tareas
- acceso a archivos y repositorios
- integración con GitHub
- flujos multiagente
- servidores MCP personalizados
- herramientas de desarrollo asistidas por IA
- buenas prácticas para instrucciones y contexto

La idea es entender cómo construir entornos de desarrollo modernos asistidos por IA, escalables y reutilizables.

---

# 🌿 Workflow de Git y Desarrollo

## Branching

Para cada nueva tarea crear una nueva rama.

Formato obligatorio:

- `feature/<descripcion-corta>`
- `fix/<descripcion-corta>`
- `refactor/<descripcion-corta>`
- `docs/<descripcion-corta>`
- `test/<descripcion-corta>`
- `chore/<descripcion-corta>`

Ejemplos:

- `feature/github-actions-setup`
- `fix/dropdown-keyboard-navigation`
- `refactor/modal-state-management`
- `docs/storybook-setup`

Nunca trabajar directamente sobre `main`.

---

## Commits

Utilizar Conventional Commits.

Formato:

```bash
type(scope): descripcion
```

Ejemplos:

```bash
feat(auth): add login validation
fix(dropdown): correct keyboard navigation
refactor(modal): simplify internal state
docs(storybook): add button component stories
test(cypress): add login flow tests
chore(ci): update github actions workflow
```

---

## Commits Incrementales

Cuando una tarea sea grande o tenga múltiples pasos:

- realizar commits pequeños e incrementales
- evitar acumular todos los cambios en un único commit
- cada commit debe representar una mejora lógica y funcional
- mantener el historial limpio, legible y fácil de revertir
- evitar commits gigantes con múltiples responsabilidades

Ejemplo recomendado:

1. setup inicial
2. implementación base
3. estilos
4. tests
5. documentación
6. refactor final

Antes de realizar un commit:

- verificar archivos modificados
- evitar archivos innecesarios
- validar que el proyecto compile
- ejecutar tests relacionados si existen
- revisar que no existan errores de linting
- mantener consistencia con la arquitectura del proyecto

---

## Pull Requests

- Mantener PRs pequeñas y enfocadas
- Explicar claramente el objetivo
- Agregar pasos de testing
- Incluir screenshots si hay cambios visuales
- Evitar PRs gigantes con múltiples objetivos
- Mantener descripciones claras y profesionales

---

# 📚 Buenas prácticas esperadas

## Testing con Cypress

El proyecto deberá incluir buenas prácticas de testing E2E usando Cypress:

- tests legibles y mantenibles
- uso correcto de `data-cy`
- evitar selectores frágiles
- estructura clara de carpetas
- reutilización de comandos custom
- separación entre fixtures, commands y specs
- tests orientados al comportamiento del usuario
- evitar `waits` innecesarios
- priorizar estabilidad y claridad en los tests

---

## 📖 Documentación con Storybook

Se espera documentar componentes utilizando Storybook:

- stories simples y claras
- documentación de props
- ejemplos de uso reales
- variantes visuales
- testing visual cuando sea posible
- mantener consistencia visual y accesibilidad
- documentar estados importantes de cada componente

---

## ⚙️ GitHub Actions

El repositorio también funcionará como laboratorio para aprender e implementar GitHub Actions:

- CI/CD básico
- ejecución automática de tests
- validación de Pull Requests
- linting automático
- builds automatizadas
- workflows reutilizables
- automatizaciones integradas con Claude
- pipelines escalables y legibles

---

# 🧠 Filosofía del proyecto

Este repositorio no busca solamente crear funcionalidades, sino también aprender:

- mejores prácticas
- arquitectura escalable
- automatización
- documentación
- colaboración entre IA y desarrollo humano
- workflows modernos de ingeniería

La prioridad será mantener:

- código legible
- estructura mantenible
- buena documentación
- consistencia en el proyecto
- aprendizaje iterativo
- automatizaciones reutilizables
- historial Git limpio y entendible

---

# 🧩 Enfoque esperado para Claude

Claude deberá:

- priorizar claridad antes que complejidad
- dividir tareas grandes en pasos pequeños
- realizar commits incrementales cuando corresponda
- respetar las convenciones del proyecto
- mantener consistencia arquitectónica
- evitar cambios innecesarios
- documentar correctamente nuevas funcionalidades
- sugerir mejoras cuando aporten valor real
- mantener una mentalidad orientada a escalabilidad y mantenimiento