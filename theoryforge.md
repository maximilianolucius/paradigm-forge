# TheoryForge — Operator-Guided Autonomous Theory Formation in Mathematics

*Subtítulo: From Theorem Proving to Definitions, Invariants, Reductions, and Research Programs*

- **Autores:** Maximiliano Lucius y José Minich (ITBA)
- **Fecha:** Checkpoint draft, 12 de julio de 2026
- **DOI:** [10.5281/zenodo.21329229](https://doi.org/10.5281/zenodo.21329229)
- **Rol en el ecosistema:** el **Discovery Plane** del Bourbaki Engine (la pieza generativa antes ausente).
- **Estado:** *checkpoint de diseño y protocolo.* La arquitectura, especificación de operadores, diseño de benchmark y protocolo están escritos y anclados en las bases de código existentes; **no se corrió ningún experimento de descubrimiento y no se reporta ningún número**. Las secciones que dependen de experimentos congelados-y-ejecutados están marcadas ⏳ TO COMPLETE, sin cifras fabricadas.

## Problema
Un modelo que imprime un enunciado desconocido **no** hizo un descubrimiento. El descubrimiento exige que el enunciado sea **verdadero** (o su falsedad sea un contraejemplo útil), **novedoso**, **no trivial** y **útil** para trabajo posterior — cuatro ejes que se juzgan por separado y ninguno se establece por fluidez.

## Cinco niveles, dos objetivos
Se distinguen (i) búsqueda de pruebas, (ii) generación de conjeturas, (iii) síntesis de programas, (iv) **formación de teoría** (red de definiciones, afirmaciones, transformaciones y estructura explicativa) y (v) **formación de programas de investigación**. TheoryForge apunta a (iv) y (v), usando (i)–(iii) como componentes — la línea que lo separa de agentes solo-de-prueba (QED, RMA, Prover Agent, LEAP) y de la búsqueda de programas (FunSearch, AlphaEvolve).

## Tarea
Dado un corpus gobernado `C@v` (células a versión content-addressed congelada), una frontera `F`, una constitución de campaña `K` y un presupuesto `B` dividido en seis reservas (exploit, explore ortogonal, contraejemplo, formalizar, barrera, reproducir), producir células tipadas creadas **solo** en estado `[DRAFT]`/`[OPEN]`/`[EVIDENCE]`, cada una con genealogía completa y frontera de información fija, maximizando **Verified Knowledge Gain** por unidad de costo — donde "verificado/novedoso" lo decide maquinaria independiente aguas abajo, **nunca** TheoryForge.

## Los trece operadores de descubrimiento
Interfaz tipada común (nombre, tipos de entrada, precondiciones, transform, estado de salida, validadores, modos de fallo, costo). Salida acotada a `[EVIDENCE]`; testigos numéricos re-chequeados en aritmética exacta (una grilla nunca es prueba):

`SPECIALIZE`, `GENERALIZE`, `COUNTEREXAMPLE_FIRST`, `NEGATE_AND_SEARCH`, `DUALIZE`, `CHANGE_REPRESENTATION`, `TRANSFER`, `MINIMAL_MISSING_LEMMA`, `EXTREMALIZE`, `STABILITY`, `BARRIER_SEARCH`, `FORMALIZE_EARLY`, `SYNTHESIZE_DEFINITION`.

(Dos operadores — `TRANSFER`, `CHANGE_REPRESENTATION` — dependen de un retrieval de analogía cross-domain que ProofContext aún no expone: un hueco v1.)

## Arquitectura y separación de autoridad
Loop cerrado pero con autoridad separada: constitución → Evidence Bundle de ProofContext → análisis de frontera → selección de operador → generación de candidatos → experimentos exactos y búsqueda de contraejemplos → formalización/certificado → **Congress independiente** → resultado promovido o fallo estructurado → corpus y priors actualizados. Como escribe a través de MIRADOR (cuya tabla de transiciones prohíbe que un autor promueva su propia célula y exige `actor=congress`, independencia ≥3), la propiedad "sin autoridad de promoción" es **estructural**. Reusa tres sustratos: MIRADOR (representación), ProofContext (retrieval) y una interfaz de reviewer compatible con CongressBench.

## Evaluación — TheoryBench (diseñado, no corrido)
Escalera de cuatro mundos: **Tier A** mundos sintéticos con reglas ocultas y verdad exacta (evidencia cuantitativa primaria); **Tier B** retrospectivo (time-slice de un corpus antes de que apareciera una definición/lema — contaminación tratada como amenaza de primer orden); **Tier C** tareas inéditas de expertos externos; **Tier D** frontera abierta bajo gobernanza experta. Protocolo preregistrado con matriz baseline/ablación y gauntlet de verificación de cinco etapas (colisión ProofContext → contraejemplo → certificado/formal → Congress de contexto fresco ≥C4 con ≥2 familias de modelo → revisión experta externa). **Verified Knowledge Gain** es un registro multidimensional, nunca un escalar opaco.

## Caso de estudio: plank afín de Bang (congelado)
Corpus hash-pinned de una campaña submission-ready sobre el triángulo (269 archivos, ≈32.9 MB, commit `68d176cd`). *Rediscovery targets* (resultados probados: marco de transport-defect, teorema de la mediana con rigidez, caracterización de tres direcciones que zanja la pregunta de Gardner, obstrucción del normalizador, y C¹¹¹∆(τ₀)=1 vía certificado de 812.651 hojas). *Novelty targets*: los subproblemas genuinamente abiertos (constante de tres planks para tripletes no-concurrentes/no-facet, el thin-plank lemma, etc.). **Contaminación declarada:** el manuscrito de Bang es público → la rediscovery no puede reportarse como libre de contaminación; se etiqueta *rediscovery*, nunca novedad. Dos honestidades notables: se **reporta pero no se resuelve** una discrepancia — no existe ningún objeto "2F+1M" del brief en el corpus congelado (requiere aclaración del PI), y no se inventa.

## Decisiones que bloquean el freeze (B1–B8)
Backend formal (Lean nombrado pero no instalado; recomienda certificado exacto + checker como tier default); huecos de ProofContext (sin retrieval de analogía ni cliente de colisión); contaminación de Bang; target "2F+1M" inexistente; presupuesto y familias de modelo; target de frontera y experto; composición del Congress; escalar VKG.

## Conexiones
Es el Discovery Plane de [[bourbaki-engine]]; consume [[mirador]] (célula/identidad), [[proofcontext]] (Evidence Bundles) y una interfaz de reviewer de [[congressbench]]; opera sobre corpus poblados por [[mathingestbench]]. Es el nivel que la propuesta **ParadigmForge** rediseña como *población evolutiva* de programas y sobre el que apila TechniqueForge/BridgeForge/ConceptForge/DomainFoundry.
