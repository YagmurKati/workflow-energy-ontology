# Workflow Energy Ontology

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24010863.svg)](https://doi.org/10.5281/zenodo.24010863)

An ontology for scientific workflow runs and their energy and carbon accounting.
Each energy, carbon, CPU time, memory and duration value is stored together with how it was obtained.

It was designed as an extension of the [VIVO](https://vivoweb.org/) ontology and is used in the FONDA VIVO portal
(https://vivo-fonda.hu-berlin.de/). It is plain OWL and can be used in any RDF store.

## Files

| File | Content |
|---|---|
| [`workflow-energy-core.ttl`](workflow-energy-core.ttl) | the core: 16 classes, 140 properties (40 object, 100 data), and the annotation property `rm:unit` |
| [`research-software.ttl`](research-software.ttl) | optional: 5 classes for research software around workflows (benchmarks and datasets, data systems, workflow environments, workflow scheduling, workflow tools and plugins). Imports the core. |

Namespace: `http://example.org/ontology/run-metadata#` (prefix `rm:`), the same in both files.

Units: every data property with a unit names it with `rm:unit`, as a [QUDT](https://qudt.org/) unit,
for example `rm:energyKWh rm:unit unit:KiloW-HR` (`unit:` = `http://qudt.org/vocab/unit/`). Gigabytes are 10^9 bytes.

## Core classes

| Class | Meaning |
|---|---|
| `rm:Workflow` | a published workflow, as code |
| `rm:WorkflowStage` | one named step of a workflow (for example a Nextflow process) |
| `rm:RunMetadata` | one execution of a workflow |
| `rm:WorkflowProcessRun` | the work of one step in one run (all its tasks, summed) |
| `rm:InputDataset`, `rm:ComputeCluster`, `rm:Backend`, `rm:WorkflowEngine`, `rm:Language` | inputs, cluster, execution backend, engine and language |
| `rm:Trace`, with `rm:RunTrace` and `rm:TraceCollection` | archived traces of one run, or of a group of runs |
| `rm:ResearchField`, `rm:WorkflowPurpose`, `rm:ApplicationDomain` | research context of a workflow |
| `rm:VisibilityLevel` | who can see a run |

Run and step records hold energy, carbon, CPU time, memory and duration. Each value has a property that says how
it was obtained, for example `rm:energyCalculationMethod`, `rm:energyMetricSource`, `rm:energyMeasurementCoverage`,
`rm:energyCalculationUsesFallbackEstimate`, `rm:cpuTimeCalculationMethod`, `rm:memoryCalculationMethod`,
`rm:idlePowerMethod`, `rm:temperatureCalculationMethod` and `rm:carbonCalculationMethod`.
Carbon values also record the intensity source, zone, time window and emissions basis.

Other vocabularies used: FOAF (`foaf:Person`), BIBO (`bibo:Document`), and from PROV-O only `prov:Entity`,
`prov:startedAtTime` and `prov:endedAtTime`.

## Use

- **Any RDF store** (for example Apache Jena Fuseki or GraphDB): load `workflow-energy-core.ttl`.
- **VIVO 1.15**: Site Admin > Add/Remove RDF data > "add mixed RDF (instance and/or ontology)", format Turtle.
  New properties are hidden on public pages until their display permissions are set.

The collectors that write these records from Kubernetes and Slurm clusters, and the VIVO pages that show them, are in
https://github.com/YagmurKati/fonda-vivo-run-publisher.

## Version and license

Version 0.9.4 (2026-11-08). All versions: [10.5281/zenodo.24010863](https://doi.org/10.5281/zenodo.24010863). Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

## Changes

- 0.9.4 (2026-11-08): new annotation property `rm:unit`. The 25 data properties with a unit in their name
  now also give it as a QUDT unit. No class or property changed, so existing data stays valid.
- 0.9.3 (2026-11-08): the range of `rm:purpose` and `rm:accessStatement` is now `rdfs:Literal` (it was `xsd:string`),
  because their values in VIVO carry a language tag, and a reasoner treats such a value as outside `xsd:string`.
- 0.9.2 (2026-11-07): new property `rm:slurmAccountingEnergyKWh`, a second measurement of a run's energy
  next to the main value `rm:energyKWh`.
- 0.9.1 (2026-11-07): the range of `rm:applicationDomainOf` is now `rm:Workflow`, the inverse of `rm:applicationDomain`
  (it was `rm:RunMetadata`). `foaf:Person` and `bibo:Document` are declared as classes. Both found with the
  OOPS! pitfall scanner.
- 0.9 (2026-11-06): first release, DOI [10.5281/zenodo.24010864](https://doi.org/10.5281/zenodo.24010864).

## Funding

Deutsche Forschungsgemeinschaft (DFG), Project No. 414984028 (SFB 1404 FONDA).
