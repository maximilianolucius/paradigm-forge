# MathIngestBench — Measuring Faithful Domain Ingestion from Mathematical Textbooks

*Subtítulo: From Pages to Versioned Claims, Dependencies, and Retrieval-Ready Mathematical Knowledge*

- **Autor:** Maximiliano Lucius (Independent Researcher / Aureus Technologies)
- **Fecha:** Checkpoint draft, 12 de julio de 2026
- **DOI:** [10.5281/zenodo.21329350](https://doi.org/10.5281/zenodo.21329350)
- **Rol en el ecosistema:** el **frente faltante** del pipeline — cómo entra la matemática conocida (de libros de texto) al sustrato de células.
- **Estado:** *checkpoint de protocolo y contratos.* Todo lo previo a ingerir un libro está hecho (tarea, gates de madurez, auditoría de corpus/licencias, arquitectura, contratos de datos, métricas, diseño experimental, threat model). **Todo lo empírico está sin hacer** y marcado `[PENDING — fase]`: ninguna fuente congelada, ningún gold standard construido, ningún pipeline corrido, ningún número reportado. Gate alcanzado: **pre-G0**.

## Problema
Un sistema RAG moderno ya responde muchas preguntas *sobre* un libro de matemática. De ahí **no** se sigue que haya **ingerido** su matemática. Responder "¿qué es el teorema de rango-nulidad?" devolviendo el párrafo requiere solo proximidad léxica/semántica. *Usar* ese teorema con seguridad — saber que los espacios deben ser de dimensión finita, qué "rango" es el correcto, que depende de una definición dada tres secciones antes, y que una edición posterior lo renumeró — exige reconstruir el conocimiento operacional que la prosa deja implícito. Eso es **ingesta de dominio**.

## Tesis
Responder preguntas superficiales sobre un libro no es ingerir su matemática. La unidad de salida **no** se inventa aquí: es la **Célula Matemática tipada y versionada de MIRADOR**. MathIngestBench no define un nuevo modelo de conocimiento; mide si un pipeline puede *poblar el existente fielmente*, y valida esa fidelidad end-to-end corriendo los consumidores aguas abajo (ProofContext, TheoryForge) sobre el corpus ingerido. Deliberadamente **no** es un benchmark de OCR ni de chunking-RAG.

## Diez capacidades → ingesta
Se distingue la ingesta de diez capacidades más débiles con las que se confunde (cada una prerequisito de la siguiente, ninguna de las primeras diez implica ingesta): extracción de bytes/texto → OCR/fórmulas → estructura del documento → segmentación de teorema/definición/prueba → extracción semántica de afirmaciones → resolución de símbolos con alcance → reconstrucción de dependencias → alineación formal (similitud vs equivalencia certificada) → indexado para retrieval → **ingesta completa de dominio** (todo lo anterior, versionado, con procedencia completa, invalidation-aware, útil aguas abajo, y con casos no resueltos **abstenidos en vez de adivinados**).

## Tarea y gates de madurez
Función de ingesta `S: (Source, Condition, Budget) → (Cells, Graph, Indexes, Obligations, Manifest)`. La ingesta es **actividad proponente**: una célula extraída de una fuente publicada es a lo sumo `[EVIDENCE]`, nunca `[PROMOTED]` (un validador rechaza cualquier envelope con estado superior). La confianza de extracción es un **tercer eje**, ortogonal a tipo y estado, y vive en el envelope, no en la célula.

Cuatro **condiciones de entrada nunca mezcladas al reportar**: I1 fuente nativa (LaTeX), I2 PDF limpio (renderizado del mismo contenido I1 congelado → delta puramente representacional), I3 PDF degradado/escaneado (receta de degradación congelada), I4 multi-fuente (libro + errata + edición posterior + solape con biblioteca formal).

Ocho **gates preregistrados G0–G7**: Preserved → Parsed → Structured → Semantic → Graphed → Retrieval-ready → Update-safe → Discovery-ready. Un dominio se llama "ingerido" solo al gate que su uso previsto requiere; nunca "comprensión completa".

## Corpus recomendado y contaminación
Auditoría de licencias (share-alike es *contagioso*; ND descalifica una fuente porque una célula extraída es un derivado). **Dominio primario recomendado: *Linear Algebra* de Hefferon** (álgebra lineal de grado, fuente LaTeX pública, licencia derivative-friendly, espina de prerequisitos casi acíclica, solape parcial con mathlib para medir tasa de falsa-equivalencia). **Caveat de contaminación declarado up front:** el libro es público desde 1996 → casi seguro en el training data de cualquier LLM; amenaza de memorización de primer orden que obliga a un *memorization probe*, un set de notación perturbada por el autor (superficie nunca publicada) y reporte separado nativo/PDF/degradado.

## Gold standard y arquitectura
Tres fuentes de verdad cruzadas (backbone LaTeX nativo parseado deterministicamente; anotación humana dual con experto adjudicando; chequeos formales/ejecutables para afirmaciones seleccionadas, con alineación a mathlib *confirmada por humano*, nunca inferida por similitud de nombre). Pipeline de referencia de **ocho etapas**, cada una produce células/aristas/índices **o** emite una obligación no resuelta tipada (refusal de primera clase): preservación de fuente → reconstrucción de documento → parsing matemático (tablas de símbolos por alcance) → extracción semántica → reconstrucción de dependencias (cada arista inferida registra extractor y confianza) → alineación formal → indexado ProofContext-compatible → quality gates y refusal.

## Métricas
**Vector de doce familias**, nunca un solo score, cada una función pura sobre salidas crudas: fidelidad de fuente; estructural; extracción de células; fidelidad semántica; símbolos/notación; grafo de dependencias (**aristas directas puntuadas aparte de la reachability transitiva** — recuperar reachability pero alucinar aristas directas no es sólido); procedencia; alineación formal (incl. **tasa de falsa-equivalencia**); retrieval readiness; utilidad aguas abajo; eficiencia; confiabilidad. Modos de fallo (células alucinadas, dependencias falsas, equivalencias falsas) se reportan **desagregados** y son **veto metrics** para promover un gate; `UKY` (usable knowledge yield) es cantidad de planeamiento operacional que nunca puede enmascarar una mala métrica de fallo.

## Diseño experimental
10 baselines (B1–B10, de chunk-RAG a pipeline completo + experto-in-the-loop), ablaciones cuyo outcome reportado es **aguas abajo** (¿qué componentes mejoran razonamiento/retrieval?), splits por bloque coherente (capítulo), controles de fuga (memorization probe antes de puntuar I2/I3, agentes downstream pinneados por hash, test sellado corrido **una sola vez**). N honesto: dentro de un libro congelado el gold es casi un censo (baja error de muestreo pero no generaliza entre libros; N independiente efectivo ≈ número de bloques coherentes).

## Conexiones
Es el frente de ingesta que apunta al contrato de célula de [[mirador]] (y hereda su regla "extracción no promueve"); alimenta a [[proofcontext]] (cuya capa de ingesta está vacía por diseño — "ProofContext-ready" = células `mirador-cell/1.0` válidas en un grafo tipado) y a [[theoryforge]] (Experimento G: ¿la ingesta fiel mejora el descubrimiento válido sin subir la falsa promoción?); su guardrail aguas abajo es la FTPR de [[congressbench]]; el Trust Plane del [[bourbaki-engine]] sigue siendo la única autoridad de promoción, que MathIngestBench nunca toca.
