# Trabajo Práctico No. 3 - Ejercicio 4: Skill de Atributos de Calidad y Árboles de Utilidad

**Materia:** Diseño de Sistemas de Software (Facultad de Ciencias Exactas - UNICEN)  
**Profesor:** J. Andrés Díaz Pace  
**Bibliografía base:** Bass, Clements & Kazman (*Software Architecture in Practice*, 4th Ed., Caps 3-14), Cervantes & Kazman (*Designing Software Architectures*), Michael Keeling (*Design It!*).

---

## 🎯 Objetivo del Ejercicio 4

Desarrollar una **Skill de IA (estilo Claude / Antigravity / Agent Skill)** capaz de:
1. **Generar atributos y escenarios de calidad** según los templates de 6 partes del SEI.
2. **Chequear y completar escenarios dados**, diagnosticando qué partes faltan o son ambiguas y proponiendo cómo completarlas (técnica de *Straw Man*).
3. **Elaborar Árboles de Utilidad (Utility Trees)** con estructura jerárquica y matriz de priorización `(Importancia para el Negocio, Dificultad Técnica)`.
4. **Testear la skill con ejemplos conocidos** del TP (Monopatines Eléctricos, BAMS, Cajero Automático, etc.).

---

## 📁 Estructura de la Skill

La skill sigue el estándar de paquetes modulares de habilidades para agentes:

```text
tp3-atributos-calidad/
├── .agents/
│   └── skills/
│       └── quality-attributes-architect/
│           ├── SKILL.md                          # Definición principal de la skill (YAML frontmatter + prompts)
│           ├── references/
│           │   ├── sei_qa_templates.md           # Catálogo de templates de 6 partes (Bass et al. Caps 3-14)
│           │   └── utility_tree_guide.md         # Guía de construcción y priorización de Árboles de Utilidad
│           └── examples/
│               └── test_cases.md                 # Ejemplos y validaciones con casos de prueba del TP3
└── README.md                                     # Este documento explicativo
```

---

## 🧠 ¿Cómo funciona cada parte de una Skill?

1. **Frontmatter YAML (`SKILL.md`):**
   - Define el `name` y la `description`. La descripción es clave: los LLMs y sistemas de agentes la leen para saber **cuándo** activar automáticamente la habilidad ante la consulta de un usuario.
2. **Instrucciones y Modos de Operación (`SKILL.md`):**
   - Establece los pasos algorítmicos que debe seguir el modelo para estructurar sus respuestas sin omitir ningún campo técnico.
3. **Referencias y Conocimiento de Dominio (`references/`):**
   - Desacopla la teoría formal (Bass et al., SEI) para que el agente consulte definiciones rigurosas sin saturar el contexto inicial (*Progressive Disclosure*).
4. **Ejemplos y Casos de Prueba (`examples/`):**
   - Provee patrones *Few-Shot* que aseguran que el formato de salida sea consistente, profesional y testeable.

---

## 🧪 Resumen de Pruebas Realizadas

Las pruebas documentadas en [`examples/test_cases.md`](./.agents/skills/quality-attributes-architect/examples/test_cases.md) demuestran el correcto funcionamiento sobre:
- **Ejercicio 2a (Cajero Automático para adultos mayores):** Diagnóstico de ambigüedades en usabilidad, propuesta de *Straw Man* y formulación de escenario formal en 6 partes.
- **Ejercicio 2b (Tolerancia a fallas en procesador de texto):** Diagnóstico y escenario de disponibilidad con tiempos de recuperación medibles.
- **Ejercicio 3 / Apéndice (Sistema de Monopatines Eléctricos):** Árbol de utilidad completo de 11 escenarios clasificados en 6 atributos y priorizados por `(Importancia, Dificultad)`, destacando los 4 *Architectural Drivers* `(H, H)`.
