# Ecosystem Drafts Timeline

This index reconstructs the likely chronological development of a subset of the RatatuiRuby ecosystem strategy documents based on their content and interdependencies.

> [!NOTE]
> These documents were generated through conversations with a large language model (Google Gemini) during the project's architectural planning phase. The "Council of Architects," "CEO/CTO," "Pizza Summit," various personas (Elmer, Reagan, Rainier, Troy, Glinda, Gemma, Gordon, Russel, Angie) and the like are **fictional roleplay constructs**—a brainstorming technique used to explore architectural trade-offs from multiple perspectives. RatatuiRuby is a solo project. There is no team, no council, and no organization behind these names.

---

## Exploration Era

The early phase of ideation, where individual proposals were submitted and debated.

### Phase 1: The Fruit Proposals

Early consultant-style documents, each named after a fruit, proposing specific features or architectural ideas.

| Codename | Core Contribution | Key Files |
|----------|------------------|-----------|
| **Apple** | `Cmd#map` for Fractal Architecture / composition scaling | Referenced in [AA24F158](AA24F158-0753-4629-A8C3-597C77B3658A.md) |
| **Banana** | "Big Tent" strategy, "Big Split" / "Forked Path" concept | [9AA5BCEF](9AA5BCEF-5DB9-48F2-9886-25CB7F29EE03.md) |
| **Cherry** | Referenced in staff summaries | [41FB10DE](41FB10DE-102E-45F8-9129-5226B7E856F5.md) |
| **Durian** | Naming conventions, gem taxonomy, ecosystem architecture | [B4B83A2A](B4B83A2A-CD47-40BF-AD2E-5018E871626A.md) |
| **Elderberry** | Referenced in staff summaries | [41FB10DE](41FB10DE-102E-45F8-9129-5226B7E856F5.md) |
| **Fig** | Glimmer integration hooks, "Sugar for Dispatch" protocol | Referenced in [D313D1DA](D313D1DA-C479-4542-ABF8-086056DF4837.md) |
| **Goji Berry** | Referenced in staff summaries | [41FB10DE](41FB10DE-102E-45F8-9129-5226B7E856F5.md) |
| **Honeydew** | Referenced in staff summaries | [41FB10DE](41FB10DE-102E-45F8-9129-5226B7E856F5.md) |
| **Ichigo** | Referenced in staff summaries | [41FB10DE](41FB10DE-102E-45F8-9129-5226B7E856F5.md) |
| **Jujube** | The Elm Architecture runtime, "Sync Logic, Async Work" concurrency model, Ractor-safety ("Trojan Horse") | [1FD9EF8A](1FD9EF8A-CBBF-4B34-A4E8-8674B7767DB9.md) |

### Phase 2: Staff Consolidation

Internal staff (Architect, Project Manager, Product Owner, Technical Writer) evaluated the fruit proposals and consolidated feedback.

| Document | Description |
|----------|-------------|
| [41FB10DE](41FB10DE-102E-45F8-9129-5226B7E856F5.md) | Staff summaries consolidating feedback on Jujube, Elderberry, Apple, Fig, Ichigo, Goji Berry |
| [777340FE](777340FE-9209-4594-A0F7-CF5DC20C71DF.md) | Staff feedback validating Jujube architecture, Trojan Horse strategy, and Cmd.map |
| [B45AEAA8](B45AEAA8-C2E7-4099-8CF8-822EA9789978.md) | Technical review of "Jujube Redux" with refinements |

### Phase 3: Council of Architects Deliberations

The Council of Architects—chaired by the Internal Architect and comprising Elmer (Elm), Reagan (React), Rainier (Rails), Troy (CLI), Glinda (Glimmer), Gemma (Gems), Gordon (Go), Russel (Rust), and Angie (App Dev)—debated and resolved architectural conflicts.

**Key Debates Resolved:**

1. **The Pivot:** "Rails for the Console" reserved for Trails (Layer 4), not TEA (Layer 2A)
2. **Composition Debate:** Adopted Apple's `Cmd#map` with Durian's "Fractal" documentation metaphor
3. **Glimmer Protocol:** Agreed on "Sugar for Dispatch" (`Msg::FieldUpdated`) for TEA compatibility
4. **CTO Rebuttal:** Procs vs. Cmd classes debate resolved in favor of Callable Objects

| Document | Description |
|----------|-------------|
| [AA24F158](AA24F158-0753-4629-A8C3-597C77B3658A.md) | Process log of three debate rounds, unanimous agreement |
| [B77A3A25](B77A3A25-E638-4840-AC32-EF075EF032F8.md) | CTO ammunition against "architecture astronaut" Cmd classes |

### Exploration Era: Early Drafts

Documents representing earlier thinking before the architecture solidified.

| Document | Description |
|----------|-------------|
| [06F304E0](06F304E0-CBAE-4356-B31C-D528D50640D8.md) | Early documentation drafts, competitive analysis |
| [07003EF5](07003EF5-DD13-4D84-8A8A-1795DC03E96D.md) | Refined ecosystem strategy, 5-layer taxonomy |
| [12409606](12409606-1BA3-4A6D-8775-2AA968739F77.md) | Gem development order, marketing drafts |
| [3A36B087](3A36B087-B0F7-4109-87C8-DC82F8D81D60.md) | Vertical slice development, "Why RatatuiRuby?" drafts |
| [307BDEEB](307BDEEB-B383-44E7-9813-F471D64E9B45.md) | Gem naming synthesis, Program class architecture |

---

## Modern Era

The post-Pizza Summit phase where architecture was ratified and documented.

### Phase 4: The Pizza Summit

The Council of Architects convened for a final consolidation meeting. File [3B278E83](3B278E83-3F73-486B-A511-DAB87C5FDA3B.md) explicitly states it was "approved by the Council of Architects after a 'Pizza Summit.'"

**Output:** The "Jujube Prime" directive—the canonical architecture specification for `ratatui-ruby-tea`.

| Document | Description |
|----------|-------------|
| [3B278E83](3B278E83-3F73-486B-A511-DAB87C5FDA3B.md) | Pizza Summit output: canonical architecture specification |
| [CAE99477](CAE99477-0565-417B-A54A-9A7564746951.md) | Gold Master specification (Jujube Prime), dated December 30, 2025 |
| [CFA475A7](CFA475A7-41C4-4E09-924D-E15A390E0257.md) | Canonical architecture (Gold Master v1.0.0) |
| [D313D1DA](D313D1DA-C479-4542-ABF8-086056DF4837.md) | Architecture Definition Document: "Jujube Prime" approved for execution |

### Phase 5: Constitution Documents

Multiple "Constitution" or "Strategy" documents synthesizing all prior work into authoritative references.

| Document | Description |
|----------|-------------|
| [47F04C43](47F04C43-0599-483D-BCE6-50F23F9ACA0E.md) | Comprehensive Constitution: Frame API, Big Tent, Jujube spec |
| [52B46CF5](52B46CF5-AD6A-42D5-BE70-416420ED836C.md) | Constitution reinforcing architecture decisions |
| [85D2619C](85D2619C-0E10-4F78-901D-51BB3948C02A.md) | Constitution unifying all strategies |
| [94BE6FE5](94BE6FE5-7FDE-451A-BAB7-41FA1286ECFD.md) | 8-chapter Constitution (Version 1.0) |
| [AC85A6F9](AC85A6F9-061B-457C-B0B6-F82EE3D1C600.md) | Ecosystem Strategy (Ratified v1.0.0) |
| [B8E82302](B8E82302-7E84-49A7-8391-F456089779ED.md) | 7-chapter Ecosystem Strategy (v1.0.0 Constitution) |
| [D362F69E](D362F69E-2801-4126-9A55-624F1481E39B.md) | Unified Ecosystem Strategy (v1.0 Active) |
| [FF410972](FF410972-36F5-4FBE-B7B5-1C62086A97DB.md) | Confidential Strategic Record (Approved for Execution) |

### Phase 6: Book Outline Attempts

The Constitution documents evolved into book-style outlines with chapters covering the full ecosystem.

**Canonical Chapter Structure:**

1. Vision & Philosophy ("The Big Tent")
2. The Engine (`ratatui_ruby`)
3. Path A: The Functional Runtime (`ratatui_ruby-tea`)
4. Path B: The Component Kit (`ratatui_ruby-components`)
5. The Syntax (`glimmer-dsl-ratatui`)
6. The Framework (`ratatui_ruby-trails`)
7. Competitive Strategy
8. Governance & Standards / Roadmap

| Document | Description |
|----------|-------------|
| [728EBC05](728EBC05-924A-4787-B161-83177C5313AB.md) | Unified Strategy with chapter structure |
| [7D64D435](7D64D435-7556-4331-94FD-B44BBE79B25B.md) | Comprehensive Strategy Document |
| [F5B46175](F5B46175-D8FC-4B54-A495-3A45DDCCB618.md) | Constitution & Strategy Guide (v2.0, 8 chapters) |

### Modern Era: Architectural Decision Records

Decision records and strategy documents from the December 2025 period.

| Document | Description |
|----------|-------------|
| [70F99F1E](70F99F1E-483E-460B-BF32-0D62A7622BCE.md) | Big Tent ecosystem decision record |
| [86560D62](86560D62-8EC1-4B0A-9132-D68D325EC46B.md) | Forked Path architecture decision record |
| [89AB3E81](89AB3E81-6195-4805-B844-F049F525A570.md) | Ecosystem Strategy & Architecture Record |
| [8E2D8AE5](8E2D8AE5-235D-4B0C-AF92-D07DE3CE4494.md) | Ecosystem Strategy & Architecture Record |
| [DB37BD54](DB37BD54-5704-4A37-A1F0-562514F62ED2.md) | Ecosystem Strategy v2.0, "Onion Peel" roadmap |
| [E42E7498](E42E7498-11E4-41E4-82B4-32789A187576.md) | ADR v2.0 (December 30, 2025) |
| [E8F43235](E8F43235-3FBF-489F-842E-B8402581ACAF.md) | Architecture v2.0 (December 2025) |
| [FE5A8FCF](FE5A8FCF-7A08-479B-B8D6-B3BC921E9E78.md) | Approval of Jujube Redux, implementation plan |

---

## Key Architectural Decisions

Across all documents, these decisions were established:

1. **Big Tent / Forked Path:** Support both Functional (TEA) and Object-Oriented (Components) paradigms on a shared Engine
2. **Frame API:** Explicit `frame.render(widget, area)` over declarative trees at Layer 1
3. **Callable Objects:** Procs/Lambdas over class inheritance for TEA
4. **Trojan Horse:** Enforce Ractor-safety now for future Ruby 4.0 compatibility
5. **Sync Logic, Async Work:** Main thread for UI, worker threads for Commands
6. **Batteries Included:** Core commands (exec, wait, tick) with stdlib only
7. **Onion Peel Roadmap:** Engine → TEA → Components → DSL → Framework

---

## Parallel Universe Note

Per the user's context, these documents may represent explorations across "parallel universes" of architectural possibility, all converging on the same fundamental decisions.
