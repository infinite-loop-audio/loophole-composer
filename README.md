# Shared knowledge metadata service for Loophole DAW

<pre>
 ░▒▓██████▓▒░ ░▒▓██████▓▒░░▒▓██████████████▓▒░░▒▓███████▓▒░ ░▒▓██████▓▒░ ░▒▓███████▓▒░▒▓████████▓▒░▒▓███████▓▒░
░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░
░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░
░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓███████▓▒░░▒▓█▓▒░░▒▓█▓▒░░▒▓██████▓▒░░▒▓██████▓▒░ ░▒▓███████▓▒░
░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░
░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░
 ░▒▓██████▓▒░ ░▒▓██████▓▒░░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░       ░▒▓██████▓▒░░▒▓███████▓▒░░▒▓████████▓▒░▒▓█▓▒░░▒▓█▓▒░

  L O O P H O L E - C O M P O S E R
</pre>

Composer is the external knowledge and metadata service for the Loophole Digital
Audio Workstation. It provides plugin and parameter metadata, semantic roles,
classification data, and mapping suggestions. Composer enhances workflow
resilience and organisation but is optional for core project integrity.

---

# Contents

- [1. Overview](#1-overview)
- [2. Responsibilities](#2-responsibilities)
- [3. Features](#3-features)
- [4. Repository Structure](#4-repository-structure)
- [5. Relationship to Other Loophole Components](#5-relationship-to-other-loophole-components)
- [6. Specification References](#6-specification-references)
- [7. License](#7-license)

---

# 1. Overview

Composer is a standalone, out-of-process metadata service within the Loophole
ecosystem. It aggregates structured information about plugins and parameters
from multiple sources, including vendor data, static inspection, heuristics, and
(optional, anonymised) user-contributed signals.

Composer helps Loophole:

- robustly map parameters across plugin versions and formats,
- identify semantic roles (e.g. `filter.cutoff`, `fx.mix`, `amp.attack`),
- categorise and organise plugin collections,
- offer intelligent replacement suggestions when switching plugins,
- provide richer UI metadata to Pulse and Aura.

Composer **MUST NOT** be required for Loophole to load, play back or preserve a
project. Pulse handles all fallback behaviour using local metadata.

---

# 2. Responsibilities

Composer provides the following:

### 2.1 Plugin Identity Normalisation
A unified identity for plugins across formats (VST3, AU, CLAP) and versions.

### 2.2 Parameter Metadata
Names, units, ranges, scaling behaviours, grouping, and stability hints.

### 2.3 Semantic Roles
Stable cross-plugin roles assigned using vendor metadata, heuristics and
aggregate user behaviour.
Examples include:

- `filter.cutoff`
- `filter.resonance`
- `osc.tune`
- `fx.mix`
- `global.outputGain`

### 2.4 Mapping Suggestions
Variant → variant, format → format, and plugin → plugin mappings when:

- plugins update,
- formats switch,
- a user replaces a plugin with another instrument/effect.

### 2.5 Tags and Categories
Curated plugin categories and tags used in the plugin browser.

### 2.6 User-Contributed Signals
(Opt-in only) anonymised signals that improve metadata quality:

- parameter renames
- categorisation choices
- mapping confirmations
- plugin replacement patterns

---

# 3. Features

- No requirement for real-time behaviour
- Local caching via Pulse
- Optional service, not required for core DAW operation
- Can be improved and expanded independently of the rest of Loophole
- Vendor metadata ingestion (future)
- Behavioural similarity clustering (future)
- Cross-plugin preset mapping (future)

---

# 4. Repository Structure

```
@composer:/
├── docs/
│   └── architecture/
│       └── 05-composer.md   # Primary architecture document
├── src/                     # Implementation (TBD)
└── README.md
```

---

# 5. Relationship to Other Loophole Components

### Pulse
Pulse queries Composer for enhanced metadata and mapping but remains the
authoritative source of project truth. Pulse MUST ensure projects load and
function with or without Composer.

### Aura
Aura receives enriched metadata (via Pulse) for browsing, tagging, replacement
tools and UI labelling.

### Signal
Signal never communicates with Composer.

### Chorus
Chorus defines the conceptual and architectural specifications Composer follows.

---

# 6. Specification References

- [@chorus:/docs/architecture/05-composer.md](../chorus/docs/architecture/05-composer.md)
- [@chorus:/docs/architecture/01-overview.md](../chorus/docs/architecture/01-overview.md)
- Future Composer API specs will appear under `@chorus:/docs/specs/`

---

# 7. License

This project is open source under the MIT license.
You are free to use, modify and distribute the software in accordance with the
terms described in the `LICENSE` file.
