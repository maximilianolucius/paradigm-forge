# MIRADOR — A Typed, Versioned Intermediate Representation for Auditable Mathematical Knowledge and Discovery

- **Autor:** Maximiliano Lucius (Independent Researcher / Aureus Technologies)
- **Fecha:** Draft, 12 de julio de 2026
- **DOI:** [10.5281/zenodo.21324524](https://doi.org/10.5281/zenodo.21324524)
- **Rol en el ecosistema:** capa de **representación** (el sustrato sobre el que operan todos los demás sistemas).

## Problema
A medida que agentes LLM, motores simbólicos y asistentes de prueba generan matemática de nivel de investigación, la restricción vinculante ya no es *producir* un argumento plausible sino *acumular* resultados entre campañas sin importar silenciosamente afirmaciones falsas, mal encuadradas o obsoletas como dependencias. La unidad reutilizable hoy es el **documento** (prosa), cuyas asunciones, dependencias, procedencia y estatus de verdad son implícitos y, por tanto, inauditables a escala.

## Tesis
El documento es la unidad equivocada. La unidad reutilizable debe ser una **Célula Matemática (Mathematical Cell)** tipada y versionada, que liga una afirmación canónica a su contexto, dependencias, transformaciones, artefactos, procedencia y **estado epistémico**.

## Cuatro compromisos mecánicos
1. **Tipo de objeto ⊥ estado epistémico:** ser "teorema" es un *rol*, no implica estar `[PROMOTED]`. (20 tipos de objeto × 11 estados epistémicos, ortogonales.)
2. **Contexto en la identidad:** la identidad de una afirmación es el hash canónico (SHA-256) de su enunciado **más su contexto Γ** (asunciones, exclusiones, regularidad, alcance). Dos enunciados con idéntico texto pero distinto alcance son afirmaciones distintas.
3. **Aristas de reducción/transformación como objetos de primera clase**, cada una con su propia obligación de solidez, auditable por separado de sus extremos.
4. **Invalidación determinista** que propaga sobre un log de eventos *append-only*, encadenado por hash y reproducible, desde el cual se reconstruye toda otra vista.

## Arquitectura de tres capas
- **Capa fundacional:** kernels lógicos pequeños y confiables (Lean 4/mathlib como primer backend, más Rocq, Isabelle, Mizar, Metamath). MIRADOR *no* es una fundación.
- **IR operacional (el objeto del paper):** las células, su identidad/versionado, aristas tipadas, ciclo de vida epistémico y log de eventos.
- **Sandbox de descubrimiento:** espacio no-autoritativo para especulación, separado del corpus promovido por una **membrana unidireccional** (la especulación puede *proponer* una célula `[OPEN]`, nunca *aseverar* una `[PROMOTED]`).

## Mecanismos clave
- **Cinco relaciones de identidad** mantenidas separadas: sintáctica, representacional (`content_hash`), de versión (`version_hash`), certificada-equivalente (link explícito con prueba) y probable (similitud tipo embedding). *La similitud propone; el link certificado adjudica* — un embedding nunca establece equivalencia.
- **Ciclo de vida epistémico de 11 estados** con tabla de transiciones cerrada y **separación de poderes** mecánica: quien genera una afirmación no puede promoverla; la promoción requiere el *Congress* con revisión de contexto fresco de independencia ≥3 y sin veto pendiente. `[SUSPECT]` es el único estado de entrada automática (por invalidación).
- **Dos auditorías de grafo continuas:** aciclicidad de la justificación y consistencia definición–prueba (comparación de hashes).
- **Compilación ≠ correspondencia:** que una declaración formal type-checkee certifica el objeto formal contra el kernel, nunca que corresponda al enunciado informal.

## Evidencia
- **Implementación de referencia** (~900 líneas, stdlib + jsonschema) con esquema ejecutable y **28 tests deterministas** que fijan cada invariante.
- **Caso de estudio: el problema del plank afín de Bang** (triángulo). Reconstrucción gobernada de un piloto manual real de 2026; corpus de 13 células, 55 eventos. Reconstruye mecánicamente **cinco fallos documentados** que una representación plana ocultaría — el emblemático: un teorema probado para C¹¹¹ (un plank por dirección) pero enunciado para C (constante irrestricta), que aquí hashea distinto y no puede fusionarse silenciosamente.
- Protocolo de evaluación falsable contra tres baselines (documento/RAG, grafo de conocimiento genérico, biblioteca formal).

## Límites declarados
No resuelve la equivalencia de enunciados ni "entiende" matemática; la canonicalización es **representacional, no semántica** (conservadora por necesidad → trata como distintas muchas afirmaciones que un humano llamaría iguales). Los operadores de descubrimiento están **especificados, no implementados**. La evidencia descansa en **n=1** campaña reconstruida: motiva los mecanismos, no los evalúa a escala.

## Conexiones
Sustrato de [[bourbaki-engine]] (gobernanza), [[proofcontext]] (recuperación) y [[theoryforge]] (descubrimiento); su contrato de célula es el objetivo de ingesta de [[mathingestbench]] y sus estados epistémicos alimentan la taxonomía de [[congressbench]].
