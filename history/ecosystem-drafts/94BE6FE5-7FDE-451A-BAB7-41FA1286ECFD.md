<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long <me@kerricklong.com>
  SPDX-License-Identifier: CC-BY-SA-4.0
-->

> [!NOTE]
> This is an archived brainstorming document generated with an LLM. Personas and organizations mentioned are fictional. See [index.md](index.md) for context.

Here is the foundational strategy document for the RatatuiRuby ecosystem. This document unifies the engineering specifications (Proposal 3, Jujube Redux) with the product strategy (The Big Tent).  
It serves as the constitution for the project moving forward.

# ---

**The RatatuiRuby Ecosystem: Constitution & Strategy**

**Version 1.0 | Status: Active**  
This document defines the architectural vision, technical specifications, and strategic positioning of the RatatuiRuby project. It supersedes all previous implementation plans. It is the single source of truth for Architects, Product Managers, and Engineers.

## ---

**Chapter 1: The "Big Tent" Architecture**

**What's in this Chapter**

* The definition of the "Forked Path" strategy.  
* How we support both Functional and Object-Oriented paradigms simultaneously.  
* The naming conventions and taxonomy of the five core gems.  
* The specific user personas for each architectural path.

Chapter Summary  
We do not force developers to choose between performance and ergonomics, nor do we force them into a single coding paradigm. We built a "Big Tent."  
The ecosystem rests on a single, high-performance foundation: ratatui\_ruby (The Engine). From there, the architecture forks into two mutually exclusive paths. **Path A (Functional)** offers The Elm Architecture (TEA) via ratatui\_ruby-tea. It targets CLI tools, intricate state machines, and developers who prefer immutability. **Path B (Object-Oriented)** offers a Stateful Component Kit via ratatui\_ruby-components. It targets dashboards, enterprise forms, and developers who prefer the "Rails/ActiveRecord" mental model.  
Both paths eventually converge under the **Glimmer DSL** (glimmer-dsl-ratatui), which provides a unified declarative syntax, and the **Trails Framework** (ratatui\_ruby-trails), which provides the application scaffolding. This architecture allows us to capture the entire Ruby market—from the script scripter to the platform engineer—without compromising the integrity of the underlying Rust engine.

## ---

**Chapter 2: The Engine (ratatui\_ruby)**

**What's in this Chapter**

* Technical specification of the Core Renderer (Layer 1).  
* The **Frame-Based API** (Proposal 3\) and why it was chosen over declarative trees.  
* The distinction between Layout Calculation and Rendering.  
* The "Missing 4" primitives: Text Measurement, Cursor Control, Modifiers, Resize Events.  
* Why Rust FFI was chosen over Go binaries or pure Ruby.

Chapter Summary  
The Engine is the bedrock. It is a stateless, immediate-mode wrapper around the Rust ratatui library. Its primary directive is Performance. It assumes no knowledge of buttons, focus, or application state.  
We implement the **Frame-Based API** (RatatuiRuby.draw { |frame| ... }). This mirrors the native Rust API, giving the developer (or the upper layers) explicit control over where widgets are drawn. This resolves the "Hit Testing" problem naturally: because the user calculates the layout Rects to pass to the Frame, they already possess the coordinates needed to detect mouse clicks. We do not ask the engine to return metadata; we respect the user's ownership of layout math.  
The Engine guarantees thread safety but enforces **Main Thread Rendering**. It exposes the "Missing 4" primitives—Text Width, Cursor Positioning, Event Modifiers, and Resize Events—not for the end-user, but to enable the higher-level abstractions in Path A and Path B to function correctly.

## ---

**Chapter 3: Path A \- The Functional Runtime (ratatui\_ruby-tea)**

**What's in this Chapter**

* Implementation of the "Jujube Redux" specification.  
* The philosophy of "Sync Logic, Async Work."  
* The **Ractor-Ready** enforcement ("The Trojan Horse").  
* Why we use Proc and Array instead of class inheritance.  
* The "Fractal Architecture" pattern for scaling reducers.

Chapter Summary  
Path A is the runtime for purity. It implements The Elm Architecture (Model-View-Update). It adheres to a strict contract: Data In, Data Out.  
The runtime operates on a "Fast Loop." The update and view functions run on the Main Thread and must be pure and instant. All side effects (Commands) are dispatched to a background Worker Pool. We enforce **Ractor Safety** immediately. In debug mode, the runtime validates that all Models and Messages are deeply frozen. If a user tries to pass a mutable object, we crash. This prepares the ecosystem for Ruby 4.0 Ractors without breaking changes later.  
We reject boilerplate. There is no class MyApp \< TEA::App. The API accepts **Callable Objects** (Procs/Lambdas). Commands are closures. Batches are Arrays. We do not ship HTTP clients or database adapters in the core; we provide the Cmd primitives to wrap them. This path is for the developer who values correctness, testability, and explicit control flow over "magic."

## ---

**Chapter 4: Path B \- The Component Kit (ratatui\_ruby-components)**

**What's in this Chapter**

* The transition from Immediate Mode (Engine) to Retained Mode (Kit).  
* The Component class lifecycle (mount, render, handle\_event).  
* The **Focus Manager** and Event Bubbling architecture.  
* How Components utilize the Frame API for self-contained Hit Testing.  
* The Strategy for "Built-in" vs. "Custom" components.

Chapter Summary  
Path B is the runtime for productivity. It bridges the gap between the stateless Engine and the stateful Application. It introduces the Component: a persistent Ruby object that remembers its state (e.g., focused, input\_value).  
The Kit acts as a **Factory**. Every frame, the Focus Manager routes events to the active Component. The Component updates its state. Then, the Component's view method generates the ephemeral Engine structures (Blocks, Paragraphs) and renders them via the Frame API. Because the Component owns its render logic, it knows exactly where it drew itself, making mouse support trivial.  
This layer handles the complex UI logic that raw Ratatui ignores: Tab cycling, Z-index stacking (Popups), and Focus traps. It is the foundation for "Enterprise" applications where encapsulation and object-oriented design are paramount.

## ---

**Chapter 5: The Syntax (glimmer-dsl-ratatui)**

**What's in this Chapter**

* The role of Glimmer as an **Adapter**, not a Runtime.  
* Supporting Path A (Unidirectional) and Path B (Bidirectional) simultaneously.  
* The translation of DSL keywords (button, vertical\_layout) into Engine/Kit objects.  
* The "No Logic in Views" rule.

Chapter Summary  
Glimmer is the skin; it is not the muscle. It provides a declarative, Ruby-native syntax that makes UI code readable. It adapts to the user's chosen path.  
When used with **Path A (TEA)**, Glimmer is a stateless tree builder. It takes the Model and returns the Engine structs. Data binding is strictly **Unidirectional** (Read-Only). When used with **Path B (Components)**, Glimmer unlocks its full power: **Bidirectional** data binding (value \<= \[model, :name\]). It auto-wires the Component's events to the Model's attributes.  
We prioritize clarity over magic. Glimmer views must not contain business logic. They exist solely to describe the hierarchy. This separation ensures that even as the syntax sugar evolves, the underlying architectural guarantees of Path A and Path B remain violated.

## ---

**Chapter 6: The Framework (ratatui\_ruby-trails)**

**What's in this Chapter**

* The vision of "Rails for the Terminal."  
* Scaffolding and Generators (trails new).  
* The MVC pattern applied to TUI (Model=ActiveRecord, View=Glimmer, Controller=Update/Component).  
* The "Omakase" stack decision.

Chapter Summary  
Trails is the endgame. It is the opinionated framework that wires the layers together. It solves the "Blank Page" problem.  
Trails assumes you want to build a database-backed application. It integrates ActiveRecord for the Model layer. It uses ratatui\_ruby-components for the View layer (by default), defaulting to the Object-Oriented path for maximum familiarity to Rails developers. It provides the generators, the directory structure, and the boot process. It turns the "RatatuiRuby Ecosystem" from a library into a product.

## ---

**Chapter 7: Competitive Strategy**

**What's in this Chapter**

* Detailed analysis of **CharmRuby** (The Port) vs. **RatatuiRuby** (The Native).  
* Detailed analysis of **Rust** (Ratatui) and **Go** (Bubble Tea).  
* Positioning statements for READMEs, Wikis, and Executive pitches.  
* The "No Denigration" policy.

Chapter Summary  
We respect our competitors, but we differentiate aggressively on architecture.  
**Vs. CharmRuby:** We win on **Runtime Purity**. CharmRuby bridges Go and Ruby, creating runtime contention. We use native Rust extensions with zero runtime overhead. We also win on **Choice**. CharmRuby enforces the Elm Architecture. We offer both Elm (TEA) and Component (OO) models, supported by a native DSL.  
**Vs. Rust/Go:** We win on **Ecosystem**. Writing a TUI in Rust or Go means losing access to Ruby's gems, metaprogramming, and Active Record. We bring the performance of Rust's rendering engine to the expressiveness of Ruby. We allow the developer to stay in the language they love without sacrificing the quality of the interface.

## ---

**Chapter 8: Governance & Standards**

**What's in this Chapter**

* Contribution guidelines and the "Trojan Horse" Ractor requirement.  
* Documentation standards (Alexandrian Form, Plain Language).  
* Versioning strategy (SemVer strictness).  
* The "Why" behind every architectural decision.

Chapter Summary  
We build for the future. Every line of code in the TEA runtime must be Ractor-safe today, even if Ractors aren't the default. Every component in the Kit must handle Resize events, even if the user has a fixed terminal. We document our decisions using the Alexandrian Form (Context \-\> Problem \-\> Solution) to ensure future maintainers understand the intent, not just the implementation.  
We write for humans. Our documentation is concise, active, and jargon-free. We follow the Federal Plain Language Guidelines. We are building a professional-grade ecosystem, and our artifacts must reflect that quality.