---
name: quality-attributes-architect
description: >-
  Expert software architecture skill based on SEI (Software Engineering Institute) and Bass et al. (Chapters 3-14).
  Use this skill to:
  1) Generate quality attribute scenarios using the official SEI 6-part template (Source, Stimulus, Artifact, Environment, Response, Response Measure).
  2) Check whether a given quality attribute scenario is complete, identify missing or ambiguous elements, and refine it using techniques like Straw Man measures.
  3) Construct, structure, and prioritize Utility Trees (Árboles de Utilidad) with (Importance, Difficulty) ratings.
---

# Quality Attributes Architect Skill (SEI / Bass et al.)

Esta skill dota al asistente de capacidades avanzadas de ingeniería de requerimientos arquitectónicos (ASRs) y atributos de calidad de acuerdo con los métodos del Software Engineering Institute (SEI) y el libro *Software Architecture in Practice* (4ta Ed., Bass, Clements, Kazman).

---

## Capacidades Principales de la Skill

La skill opera en 3 modos o tareas según la solicitud del usuario:

```text
                               ┌─> 1. Generación de Escenarios SEI (6 partes)
                               │
[ Solicitud de Arquitectura ] ─┼─> 2. Chequeo y Completitud de Escenarios
                               │
                               └─> 3. Elaboración de Árbol de Utilidad (Utility Tree)
```

---

## Modo 1: Generación de Escenarios de Atributos de Calidad (SEI 6-Part Template)

Cuando se solicite formular, especificar o refinar un requerimiento no funcional / de calidad en un escenario formal del SEI, se debe generar una ficha estructurada con las 6 partes obligatorias:

### Estructura de Salida Obligatoria:

```markdown
### Escenario [ID]: [Nombre descriptivo del escenario]
- **Atributo de Calidad:** [Availability | Modifiability | Performance | Security | Usability | Testability | Interoperability | Scalability]
- **1. Fuente del Estímulo (Source):** [Quién o qué origina el evento/estímulo]
- **2. Estímulo (Stimulus):** [Condición, falla, petición o cambio que arriba al sistema]
- **3. Artefacto (Artifact):** [Parte del sistema afectada: UI, backend, base de datos, sistema completo]
- **4. Ambiente / Entorno (Environment):** [Estado operativo: normal, sobrecarga/pico, sin conectividad, degradado, desarrollo, etc.]
- **5. Respuesta (Response):** [Comportamiento observable del sistema ante el estímulo]
- **6. Medida de Respuesta (Response Measure):** [Métrica cuantitativa, testeable y medible: latencia, uptime %, MTTR, costo en horas]

> **Resumen narrativo:** "[Enunciado fluido que integra las 6 partes en una sola oración concisa]".
```

### Reglas de Diseño:
- Consultar [references/sei_qa_templates.md](./references/sei_qa_templates.md) para emplear el vocabulario y métricas estándar de cada atributo según Bass et al.
- La **Medida de Respuesta** NUNCA debe ser vaga (evitar "rápido", "fácil", "seguro", "sin problemas"). Siempre debe contener números, porcentajes o unidades de tiempo/esfuerzo.

---

## Modo 2: Chequeo, Diagnóstico y Completitud de Escenarios

Cuando se provea un requerimiento informal o un escenario preliminar ("crudo" / *raw scenario*), la skill debe ejecutar un diagnóstico de completitud y proponer el escenario completado.

### Proceso de Evaluación en 4 Pasos:
1. **Identificación del Atributo de Calidad principal:** Clasificar a qué atributo pertenece (o si es un requerimiento funcional puro o una restricción de diseño).
2. **Matriz de Chequeo de las 6 Partes:** Evaluar elemento por elemento con estado:
   - ✅ **Presente y claro**
   - ⚠️ **Ambiguo o incompleto**
   - ❌ **Faltante / Ausente**
3. **Detección de Ambigüedades y Aplicación de *Straw Man*:** Si la medida de respuesta falta o es subjetiva (ej. "el sistema debe ser rápido"), proponer una medida concreta inicial basada en *Straw Man Response Measure* (Michael Keeling) para abrir la discusión técnica.
4. **Escenario Completo Refinado:** Presentar la versión final completa en el template de 6 partes.

### Formato de Salida para Chequeo:

```markdown
### Diagnóstico de Escenario
- **Enunciado Original:** "[Texto provisto]"
- **Atributo Identificado:** [Nombre del atributo]

| Elemento SEI | Estado | Valor Detectado / Observación |
| :--- | :---: | :--- |
| **1. Fuente** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |
| **2. Estímulo** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |
| **3. Artefacto** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |
| **4. Ambiente** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |
| **5. Respuesta** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |
| **6. Medida de Respuesta** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |

#### Partes Faltantes y Recomendación de Refinamiento:
- [Explicación de qué falta y justificación de las asunciones/straw man propuestos]

#### Escenario Completado (SEI 6 Partes):
[Presentar el template completo con las 6 partes rellenas]
```

---

## Modo 3: Elaboración de Árboles de Utilidad (Utility Trees)

Cuando se analice un sistema completo o un caso de estudio (ej. Sistema de Monopatines Eléctricos, BAMS, etc.), la skill debe generar el Árbol de Utilidad completo.

### Proceso de Construcción:
1. **Nodo Raíz:** `Utility` (Utilidad del Sistema).
2. **Nivel 1 (Atributos):** Identificar los 4 a 6 atributos de calidad clave del sistema según los objetivos de negocio y stakeholders.
3. **Nivel 2 (Sub-factores):** Dividir cada atributo en 2 o más categorías de refinamiento.
4. **Nivel 3 (Escenarios Refinados):** Formular escenarios concretos y medibles para cada hoja del árbol.
5. **Priorización `(Importancia, Dificultad)`:** Asignar la tupla `(H/M/L, H/M/L)` justificando las calificaciones:
   - Primer valor: **Importancia para el Negocio** (H = High, M = Medium, L = Low).
   - Segundo valor: **Dificultad/Riesgo Arquitectónico** (H = High, M = Medium, L = Low).
   - Identificar cuáles escenarios son los **Architectural Drivers `(H, H)`**.

### Formato de Salida para Árbol de Utilidad:
Presentar tanto:
1. **Diagrama Jerárquico Mermaid o Texto Estructurado**.
2. **Tabla Detallada de Escenarios y Priorización** según la guía [references/utility_tree_guide.md](./references/utility_tree_guide.md).
3. **Identificación de Escenarios Críticos `(H, H)`** y recomendaciones tácticas iniciales.

---

## Referencias y Guías Adicionales
- Catálogo de Atributos SEI: [references/sei_qa_templates.md](./references/sei_qa_templates.md)
- Guía de Árbol de Utilidad: [references/utility_tree_guide.md](./references/utility_tree_guide.md)
- Casos de Prueba y Ejemplos: [examples/test_cases.md](./examples/test_cases.md)
