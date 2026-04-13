# disclaimer

This document establishes, on the record, what Mirkwood is, what it is not, what claims the project makes, what claims it does not make, and what uses are outside its scope. It exists so that any future reader, hostile or sympathetic, encounters the project's own framing before encountering anyone else's.

---

## What Mirkwood is

A research instrument demonstrating that per-channel pseudonymity across independent surveillance channels fails under joint observation. The demonstration runs entirely on synthetic data produced by a physically contained lab. The published artifacts are a conceptual architecture, a synthetic-scenario demonstration, and documentation. The reproduction path is not published and will not be.

## What Mirkwood is not

Mirkwood is not operational tooling. It is not a field kit. It is not a surveillance platform. It is not a guide to building one. It is not an offensive capability against any person, agency, or organization. It is not evidence admissible in any proceeding, because it has never touched real data. It is not a recipe, a how-to, a tutorial, or a starting point for construction. No part of this repository is intended to function as such, and no part of this repository is sufficient to function as such.

## The honest claim

Mirkwood demonstrates reconstruction parity, not operational parity.

Reconstruction parity means the ability to turn ambient emissions that were already observable into a coherent after-the-fact picture. That is strictly weaker than operational parity, and strictly stronger than the status quo in which the joint pattern across channels was treated as deniable by default. The research contribution is the framing: treat ambient emissions as a joint distribution, not as independent channels.

The project does not claim to level any field. It claims to make one specific form of after-the-fact reconstruction legible to the accountability community, under conditions that do not meaningfully advantage the side that already had the data advantage.

## Bidirectionality

The methodology is symmetric. The math that allows after-the-fact reconstruction in one direction allows it in the other. The power asymmetry between institutional surveillance operators and the people subject to their attention does not disappear because the math is symmetric. It reasserts itself at the next layer up, where sensors, compute, legal cover, and operational tempo decide the exchange.

This project publishes on one side of that symmetry because the side with the resource advantage does not need the framing made legible to them, and the side without it does. That is a defensible position. It is not a claim of parity.

## Prohibited framings and uses

The following uses are explicitly outside the scope of this project and are disclaimed in advance:

Real-time identification of individuals from fused emissions. The methodology is a tool for establishing patterns. It is not a tool for identifying specific people in the moment, and any attempt to use it that way will produce false positives at a rate that guarantees harm to people who are not the intended subject.

Offensive action on the basis of a correlation lock. A correlation lock is a hypothesis, not an identification. Acting on one without independent verification is the failure mode the tool's apparent precision most actively disguises. The project takes no position on what action, if any, is appropriate after verification, and takes the strongest possible position against action before it.

Construction of a field-deployable version against real targets. The synthetic envelope is load-bearing. Removing it changes the legal, ethical, and operational character of the work entirely, and nothing in this repository authorizes or assists that change.

Use as a predicate for doxxing, harassment, stalking, confrontation, interference with lawful operations, or coordination of illegal acts. Passive listening to unencrypted radio and BLE sniffing are, in most jurisdictions, individually lawful. Acting on the fused product for any of the above purposes is not, and the lawfulness of the inputs does not transfer to the lawfulness of the outputs.

## The false-positive problem, stated plainly

Cross-modal correlation against unhardened emissions produces loose matches at a rate that is not intuitive. A BLE hash that stays within 80 meters of a radio logical ID across several transitions is a hypothesis worth investigating under lawful process. It is not proof of identity. It is not proof of affiliation. It is not proof of presence. Anyone who treats it as proof is going to hurt someone who is not the target, and the tool's interface will not stop them, because the interface is designed to surface candidates, not to adjudicate them.

This is the operational risk that matters most and the one most actively disguised by the apparent precision of the output.

## Legal exposure

Nothing in this document constitutes legal advice. The project is not a law firm and its author is not your lawyer. The following is stated as caution, not counsel.

Passive reception of unencrypted radio and BLE traffic is generally lawful in most jurisdictions. Acting on the fused product in ways that block, dox, harass, stalk, confront, or interfere is not, and courts have consistently treated pattern evidence differently when gathered through lawful process (FOIA, discovery, independent investigation) versus real-time operational exploitation. The methodology described in this project is offered to the first category, not the second. Anyone contemplating the second should consult counsel before, not after, and should understand that the lawfulness of the inputs does not transfer to the lawfulness of the outputs.

## The synthetic envelope

The synthetic-data boundary is not a temporary convenience. It is the permanent operating envelope of the project. It is enforced by the physics of a contained lab, not by software policy, and it is the condition under which the research can demonstrate a methodology without touching real identifiers, real locations, real people, or real evidence.

Anyone wanting to weaponize the idea in an operational context would still have to build the full pipeline themselves, including the capture hardware, the firmware, the ingestion adapters, the schema, the thresholds, the calibration, and the deployment stack. None of that is in this repository. None of that will be in this repository. The build discipline, the emission discipline, the operational noise, and the detection surface required to do that work are real costs that this project deliberately does not reduce.

## No warranty, no liability

This project is provided as research documentation, with no warranty of any kind. The author makes no representation that the published artifacts are correct, complete, fit for any purpose, or safe to apply in any context. Any use of the ideas in this repository is at the sole risk and responsibility of the user. The author accepts no liability for any outcome arising from any use or misuse of any part of this project.

## Contact

Correspondence regarding the methodology, its accountability applications, or principled disagreement with any claim in this document is welcome through the project's public contact surface. Requests for withheld implementation details, hardware specifications, firmware, schemas, thresholds, or any other component of the reproduction path will be ignored. Requests framed as operational use cases will be ignored and, depending on content, reported.

This document is the record. If a future reader encounters Mirkwood in any context, this is the framing the project itself offers, in its own words, before any other.
