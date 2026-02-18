# PQEA -- Post-Quantum Embodied Agent Governance

* **Specification Version:** 1.0.0
* **Status:** Implementation Ready
* **Date:** 2026
* **Author:** rosiea
* **Contact:** [PQRosie@proton.me](mailto:PQRosie@proton.me)
* **Licence:** Apache License 2.0 — Copyright 2026 rosiea
* **PQ Ecosystem:** CORE — The PQ Ecosystem is a post-quantum security framework built on deterministic enforcement, fail-closed semantics, and refusal-driven authority. Bitcoin is the reference deployment. It is not the scope.

---

**Namespace status:** `pqea.*` is a reserved receipt namespace. Consumers MUST treat `pqea.*` receipts as invalid unless policy explicitly permits the namespace and pins the producing specification version and hash.

---

## Summary

PQEA defines deterministic, evidence-only governance artefacts for embodied autonomous agents (robots, drones, autonomous vehicles, industrial manipulators, and any AI-mediated system capable of causing physical side effects) operating under post-quantum security constraints.

It specifies canonical intent binding, schema registry mechanisms, constraint map binding, runtime integrity profiles, drift and perception uncertainty evidence, execution lease and heartbeat re-evaluation patterns, delegation receipts, outcome attestation receipts, environment model integrity evidence, safety state evidence, and a minimal multi-agent coordination hook.

PQEA does not authorize actions and does not enforce policy. All enforcement decisions based on PQEA artefacts are evaluated exclusively by PQSEC.

---

## Index

1. [Purpose and Scope](#1-purpose-and-scope)
   - [1.1 Purpose](#11-purpose)
   - [1.2 Scope](#12-scope)
   - [1.3 Terminology](#13-terminology)
   - [1.4 Hardware Reality and Evidence Authenticity](#14-hardware-reality-and-evidence-authenticity-normative)
   - [1.5 Real-Time Separation Boundary](#15-real-time-separation-boundary-normative)
2. [Explicit Dependencies](#2-explicit-dependencies)
3. [Canonical Operation Envelope](#3-canonical-operation-envelope-normative)
4. [Schema Registry Mechanism](#4-schema-registry-mechanism-normative)
5. [Constraint Map Binding](#5-constraint-map-binding-normative)
6. [Runtime Profile](#6-runtime-profile-normative)
7. [Drift Evidence](#7-drift-evidence-normative)
8. [Perception Uncertainty Discipline](#8-perception-uncertainty-discipline-normative)
   - [8.5 Perception Insufficiency Mapping](#85-perception-insufficiency-mapping-normative)
9. [Safety State Evidence](#9-safety-state-evidence-normative)
   - [9A. SafetyStateEvidence Profiles](#9a-safetystateevidence-profiles-normative)
10. [Execution Lease and Heartbeat Re-Evaluation](#10-execution-lease-and-heartbeat-re-evaluation-normative)
    - [10A. Lease/Control Window Separation](#10a-leasecontrol-window-separation-normative)
    - [10B. Governance Boundary Reinforcement](#10b-governance-boundary-reinforcement-informative)
11. [Outcome Attestation](#11-outcome-attestation-normative)
12. [Delegation for Embodied Sub-Operations](#12-delegation-for-embodied-sub-operations-normative)
13. [Environment Model Integrity](#13-environment-model-integrity-normative)
14. [Multi-Agent Coordination Hook](#14-multi-agent-coordination-hook-normative)
15. [Supervision Lattice and Autonomy Levels](#15-supervision-lattice-and-autonomy-levels-normative)
    - [15A. Actuation Domain Scoping](#15a-actuation-domain-scoping-normative)
16. [Deliberation Enforcement Class Escalation](#16-deliberation-enforcement-class-escalation-informative)
17. [Refusal Codes](#17-refusal-codes-normative)
18. [Signing Discipline](#18-signing-discipline-normative)
19. [Interfaces and Required Flows](#19-interfaces-and-required-flows-normative)
    - [19.2A Evaluation Ordering and Real-Time Safety](#19-2a-evaluation-ordering-and-real-time-safety-informative)
20. [Authority Boundary](#20-authority-boundary-normative)
21. [Out of Scope and Governance Limits](#21-out-of-scope-and-governance-limits-normative)
22. [Conformance Checklist](#22-conformance-checklist-normative)
23. [Conformance Test Scenarios](#23-conformance-test-scenarios-normative)
* [Annex A -- Reference Pseudocode: Intent Hash](#annex-a----reference-pseudocode-intent-hash)
* [Annex B -- Reference Pseudocode: Lease Heartbeat Loop](#annex-b----reference-pseudocode-lease-heartbeat-loop)
* [Annex C -- Receipt Signing Rule](#annex-c----receipt-signing-rule)
* [Acknowledgements](#acknowledgements-informative)

---

## 1. Purpose and Scope

### 1.1 Purpose

Embodied agents convert digital instructions into physical side effects. Governance failures in embodied systems are often caused by:

* ambiguous action parameters (stringified JSON, unit drift, injection),
* lack of deterministic constraints binding,
* lack of runtime integrity binding to the executing adapter,
* acting under inadequate perception conditions (occlusion, sensor fault),
* long-running operations that continue after context changes (revoked consent, worsening drift),
* ungoverned delegation chains to sub-systems (doors, elevators, payment endpoints),
* inability to verify whether the action matched the intent after execution.

PQEA defines protocol artefacts that make these conditions enforceable without granting authority.

### 1.2 Scope

PQEA normatively defines:

* canonical intent binding for embodied operations,
* schema registry conventions for operation parameter schemas,
* constraint map encoding, hashing, and binding rules,
* runtime profile receipts (adapter integrity and update invalidation),
* drift evidence receipts with domain scoping,
* perception uncertainty evidence receipts and mandatory refusal thresholds,
* safety state evidence receipts (including E-Stop state),
* execution lease and heartbeat re-evaluation pattern for long-running actions,
* outcome attestation receipts bound to `intent_hash`,
* delegation receipts for bounded sub-operations,
* environment model integrity receipts (map/model provenance and freshness),
* a minimal multi-agent coordination hook (evidence-only),
* supervision lattice vocabulary for policy mapping,
* refusal codes and conformance tests.

PQEA does not define robot coordinate frames, units, navigation algorithms, manipulation planning, collision avoidance, or actuator control loops. Those are domain and platform concerns.

### 1.3 Terminology

* **Embodied agent:** a system that can produce physical side effects.
* **Adapter:** the executable control bridge that actuates tools and motors.
* **Operation schema (`op_schema`):** a named, versioned parameter schema identifier.
* **ConstraintMap:** deterministic constraint structure bound by hash.
* **RuntimeProfile:** evidence identifying and constraining the executing adapter build.
* **DriftEvidence:** fixed-point evidence indicating domain drift.
* **PerceptionEvidence:** fixed-point evidence indicating whether sensing is sufficient.
* **SafetyStateEvidence:** evidence including E-Stop and safety controller state.
* **ExecutionLease:** time-bounded lease requiring periodic re-evaluation.
* **Heartbeat:** periodic evidence confirming continued admissibility.
* **OutcomeReceipt:** evidence of completion and measured outcome binding.
* **DelegationReceipt:** bounded, revocable delegation for sub-operations.

### 1.4 Hardware Reality and Evidence Authenticity (Normative)

Implementations MUST NOT simulate or fabricate:

* PerceptionEvidence
* SafetyStateEvidence
* RuntimeProfileEvidence
* AdapterIntegrityEvidence

if the underlying hardware or trusted execution source cannot produce such evidence.

If required hardware inputs are unavailable or unverifiable, the system MUST:

* Refuse Authoritative embodied operations that depend on those artefacts, OR
* Downgrade to a declared implementation profile that prohibits such operation classes.

Implementations MUST NOT claim conformance for operation categories that depend on absent hardware capability.

Evidence artefacts MUST correspond to measurable, hardware-backed, or attested sources.

Paper compliance without hardware capability is non-conformant.

### 1.5 Real-Time Separation Boundary (Normative)

PQEA governance operates at the evidence and policy evaluation layer. It does not operate inside hard real-time control loops.

1. PQSEC predicate evaluation for embodied operations MUST occur at governance-layer boundaries: lease issuance, lease renewal (heartbeat), and operation admission. PQSEC MUST NOT be invoked at control-loop frequency (e.g. servo update rates, sensor polling rates).
2. Between governance evaluation points, the adapter executes under the authority of the active ExecutionLease. The adapter is responsible for real-time safety within the bounds of the lease.
3. GovernanceCadence constraints (PQSEC 18X) apply to the frequency at which PQSEC re-evaluates predicates for embodied operations. These constraints prevent governance-layer jitter from coupling to control-loop timing.
4. Hardware-level safety mechanisms (E-Stop, torque limits, collision avoidance) operate independently of PQEA governance and MUST NOT be gated on PQSEC evaluation latency.
5. If a safety-critical condition occurs between governance evaluation points, the adapter MUST enter safe-stop independently using its own safety mechanisms. The adapter MUST then report the event via `pqea.execution_abort` at the next governance evaluation point.

This separation ensures that PQEA governance adds verifiable evidence to embodied operations without introducing latency into safety-critical control paths.

---

## 2. Explicit Dependencies

| Specification | Minimum Version | Purpose |
|---|---:|---|
| PQSF | >= 2.0.3 | Canonical encoding, CryptoSuiteProfile indirection, ReceiptEnvelope, evidence classification |
| PQSEC | >= 2.0.3 | Enforcement of embodied predicates, refusal semantics, governance cadence |
| PQAI | >= 1.2.0 | Tool governance patterns, drift semantics alignment, safety domain classification (optional) |
| Epoch Clock | >= 2.1.0 | Deterministic time binding (ticks) |

PQEA MAY reference PQSEC Annex AV (Deliberation Enforcement Class) for high-consequence embodied operations.

---

## 3. Canonical Operation Envelope (Normative)

PQEA defines a canonical envelope for an embodied operation request.

### 3.1 Operation Envelope

`EmbodiedOpEnvelope` MUST be deterministic CBOR:

```
EmbodiedOpEnvelope = {
  v: uint,                               ; MUST be 1
  op_schema: tstr,                       ; schema identifier
  op_params: bstr,                       ; opaque canonical payload bytes (schema-defined)
  constraints_hash: bstr(32),            ; SHAKE256-256(canonical ConstraintMap bytes)
  runtime_profile_ref: bstr(32),         ; hash reference to RuntimeProfile receipt
  drift_refs: [* bstr(32)] / null,       ; hashes of drift evidence receipts
  perception_refs: [* bstr(32)] / null,  ; hashes of perception evidence receipts
  safety_refs: [* bstr(32)] / null,      ; hashes of safety state evidence receipts
  session_id: bstr(16) / null,
  exporter_hash: bstr(32) / null,
  issued_tick: uint,
  expiry_tick: uint,
  intent_hash: bstr(32)                  ; derived as defined in 3.2
}
```

### 3.2 Intent Binding

`intent_hash` MUST be:

```
intent_hash = SHAKE256-256(canonical_cbor(EmbodiedOpEnvelope_without_intent_hash))
```

Rules:

1. `intent_hash` MUST be computed with the `intent_hash` field omitted or set to CBOR null during hashing.
2. `op_params` MUST be canonical CBOR bytes as defined by `op_schema`. `op_params` is opaque at this layer.
3. Any change to any field (including refs, ticks, or `constraints_hash`) MUST change `intent_hash`.
4. `issued_tick` and `expiry_tick` MUST be Epoch Clock ticks. `current_tick >= expiry_tick` MUST cause refusal.
5. `expiry_tick` MUST be strictly greater than `issued_tick`. If `expiry_tick <= issued_tick`, the envelope MUST be refused as invalid.

### 3.3 Determinism Requirement

Given identical inputs, independent implementations MUST compute byte-identical `intent_hash` values.

---

## 4. Schema Registry Mechanism (Normative)

### 4.1 Schema Identifier Format

`op_schema` MUST be a dot-separated identifier:

```
pqea.params.<domain>.<op>.v<N>
```

Where:

* `<domain>` MUST be a DNS-style identifier or subdomain-like token set: lowercase ASCII, digits, hyphen, dot.
* `<op>` MUST be lowercase ASCII, digits, hyphen, dot.
* `v<N>` MUST be `v` followed by a positive integer.

Examples:

* `pqea.params.nav.move_to.v1`
* `pqea.params.manip.grasp.v1`
* `pqea.params.purchase.buy_item.v1`
* `pqea.params.acme.robotics.dock.v1`

### 4.2 Schema Rules

1. Schema definitions MUST be immutable for a given identifier.
2. Any backward-incompatible change MUST increment the version suffix.
3. Schema definitions MUST be deterministic CBOR structures (no floating point).
4. Unknown schema identifiers MUST cause refusal when schema validation is required.

### 4.3 Schema Registration

PQEA does not publish parameter schemas. Deployments MUST publish a registry mapping `op_schema` identifiers to deterministic schema definitions and validation rules. The schema registry is a deployment-managed resource.

---

## 5. Constraint Map Binding (Normative)

Constraint maps are PQ artefacts and MUST use SHAKE256-256 for hashing.

### 5.1 Constraint Map Structure

```
ConstraintMap = {
  v: uint,                          ; MUST be 1
  schema: tstr,                     ; MUST equal op_schema
  allowlist: map / null,            ; deployment-defined, deterministic types only
  bounds: map / null,               ; deployment-defined, fixed-point only
  geofence_ref: bstr(32) / null,
  spend_policy_ref: bstr(32) / null,
  safety_policy_ref: bstr(32) / null
}
```

### 5.2 Constraint Hash

```
constraints_hash = SHAKE256-256(canonical_cbor(ConstraintMap))
```

Rules:

1. `ConstraintMap.schema` MUST equal the operation `op_schema`.
2. `constraints_hash` MUST be included in `EmbodiedOpEnvelope` and validated by PQSEC.
3. Unknown fields are permitted only if the deployment documents them as deterministic and schema-bound.

### 5.3 Constraint Validation Rules

1. If `ConstraintMap.schema != op_schema`, refuse with `E_EMBODIED_CONSTRAINT_SCHEMA_MISMATCH`.
2. If computed `constraints_hash` does not match the envelope, refuse with `E_EMBODIED_CONSTRAINT_HASH_MISMATCH`.
3. Constraint widening (e.g., larger geofence, higher speed cap, higher spend cap) MUST be treated as an Authoritative policy mutation. Policy MAY require DEC for constraint widening operations.

---

## 6. Runtime Profile (Normative)

Runtime integrity is enforced by binding the executing adapter build to a signed profile.

### 6.1 Receipt Type

ReceiptEnvelope.type: `pqea.runtime_profile`

### 6.2 Minimum Fields

```
RuntimeProfile = {
  v: uint,                      ; MUST be 1
  runtime_id: bstr(32),         ; SHAKE256-256(canonical runtime manifest)
  adapter_id: bstr(32),         ; SHAKE256-256(adapter binary bytes or image bytes)
  op_schema_allowlist: [* tstr],
  issued_tick: uint,
  expiry_tick: uint / null,
  suite_profile: tstr,
  signature: bstr
}
```

### 6.3 Validation Rules

1. Canonical encoding MUST verify.
2. Signature MUST verify under `suite_profile`.
3. `runtime_id` and `adapter_id` MUST match the executing runtime and adapter identity computed locally.
4. If `op_schema` is not in `op_schema_allowlist`, refuse with `E_EMBODIED_OP_SCHEMA_NOT_PERMITTED`.

### 6.4 Adapter Update Invalidation (Normative)

Adapter updates change `adapter_id`.

Rules:

1. If the adapter binary or image changes, `adapter_id` changes.
2. A changed `adapter_id` MUST cause the existing RuntimeProfile to fail validation.
3. The system MUST fail closed until a new signed RuntimeProfile is issued and validated.
4. Implementations MUST NOT grandfather old runtime profiles across adapter updates. This is a security boundary.
5. Adapter updates MUST be classified as Authoritative operations.
6. Policy MAY require DEC classification for adapter updates.

#### Operator State Evidence (Optional)

Embodied or tele-operated systems MAY use Neural Lock as an operator state evidence source for coercion and impairment resistance.

Neural Lock evidence is descriptive only and MUST NOT grant authority. Any operation gating based on operator state remains enforced by PQSEC via policy predicates.

If Neural Lock evidence is used, the runtime profile MUST declare:
- required operation classes for operator evidence
- accepted Neural Lock profile identifiers
- freshness budgets for operator attestations

---

## 7. Drift Evidence (Normative)

### 7.1 Receipt Type

ReceiptEnvelope.type: `pqea.drift_evidence`

### 7.2 Evidence Structure

```
EmbodiedDriftEvidence = {
  v: uint,                              ; MUST be 1
  intent_hash: bstr(32),
  drift_domain: tstr,                   ; see 7.3
  drift_value: uint,
  drift_scale: uint,
  drift_state: "NONE" / "WARNING" / "CRITICAL",
  indicators: [* tstr] / null,
  issued_tick: uint,
  expiry_tick: uint,
  suite_profile: tstr,
  signature: bstr
}
```

### 7.3 Drift Domains

`drift_domain` MUST be one of:

* `navigation`
* `manipulation`
* `perception`
* `comms`
* `purchase`
* `other:<deployment_namespace>`

`<deployment_namespace>` MUST be a DNS-style identifier: lowercase ASCII letters, digits, and hyphens only, dot-separated labels permitted, each label 1-63 characters, total length <= 253, MUST NOT begin or end with a hyphen.

Non-conforming namespace values MUST be rejected.

### 7.4 Drift Rules

1. Fixed-point only. No floats.
2. Drift evidence MUST be time-bounded.
3. Drift evidence MUST bind to `intent_hash` when used for gating.
4. CRITICAL drift MUST be treated as refusal for operations that require that domain.

### 7.5 Drift Threshold Policy

Thresholding is policy-defined. PQEA does not hardcode drift threshold enforcement. PQSEC evaluates drift evidence under active policy.

---

## 8. Perception Uncertainty Discipline (Normative)

### 8.1 Rationale

Embodied agents MUST refuse to act when sensing is insufficient for safe execution. Existing robotics frameworks treat perception uncertainty as a planner input. PQEA treats it as a first-class refusal condition with verifiable evidence.

### 8.2 Receipt Type

ReceiptEnvelope.type: `pqea.perception_evidence`

### 8.3 Evidence Structure

```
PerceptionEvidence = {
  v: uint,                                  ; MUST be 1
  intent_hash: bstr(32),
  perception_domain: tstr,                  ; deployment-defined or schema-bound
  confidence_value: uint,
  confidence_scale: uint,
  sufficiency: "SUFFICIENT" / "INSUFFICIENT",
  degradation_factors: [* tstr] / null,
  sensor_subset_hash: bstr(32) / null,
  issued_tick: uint,
  expiry_tick: uint,
  suite_profile: tstr,
  signature: bstr
}
```

### 8.4 Perception Rules

1. Fixed-point only.
2. If `sufficiency = INSUFFICIENT`, operations that require perception MUST be refused.
3. Absence of required perception evidence MUST be treated as UNAVAILABLE and fail closed for Authoritative operations.

### 8.5 Perception Insufficiency Mapping (Normative)

Perception Insufficiency represents a hardware-level UNAVAILABLE state.

For all actuation domains classified as Authoritative, PQSEC MUST promote UNAVAILABLE perception evidence to DENY.

Perception Insufficiency MUST NOT be treated as a soft failure or advisory warning for Authoritative actuation.

PQEA does not define refusal semantics. Enforcement remains exclusively within PQSEC.

---

## 9. Safety State Evidence (Normative)

This section provides a governance-visible safety gate without implementing real-time motor safety.

### 9.1 Receipt Type

ReceiptEnvelope.type: `pqea.safety_state`

### 9.2 Evidence Structure

```
SafetyStateEvidence = {
  v: uint,                                    ; MUST be 1
  intent_hash: bstr(32) / null,               ; MAY be unbound for session scope
  estop_active: bool,
  safety_controller_state: "OK" / "DEGRADED" / "FAULT",
  fault_codes: [* tstr] / null,
  issued_tick: uint,
  expiry_tick: uint,
  suite_profile: tstr,
  signature: bstr
}
```

### 9.3 Safety State Rules

1. If `estop_active = true`, all Authoritative operations MUST be refused.
2. If `safety_controller_state = FAULT`, all Authoritative operations MUST be refused.
3. If safety evidence is required by policy and absent, treat as UNAVAILABLE.

### 9.4 E-Stop Override Rule (Normative)

If `estop_active = true`:

1. E-Stop state MUST override drift, perception, and other positive predicates.
2. Existing execution leases MUST terminate.
3. Execution heartbeats MUST fail closed.
4. No predicate combination may override an active E-Stop.

### 9A. SafetyStateEvidence Profiles (Normative)

#### 9A.1 Purpose

This section defines profile-scoped requirements for SafetyStateEvidence, enabling policy to require different evidence depth for different deployment types.

#### 9A.2 Profile Identifiers

```
SafetyProfile = "basic" / "industrial" / "mobile_platform" / "surgical"
```

**basic:** Requires `estop_active` and `safety_state` fields only.

**industrial:** Requires `estop_active`, `safety_state`, and at least one zone-monitoring evidence reference (e.g. light curtain state, safety PLC heartbeat).

**mobile_platform:** Requires `estop_active`, `safety_state`, and at least one motion-monitoring evidence reference (e.g. IMU state, proximity sensor array state).

**surgical:** Requires `estop_active`, `safety_state`, force-monitoring evidence, and instrument-state evidence. This profile is advisory and deployment-defined until domain-specific standards are integrated.

#### 9A.3 Profile Binding

1. The active SafetyProfile MUST be declared in the RuntimeProfile for the adapter.
2. If policy requires a SafetyProfile and the presented SafetyStateEvidence does not satisfy the profile requirements, PQSEC MUST refuse with `E_EMBODIED_SAFETY_NOT_OK`.
3. SafetyProfile is evidence classification only. It does not alter E-Stop override semantics (9.4).

---

## 10. Execution Lease and Heartbeat Re-Evaluation (Normative)

This section closes the continuous authorization gap for long-running physical actions.

### 10.1 Model

For operations expected to exceed a policy-defined duration, execution MUST be governed by an ExecutionLease with periodic Heartbeat re-evaluation. Authorization is not permanent. It decays, and must be renewed.

### 10.2 Receipt Types

* `pqea.execution_lease`
* `pqea.execution_heartbeat`
* `pqea.execution_abort`
* `pqea.execution_complete` (see Section 11)

### 10.3 ExecutionLease Structure

ReceiptEnvelope.type: `pqea.execution_lease`

```
ExecutionLease = {
  v: uint,                          ; MUST be 1
  lease_id: bstr(16),
  intent_hash: bstr(32),
  issued_tick: uint,
  expiry_tick: uint,
  heartbeat_interval_ticks: uint,
  suite_profile: tstr,
  signature: bstr
}
```

Rules:

1. Lease MUST be time-bounded and short relative to the operation.
2. `heartbeat_interval_ticks` MUST be > 0.
3. Lease validity MUST be checked using Epoch Clock ticks.
4. `expiry_tick` SHOULD be strictly greater than `issued_tick + heartbeat_interval_ticks`. Leases that expire before the first heartbeat is due are valid but will terminate immediately.

### 10.4 Heartbeat Structure

ReceiptEnvelope.type: `pqea.execution_heartbeat`

```
ExecutionHeartbeat = {
  v: uint,                          ; MUST be 1
  lease_id: bstr(16),
  intent_hash: bstr(32),
  seq: uint,
  issued_tick: uint,
  suite_profile: tstr,
  signature: bstr
}
```

Rules:

1. Heartbeat `issued_tick` MUST be monotonic non-decreasing for the same `lease_id`.
   - If `issued_tick` equals the prior heartbeat `issued_tick`, `seq` MUST still strictly increase.
   - If `issued_tick` is less than the prior heartbeat `issued_tick`, refuse with `E_EMBODIED_LEASE_INVALID`.
2. Heartbeat `seq` MUST be strictly increasing per `lease_id`. Reuse or rollback MUST cause refusal with `E_EMBODIED_LEASE_INVALID`.
3. Heartbeats MUST be emitted no less frequently than `heartbeat_interval_ticks`.
4. Missing heartbeat MUST cause safe-stop behaviour (see 10.6).

### 10.5 Heartbeat Re-Evaluation Rule

At each heartbeat boundary, the executor MUST submit to PQSEC:

* the active `EmbodiedOpEnvelope`,
* the active `ExecutionLease`,
* the latest Heartbeat,
* any required drift, perception, and safety evidence refreshed within their expiry windows.

PQSEC MUST re-evaluate required predicates. If PQSEC refuses or is unavailable, the executor MUST enter safe-stop.

### 10.6 Safe-Stop Requirement

ReceiptEnvelope.type: `pqea.execution_abort`

When lease expires, heartbeat is missing, or PQSEC returns DENY:

1. The executor MUST transition to a safe-stop state.
2. The executor MUST emit `pqea.execution_abort` with reason code.

```
ExecutionAbort = {
  v: uint,
  lease_id: bstr(16),
  intent_hash: bstr(32),
  reason: tstr,
  issued_tick: uint,
  suite_profile: tstr,
  signature: bstr
}
```

### 10A. Lease/Control Window Separation (Normative)

#### 10A.1 Purpose

This section clarifies the relationship between ExecutionLease validity and control-loop execution, preventing confusion between governance-layer lease boundaries and real-time control timing.

#### 10A.2 Rules

1. ExecutionLease `expiry_tick` defines the governance validity window. It determines when PQSEC governance authority over the operation expires.
2. Control-loop execution rate is determined by the adapter and the physical system, not by lease timing. An adapter MAY execute thousands of control iterations within a single lease window.
3. GovernanceCadence (PQSEC 18X) constrains the frequency of PQSEC re-evaluation, not the frequency of control-loop execution.
4. Between heartbeat boundaries, the adapter operates under the authority of the active lease. No PQSEC evaluation is required per control-loop iteration.
5. If the lease expires or a heartbeat is missed, the adapter MUST enter safe-stop regardless of control-loop state.

#### 10A.3 Timing Invariant

```
control_loop_period << heartbeat_interval_ticks << lease_duration_ticks
```

This invariant is not enforced by PQEA but represents the expected relationship. Deployments where `heartbeat_interval_ticks` approaches control-loop frequency are misconfigured and will experience governance-layer jitter.

### 10B. Governance Boundary Reinforcement (Informative)

ExecutionLease does not replace predicate evaluation. It constrains execution duration between governance checkpoints.

At no time may an actuation command be initiated without a prior ALLOW outcome from PQSEC. Lease issuance presupposes successful predicate evaluation, including perception sufficiency where required.

---

## 11. Outcome Attestation (Normative)

Outcome attestation closes the governance loop: did the action match the intent?

### 11.1 Receipt Type

ReceiptEnvelope.type: `pqea.execution_complete`

### 11.2 Structure

```
ExecutionComplete = {
  v: uint,                            ; MUST be 1
  lease_id: bstr(16),
  intent_hash: bstr(32),
  outcome_hash: bstr(32),
  outcome_class: "SUCCESS" / "FAIL" / "PARTIAL" / "ABORTED",
  sensor_subset_hash: bstr(32) / null,
  issued_tick: uint,
  suite_profile: tstr,
  signature: bstr
}
```

### 11.3 Outcome Rules

1. `outcome_hash` MUST be SHAKE256-256 over canonical CBOR of a schema-defined outcome object for the `op_schema`.
2. `outcome_hash` is evidence only. It does not prove correctness, but it binds the executor's measured outcome statement to the `intent_hash`.
3. Every completed or aborted execution MUST emit an `ExecutionComplete` receipt.
4. Completion attestation does not retroactively authorize execution. If `outcome_class` is FAIL or ABORTED, policy MAY require supervisory review before subsequent operations using the same `op_schema`.
5. Completion attestation MUST NOT be reused as intent evidence for future operations.

---

## 12. Delegation for Embodied Sub-Operations (Normative)

Delegation allows a governed agent to request sub-operations from other systems without leaking authority.

### 12.1 Receipt Type

ReceiptEnvelope.type: `pqea.delegation`

### 12.2 Structure

```
DelegationReceipt = {
  v: uint,                           ; MUST be 1
  delegation_id: bstr(16),
  parent_intent_hash: bstr(32),
  delegated_op_schema: tstr,
  delegated_constraints_hash: bstr(32),
  valid_from_tick: uint,
  valid_until_tick: uint,
  max_uses: uint,
  suite_profile: tstr,
  signature: bstr
}
```

### 12.3 Delegation Rules

1. Delegation MUST be time-bounded and use-bounded (`max_uses`).
2. Delegated constraints MUST be equal or stricter than the parent constraints (policy-defined comparator). Delegation MUST NOT expand capability scope beyond the parent constraint map.
3. Delegation use-bounding MUST be enforced by a deterministic use counter scoped to `delegation_id`.
   - Each successful admitted use MUST increment a persistent `uses_consumed` counter for that `delegation_id`.
   - If `uses_consumed >= max_uses`, the delegation MUST be treated as invalid and MUST be refused with `E_EMBODIED_DELEGATION_INVALID`.
   - If the implementation cannot prove integrity of the `uses_consumed` counter state (for example due to state loss or corruption), the delegation MUST fail closed for Authoritative operations and MUST be refused with `E_EMBODIED_DELEGATION_STATE_LOST`.
4. Delegation is revocable by policy using standard revocation mechanisms. Absence of a revocation system MUST mean delegation is not supported.
5. Delegation grants no authority. It records the scope of a bounded sub-operation request. The delegatee's own enforcement (if any) decides admission.

---

## 13. Environment Model Integrity (Normative)

### 13.1 Purpose

Actions can be well-governed but physically unsafe if the environment model is stale or poisoned. This section enables policy to require evidence that the environment model is fresh and has known provenance.

### 13.2 Receipt Type

ReceiptEnvelope.type: `pqea.environment_model`

### 13.3 Structure

```
EnvironmentModelEvidence = {
  v: uint,                           ; MUST be 1
  model_id: tstr,
  model_hash: bstr(32),              ; SHAKE256-256(model bytes)
  model_kind: tstr,                  ; e.g. "map", "occupancy_grid", "semantic"
  issued_tick: uint,
  expiry_tick: uint,
  provenance_ref: bstr(32) / null,
  suite_profile: tstr,
  signature: bstr
}
```

### 13.4 Environment Model Rules

1. If policy requires environment integrity, this evidence MUST be present and fresh.
2. `model_hash` MUST be computed over canonical model bytes as defined by the deployment.
3. Stale or absent environment evidence when required MUST cause refusal with `E_EMBODIED_ENV_MODEL_STALE`.
4. If `current_tick >= expiry_tick`, the environment model MUST be treated as stale.

---

## 14. Multi-Agent Coordination Hook (Normative)

PQEA does not standardize inter-agent protocols, but provides a minimal evidence hook for deployments that require coordinated operations.

### 14.1 Receipt Type

ReceiptEnvelope.type: `pqea.peer_posture`

### 14.2 Structure

```
PeerPostureEvidence = {
  v: uint,
  domain_id: tstr,
  peer_id: bstr(32),
  peer_runtime_profile_ref: bstr(32),
  peer_intent_hash: bstr(32) / null,
  issued_tick: uint,
  expiry_tick: uint,
  suite_profile: tstr,
  signature: bstr
}
```

### 14.3 Peer Posture Rules

If a deployment requires coordinated operations, policy MAY require valid peer posture evidence. PQEA does not define enforcement logic for multi-agent coordination. That is a deployment concern.

---

## 15. Supervision Lattice and Autonomy Levels (Normative)

Supervision levels form a partial order that policy can map to evidence states.

### 15.1 Supervision Levels

```
TELEOPERATED < SUPERVISED < AUTONOMOUS
```

Where:

* **TELEOPERATED:** human directly controls all actions.
* **SUPERVISED:** agent operates autonomously with human oversight and interrupt capability.
* **AUTONOMOUS:** agent operates independently within constraint boundaries.

### 15.2 Policy Mapping Rules

1. Policy MAY map evidence states to required supervision levels (e.g., drift WARNING -> downgrade from AUTONOMOUS to SUPERVISED).
2. Perception INSUFFICIENT MUST refuse regardless of supervision level.
3. DEC escalation is separate and reserved for sovereignty transitions (see Section 16).
4. Supervision level is a policy input, not an enforcement primitive. PQSEC evaluates predicates; supervision vocabulary informs which predicates policy requires.
5. Supervision downgrade MUST be deterministic and policy-documented.
6. Supervision upgrade (e.g., SUPERVISED to AUTONOMOUS) MUST require an Authoritative policy mutation.

### 15A. Actuation Domain Scoping (Normative)

#### 15A.1 Purpose

This section defines actuation domain identifiers that enable policy to scope embodied governance to specific physical capability domains.

#### 15A.2 Actuation Domains

| Domain | Meaning |
|--------|---------|
| `locomotion` | Movement through space (wheels, legs, flight surfaces) |
| `manipulation` | Object interaction (grippers, arms, end-effectors) |
| `perception_active` | Active sensing that affects the environment (LIDAR emission, sonar) |
| `environmental_control` | HVAC, lighting, door locks, barrier control |
| `medical_device` | Medical instrument actuation |

Deployments MAY define additional domain identifiers. Custom domain identifiers MUST use a vendor prefix (e.g. `acme:welding_torch`).

#### 15A.3 Domain Binding Rules

1. Each `EmbodiedOpEnvelope` SHOULD include an `actuation_domain` field identifying which physical capability domain the operation targets.
2. Policy MAY require specific evidence sets per actuation domain. For example, `manipulation` operations may require force-monitoring evidence that `locomotion` operations do not.
3. If an operation references an actuation domain that is unknown or unsupported by the enforcement system, PQSEC MUST refuse with `E_EMBODIED_ACTUATION_DOMAIN_UNSUPPORTED` (PQSEC Annex AE.45).
4. Actuation domain scoping is evidence classification only. It does not grant authority or modify predicate evaluation semantics.

#### 15A.4 PQAI SafetyDomain Interaction

When a PQAI evidence artefact carries `safety_domain = "embodied_control"` (PQAI 27.13), the PQAI artefact SHOULD include a cross-reference to the applicable actuation domain defined in this section. This enables policy to correlate AI evidence with specific physical capability requirements.

---

## 16. Deliberation Enforcement Class Escalation (Informative)

PQEA operations MAY be escalated to the Deliberation Enforcement Class (DEC) defined by PQSEC Annex AV for irreversible physical actions.

### 16.1 Recommended DEC Candidates

* Entering private premises (door unlock + traversal).
* Disabling safety constraints.
* Elevating tool capability profiles.
* Mutating enforcement policy.

### 16.2 Not Recommended for DEC

DEC is not inherently required for ordinary purchases. Use spend caps and constraint maps for ordinary purchasing. Reserve DEC for sovereignty transitions.

### 16.3 Enforcement Path

If policy marks an operation as DEC-required and DEC predicates are not satisfied, predicate evaluation MUST return FALSE.

---

## 17. Refusal Codes (Normative)

PQEA registers the following refusal codes for PQSEC mapping:

| Code | Meaning |
|---|---|
| `E_EMBODIED_ENCODING_NONCANONICAL` | Envelope or receipt encoding is not canonical |
| `E_EMBODIED_INTENT_HASH_MISMATCH` | Computed intent_hash does not match |
| `E_EMBODIED_ENVELOPE_EXPIRED` | Envelope expiry_tick has passed |
| `E_EMBODIED_OP_SCHEMA_NOT_PERMITTED` | op_schema not in runtime profile allowlist |
| `E_EMBODIED_CONSTRAINT_SCHEMA_MISMATCH` | ConstraintMap.schema != op_schema |
| `E_EMBODIED_CONSTRAINT_HASH_MISMATCH` | Computed constraints_hash does not match envelope |
| `E_EMBODIED_RUNTIME_PROFILE_INVALID` | Runtime profile missing, expired, or mismatched |
| `E_EMBODIED_ADAPTER_MISMATCH` | Measured adapter identity differs from runtime profile |
| `E_EMBODIED_DRIFT_EVIDENCE_INVALID` | Drift evidence missing, expired, or malformed |
| `E_EMBODIED_DRIFT_CRITICAL` | Drift state is CRITICAL for required domain |
| `E_EMBODIED_PERCEPTION_INSUFFICIENT` | Perception sufficiency is INSUFFICIENT |
| `E_EMBODIED_PERCEPTION_UNAVAILABLE` | Required perception evidence is absent |
| `E_EMBODIED_SAFETY_NOT_OK` | Safety state is FAULT or E-Stop active |
| `E_EMBODIED_LEASE_INVALID` | Execution lease missing, expired, or mismatched |
| `E_EMBODIED_HEARTBEAT_MISSING` | Required heartbeat not received within interval |
| `E_EMBODIED_DELEGATION_INVALID` | Delegation receipt missing, expired, or scope exceeded |
| `E_EMBODIED_DELEGATION_STATE_LOST` | Delegation uses counter state lost or corrupted; fail-closed for Authoritative |
| `E_EMBODIED_ENV_MODEL_STALE` | Required environment model evidence is stale or absent |
| `E_EMBODIED_PEER_POSTURE_INVALID` | Required peer posture evidence is invalid |
| `E_EMBODIED_PROHIBITED_OPERATION` | Operation is unconditionally prohibited by policy |
| `E_EMBODIED_ACTUATION_DOMAIN_UNSUPPORTED` | Operation references unknown or unsupported actuation domain identifier |

Implementations MUST NOT emit these codes unless they are registered in PQSEC Annex AE.

---

## 18. Signing Discipline (Normative)

All PQEA receipt bodies MUST be canonical CBOR and MUST be signed over the canonical CBOR encoding with the `signature` field omitted.

1. Each receipt MUST include `suite_profile`.
2. `suite_profile` MUST reference a valid CryptoSuiteProfile (PQSF).
3. Signature verification MUST be performed by consumers (PQSEC) locally.
4. Unknown suite profiles MUST cause refusal.

---

## 19. Interfaces and Required Flows (Normative)

### 19.1 Required Host Interface

A conformant implementation MUST provide:

* `build_envelope(op_schema, op_params, constraints_hash, runtime_profile_ref, ...) -> EmbodiedOpEnvelope`
* `submit_to_pqsec(envelope, evidence_refs...) -> EnforcementOutcome`
* `start_execution(envelope) -> ExecutionLease`
* `heartbeat(lease, envelope, refreshed_evidence...) -> continue | abort`
* `complete(lease, outcome_hash, outcome_class) -> ExecutionComplete`

### 19.2 Minimal Execution Flow

1. Build `EmbodiedOpEnvelope`.
2. Compute and fill `intent_hash`.
3. Gather required evidence receipts (runtime profile, drift, perception, safety, environment model as policy requires).
4. Submit to PQSEC for evaluation.
5. If operation is long-running, obtain `ExecutionLease` and heartbeat under Section 10.
6. Execute only while lease is valid and heartbeats pass.
7. Emit completion receipt.

### 19.2A Evaluation Ordering and Real-Time Safety (Informative)

Perception and safety evidence are governance-layer inputs evaluated by PQSEC prior to any governed actuation.

Conformant implementations MUST NOT construct, emit, or transmit an actuation command for an Authoritative operation until PQSEC has evaluated all required predicates and returned an ALLOW outcome.

PQSEC evaluation short-circuits on the first failure of a required predicate (PQSEC §26.2). If perception evidence evaluates to FALSE or UNAVAILABLE for an Authoritative operation, PQSEC refuses and no actuation command is produced.

For long-running embodied operations, governance evaluation occurs at heartbeat boundaries under the ExecutionLease model (§10). Control-loop execution (e.g., servo updates or actuator cycles) occurs within lease bounds and is not subject to per-iteration governance evaluation.

If any of the following occurs during execution:

* PQSEC returns DENY,
* A required heartbeat is missed,
* The ExecutionLease expires,
* Required perception evidence becomes insufficient or unavailable,

the executor MUST enter safe-stop behaviour as defined in §10.6.

This subsection clarifies ordering and separation of governance evaluation from real-time control. It does not modify predicate semantics or enforcement requirements.

---

## 20. Authority Boundary (Normative)

PQEA artefacts are evidence only and MUST NOT grant authority, expand capability, or bypass PQSEC enforcement under any condition.

They MUST NOT:

* grant authority,
* trigger execution,
* replace PQSEC decisions,
* be interpreted as permission.

All authority decisions are produced exclusively by PQSEC.

---

## 21. Out of Scope and Governance Limits (Normative)

This specification defines governance, evidence production, and enforcement discipline for embodied autonomous agents. It does not define or implement real-time physical safety mechanisms, hardware correctness, or ground-truth validation.

### 21.1 Actuator-Level Safety Enforcement

PQEA may require evidence that safety constraints exist (for example via `pqea.safety_state`), but it does not implement motor torque limits, guarantee collision avoidance correctness, enforce hard real-time safety bounds, verify closed-loop control stability, or validate firmware-level emergency stop logic.

These functions belong to hardware, firmware, and real-time control systems. PQEA can require attestation that such systems exist and are active. It cannot implement them.

### 21.2 Sensor Ground Truth

PQEA may require perception confidence evidence, domain-scoped drift evidence, and localization integrity attestations. However, PQEA cannot prove that sensor input reflects physical reality. A compromised or spoofed sensor may emit structurally valid but false measurements. Corroboration reduces risk but does not guarantee correctness.

Sensor ground truth is a trusted computing and hardware integrity problem, not a governance-layer problem.

### 21.3 Adversarial Physical Manipulation

PQEA governs autonomous actions. It cannot prevent physical displacement of the device, forced actuation by external pressure, power cycling, hardware reconfiguration, or direct physical override of actuators.

PQEA may detect abnormal state transitions after such manipulation and refuse further operations. It cannot prevent the manipulation itself.

### 21.4 Supply-Chain Trust

Adapter integrity binding (`adapter_id`), runtime profiles, and firmware hashes reduce supply-chain risk. However, PQEA does not verify fabrication authenticity of hardware, prove absence of hardware implants, validate sensor silicon provenance, or prevent malicious firmware signed by a trusted manufacturer.

Supply-chain trust is external to governance enforcement.

### 21.5 Task Rollback and Physical Reversibility

PQEA can require completion attestations and safe-stop semantics. It cannot guarantee that a physical action is reversible, restore damaged property, or undo physical consequences. Rollback semantics require stateful control primitives defined in the robotics control layer.

PQEA may require evidence that rollback primitives were invoked. It cannot guarantee physical reversibility.

### 21.6 Trusted User Interface

PQEA may require interactive confirmation receipts, DEC classification, and supervision level declarations. It does not provide secure display hardware, guarantee absence of screen overlay attacks, validate integrity of input peripherals, or ensure a trusted path between user and device.

Trusted UI is a hardware and OS-level concern.

### 21.7 Governance Boundary Principle

PQEA governs authorization, constraint enforcement, drift gating, perception sufficiency, execution leases, supervision level transitions, adapter integrity, and evidence binding. PQEA does not govern physics, hardware correctness, real-time safety loops, or physical coercion prevention.

All enforcement decisions remain refusal-first and evidence-based. Physical reality remains outside protocol control.

These limitations apply even if all governance predicates evaluate to TRUE. An ALLOW decision at the governance layer does not imply mechanical safety, physical correctness, or environmental truth.

### 21.8 Weaponization and Harmful Applications

PQEA makes no claim about weaponization, military, or law enforcement applications. This specification is designed for consumer, industrial, and commercial embodied agents operating under holder-defined policy.

### 21.9 Determinism Requirement (Normative)

Given identical canonical inputs and identical evidence receipts, predicate evaluation MUST produce identical outcomes across conforming implementations. Floating-point arithmetic is prohibited in all canonical artefacts and evaluation logic.

---

## 22. Conformance Checklist (Normative)

A PQEA-conformant implementation MUST:

1. Compute intent hashes deterministically using SHAKE256-256.
2. Enforce schema-bound canonical `op_params`.
3. Enforce SHAKE256-256 constraint map hashing and binding.
4. Enforce runtime profile adapter integrity and update invalidation.
5. Emit fixed-point drift and perception evidence when required.
6. Refuse on perception insufficiency when required.
7. Support execution leases and heartbeats for long-running operations when claimed.
8. Emit outcome attestation receipts on completion.
9. Implement signing discipline for all receipts.
10. Enforce real-time separation boundary per 1.5.
11. If SafetyProfile is required, validate SafetyStateEvidence against profile requirements per 9A.
12. If actuation domain scoping is used, enforce domain binding rules per 15A.
13. Ensure lease/control window separation per 10A.

---

## 23. Conformance Test Scenarios (Normative)

Implementations MUST pass:

1. Intent hash mismatch -> `E_EMBODIED_INTENT_HASH_MISMATCH`.
2. Constraint schema mismatch -> `E_EMBODIED_CONSTRAINT_SCHEMA_MISMATCH`.
3. Adapter update without new RuntimeProfile -> `E_EMBODIED_RUNTIME_PROFILE_INVALID`.
4. Perception INSUFFICIENT with required perception -> `E_EMBODIED_PERCEPTION_INSUFFICIENT`.
5. Drift CRITICAL for required domain -> refusal.
6. Lease expires mid-operation -> safe-stop + `E_EMBODIED_LEASE_INVALID`.
7. Heartbeat missing -> safe-stop + `E_EMBODIED_HEARTBEAT_MISSING`.
8. Outcome receipt emitted and bound to `intent_hash` after completion.
9. Environment model stale when required -> `E_EMBODIED_ENV_MODEL_STALE`.

---

### Annex A -- Reference Pseudocode: Intent Hash

```python
def compute_intent_hash(envelope_without_intent_hash: bytes) -> bytes:
    return shake256_256(envelope_without_intent_hash)
```

---

### Annex B -- Reference Pseudocode: Lease Heartbeat Loop

```python
def execute_with_lease(envelope, lease, pqsec_client, evidence_provider):
    seq = 0
    while True:
        evidence = evidence_provider.refresh(envelope.intent_hash)

        hb = build_heartbeat(lease.lease_id, envelope.intent_hash, seq)
        seq += 1

        outcome = pqsec_client.evaluate(envelope, lease, hb, evidence)
        if outcome.decision != "ALLOW":
            safe_stop()
            emit_abort(lease, envelope.intent_hash, "pqsec_denied")
            return False

        if is_complete():
            emit_complete(lease, envelope.intent_hash, outcome_hash(), "SUCCESS")
            return True
```

---

### Annex C -- Receipt Signing Rule

Signature input = canonical CBOR bytes of the receipt body with the `signature` field omitted.

All PQEA receipts follow this rule without exception.

---

## Changelog

### Version 1.0.0

* Added **Section 1.5 -- Real-Time Separation Boundary**: defines governance-layer / control-loop separation, prevents PQSEC evaluation inside hard real-time paths, clarifies adapter safe-stop independence.
* Added **Section 9A -- SafetyStateEvidence Profiles**: defines profile-scoped safety evidence requirements (basic, industrial, mobile_platform, surgical).
* Added **Section 10A -- Lease/Control Window Separation**: clarifies ExecutionLease governance validity vs control-loop execution timing, includes timing invariant.
* Added **Section 15A -- Actuation Domain Scoping**: defines actuation domain identifiers (locomotion, manipulation, perception_active, environmental_control, medical_device), domain binding rules, and PQAI SafetyDomain interaction. Introduces `E_EMBODIED_ACTUATION_DOMAIN_UNSUPPORTED` refusal code.
* Added **Section 8.5 -- Perception Insufficiency Mapping**: normative statement that perception insufficiency represents a hardware-level UNAVAILABLE state; PQSEC MUST promote to DENY for Authoritative actuation domains.
* Added **Section 10B -- Governance Boundary Reinforcement**: informative clarification that ExecutionLease constrains duration between governance checkpoints and does not replace predicate evaluation.
* Added **Section 19.2A -- Evaluation Ordering and Real-Time Safety**: informative clarification that governance-layer perception evaluation occurs before actuation command construction; cross-references PQSEC §26.2 short-circuit semantics; reiterates safe-stop triggers during execution.
* Updated **dependency table** to require PQSF ≥ 2.0.3, PQSEC ≥ 2.0.3, PQAI ≥ 1.2.0.
* Updated **Conformance Checklist** (§22) with entries for all new sections.
* Initial specification.

---

## Acknowledgements (Informative)

This specification exists because embodied agents operating in the physical world require governance that is at least as rigorous as the governance applied to digital custody operations. An autonomous system that can open a door, move through a space, or spend money on behalf of a human deserves the same refusal-first discipline that governs a Bitcoin transaction.

The author acknowledges the robotics safety community, whose decades of work on hardware-level safety envelopes and motion planning provide the foundation on which governance layers like PQEA can be built.

Any omissions are unintentional.

---

If you find this work useful and wish to support continued development, donations are welcome:

**Bitcoin:**
bc1q380874ggwuavgldrsyqzzn9zmvvldkrs8aygkw