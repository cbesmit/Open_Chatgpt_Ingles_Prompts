---
Activo: true
Clasificación:
  - Documento
---
# Proyecto: Aprendizaje con ChatGPT (Plantilla GENÉRICA)

> **Objetivo**: 5 chats + 3 archivos JSON para aprender **cualquier tema** (MCP, Python, Patrones, etc.) desde **principiante** hasta **avanzado/experto**, con trazabilidad, métricas y versiones.

---

## 1. Estrategia general

Sistema de aprendizaje dentro de un **proyecto** de ChatGPT con:

- **Contexto persistente** en `evaluaciones.json.md`, `temario.json`, `terminos.json`.
    
- **Flujo cíclico**: diagnóstico/examen → plan → clases → práctica → términos → re‑evaluación.
    
- **Decisiones por umbrales** (promoción / mantener / replanificar) y reglas de consistencia entre chats/archivos.
    

**Beneficios**

1. **Trazabilidad** (historial, versiones, evidencias).
    
2. **Adaptación por datos** (scores, tendencia, retención SRS).
    
3. **Resultados medibles** (criterios de aceptación y mini‑rúbricas).
    
4. **Portabilidad** (misma estructura para cualquier tema).
    
5. **Menor deriva de contexto** (reglas de consistencia).
    
6. **Aprendizaje activo** (teoría, práctica, aplicación).
    
7. **Memoria semántica** (consolidación de conceptos/snippets en `terminos.json`).
    
8. **Iteración rápida** (frases de control + reemplazo completo de archivos).
    
9. **Auditable** (decisiones justificadas en métricas).
    
10. **Escalable** (proyectos independientes por tema con el mismo protocolo).
    

---

## 2. Estructura general del proyecto

### Chats principales

> **Todos los chats** leen los 3 archivos y proponen **actualizaciones completas** (tú haces backup y reemplazas).

#### 1) **evaluaciones** (diagnóstico + exámenes)

- **Propósito**: medir nivel y progreso (secciones: **teoría**, **práctica**, **aplicación**).
    
- **Entradas**: diagnóstico inicial o examen periódico (quiz/kata/reto), tiempo y rúbrica.
    
- **Salidas**: `score/100` por sección y total, fortalezas/debilidades, **decisión propuesta** (promoción/mantener/replanificar), acciones inmediatas.
    
- **Reglas**: compara con `umbral_promocion_total`, `umbral_mejora_pct`, `umbral_banda_roja`; calcula `tendencia_pct` y actualiza métricas.
    
- **Impacto**: habilita **temario** para crear nueva versión **ACTUAL**.
    

#### 2) **temario** (plan/roadmap versionado)

- **Propósito**: mantener un plan **ACTUAL** coherente con nivel y métricas; archivar versiones previas como **HISTÓRICO**.
    
- **Entradas**: resultados de evaluaciones, lagunas de `terminos.json`, disponibilidad semanal.
    
- **Salidas**: objetivos por periodo, **módulos** con tareas + criterios de aceptación, observaciones, **evidencias** requeridas.
    
- **Reglas**: una sola versión `estado=ACTUAL`; cambios mayores crean nueva versión con registro en `historial`.
    

#### 3) **clases** (enseñanza breve + 3 ejercicios + mini‑test)

- **Propósito**: explicar el siguiente contenido del módulo vigente y validar comprensión.
    
- **Entradas**: resumen teórico, 3 ejercicios graduados, mini‑test (5 ítems), dudas bloqueantes.
    
- **Salidas**: próximos pasos, evidencias a producir, términos clave.
    
- **Trigger**: `FINALIZAR_CLASE` → propone progreso de módulo y altas/cambios en `terminos.json`.
    

#### 4) **ejercicios y prácticas** (simulaciones/katas/retos)

- **Propósito**: práctica aplicada con criterios de aceptación y micro‑feedback.
    
- **Entradas**: briefs de tarea, escenarios o katas; restricciones/criterios.
    
- **Salidas**: evaluación por ejercicio, pendientes (issues), refuerzos, términos detectados.
    
- **Trigger**: `REVISAR_PRACTICA` → puede proponer ajustes de `temario.json` y altas en `terminos.json`.
    

#### 5) **términos** (SRS de conceptos/definiciones/snippets)

- **Propósito**: consolidar conceptos/patrones/fórmulas/snippets con repaso espaciado (SRS ligero).
    
- **Entradas**: altas desde clases/prácticas; sesiones de repaso (quiz/cloze/flashcards).
    
- **Salidas**: estados SRS y `proximo_repaso`; alertas de lagunas hacia **temario**/**evaluaciones**.
    
- **Reglas**: cambios de estado/dificultad impactan `retencion_srs` y recomendaciones del plan.
    

**Tabla de resumen (chats)**

|Chat|Usa archivos|Actualiza (indirecto)|Propósito breve|
|---|---|---|---|
|**evaluaciones**|`evaluaciones.json.md`, `temario.json`|`evaluaciones.json.md`|Diagnóstico y exámenes; propone decisión|
|**temario**|`evaluaciones.json.md`, `terminos.json`|`temario.json`|Plan ACTUAL y versiones|
|**clases**|`evaluaciones.json.md`, `temario.json`, `terminos.json`|`temario.json`, `terminos.json`|Enseñanza + mini‑test + términos|
|**ejercicios y prácticas**|`evaluaciones.json.md`, `temario.json`, `terminos.json`|`terminos.json` (y posible `temario.json`)|Práctica aplicada y feedback|
|**términos**|`evaluaciones.json.md`, `temario.json`, `terminos.json`|`terminos.json`|SRS de conceptos/snippets|

### Archivos compartidos (detallado: propósito, campos y uso cruzado)

#### `evaluaciones.json.md`

- **Propósito**: diagnóstico inicial, exámenes periódicos, métricas y reglas de decisión de nivel.
    
- **Campos clave**: `perfil (tema, nivel_actual, nivel_objetivo, ultima_decision)`, `diagnostico_inicial (fecha, score_total, criterios, justificacion)`, `examenes[] (fecha, scores{teoria, practica, aplicacion, total}, tiempo_min, observaciones, recomendaciones)`, `metricas (promedio_ult2, promedio_ult3, tendencia_pct, tiempo_semana_min, tasa_completitud, retencion_srs)`, `reglas (umbral_promocion_total, umbral_mejora_pct, umbral_banda_roja)`, `historial[]`.
    
- **Uso cruzado**: fija nivel y prioridades para **temario**; alimenta refuerzos con `retencion_srs` desde **términos**.
    

#### `temario.json`

- **Propósito**: plan versionado; guía clases y práctica.
    
- **Campos clave**: `versiones[] {fecha, estado( ACTUAL|HISTORICO ), nivel_base, objetivos_periodo[], modulos[] {id, titulo, contenidos[], tareas[] {id, tipo, criterios_aceptacion[]}, estado, evidencias[]}, observaciones}`.
    
- **Uso cruzado**: define qué imparten **clases** y **ejercicios**; consume lagunas de **términos** y resultados de **evaluaciones**.
    

#### `terminos.json`

- **Propósito**: base de conceptos/definiciones/snippets con SRS (estado y `proximo_repaso`).
    
- **Campos clave**: `items[] {id, tipo(concepto|snippet|definicion|flashcard), nombre, descripcion, codigo?, tags[], estado, proximo_repaso}`.
    
- **Uso cruzado**: genera refuerzos en **temario** y afecta `retencion_srs` en **evaluaciones**.
    

---

## 3. Ciclo de funcionamiento general

### **Inicio del proyecto (primer uso)**

1. **Instrucciones del proyecto**: crear `contexto.md` en la **raíz del proyecto**. Debe incluir: **TEMA**, **objetivo**, **niveles esperados**, **rol del asistente** (experto + docente), **estilo de respuestas** (técnico y conciso), **frases de control por chat**, **reglas de consistencia**, **políticas** (no inventar datos; citar archivos), y **métricas/umbrales** de `evaluaciones.json.md`. **Todos los chats leen este archivo como punto de verdad**.
    
2. **Preparar archivos base**: crear `evaluaciones.json.md`, `temario.json`, `terminos.json` (vacíos o plantillas); definir `perfil.tema` y `nivel_objetivo`.
    
3. **Ejecutar diagnóstico**: en **evaluaciones** → `EJECUTAR_ASSESSMENT` → registrar `diagnostico_inicial`.
    
4. **Plan inicial**: en **temario** → `GENERAR_ACTUALIZACION` del archivo `temario.json` → crear versión **ACTUAL** con módulos/objetivos iniciales, criterios de aceptación y evidencias esperadas.
    
5. **Configurar términos iniciales**: en **términos** → dar de alta conceptos/snippets base detectados; programar SRS.
    
6. **Verificación de consistencia**: nivel ↔ plan **ACTUAL**; evidencias previstas; backup inicial.
    

### **Ciclo operativo continuo (uso regular)**

1. **Enseñar (clases)**: impartir módulo vigente; mini‑test; `FINALIZAR_CLASE` → propuesta de cambios al plan y altas a términos.
    
2. **Practicar (ejercicios y prácticas)**: ejecutar tareas/katas con criterios; `REVISAR_PRACTICA`; registrar pendientes/evidencias.
    
3. **Consolidar (términos)**: repaso SRS y nuevas altas; `GENERAR_ACTUALIZACION` del archivo `terminos.json`.
    
4. **Re‑evaluar (evaluaciones)**: examen periódico; actualizar métricas (`promedio_ult2/3`, `tendencia_pct`, etc.); propuesta (promoción/mantener/replanificar).
    
5. **Replanificar (temario)**: si aplica, nueva versión **ACTUAL**; mover anterior a **HISTÓRICO**.
    
6. **Control de consistencia**: si falta evidencia para marcar "hecho", **pausar** cambios y abrir issue; backup y reemplazo de archivos en cada actualización.
    

### **Ciclo de mantenimiento y mejora**

1. **Revisión mensual**: auditoría de coherencia (nivel↔plan, SRS↔métricas, evidencias↔estado de módulos).
    
2. **Curación de términos**: fusionar duplicados, depurar `tags`, ajustar dificultad, recalcular `proximo_repaso`.
    
3. **Ajuste de umbrales y KPIs**: calibrar `umbral_promocion_total`, `umbral_mejora_pct`, y `tiempo_semana_min` según disponibilidad real.
    
4. **Refactor del plan**: reordenar módulos, añadir/quitar tareas, actualizar criterios de aceptación.
    
5. **Archivado y reportes**: cerrar iteraciones con breve informe; snapshot de `versiones` y `historial`.
    

---

## 4. Funcionamiento y relación entre los chats

**Tabla de relación por chat**

|Chat|Archivos que usa|Archivos que actualiza (indirecto)|Propósito|
|---|---|---|---|
|**evaluaciones**|`evaluaciones.json.md`, `temario.json`|`evaluaciones.json.md`|Medir nivel y progreso; proponer promoción/mantener/replanificar según umbrales|
|**temario**|`evaluaciones.json.md`, `terminos.json`|`temario.json`|Mantener el plan **ACTUAL** y versionar cambios alineados a resultados/lagunas|
|**clases**|`evaluaciones.json.md`, `temario.json`, `terminos.json`|`temario.json`, `terminos.json`|Impartir contenido del módulo vigente, mini‑test y alta de términos/progreso|
|**ejercicios y prácticas**|`evaluaciones.json.md`, `temario.json`, `terminos.json`|`terminos.json` (y posible `temario.json`)|Ejecutar práctica aplicada con criterios y micro‑feedback; detectar términos; proponer ajustes del plan|
|**términos**|`evaluaciones.json.md`, `temario.json`, `terminos.json`|`terminos.json`|Consolidar conceptos/snippets mediante SRS; retroalimentar retención y refuerzos|

> **Consistencia**: para marcar un módulo como "hecho" se requieren **evidencias**. Si faltan, **pausar** cualquier cambio y registrar el issue.

---

## 5. Mecanismo de actualización de archivos

### Lectura

Todos los chats **leen** los archivos del proyecto (JSON) y siempre usan la versión más reciente cargada en el proyecto.

### Escritura (actualización indirecta y mantenimiento manual)

ChatGPT **no guarda archivos**. Cuando pidas una actualización, imprimirá la **versión completa** del archivo correspondiente (JSON) para que tú la reemplaces manualmente.

1. Tú le pides a la IA que actualice el archivo que está utilizando.
    
2. La IA imprime el archivo completo (p. ej., `evaluaciones.json.md`) con la versión nueva
    
3. Tú creas un respaldo: `<archivo>_backup_YYYYMMDD_vN.json`.
    
4. Reemplazas el archivo y lo vuelves a subir al proyecto.
    

### Regla de consistencia

- Incrementa `version` en cada cambio y actualiza `updated_at` (ISO) y `autor` (`usuario` | `IA`).
    
- Sincroniza variables transversales tras cada reemplazo (`nivel_actual`, métricas, versiones de temario, estados SRS).
    
- Añade a `historial` un item `{fecha, version, cambios_resumen, autor}` (append-only).
    
- Si hay incoherencias detectadas por el chat, debe pedir revisión antes de proponer nuevos cambios.
    

**Frases de control por chat**

|Chat|Frases de control|Archivos que pueden actualizar|
|---|---|---|
|**evaluaciones**|`EJECUTAR_ASSESSMENT`, `FORZAR_DIAGNOSTICO`, `FORZAR_EXAMEN`, `GENERAR_ACTUALIZACION evaluaciones.json.md`|`evaluaciones.json.md`|
|**temario**|`GENERAR_ACTUALIZACION temario.json`|`temario.json`|
|**clases**|`FINALIZAR_CLASE` → luego `GENERAR_ACTUALIZACION temario.json` y, si hubo términos, `GENERAR_ACTUALIZACION terminos.json`|`temario.json`, `terminos.json`|
|**ejercicios y prácticas**|`REVISAR_PRACTICA` → si emergen conceptos, `GENERAR_ACTUALIZACION terminos.json`|`terminos.json`|
|**términos**|`GENERAR_ACTUALIZACION terminos.json`|`terminos.json`|

---

## 6. Descripción de los archivos base

- **`evaluaciones.json.md`**: perfil (tema/nivel), diagnóstico, exámenes (teoría/práctica/aplicación), métricas y reglas; decide el rumbo (promoción/mantener/replanificar) y registra cambios en `historial`.
    
- **`temario.json`**: versiones del plan y una versión `ACTUAL` con objetivos, módulos, tareas, criterios y evidencias; guía qué impartir y practicar.
    
- **`terminos.json`**: conceptos/definiciones/snippets con estado SRS y fechas de repaso; alimenta refuerzos y mide `retencion_srs`.
    

---

## 7. Flujo completo del ciclo de aprendizaje

### Tabla operativa (paso a paso)

|Paso|Chat|Entrada|Comando|Archivo actualizado|Salida principal|
|---|---|---|---|---|---|
|1|**evaluaciones**|Sin nivel o inicio de ciclo|`EJECUTAR_ASSESSMENT`|`evaluaciones.json.md`|Diagnóstico inicial (scores, fortalezas/debilidades)|
|2|**temario**|Diagnóstico y prioridades|`GENERAR_ACTUALIZACION` del archivo `temario.json`|`temario.json`|Versión **ACTUAL** del plan (objetivos, módulos, tareas)|
|3|**clases**|Módulo vigente|`FINALIZAR_CLASE`|`temario.json` y altas en `terminos.json`|Progreso de módulo, mini‑test, términos detectados|
|4|**ejercicios y prácticas**|Tareas/katas del módulo|`REVISAR_PRACTICA`|Altas/ajustes en `terminos.json` (y si aplica `temario.json`)|Feedback por ejercicio, pendientes, refuerzos|
|5|**términos**|Conceptos/snippets a consolidar|`GENERAR_ACTUALIZACION` del archivo `terminos.json`|`terminos.json`|Estados SRS y `proximo_repaso` actualizados|
|6|**evaluaciones**|Cierre de ciclo corto|(examen periódico)|`evaluaciones.json.md`|Decisión: **promoción / mantener / replanificar** y retorno al paso 2|

### Secuencia detallada

1. **Evaluaciones → Diagnóstico inicial**: se obtienen scores por teoría/práctica/aplicación, se calculan métricas base y recomendaciones inmediatas.
    
2. **Temario → Plan ACTUAL**: se materializan objetivos del periodo, módulos con tareas y criterios de aceptación; se registran evidencias esperadas.
    
3. **Clases → Enseñanza y mini‑test**: explicación breve, 3 ejercicios, validación rápida; se proponen cambios de estado del módulo y términos clave.
    
4. **Ejercicios y prácticas → Aplicación**: ejecución de tareas/retos con micro‑rúbricas; se documentan pendientes y posibles ajustes del plan.
    
5. **Términos → SRS**: alta de conceptos/snippets y programación de repaso; impacto directo en `retencion_srs`.
    
6. **Evaluaciones → Examen periódico**: medición objetiva del avance, actualización de métricas y **decisión** para el siguiente ciclo.
    

---

## 8. Buenas prácticas

- Mantener **una** versión `ACTUAL`; el resto `HISTORICO`.
    
- Registrar **evidencias** antes de marcar un módulo como hecho.
    
- Hacer **backup** antes de cada reemplazo de archivo.
    
- Revisión de consistencia mensual (nivel↔plan, SRS↔métricas, evidencias↔estado).
    
- Nombrar chats con índice y fecha: `01_evaluaciones_YYYY-MM-DD`, etc.
    

---

## 9. Resultados y beneficios esperados del aprendizaje

### Resultados (qué obtienes)

- **Ruta comprobable INICIAL → EXPERTO** con evidencia y `historial` trazable.
    
- **Progreso medible** por ciclo: `score_total`, `tendencia_pct`, `promedio_ult2/3` y `tasa_completitud`.
    
- **Aplicación práctica**: tareas/retos con **criterios de aceptación** y evidencias validadas.
    
- **Consolidación de conocimiento**: mejora de `retencion_srs` por repaso espaciado.
    
- **Plan vivo** que se re‑calibra ante estancamientos o bandas rojas (< umbral).
    
- **Portafolio** de entregables (repos, notas, capturas) ligado a módulos "hecho".
    
- **Tiempo optimizado**: seguimiento de `tiempo_semana_min` y **streak** de estudio.
    
- **Transferencia**: términos/snippets reutilizables para acelerar futuros proyectos.
    

### Beneficios (por qué sirve)

- **Menos deriva de contexto**: una única fuente de verdad (archivos + instrucciones).
    
- **Feedback inmediato**: micro‑rúbricas en clases y prácticas.
    
- **Decisiones objetivas**: promoción/mantener/replanificar basadas en datos.
    
- **Escalabilidad personal**: múltiples proyectos con el mismo marco operativo.
    
- **Auditoría y reporting**: versiones y métricas listas para informes.
    

### KPIs y metas recomendadas (por defecto)

|Indicador|Qué refleja|Fuente|Meta inicial|
|---|---|---|---|
|`score_total`|Dominio global del ciclo|evaluaciones|≥ 75/100|
|`tendencia_pct`|Mejora vs. promedio histórico|evaluaciones|≥ +10%|
|`tasa_completitud`|Avance vs. plan del periodo|temario|≥ 80%|
|`retencion_srs`|Recuerdo a mediano plazo|terminos|≥ 70%|
|`tiempo_semana_min`|Dedicación mínima semanal|evaluaciones|≥ 180 min|
|`evidencias_validadas`|Entregables aceptados|temario|≥ 1 por módulo|
|`tiempo_a_subir_nivel`|Ciclos para promoción|evaluaciones/temario|≤ 3 ciclos|

### Hitos sugeridos por nivel

|Nivel|Competencias mínimas|Evidencias de salida|
|---|---|---|
|INICIAL|Vocabulario/Conceptos base; primeros ejercicios guiados|1 mini‑reto aceptado|
|BÁSICO|Tareas estándar con guía ligera|2 módulos "hecho" con evidencias|
|INTERMEDIO|Resolución de problemas comunes; integración de conceptos|1 reto aplicado y notas técnicas|
|AVANZADO|Diseño/optimización; prevención de errores|1 proyecto integrador con criterios cumplidos|
|EXPERTO|Autonomía; mejora continua; mentoría|1 entregable público (repo/post/talk) + guía de buenas prácticas|