# CongressBench — Measuring False Theorem Promotion in Agentic Mathematics

*Subtítulo: Fresh-Context Review, Epistemic Separation, and Statement-Boundary Defects*

- **Autores:** Maximiliano Lucius y José Minich
- **Fecha:** Checkpoint draft, 12 de julio de 2026
- **DOI:** [10.5281/zenodo.21328878](https://doi.org/10.5281/zenodo.21328878)
- **Rol en el ecosistema:** el **instrumento de evaluación del Trust Plane** (el análogo del Congress a lo que MathScopeBench es para ProofContext).
- **Estado:** *checkpoint.* Protocolo, taxonomía, mecanismo de aislamiento, encoding del caso Bang y análisis de potencia completos; harness testeado end-to-end con un reviewer sintético. **No se reportan resultados empíricos de reviewers** — ejecutar el estudio confirmatorio está bloqueado por presupuesto fijo y un backend formal cableado.

## Problema
Un agente que no encuentra una prueba falla visible y barato. Uno que **acepta una prueba errónea** falla invisible y caro: el resultado falso entra al corpus, se cita y corrompe todo lo construido encima. El fallo dominante no es la incapacidad de probar sino la aceptación de un artefacto plausible-pero-incorrecto: una **falsa promoción de teorema**.

## Qué mide
El Bourbaki Engine plantea un principio explícitamente falsable:
> P(defecto detectado | contexto fresco) > P(defecto detectado | historia de generación compartida),
a lo largo de una escalera de independencia (self-review; watched; single fresh-context; multi-lens panel).

El motor lo motiva con **una** campaña (n=1: "seis auditorías internas no vieron lo que un pase externo sí"; todo defecto ocurrió *en un límite de enunciado*, no dentro de una prueba). CongressBench está construido para zanjarlo en **n>1**.

**Definición central — FTPR:** un artefacto está *falsamente promovido* bajo una condición de revisión cuando un artefacto defectuoso recibe veredicto ACCEPT. `FTPR = #{defectuosos aceptados} / #{defectuosos revisados}`. Es una cantidad *por-condición*, nunca colapsada con detección o costo en un solo score. *Un sistema que resuelve más subiendo su FTPR es peor.*

## Contribuciones
1. **Tarea + taxonomía de 15 clases de defecto** [impl], cada clase con un mecanismo dueño en un sistema companion (así "¿lo atrapó el reviewer?" queda bien planteado). Cada ítem se marca por localidad: *node-local* (dentro del cuerpo de prueba), *statement-boundary* (en el enunciado/contexto), *edge/dependency* (en las aristas tipadas) — el pivote de la hipótesis H4.
2. **Seis condiciones de aislamiento epistémico C1–C6** [impl], mapeadas verbatim a la escalera de independencia de MIRADOR. C1–C3 reviewers únicos; C4–C6 paneles de tres lentes. C5 añade Evidence Bundle de ProofContext; C6 añade chequeo formal y auditoría de correspondencia. **El aislamiento se *impone*, no se pide**: el harness arma el contexto de cada reviewer solo con las claves permitidas y un checker independiente *anula* (voids) cualquier observación filtrada en vez de puntuarla.
3. **Diseño clusterizado preregistrado** [design]: regresión logística de efectos mixtos con clustering por artefacto base, familia de hipótesis congelada (H1–H4 confirmatorias en el Stratum A, controladas por Holm α=0.05) y análisis de potencia computado.
4. **Harness open-source determinista** [impl]: corpus, aislamiento, adjudicación, scoring y potencia, validado end-to-end con un `StubReviewer`.

## Corpus (tres estratos)
- **Stratum A — mutaciones formalmente fundadas** [planned]: mutaciones controladas de teoremas base version-pinned (revertir cuantificador, quitar hipótesis, bumpear versión de definición, debilitar arista, introducir circularidad, formalizar enunciado más débil, promover evidencia numérica). Un mutante es `CONFIRMED` solo tras que un prover/cómputo exacto verifique que la propiedad falla en el mutante y vale en la base. Los mutadores son [impl]; falta cablear el backend Lean4/mathlib.
- **Stratum B — artefactos informales de expertos** [planned/deferred]: sin panel asegurado.
- **Stratum C — la campaña de Bang** [impl]: 8 defectos documentados como pares defectuoso/limpio con ground truth histórico; se mantiene **fuera** de la inferencia agregada (n=1).

## Análisis de potencia (evidenciado)
Como las observaciones se agrupan por base, el tamaño ingenuo se infla por DEFF = 1+(m−1)·ICC. En el punto operativo candidato (FTPR base 0.45, ICC 0.10, potencia 0.80) el diseño necesita **40 artefactos base (240 ítems)**; el ICC se re-estimará de un piloto antes del freeze.

## Honestidad metodológica
No se reporta ningún resultado de reviewer. El self-test con `StubReviewer` **encodea las direcciones hipotetizadas por construcción** — testea que el harness *mide* esas direcciones, no que *se cumplan* en la realidad (14 tests deterministas, ~4s, incluidos cuatro que verifican que el contexto filtrado se anula, y reproducibilidad bit-a-bit). Amenazas cubiertas: contaminación de modelo, defectos sintéticos, error de adjudicación, fuga de contexto (anulada mecánicamente), dependencia de familia de modelo, no-independencia (inferencia base-clusterizada), etc. La amenaza viva mayor: *el estudio está sin correr*.

## Conexiones
Se apoya en [[mirador]] (célula tipada + escalera de independencia) y [[proofcontext]] (Evidence Bundle, testeado por C5); mide la hipótesis de contexto fresco del [[bourbaki-engine]]; su esquema de salida de reviewer (`review.schema.json`) es el contrato público que consume [[theoryforge]] como "reviewer compatible con CongressBench"; su FTPR es la guardrail aguas abajo de [[mathingestbench]].
