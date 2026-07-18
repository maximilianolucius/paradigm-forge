# The Bourbaki Engine — A Governed Discovery Architecture for Long-Horizon Autonomous Mathematics

- **Autores:** Maximiliano Lucius y José Minich (ITBA, CABA/Córdoba)
- **Fecha:** Draft, 12 de julio de 2026
- **DOI:** [10.5281/zenodo.21328015](https://doi.org/10.5281/zenodo.21328015)
- **Rol en el ecosistema:** el **paper arquitectónico paraguas** que integra todos los demás.

## Problema
Los sistemas autónomos ya producen matemática de nivel de investigación plausible. Aparecen dos restricciones *distintas*: **admisión** (¿bajo qué protocolo una afirmación generada puede volverse dependencia load-bearing?) y **descubrimiento** (¿cómo se produce nueva estructura a lo largo de un programa que sobrevive a cualquier contexto único?). Gobernar la acumulación no genera estructura; una máquina que solo evita promociones falsas puede quedarse eternamente en una frontera vacía.

## Tesis dual (Principle 1)
La matemática autónoma confiable requiere **dos sistemas acoplados pero institucionalmente separados**:
- **Discovery Plane:** expande el espacio de posibilidades; puede ser exploratorio, redundante, especulativo e internamente contradictorio (juzgado por *yield* y diversidad; se le permite equivocarse).
- **Trust Plane:** gobierna qué salidas se vuelven conocimiento reutilizable; conservador, auditable, controlado por un TCB pequeño y explícito (juzgado por su tasa de falsas promociones; se le permite ser lento).

Ninguno puede tener la autoridad del otro. La **membrana** entre ambos es unidireccional en autoridad: Discovery propone nodos/aristas; solo Trust promueve. Colapsarlos —dejar que un generador certifique su propia salida— es precisamente el fallo que la arquitectura existe para prevenir.

## Espina preservada del predecesor
La promoción de afirmaciones como unidad de progreso confiable; obligaciones de solidez a nivel de arista; separación epistémica de poderes; independencia de contexto fresco como hipótesis falsable; disciplina de certificados (`search ≠ evidence ≠ certificate ≠ checker`); sustrato *event-sourced*; invalidación dirigida por dependencias hacia `[SUSPECT]`; y la **False Theorem Promotion Rate (FTPR)** como métrica de confianza primaria.

## Siete módulos (con ledger de madurez explícito)
Reemplaza el goal graph por un **hipergrafo de investigación tipado** (tipo de objeto ⊥ estado epistémico). Módulos:
1. **Mirador** — representación (implementado, prototipo + caso de estudio). → [[mirador]]
2. **ProofContext** — retrieval que devuelve Evidence Bundles (implementado, con evaluación controlada). → [[proofcontext]]
3. **Discovery Plane** — biblioteca de operadores machine-actionable (especificado). → [[theoryforge]]
4. **Plano de formalización** (Lean/mathlib) — especificado.
5. **Laboratorio computacional certificado** — una instancia (certificado de 812.651 hojas + checker).
6. **Congress adversarial ampliado** — manualmente ejercido.
7. **Scheduler jerárquico de investigación** — especificado.

Ningún módulo alcanza validación externa (columna X vacía): nada ha sobrevivido peer review como sistema.

## Piezas destacadas
- **Constitución de campaña:** documento congelado y hash-pinned que fija target, definiciones, backends/axiomas admisibles, requisitos de certificación, política de novedad, métricas y condiciones de parada/retirada **antes** de expandir el grafo. Hace la falla emblemática del piloto (target C atacado como C¹¹¹) detectable *antes de escribir una sola prueba*.
- **Congress:** roles Stratège, Scribe, Expérimentateur, Bibliothécaire, Réfutateurs, Auditeur, Greffier + nuevas lentes de contexto fresco (correspondencia informal–formal, novedad/prioridad, procedencia de retrieval, asunción/alcance, independencia del checker, integración/invalidación). Promoción requiere **unanimidad**; el veto es un objeto estructurado.
- **Principio de independencia de contexto fresco (falsable):** P(defecto detectado | fresco) > P(defecto detectado | historia de generación compartida), a lo largo de una escalera de independencia — hipótesis que el piloto (n=1) motiva pero no puede zanjar (la mide [[congressbench]]).
- **Memoria event-sourced:** el log es la fuente de verdad (encadenado por hash, patrón de *certificate transparency*); el grafo es una caché truth-maintained con propagación de retracción; el corpus (*Les Éléments*) es inmutable y content-addressed; la memoria heurística no tiene autoridad.

## Escalera de madurez R0–R9 y programa de tres pistas
R0 (representación) y R1 (retrieval) tienen evidencia de prototipo; **R2–R9 abiertos**. El tope (R9, validación comunitaria) **el motor no puede autodeclararlo**. Tres pistas concurrentes: A (ingeniería/reconstrucción, el piloto de Bang), B (evaluación científica ciega con problemas de matemáticos externos, la única forma honesta de medir descubrimiento), C (preparación Millennium — solo constitución/atlas/definiciones/barreras hasta pasar gates de A y B).

## Posición
El motor **no** promete resolver un problema del Milenio; un problema Millennium es un *stress test* con puerta cuya "solución" se define enteramente fuera del motor. El activo reutilizable es **horizontal**: infraestructura de razonamiento verificado aplicable dondequiera que las afirmaciones sean baratas de generar y caras de confiar. Un sistema que resuelve más problemas *subiendo* su FTPR es, por este estándar, peor.

## Conexiones
Integra [[mirador]], [[proofcontext]], [[theoryforge]] y [[congressbench]]; su plano de ingesta lo cubre [[mathingestbench]]. Es el marco que la propuesta **ParadigmForge** (Bourbaki Novelty Stack) busca extender de "gobernar conocimiento" a "gobernar la mutación del conocimiento".
