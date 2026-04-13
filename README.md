# Mirkwood
To see is to be bound.

A synthetic-data demonstration of cross-channel correlation as a reconstruction methodology for passively observable emissions.

-----

What this is

Operators conducting surveillance across multiple channels — trunked voice, short-range wireless beacons, vehicle plate events — frequently rely on per-channel pseudonymity (rotating talkgroup assignments, randomized device identifiers, handle systems) as an operational assumption that activity across channels is not jointly observable. That assumption systematically overstates the operator’s position. A passive observer capturing multiple channels simultaneously can collapse pseudonyms across channels by temporal and spatial correlation alone, without breaking any individual identifier, without decrypting any protected payload, and without querying any cooperative infrastructure.

The gap between “each channel is individually deniable” and “the joint distribution is not deniable” is the subject of this project. It is not a cryptographic failure. It is a threat model failure that persists because the operators with the largest interest in maintaining it are the ones least likely to publish about it.

Mirkwood is a research instrument, not an operational platform. It exists to characterize a reconstruction methodology that is already available to any sufficiently-motivated observer, and to make that methodology legible to the accountability community investigative journalists, civil rights litigators, public defenders, academic oversight researchers, and institutional review bodies whose work depends on establishing patterns of activity that were designed to be deniable.

Scope boundary

Mirkwood operates on fully synthetic data. No real captures are performed, ingested, stored, or displayed at any point in the pipeline.**

The synthetic corpus is produced by a scenario generator modeled on the publicly documented characteristics of realistic RF and sensor environments. All identifiers, locations, timestamps, and events are fabricated. The synthetic-data boundary is load-bearing: it is the condition under which this research can demonstrate a methodology without prejudicing any lawfully-collected evidence, any FOIA-released records, any ongoing investigation, or any individual’s privacy. Any real-world application of the methodology belongs to the people who hold the lawful predicate to apply it, under the standards that govern their work.

This boundary is not a temporary convenience. It is the project’s operating envelope.

What the demonstration shows

A unified spatiotemporal event store ingests three representative synthetic channels into a common schema, and exposes a natural-language analyst interface that translates intent into structured queries against the fused corpus. Representative queries of the form *“show all short-range device hashes that maintained ±80m proximity to logical ID 0x3A9F during its last 5 tactical transitions”* resolve to correlation locks in real time against the synthetic scenario.

The accessibility of the interface is part of the argument. The methodology does not require RF domain expertise or query-language fluency from its user. That accessibility is the point: a threat whose practical barrier is “knowing the right SQL” has a different risk profile from one whose practical barrier is “typing the question in English.” The latter is the honest description of where the field is.

What is not provided

This repository does not include, and will not include, materials sufficient to reproduce the correlation methodology against real-world data. Specifically withheld:

- Ingestion adapters for any real-world protocol
- Schema definitions beyond those necessary to exercise the synthetic scenario
- Correlation thresholds, windowing parameters, and estimator tuning
- The scenario generator’s calibration against real environments
- Any configuration presets, deployment guides, or hardware specifications

## Methodology for accountability use

The research output that is generalizable, and that is offered to the accountability community, is the methodology itself rather the observation that crossmodal correlation against lawfully collected or lawfully released ambient data can establish patterns of activity whose individual channels were believed to be deniable. The productive applications, for practitioners holding the lawful predicate:

- Reconstruction of activity patterns from FOIA released records that span multiple channels or sensor modalities
- Pattern analysis in civil rights litigation where individual incidents are disputed but the aggregate pattern is the claim
- Corroboration of testimony against telemetry whose provenance is independently established
- Independent review of official incident reconstructions where the underlying data is available under lawful process

In each case, the methodology is a framing treat ambient emissions as a joint distribution, not as independent channels or a tool. The tool is whatever the practitioner already has lawful access to; Mirkwood does not need to be in the loop for the methodology to apply.

## Ethical considerations

Countersurveillance forensics has an unavoidable dual use property: a methodology that lets the accountability community reconstruct a surveillance operator’s activity is the same methodology the operator uses against their targets. This project does not pretend otherwise. The justification for publishing on one side of that symmetry and not the other is the power asymmetry between institutional operators and the people subject to their attention: the operators have resources, legal cover, and secrecy; the targets have, at best, ambient evidence and a legal system that requires patterns of practice to act. Research that makes the reconstruction side legible, under a synthetic data envelope and without a reproduction path, is a contribution to that asymmetry’s correction. Research that shipped a turnkey reconstruction tool would not be.

The author acknowledges this calculation is contestable and welcomes principled disagreement. Contestable is not the same as wrong; it is the condition under which research in this domain proceeds responsibly.

## Status

In active development. The published artifacts consist of the conceptual architecture, the synthetic scenario demonstration, and this document. The reproduction path is not, and will not be, among the published artifacts.

## Contact

Correspondence regarding the methodology, the accountability applications, or principled objections is welcome through the project’s public contact surface. Requests for withheld implementation details are not.
