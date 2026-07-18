# ProofContext — Dependency-Aware Retrieval of Verifiable Context for Mathematical Reasoning

- **Autor:** Maximiliano Lucius (Aureus Technologies / Independent Researcher, CABA)
- **Fecha:** 2026
- **DOI:** [10.5281/zenodo.21325289](https://doi.org/10.5281/zenodo.21325289)
- **Rol en el ecosistema:** capa de **recuperación** (retrieval), montada de solo-lectura sobre MIRADOR.

## Problema
La RAG convencional indexa pasajes y los rankea por similitud léxica o de embedding. En matemática eso no es solo imperfecto sino **activamente inseguro**: dos enunciados casi idénticos en redacción pueden diferir en un cuantificador, una dimensión, una asunción de simetría, una normalización, una versión de definición o un estatus epistémico. Una recuperación "tópicamente perfecta" puede ser matemáticamente inutilizable o engañosa.

## Tesis y tarea nueva
En lugar del pasaje más similar, un recuperador debe devolver **el contexto verificable más pequeño en el que una afirmación puede usarse con seguridad**. Se llama **reconstrucción de proof-context**: compilar la consulta en una **Problem Signature** tipada y devolver el **Evidence Bundle** mínimo — definiciones, premisas cerradas por dependencia, procedencia, barreras y estado epistémico — dependency-sufficient y sin servir ningún near-miss incompatible.

## Componentes definitorios
1. **Filtros duros de compatibilidad matemática:** cada candidato se verifica contra ejes tipados (asunción/alcance/versión) con veredicto `compatible / incompatible / requires_translation / unknown`. Un candidato incompatible en un eje tipado se **rechaza sin importar su score de similitud**. La direccionalidad importa (un resultado para todo *d* especializa a *d=2*; uno de *d=2* no responde a *d* general).
2. **Membrana de autoridad:** el retrieval puede rankear y anotar (crear anotaciones `[DRAFT]`) pero **nunca promover**, certificar ni convertir similitud en equivalencia. Cualquier intento lanza `MembraneViolation`; un run deja el sustrato byte-idéntico.
3. **Invalidación consciente de versión:** cuando se revisa una definición o se refuta una afirmación, MIRADOR propaga `[SUSPECT]`; ProofContext extiende esto al retrieval — reconstruye índices y **invalida por hash todo bundle cacheado** que citara una célula degradada. Una afirmación obsoleta ya no se sirve como autoritativa.

## Pipeline
Problem Signature → generación híbrida de candidatos (BM25 léxico, LSA denso, índice de fórmulas/símbolos, índice de declaraciones formales, difusión sobre el grafo de dependencias) → **filtros duros de compatibilidad (antes del reranking)** → rerank lineal transparente `S(c,σ)` (cada término inspeccionable, devuelto en el bundle; ordena, no es una probabilidad de verdad) → expansión mínima de dependencias → Evidence Bundle con reporte de compatibilidad (near-misses rechazados **registrados**, no descartados en silencio).

## Evaluación — MathScopeBench
Benchmark adversarial de near-misses matemáticos, anclado en la literatura del plank afín de Bang. Dos splits inmutables: 14 queries dev (pares documentados del piloto) y 10 test (mutación controlada de un solo eje, held-out). Categorías: cover type (C¹¹¹ vs C), simetría, normalización, dimensión, alcance de conclusión, autoridad epistémica, sub-recuperación de barreras, misma-notación/distinta-definición.

**Resultado principal:** en las categorías duro-rechazables, ProofContext sirve **0%** de near-matches incompatibles vs **44–75%** de léxico/denso/híbrido/híbrido-sin-filtros, a recall igual o mayor. La ventaja **desaparece exactamente al quitar los filtros** (ablación → aísla a los filtros como causa). Dependency sufficiency sube a 0.93 (vs 0.43); correct refusal 1.00 (vs 0.00). Implementación determinista (LSA con seed fijo, sin descarga de embeddings neuronales), 21 tests. Robustez: rechaza células envenenadas y prompt-injection (tratado como dato); mismo query dos veces → hash de bundle byte-idéntico.

## Posicionamiento honesto
No reclama como novedad la selección de premisas, la búsqueda densa, el query rewriting ni la expansión de grafo (LeanSearch v2, ReProver/LeanDojo, GraphRAG, HyDE son prior art). La novedad defendible: **(1)** filtrado duro de compatibilidad tipada y **(2)** la membrana de autoridad — ningún sistema de retrieval previo las provee.

## Límites
Corpus piloto de 25 células y 24 queries: demostración de mecanismo, no estudio IR a escala; los números absolutos son ilustrativos, el hallazgo *comparativo* es la afirmación. La colisión notación-igual/definición-distinta **no** se rechaza en duro (se expone como conflicto). El compilador de Problem Signature es keyword-driven sobre un dominio. Downstream medido como proxy (sin prover vivo).

## Conexiones
Importa el sustrato [[mirador]] de solo-lectura. Provee Evidence Bundles a [[theoryforge]]; su evaluación de near-miss se reutiliza en [[mathingestbench]] (Experimento F); es el componente de retrieval del [[bourbaki-engine]]; su valor promotor lo testea la condición C5 de [[congressbench]].
