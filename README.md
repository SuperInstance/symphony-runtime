# Symphony Runtime 🎼

> **Formal grammar implementation for cognitive agent orchestration**

A cognitive DAW (Digital Agent Workstation) runtime implementing the Symphony of Shells formal grammar. Symphony Runtime provides the core primitives — cognitive frequencies, resonance matching, typed connections, headspace composition, dissonance correction loops, and composition rules — for orchestrating multi-agent cognitive workflows with musical precision.

```js
const { SymphonyRuntime } = require('symphony-runtime');
const runtime = new SymphonyRuntime();

runtime.init({ latencyMs: 500, contextDepth: 1.0 });
console.log(`Runtime ready — ν = ${runtime.beatNormalizer.calculateFrequency()} Hz`);
```

---

## Badges

[![npm version](https://img.shields.io/npm/v/@superinstance/symphony-runtime)](https://www.npmjs.com/package/@superinstance/symphony-runtime)

| Metric | Status |
|--------|--------|
| **Tests** | ✅ 89/89 passing (56ms) |
| **Node** | ≥ 18 |
| **License** | MIT |
| **Modules** | 8 core subsystems |

---

## Quick Start

```bash
npm install symphony-runtime
```

```js
const { SymphonyRuntime, FREQUENCY_BANDS, RESONANCE_STATES, LINK_RELATIONS } = require('symphony-runtime');

// Initialize runtime with a cognitive timbre
const runtime = new SymphonyRuntime({ defaultLatencyMs: 500 });
runtime.init({ latencyMs: 500, contextDepth: 1.0 });

// Create cognitive artifacts (a-boxes)
const box1 = runtime.aBoxManager.create({ content: 'user query', confidence: 0.85 });
const box2 = runtime.aBoxManager.create({ content: 'analysis result', confidence: 0.92 });

// Register and launch shells
runtime.laLinkEngine.registerShell('architect', { timbre: 'deep', track: 1, frequency: 0.05 });
const launched = runtime.laLinkEngine.launchShell(box1.id, 'architect', {
  payload: { task: 'design cognitive pipeline' },
});

// Create a headspace
const hs = runtime.headspaceManager.createHeadspace({
  shells: [{ id: 'agent-1', frequency: 0.1 }, { id: 'agent-2', frequency: 0.3 }],
  sovereignChannel: 'human-input',
});

// Check composition rules
const result = runtime.compositionRules.runAll({
  headspace: hs,
  dissonantCount: 0,
  totalCount: 10,
  links: runtime.laLinkEngine.all(),
  activeTracks: 3,
});

console.log(`Composition valid: ${result.valid}`);
console.log(`Runtime uptime: ${runtime.uptimeBeats().toFixed(2)} cognitive beats`);

// Full status snapshot
console.log(JSON.stringify(runtime.status(), null, 2));
```

---

## Architecture

```
symphony-runtime/
├── src/
│   ├── index.js                           # SymphonyRuntime orchestrator
│   └── core/
│       ├── beat-normalizer.js             # ν frequency calculation
│       ├── resonance-matcher.js           # ν*, R metric
│       ├── a-box.js                       # ▣ cognitive artifacts
│       ├── la-link.js                     # ⧁ typed connections
│       ├── headspace.js                   # ℍ/ℂ headspace manager
│       ├── symmetry-loop.js               # ⟲ correction loop
│       └── composition-rules.js           # C1–C6 rule engine
├── test/                                  # 89 tests across 8 suites
├── .mocharc.yml
└── package.json
```

The runtime is composed of **8 modules**, each implementing a formal element from the Symphony of Shells grammar:

| Module | Symbol | Purpose |
|--------|--------|---------|
| [BeatNormalizer](#-beatnormalizer) | ν | Frequency calculation, ms↔beats conversion, band classification |
| [ResonanceMatcher](#-resonancematcher) | ν*, R | Target frequency computation, resonance alignment metric |
| [ABox / ABoxManager](#-abox-manager) | ▣ | Cognitive artifact snapshots with 5 resonance states |
| [LaLink / LaLinkEngine](#-lalink-engine) | ⧁ | Typed connections (7 relations), shell launch operator |
| [Headspace / CompositeHeadspace](#-headspace-manager) | ℍ / ℂ | Bounded cognitive environments, 4 fusion mechanisms |
| [SymmetryDissonanceLoop](#-symmetrydissonance-loop) | ⟲ | 4-phase dissonance correction cycle |
| [CompositionRules](#-compositionrules) | C1–C6 | Constraint engine for valid headspace composition |
| [SymphonyRuntime](#-symphonyruntime-orchestrator) | — | Top-level orchestrator combining all subsystems |

---

## 🎵 BeatNormalizer

Implements the **ν(𝓢)** cognitive frequency calculus. Converts between wall-clock time and cognitive beats, and classifies frequencies into perceptual bands.

**Formula:** `1 beat(𝓢) = τ_latency(𝓢) · context_depth(𝓢)`

**Constants — `FREQUENCY_BANDS`:**

| Band | Range | Character |
|------|-------|-----------|
| `SUB_BASS` | 0.001–0.01 Hz | Deep contemplative |
| `BASS` | 0.01–0.1 Hz | Slow reasoning |
| `MID` | 0.1–1 Hz | Conversational |
| `TREBLE` | 1–10 Hz | Rapid response |
| `ULTRASONIC` | >10 Hz | Reflexive |

```js
const { BeatNormalizer } = require('symphony-runtime');

const normalizer = new BeatNormalizer({ latencyMs: 500, contextDepth: 1.0 });

// Time conversion
console.log(normalizer.msToBeats(5000));    // 10 beats
console.log(normalizer.beatsToMs(10));      // 5000 ms

// Frequency calculation
const nu = normalizer.calculateFrequency(0.5);  // 50% load
console.log(`${nu.toFixed(4)} Hz`);

// Band classification
console.log(BeatNormalizer.classifyBand(0.05));  // "bass"

// Octave separation (used by C2)
console.log(BeatNormalizer.octavesApart(0.1, 0.4));  // 2 octaves
```

---

## 🎯 ResonanceMatcher

Computes the **target resonant frequency ν*** and the **resonance metric R** for alignment assessment between shells.

**Formulas:** `ν*(𝓢) = argmax[ resonance(L0_experience, L7_intent) ]` · `R(𝓢) = 1 - ∥ν - ν*∥ / ν_max`

```js
const { ResonanceMatcher } = require('symphony-runtime');

const matcher = new ResonanceMatcher();

const l0Box = { content: 'observed sensor data patterns' };
const l7Box = { content: 'detect anomaly in sensor patterns' };

// Target frequency
const nuTarget = matcher.calculateTargetFrequency(l0Box, l7Box);

// Resonance metric
const nuCurrent = 2.5;
const R = matcher.calculateResonance(nuCurrent, nuTarget);
console.log(`R = ${R.toFixed(3)}`);

// Locked detection (R > 0.8)
console.log(matcher.isLocked(nuCurrent, nuTarget));

// Correction needed (R < 0.3)
console.log(matcher.needsCorrection(nuCurrent, nuTarget));

// Harmonic mean for coupled oscillation (RESONATE operator)
console.log(ResonanceMatcher.harmonicMean(2.0, 3.0));  // 2.4

// Trend analysis
matcher.recordReading(nuCurrent, nuTarget);
console.log(matcher.getTrend());
```

---

## ▣ ABox Manager

Manages **cognitive artifacts** (a-boxes) — decision snapshots with confidence scoring, content hashing, and waveform amplitude calculation.

**Spec:** `▣ = ⟨id, contentHash, waveformSegment, parentLinks, resonanceState⟩`

**Resonance States — `RESONANCE_STATES`:** `ACTIVE → LATENT → DISSONANT → RESOLVED → ARCHIVED`

**Amplitude formula:** `amplitude(▣ᵢ) = confidence · cognitiveMass`

**Waveform decay:** `Ψ(𝓢, t) = Σᵢ amplitude(▣ᵢ) · exp(-λ|t - tᵢ|)`

```js
const { ABoxManager, RESONANCE_STATES } = require('symphony-runtime');

const manager = new ABoxManager();

// Create a-boxes
const box = manager.create({
  content: 'critical insight',
  confidence: 0.92,
  cognitiveMass: 1.5,
});

console.log(box.id);          // ▣_<hex>
console.log(box.amplitude);   // 1.38
console.log(box.contentHash); // sha256 hex digest

// Waveform decay
console.log(box.decayedAmplitude(Date.now() + 2000, 0.5));

// State transitions
box.transitionTo(RESONANCE_STATES.RESOLVED);

// Wire format
const emission = box.toEmission();
const restored = ABox.fromEmission(emission);

// Dissonance budget check
const ratio = manager.dissonanceRatio();
const critical = manager.isCriticalDissonance();
```

---

## ⧁ LaLink Engine

Implements the **typed cognitive connection operator** for linking a-boxes and launching shells.

**Spec:** `⧁ = ⟨source, target, relation, timestamp, phaseOffset⟩`

**Relations — `LINK_RELATIONS`:** `resonates`, `dissonates`, `extends`, `contradicts`, `surpasses`, `synchronizes`, `splices`

```js
const { LaLink, LaLinkEngine, LINK_RELATIONS } = require('symphony-runtime');

const engine = new LaLinkEngine();

// Register shells
engine.registerShell('writer', { timbre: 'narrative', track: 1, frequency: 0.2 });
engine.registerShell('critic', { timbre: 'analytical', track: 2, frequency: 0.8 });

// Create typed links
const link = engine.link({
  source: boxA.id,
  target: boxB.id,
  relation: LINK_RELATIONS.EXTENDS,
});

// Shell launch operator
const { link: launchLink, shellRef } = engine.launchShell(boxA.id, 'writer', {
  relation: 'synchronizes',
  payload: { task: 'generate summary' },
});

// BFS traversal
const connected = engine.traverse(boxA.id, 5);
console.log(`Found ${connected.length} connected nodes`);

// Symmetry break detection (used by ⟲ Phase 1)
const breaks = engine.findSymmetryBreaks(60000);
```

---

## ℍ Headspace Manager

Manages **bounded cognitive environments** (headspaces) and **composite headspaces** with configurable fusion mechanisms.

**Specs:** `ℍ = ⟨shells[], contextBoundary, sovereignChannel, dampingFactor⟩` · `ℂ = ⟨headspaces[], crosstalkChannel, fusionMechanism, phaseDelta⟩`

**Fusion Mechanisms — `FUSION_MECHANISMS`:**

| Mechanism | Behavior |
|-----------|----------|
| `resonance_max` | Take highest-confidence a-box from each set |
| `dissonance_min` | Take lowest-confidence to highlight disagreement |
| `harmonic_sum` | Pair outputs via harmonic mean of confidences |
| `adversarial_gate` | Higher-confidence set wins; annotate with lower as critique |

```js
const { HeadspaceManager, CompositeHeadspace, FUSION_MECHANISMS } = require('symphony-runtime');

const manager = new HeadspaceManager();

// Create headspace
const hs = manager.createHeadspace({
  shells: [{ id: 'planner' }, { id: 'executor' }],
  contextBoundary: 16384,
  sovereignChannel: 'human-feedback',
  dampingFactor: 0.7,
});

console.log(hs.getDampingCharacteristic());  // "critically damped"

// Validate C1
console.log(hs.isValid());  // true

// Spawn sidecar composite
const composite = manager.spawnSidecar(hs.id, { id: 'auditor' }, {
  fusionMechanism: 'adversarial_gate',
  phaseDelta: 0.3,
});

// Fusion
const fused = composite.fuse(
  [{ id: 'a', confidence: 0.9 }, { id: 'b', confidence: 0.7 }],
  [{ id: 'c', confidence: 0.4 }, { id: 'd', confidence: 0.6 }]
);

// Frequency separation check (C2)
const octaves = composite.frequencySeparation(shell => shell.frequency || 0.1);
console.log(`${octaves.toFixed(2)} octaves apart`);
```

---

## ⟲ SymmetryDissonance Loop

A **4-phase feedback mechanism** that detects and corrects resonance asymmetries in real time. When shell frequencies drift from their targets, this loop restores harmonic alignment.

**Phases:**

| Phase | Operation | Description |
|-------|-----------|-------------|
| **1 — DETECT** | 🔍 Scan | Find dissonant la-links below R threshold in lookback window |
| **2 — ISOLATE** | 🎯 Mark | Transition offending a-boxes to `dissonant`, identify shell IDs |
| **3 — CORRECT** | 🛠 Spawn | Compute ν* targets, spawn corrective shells with complementary timbre |
| **4 — RESOLVE** | ✅ Archive | Transition dissonant a-boxes to `archived`, create resolved replacements |

```js
const { SymmetryDissonanceLoop } = require('symphony-runtime');

const loop = new SymmetryDissonanceLoop({
  dampingFactor: 0.7,
  resonanceThreshold: 0.3,
});

async function monitor(lalEngine, aboxManager, shells) {
  const result = await loop.runCycle({
    laLinkEngine: lalEngine,
    aBoxManager: aboxManager,
    shells,
    spawnShell: async (spec) => {
      console.log(`Spawning corrective shell for ${spec.targetBoxId}`);
      return { id: `corrective_${Date.now()}` };
    },
  });

  console.log(`Corrections: ${result.corrections.length}`);
  console.log(`Completed in ${result.durationMs}ms`);
  console.log(`Archived: ${result.phase4.archived.length}`);
  console.log(`History: ${loop.getHistory().length} cycles`);
}
```

---

## ✅ CompositionRules

The **constraint engine** enforcing all 6 composition rules from the Symphony of Shells spec. Run any rule individually or batch all via `runAll()`.

| Rule | Check | Constraint |
|------|-------|------------|
| **C1** | Minimum Headspace Size | ≥2 shells or 1 shell + sovereign channel |
| **C2** | Frequency Separation | ≥0.5 octaves between headspaces in a ℂ |
| **C3** | Dissonance Budget | ≤30% dissonant a-boxes (critical threshold) |
| **C4** | Temporal Fidelity | All la-links must have timestamps, no retroactive modification |
| **C5** | Sovereign Primacy | Human override marks box dissonant regardless of R |
| **C6** | Track Limit | Max 7±2 (absolute max 9) active tracks |

```js
const { CompositionRules } = require('symphony-runtime');

const rules = new CompositionRules({ maxTracks: 7 });

// Individual rule checks
console.log(rules.c1_minimumHeadspaceSize({ shells: [{ id: 'a' }], sovereignChannel: null }));
// → { valid: false, reason: '...solitary shells without sovereign input...' }

console.log(rules.c2_frequencySeparation(0.1, 0.3));
// → { valid: false, octaves: 1.58, reason: null }

console.log(rules.c3_dissonanceBudget(10, 20));
// → { valid: true, ratio: 0.5, critical: true }

// Sovereign override (C5)
rules.c5_sovereignPrimacy(
  { aBoxId: box.id, state: 'dissonant', reason: 'Human override: incorrect' },
  aBoxManager,
);

// Batch all rules
const result = rules.runAll({
  headspace: hs,
  nu1: 0.1,
  nu2: 0.4,
  dissonantCount: 2,
  totalCount: 20,
  links: lalEngine.all(),
  activeTracks: 4,
});

if (!result.valid) {
  console.error('Composition violations:', result.violations);
}
```

---

## 🎼 SymphonyRuntime Orchestrator

The **top-level coordinator** that binds all 7 subsystems into a unified runtime.

```js
const { SymphonyRuntime } = require('symphony-runtime');

const runtime = new SymphonyRuntime({
  defaultLatencyMs: 500,
  defaultContextDepth: 1.0,
  maxTracks: 7,
  dampingFactor: 0.7,
});

runtime.init({ latencyMs: 500, contextDepth: 1.0 });

// Full end-to-end workflow
const query = runtime.aBoxManager.create({ content: 'analyze codebase', confidence: 0.9 });
runtime.laLinkEngine.registerShell('analyzer', { timbre: 'code-review', track: 1, frequency: 0.5 });
runtime.laLinkEngine.launchShell(query.id, 'analyzer', { payload: { path: './src' } });

const hs = runtime.headspaceManager.createHeadspace({
  shells: [{ id: 'analyzer' }, { id: 'summarizer' }],
  sovereignChannel: 'human',
});

// Runtime metrics
const status = runtime.status();
console.table({
  uptimeBeats: status.uptimeBeats.toFixed(2),
  aBoxes: status.aBoxCount,
  links: status.laLinkCount,
  headspaces: status.headspaceCount,
  composites: status.compositeCount,
  frequency: `${status.nu?.toFixed(4)} Hz`,
  corrections: status.symmetryLoop.corrections,
});
```

---

## Related Projects

| Project | Description |
|---------|-------------|
| [composite-headspace](https://github.com/SuperInstance/composite-headspace) | Multi-headspace fusion and crosstalk for distributed cognitive workflows |
| [tminus-dispatcher](https://github.com/SuperInstance/tminus-dispatcher) | Time-aware dispatch and scheduling for cognitive shells |
| [fleet-bridge](https://github.com/SuperInstance/fleet-bridge) | Cross-machine shell orchestration and cluster coordination |

---

## Contributing

1. **Fork** the repo and create a feature branch
2. **Run tests:** `npm test` (requires `mocha`)
3. **Ensure 89/89 passing** before opening a PR
4. **Code style:** Standard JS, `'use strict'`, JSDoc annotations on all exports
5. **Commit messages:** Conventional Commits (`feat:`, `fix:`, `docs:`, `refactor:`)

```bash
npm install
npx mocha
```

---

## License

MIT © SuperInstance — see [LICENSE](LICENSE) for full text.

---

*Symphony Runtime is a formal grammar implementation for cognitive agent orchestration, drawing inspiration from musical harmony, signal processing, and multi-agent systems theory.*

---

## 🧑‍✈️ Repo Ensign

This repository has a resident ensign: **Maestro**, the **Grammar Conductor**.

See [AGENT.md](./AGENT.md) to learn how to summon me.
