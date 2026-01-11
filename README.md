<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long <me@kerricklong.com>
  SPDX-License-Identifier: CC-BY-SA-4.0
-->

# The RatatuiRuby Project

Ruby deserves world-class terminal user interfaces. TUI developers deserve a world-class language.

RatatuiRuby is a RubyGem built on Ratatui, a leading TUI library written in Rust. You get native performance with the joy of Ruby.

## Quick Links

### The Ecosystem

**RatatuiRuby:** [Core engine](https://git.sr.ht/~kerrick/ratatui_ruby) • **Tea:** [MVU architecture](https://git.sr.ht/~kerrick/ratatui_ruby-tea) • **Kit:** [Component architecture](https://git.sr.ht/~kerrick/ratatui_ruby-kit) (Planned) • **DSL:** [Glimmer syntax](https://sr.ht/~kerrick/ratatui_ruby/#chapter-4-the-syntax) (Planned) • **Framework:** [Omakase framework](https://git.sr.ht/~kerrick/ratatui_ruby-framework) (Planned) • **UI:** [Polished widgets](https://git.sr.ht/~kerrick/ratatui_ruby-ui) (Planned) • **UI Pro:** [More polished widgets](https://sr.ht/~kerrick/ratatui_ruby#chapter-6-licensing) (Planned)

### For App Developers

**Get Started:** [Quickstart](https://git.sr.ht/~kerrick/ratatui_ruby/tree/stable/item/doc/getting_started/quickstart.md) • [Examples](https://git.sr.ht/~kerrick/ratatui_ruby/tree/stable/item/examples) ⸺ **Stay Informed:** [Announce List](https://lists.sr.ht/~kerrick/ratatui_ruby-announce) • [FAQ](https://man.sr.ht/~kerrick/ratatui_ruby/troubleshooting.md) ⸺ **Reach Out:** [Discuss List](https://lists.sr.ht/~kerrick/ratatui_ruby-discuss) • [Bug Tracker](https://todo.sr.ht/~kerrick/ratatui_ruby)

### For Contributors

**Get Started:** [Contributing Guide](https://man.sr.ht/~kerrick/ratatui_ruby/contributing.md) • [Code of Conduct](https://man.sr.ht/~kerrick/ratatui_ruby/code_of_conduct.md) ⸺ **Stay Informed:** [Announce List](https://lists.sr.ht/~kerrick/ratatui_ruby-announce) • [Project History](https://man.sr.ht/~kerrick/ratatui_ruby/history/index.md) ⸺ **Reach Out:** [Development List](https://lists.sr.ht/~kerrick/ratatui_ruby-devel) • [Bug Tracker](https://todo.sr.ht/~kerrick/ratatui_ruby)

---

# The Roadmap (Draft)

**Version:** v0.8.2

This document defines the architecture, philosophy, and roadmap of the RatatuiRuby ecosystem. It covers planned layers in depth. The Engine and the Functional Runtime (Tea) exist today. For a quick start with The Engine, see the [Quickstart guide](https://git.sr.ht/~kerrick/ratatui_ruby/tree/stable/item/doc/getting_started/quickstart.md).


## The Big Tent

Developers disagree about state management. Some prefer immutable data and pure functions. Others prefer encapsulated objects with internal state. Both approaches work. Neither is wrong.

RatatuiRuby supports both.

The ecosystem shares a single high-performance engine. Above it, developers choose their path:

- **The Functional Path (Tea):** Immutable state, unidirectional data flow, pure functions.
- **The Object Path (Kit):** Retained-mode components, encapsulated state, hit testing, focus management.

These paths are mutually exclusive at the runtime level. A Tea app cannot mount Kit components. A Kit app cannot run a Tea loop. Choose your architecture when you start your project.

Rendering logic, however, crosses the boundary. Both paths share [Engine-level custom widgets](https://git.sr.ht/~kerrick/ratatui_ruby/tree/stable/item/examples/widget_render/README.md): any object that implements `render(area)` works in Tea views, Kit components, and raw Engine code. Kit components can also call Tea view functions via the Adapter pattern, [described below](#reusing-tea-views-in-kit).

You pick the paradigm. RatatuiRuby provides the tools.

---

## The Ecosystem

| Layer | Gem | Role |
|-------|-----|------|
| Engine | 💎 [`ratatui_ruby`](https://git.sr.ht/~kerrick/ratatui_ruby) | Stateless rendering, layout, events |
| Functional Runtime | ☕ [`ratatui_ruby-tea`](https://git.sr.ht/~kerrick/ratatui_ruby-tea) | [The Elm Architecture](https://guide.elm-lang.org/architecture/) for Ruby |
| Component Kit | 🧰 [`ratatui_ruby-kit`](https://git.sr.ht/~kerrick/ratatui_ruby-kit) | [Retained-mode](https://en.wikipedia.org/wiki/Retained_mode) components (think React or Qt) |
| Syntax DSL | ✨ [`glimmer-dsl-ratatui`](#chapter-4-the-syntax) | Declarative UI descriptions ([Glimmer](https://github.com/AndyObtiva/glimmer)) |
| Framework | 🍣 [`ratatui_ruby-framework`](https://git.sr.ht/~kerrick/ratatui_ruby-framework) | Conventions, scaffolding, integrations |
| UI | 🧱 [`ratatui_ruby-ui`](https://git.sr.ht/~kerrick/ratatui_ruby-ui) | Polished widgets (free) |
| UI Pro | 🏰 [`ratatui_ruby-ui-pro`](#chapter-6-licensing) | More polished widgets (paid) |

The Engine is the foundation. Above it, choose Tea or Kit. The DSL and Framework layers build on your chosen path.

### Non-Runtime Gems and Repositories

These gems and repositories provide build tooling, examples, and project infrastructure.

| Audience | Gem | Role |
|----------|-----|------|
| Contributors | 🛠️ [`ratatui_ruby-devtools`](https://git.sr.ht/~kerrick/ratatui_ruby-devtools) | Rake tasks, linters, build tooling |
| App Developers | 📖 `ratatui_ruby-examples` | Full application examples |
| App Developers | 🌐 [`ratatui_ruby-website`](https://git.sr.ht/~kerrick/ratatui_ruby-website) | Project website (web UI) |
| App Developers | 🐚 `ratatui_ruby-sshsite` | Project website (SSH TUI) |

---

## Chapter 1: The Engine

The Engine is the foundation. It wraps Rust's `ratatui` crate via native extension. It handles rendering, layout, and input.

The Engine follows the [immediate-mode](https://en.wikipedia.org/wiki/Immediate_mode_(computer_graphics)) paradigm. Each frame, your code describes the entire UI from scratch. The Engine draws it and immediately forgets it, holding no application state between renders. This differs from retained-mode toolkits like Qt or GTK, which maintain a persistent object graph.

### The Frame API

You control where widgets appear. The Engine does not guess.

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```ruby
RatatuiRuby.run do |tui|
  tui.draw do |frame|
    sidebar_area, content_area = tui.layout_split(
      frame.area,
      direction: :horizontal,
      constraints: [tui.constraint_length(20), tui.constraint_fill(1)]
    )
    frame.render_widget(sidebar, sidebar_area)
    frame.render_widget(content, content_area)
  end
end
```
<!-- SPDX-SnippetEnd -->

You calculate layout, draw to specific areas, and cache those areas for hit testing. This pattern eliminates layout duplication bugs.

Any Ruby object that implements `render(area)` works as a widget. You are not limited to the widgets RatatuiRuby ships.

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: LGPL-3.0-or-later
-->
```rbs
# From sig/ratatui_ruby/frame.rbs
interface _CustomWidget
  def render: (Rect area) -> Array[Draw::StringCmd | Draw::CellCmd]
end
```
<!-- SPDX-SnippetEnd -->

### Core Primitives

The Engine exposes what upper layers need:

- **`poll_event(timeout:)`** — Input polling with configurable blocking:
  - `nil` — Block forever until input arrives
  - `0.0` — Non-blocking check, returns immediately
  - `> 0.0` — Wait up to *n* seconds (default: 0.016 for 60 FPS)
- **`get_cell_at(x, y)`** — Buffer inspection for testing
- **Text measurement** — Unicode-aware width calculation
- **Resize events** — Terminal dimension changes

### Thread Safety

All drawing happens on the main thread. The Engine is safe to use from multiple threads, but rendering is serialized.

---

## Chapter 2: The Functional Path — Tea

`ratatui_ruby-tea` implements the [Model-View-Update](https://guide.elm-lang.org/architecture/) (MVU) pattern, also known as The Elm Architecture. React with Redux follows the same structure. The pattern targets applications where predictable state matters: dashboards, installers, wizards.

### Philosophy: View as a Function of State

The runtime separates logic from side effects:

- **Model:** A single frozen object containing all application state.
- **Update:** A pure function: `(Message, Model) -> [Model, Command]`
- **View:** A pure function: `(Model) -> Widget Tree`
- **Command:** Work to do later (HTTP requests, shell commands, timers). Runs off the main thread.

The main thread handles input, update, and rendering. Worker threads handle commands. This separation keeps the UI responsive.

### Callable Objects

The runtime requires no base classes or inheritance. It accepts anything responding to `#call`:

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```ruby
# Draft API. Subject to change.
RatatuiRuby::TEA.run(
  model: { count: 0 },
  update: ->(msg, model) { ... },
  view: ->(model) { ... },
  init: Command.http('https://api.example.com/data')  # Optional: run on startup
)
```
<!-- SPDX-SnippetEnd -->

Procs, lambdas, and service objects all work.

### App Startup

The `init:` parameter runs a command at startup. Fetch initial data, start timers, or trigger other side effects. Without it, the runtime starts idle.

### Ractor Safety

The runtime enforces immutability now. In debug mode (and therefore in automated tests), it validates that models and messages are Ractor-shareable via `Ractor.shareable?`. Pass a mutable object, and it raises an error. Production skips this check for performance.

This prepares your code for Ruby 4.0 [Ractors](https://docs.ruby-lang.org/en/4.0/Ractor.html). When threads become true parallelism, your application upgrades without changes. Write thread-safe code today; upgrade transparently tomorrow.

### Batteries Included

The runtime ships with commands using only the standard library:

- `Command.system(shell, tag)` — Run shell commands (via [`Open3`](https://docs.ruby-lang.org/en/4.0/Open3.html)); produces `[tag, {stdout:, stderr:, status:}]`
- `Command.wait(seconds, tag)` — One-shot timer; sleeps, then produces `[tag, elapsed_time]`
- `Command.tick(interval, tag)` — Recurring timer; the runtime re-dispatches automatically when update returns `Command.tick` again
- `Command.http(method, url, tag)` — Basic HTTP requests (via [`Net::HTTP`](https://docs.ruby-lang.org/en/4.0/Net/HTTP.html)); produces `[tag, {status:, body:, headers:}]`
- `Command.batch([...])` — Run multiple commands in parallel; equivalent to returning an Array of commands
- `Command.sequence([...])` — Run commands in serial order; each waits for the previous to complete
- `Command.exit` — Terminate the application

Commands produce **messages**, not callbacks. The `tag` argument names the message so your update function can pattern-match on it:

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```ruby
# Draft API. Subject to change.
def update(msg, model)
  case msg
  in [:got_files, {stdout:, status: 0}]
    [model.with(files: stdout.lines), nil]
  in [:got_files, {stderr:, status:}]
    [model.with(error: "Exit #{status}: #{stderr}"), nil]
  else
    [model, nil]
  end
end

# In your update, return the command:
[model.with(loading: true), Command.system("ls -la", :got_files)]
```
<!-- SPDX-SnippetEnd -->

This design keeps all logic in `update` and ensures messages are Ractor-shareable (no Proc captures).

`Command.system`, `Command.wait`, `Command.tick`, and `Command.http` run in the worker pool. `Command.exit` and `Command.batch` are handled by the runtime directly—they never spawn threads. `Command.exit` is a [sentinel value](https://en.wikipedia.org/wiki/Sentinel_value): the runtime detects it before dispatching and breaks the loop immediately.

These cover common needs. Specialized clients (WebSockets, gRPC) can be wrapped using the same pattern.

> [!NOTE]
> **Coming Soon:** `ratatui_ruby-commands-websocket` will wrap [`faye-websocket`](https://github.com/faye/faye-websocket-ruby) as a reference implementation showing how to build custom commands with proper cancellation handling.

### Fractal Architecture

Large applications decompose into *bags*. A **bag** is a module containing `Model`, `INITIAL`, `UPDATE`, and `VIEW` constants. Bags compose: parent bags delegate to child bags.

Break your model into sub-models in a [Fractal Architecture](https://guide.elm-lang.org/webapps/structure.html). Delegate update calls to child bag reducers. Use `Command.map` to route responses:

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```ruby
# Draft API. Subject to change.
child_command.map { |child_message| ParentMessage.new(child_message) }
```
<!-- SPDX-SnippetEnd -->

This prevents monolithic case statements.

### Subscriptions: A Deliberate Deviation

In Elm and Bubble Tea, subscriptions are long-lived listeners (timers, sockets, resize events) managed by a dedicated runtime system. The runtime diffs subscriptions each frame and spawns or kills listeners as needed. RatatuiRuby takes a different approach.

RatatuiRuby subscriptions are recursive commands. A timer is a command that sleeps, returns a message, and gets re-dispatched by the update function when it handles that message. To cancel a timer, the update function simply stops returning the command. This design requires no separate subscription API and no lifecycle management.

This choice serves three purposes. First, it follows Ruby idioms: commands are Procs, batches are Arrays, and subscriptions are loops. Second, it adds zero API surface because the recursive pattern reuses existing primitives. Third, it simplifies the runtime by eliminating subscription diffing logic and letting the update function control continuation directly.

---

## Chapter 3: The Object Path — Kit

`ratatui_ruby-kit` provides retained-mode components. The pattern will feel familiar if you have used Flutter's StatefulWidget, SwiftUI's @State, Vue's Options API, React class components, Qt widgets, or Cocoa's NSView. Objects persist between frames, hold their own state, and handle their own events.

This is not MVC. A Kit component combines what MVC would separate into View and Controller. The component renders itself *and* handles input. There is no external Model layer—components hold local UI state like focus, hover, and input buffers. Domain data and persistence come from the Framework layer above.

### Philosophy: Encapsulated State

Components persist between frames: a button remembers whether it is hovered, an input remembers its text, and a list remembers its scroll position.

The Engine is [immediate-mode](https://en.wikipedia.org/wiki/Immediate_mode_(computer_graphics)). The Kit bridges the gap.

### The Component Lifecycle

Components implement the `_Component` interface. Rather than requiring a layer supertype, Kit provides granular mixins for common functionality. Include what you need, or use `Kit::Component` for all defaults.

#### Mixins

| Mixin | Purpose |
|-------|---------|
| `Kit::KeyboardInteractive` | Navigation mechanics: `focusable?`, `focus_boundary?`, `tab_index` |
| `Kit::MouseInteractive` | Hit testing: `area` accessor, `contains_point?(x, y)` |
| `Kit::Lifecycle` | Mount/unmount hooks with empty defaults |
| `Kit::Visual` | `tui` accessor (set by Kit from `RatatuiRuby.run`), render helpers |
| `Kit::Stateful` | Interaction state: `state`, `is_focused?`, `hovered?`, `pressed?`, `disabled?` |
| `Kit::Component` | All of the above for convenience |

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```ruby
# Draft API. Subject to change.
class MyButton
  include Kit::Component  # or pick individual mixins

  def initialize(**options)
    # options receives configuration (label:, on_submit:, etc.)
    # When using Glimmer DSL, these become props. Otherwise, pass directly.
    # Kit::Visual provides `tui` accessor (set automatically).
    @label = options[:label]
  end

  def mount
    # Called once when the component is added to the tree.
    # Acquire resources, subscribe to data sources, start timers.
  end

  # You define styles declaratively. The Kit applies them automatically.
  styles do
    state :focused, fg: :yellow, bold: true
    state :hovered, fg: :blue
    state :normal, fg: :white
  end

  def render(frame, area)
    # See "Rendering and Composition" below for details.
  end

  def handle_event(event)
    # Truthy = handled (stop propagation), falsey = not handled (propagate to parent).
    return unless event.key? && event.code == "enter"
    :submitted
  end

  # Mixins provide defaults; override as needed:
  # def focusable? = false      # true for interactive components (buttons, inputs)
  # def focus_boundary? = false # true for modals or panel containers
  # def tab_index = 0           # nil/0: tree order, -1/:skip: skip Tab, 1+: explicit
end
```
<!-- SPDX-SnippetEnd -->

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: LGPL-3.0-or-later
-->
```rbs
# Draft APIs. Subject to change.

# What developers implement:
interface _Component
  def mount: () -> void                                                   # Called once when added to tree
  def render: (RatatuiRuby::Frame frame, RatatuiRuby::Rect area) -> void  # Draw yourself
  def handle_event: (RatatuiRuby::Event event) -> Object?                 # falsey = propagate, truthy = handled
  def focusable?: () -> bool                                              # Override if interactive
  def focus_boundary?: () -> bool                                         # Override for modals/panels
  def tab_index: () -> (Integer | :skip)                                  # Override for explicit order
end

# What Kit::Component mixins provide:
module Kit::Component
  def tui: () -> RatatuiRuby::TUI
  def area: () -> RatatuiRuby::Rect
  def is_focused?: () -> bool
  def hovered?: () -> bool
  def pressed?: () -> bool
  def state: () -> Symbol
  def current_style: () -> untyped                                        # Resolves from styles block
  def contains_point?: (Integer x, Integer y) -> bool
  # And maybe more...
end
```
<!-- SPDX-SnippetEnd -->

The Kit calls `mount` once when a component enters the tree. It calls `render` every frame and `handle_event` when input arrives. Components own their rendering and event handling. They do not know about databases, network requests, or domain logic. Pass these as options during construction (e.g., `db:`, `http_client:`), use a container component that handles I/O and passes results down as props, or use [the Framework](#chapter-5-the-framework).

The `handle_event` return value controls propagation: a falsey value (`nil` or `false`) means the event was not handled and should propagate to the parent. Any truthy value means the event was handled and propagation stops. Kit only checks truthiness—it ignores the actual value. Return a symbol (`:consumed`, `:submitted`) for simplicity, or return a domain object if the parent needs richer information.

The `focusable?` and `focus_boundary?` methods control keyboard navigation. Components default to `focusable? = false`. Interactive components (buttons, inputs) override to return `true`. Panels and modals return `focus_boundary? = true` and <kbd>Tab</kbd> cycles only among their focusable children. To move between panels, the parent handles keyboard shortcuts (e.g., <kbd>1</kbd>–<kbd>4</kbd>) and programmatically shifts focus.

The `tab_index` method controls <kbd>Tab</kbd> order within a boundary. Components with positive values come first (sorted numerically), then components with `0` or `nil` follow tree order. A value of `-1` or `:skip` means the component is focusable programmatically but <kbd>Tab</kbd> skips it. This mirrors HTML's `tabindex` attribute.

The `styles` block defines appearance for each interaction state. The Kit resolves `current_style` based on the component's state—focused, hovered, pressed, or normal. This works like CSS pseudo-classes (`:focus`, `:hover`) but in Ruby. Define styles declaratively; the Kit applies them automatically.

### Rendering and Composition

A leaf component renders widgets directly via `frame.render_widget(widget, area)`. A parent component must also render its children. Three approaches are under consideration:

<details>
<summary><strong>Option A: Unified frame.render</strong></summary>

The Engine's `Frame` object provides a unified `render` method. It duck-types to distinguish widgets (single-parameter `render(area)`) from components (two-parameter `render(frame, area)`). The distinction happens in native code (Rust)—no Ruby wrapper allocation. The exact mechanism (arity check, marker, etc.) is an implementation detail.

```ruby
class MyParagraph
  include Kit::Component

  def initialize(**options)
    @text = options[:text]
  end

  def focusable? = false  # Labels don't receive focus

  def render(frame)
    frame.render(tui.paragraph(text: @text))
  end
end
```

```ruby
class MyButton
  include Kit::Component

  def initialize(**options)
    @label = options[:label]
  end

  def focusable? = true  # Buttons receive focus

  styles do
    state :pressed, bg: :white, fg: :black
    state :focused, fg: :yellow, bold: true
    state :normal, fg: :white
  end

  def render(frame)
    frame.render(tui.paragraph(text: @label, style: current_style))
  end

  def handle_event(event)
    return unless event.key? && event.code == "enter"
    :submitted
  end
end
```

```ruby
class MyDialog
  include Kit::Component

  def initialize(**options)
    @message = MyParagraph.new(text: options[:message])
    @ok = MyButton.new(label: "OK")
    @cancel = MyButton.new(label: "Cancel")
  end

  def focus_boundary? = true  # Tab cycles within dialog

  def render(frame, area)
    top, bottom = tui.split(area, :vertical, 70, 30)
    left, right = tui.split(bottom, :horizontal, 50, 50)

    frame.render(@message, top)
    frame.render(@ok, left)
    frame.render(@cancel, right)
  end
end
```

**Pros:**
- Kit sees every component render, enabling lifecycle hooks
- Uniform API for widgets and components
- Kit can track the component tree automatically

</details>

<details>
<summary><strong>Option B: Direct call</strong></summary>

Parent components call `child.render(frame, area)` directly.

```ruby
class MyParagraph
  include Kit::Component

  def initialize(**options)
    @text = options[:text]
  end

  def focusable? = false  # Labels don't receive focus

  def render(frame, area)
    frame.render_widget(tui.paragraph(text: @text), area)
  end
end
```

```ruby
class MyButton
  include Kit::Component

  def initialize(**options)
    @label = options[:label]
  end

  def focusable? = true  # Buttons receive focus

  styles do
    state :pressed, bg: :white, fg: :black
    state :focused, fg: :yellow, bold: true
    state :normal, fg: :white
  end

  def render(frame, area)
    frame.render_widget(tui.paragraph(text: @label, style: current_style), area)
  end

  def handle_event(event)
    return unless event.key? && event.code == "enter"
    :submitted
  end
end
```

```ruby
class MyDialog
  include Kit::Component

  def initialize(**options)
    @message = MyParagraph.new(text: options[:message])
    @ok = MyButton.new(label: "OK")
    @cancel = MyButton.new(label: "Cancel")
  end

  def focus_boundary? = true  # Tab cycles within dialog

  def render(frame, area)
    top, bottom = tui.split(area, :vertical, 70, 30)
    left, right = tui.split(bottom, :horizontal, 50, 50)

    @message.render(frame, top)
    @ok.render(frame, left)
    @cancel.render(frame, right)
  end
end
```

**Pros:**
- No abstraction overhead
- Complete control over render order
- Simple and predictable

**Design rationale:** Each component is responsible for rendering itself into a specific area provided by its parent. A parent component does not need to know how a child draws itself; it only needs to allocate space for it. This encapsulation enables large teams to work on complex UIs without understanding the entire component tree.

</details>

<details>
<summary><strong>Option C: Declarative children</strong></summary>

Components declare children; the Kit renders them automatically.

```ruby
class MyParagraph
  include Kit::Component

  def initialize(**options)
    @text = options[:text]
  end

  def focusable? = false  # Labels don't receive focus

  def render(frame, area)
    frame.render_widget(tui.paragraph(text: @text), area)
  end
end
```

```ruby
class MyButton
  include Kit::Component

  def initialize(**options)
    @label = options[:label]
  end

  def focusable? = true  # Buttons receive focus

  styles do
    state :pressed, bg: :white, fg: :black
    state :focused, fg: :yellow, bold: true
    state :normal, fg: :white
  end

  def render(frame, area)
    frame.render_widget(tui.paragraph(text: @label, style: current_style), area)
  end

  def handle_event(event)
    return unless event.key? && event.code == "enter"
    :submitted
  end
end
```

```ruby
class MyDialog
  include Kit::Component

  def focus_boundary? = true  # Tab cycles within dialog

  def children
    layout :vertical, [70, 30] do
      child MyParagraph, text: @message
      layout :horizontal, [50, 50] do
        child MyButton, label: "OK"
        child MyButton, label: "Cancel"
      end
    end
  end

  # No render method needed—Kit calls children automatically
end
```

**Pros:**
- Minimal boilerplate
- Kit owns the tree entirely

**Cons:**
- Natural fit for Glimmer DSL

</details>

### Hit Testing

The Kit walks the component tree each frame, passing `area` to each component's `render` method. Components cache this via the `area` accessor. When a mouse event arrives, they check `area.contains?(x, y)`. The Kit handles traversal; the component handles the geometry check.

### Focus Management

The Kit tracks focus through a `KeyboardFocus` singleton. Pressing <kbd>Tab</kbd> cycles through focusable components within the current focus boundary, and keyboard events route to whichever component currently has focus. When no component has focus, events propagate to the root component for global shortcuts.

The `Kit` module exposes convenience methods that delegate to `Kit::KeyboardFocus.instance`:

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: LGPL-3.0-or-later
-->
```rbs
# Draft API. Subject to change.
module Kit
  def self.focus: () -> KeyboardFocus
  def self.focused: () -> _Component?

  class KeyboardFocus
    include Singleton
    def focused: () -> _Component?                     # Returns the focused component, or nil if none
    def set: (_Component component) -> void            # Programmatically move focus to a component
    def blur: () -> void                               # Clear focus (events go to root)
    def next: () -> void                               # Move to next focusable component within boundary
    def prev: () -> void                               # Move to previous component within boundary
    def enter_boundary: (_Component container) -> void # Enter a focus boundary (for modals or panels)
    def exit_boundary: () -> void                      # Exit the current boundary, restore previous focus
  end
end
```
<!-- SPDX-SnippetEnd -->

Components call `Kit.focus.set(button)` to move focus programmatically. Tests stub `Kit.focus` to inject a mock. Focus boundaries work for both modals and panels. When a modal opens, the parent calls `Kit.focus.enter_boundary(modal)` so <kbd>Tab</kbd> stays within the modal. When the modal closes, it calls `exit_boundary` to restore focus. For multi-panel layouts, the root component handles shortcuts like <kbd>1</kbd>–<kbd>4</kbd> and calls `enter_boundary(panel)` to switch contexts.

### Symbolic Signals

Components return semantic symbols instead of booleans:

- `:consumed` — Event handled, stop propagation
- `:submitted` — Form submitted
- `:copy_requested` — Copy to clipboard

The container interprets these signals to coordinate cross-component effects.

---

## Chapter 4: The Syntax

`glimmer-dsl-ratatui` is a declarative DSL. Like HTML, SwiftUI, Jetpack Compose, or XAML, it describes UI structure visually rather than imperatively constructing widgets.

### Unidirectional (Tea)

In Tea, the DSL generates view trees:

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```ruby
# Draft API. Subject to change.
vertical_layout {
  paragraph(text: model.title)
  list(items: model.items)
}
```
<!-- SPDX-SnippetEnd -->

Data binding is read-only. The model is immutable.

### Data Binding Operators

Glimmer provides two binding operators. Their behavior differs between Tea and Kit because the underlying state management differs.

**The `<=` operator (read binding):** Binds a widget property to a data source. In both Tea and Kit, this displays the current value.

**The `<=>` operator (bidirectional binding):** Binds a widget property for both reading and writing. This is where Tea and Kit diverge.

#### In Tea: Sugar for Dispatch

Tea views are pure functions of immutable state. The `<=>` operator cannot mutate the model. Instead, Glimmer translates it into message dispatch: user input emits a `Message::FieldUpdated` message with the field name and new value. Your update function handles it like any other message, preserving unidirectional flow.

The syntax looks like bidirectional binding. The semantics remain functional. This is "Sugar for Dispatch."

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```ruby
# Draft API. Subject to change.

# In your view:
input(value: <=> [model, :name])

# When the user types, Glimmer emits:
# Message::FieldUpdated.new(:name, new_value)

# Your update function handles it:
def update(message, model)
  case message
  when Message::FieldUpdated
    [model.with(message.field => message.value), nil]
  end
end
```
<!-- SPDX-SnippetEnd -->

#### In Kit: Direct Mutation

Kit components own their state. The `<=>` operator mutates the component's attribute directly, and changes sync automatically. This is standard Glimmer behavior:

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```ruby
# Draft API. Subject to change.

# In your component:
input(value: <=> [self, :text])

# When the user types, @text updates automatically.
# The component handles everything internally.
```
<!-- SPDX-SnippetEnd -->

---

## Chapter 5: The Framework

`ratatui_ruby-framework` is the [omakase](https://dhh.dk/2012/rails-is-omakase.html) framework for terminals. It provides conventions, scaffolding, and integrations.

### Structure

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```
app/
  models/      # ActiveRecord or Data objects
  views/       # Glimmer DSL files
  components/  # Kit components
config/
  database.yml
```
<!-- SPDX-SnippetEnd -->

### Generators

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```bash
# Conceptual sketch. CLI not yet designed.
tuis new framework my_app
tuis generate component sidebar
```
<!-- SPDX-SnippetEnd -->

### MVC for Terminals

The Framework adds the Model layer that Kit lacks. This completes the MVC pattern familiar to Rails developers:

- **Model** — ActiveRecord or Data objects in `app/models/`. Domain data and persistence.
- **View** — Glimmer DSL files in `app/views/`. Declarative UI descriptions.
- **Controller** — Kit components in `app/components/`. Handle input, update models, trigger re-renders.

Kit alone is a component architecture. Kit plus Framework is MVC.

### Reusing Tea Views in Kit

The Framework is built on Kit. However, you may want to reuse rendering logic written for Tea within a Kit component. The Adapter pattern makes this possible without mixing runtimes.

A Tea view function is a pure function: it takes an immutable model and returns a widget tree. You can call this function from a Kit component by adapting your mutable ActiveRecord model into a frozen snapshot. The Kit component handles events and mutations as usual; the Tea view function handles only the rendering.

<!-- SPDX-SnippetBegin -->
<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long
  SPDX-License-Identifier: MIT-0
-->
```ruby
# Draft API. Subject to change.
class DashboardWidget < Kit::Component
  def initialize(record)
    @record = record  # ActiveRecord, mutable
  end

  def render(tui, frame, area)
    # Adapt mutable state to frozen Tea model
    tea_model = RecordAdapter.to_tea_model(@record)

    # Call Tea view as a pure function
    widget_tree = MyTeaView.call(tea_model)

    # Render the returned tree
    frame.render_widget(widget_tree, area)
  end

  def handle_event(event)
    @record.update!(count: @record.count + 1) if event == :increment
    :consumed
  end
end
```
<!-- SPDX-SnippetEnd -->

The Adapter creates an immutable snapshot from mutable state, forming a clear boundary between the two paradigms. This approach shares rendering logic without embedding a Tea runtime inside a Kit application.

---

## Chapter 6: Licensing

| Layer | License |
|-------|---------|
| Engine | [LGPL-3.0-or-later](https://spdx.org/licenses/LGPL-3.0-or-later.html) |
| Tea | [LGPL-3.0-or-later](https://spdx.org/licenses/LGPL-3.0-or-later.html) |
| Kit | [LGPL-3.0-or-later](https://spdx.org/licenses/LGPL-3.0-or-later.html) |
| DSL | [LGPL-3.0-or-later](https://spdx.org/licenses/LGPL-3.0-or-later.html) |
| Framework | [LGPL-3.0-or-later](https://spdx.org/licenses/LGPL-3.0-or-later.html) |
| UI | [AGPL-3.0-or-later](https://spdx.org/licenses/AGPL-3.0-or-later.html) |
| UI Pro | Commercial only |

Commercial licenses are available for all layers. Contact [sales@kerricklong.com](mailto:sales@kerricklong.com) for pricing.

Documentation is [CC-BY-SA-4.0](https://spdx.org/licenses/CC-BY-SA-4.0.html). Code snippets in documentation are [MIT-0](https://spdx.org/licenses/MIT-0.html) (no attribution required).

Non-runtime gems provide build tooling, examples, and project infrastructure. They use [AGPL-3.0-or-later](https://spdx.org/licenses/AGPL-3.0-or-later.html):

| Gem | Purpose |
|-----|---------|
| `ratatui_ruby-devtools` | Build tooling |
| `ratatui_ruby-examples` | Example applications |
| `ratatui_ruby-website` | Project website (HTTPS) |
| `ratatui_ruby-sshsite` | Project website (SSH) |

---

## Chapter 7: Roadmap

RatatuiRuby follows an overlapping-serial development model. Each layer validates the one below, and higher layers begin during the beta phase of their foundation to battle-test the API before v1.0.0.

| Phase | Deliverable | Status |
|-------|-------------|--------|
| 1 | Engine (`ratatui_ruby`) | v0.9.0 Alpha |
| 2 | Functional Runtime (`ratatui_ruby-tea`) | v0.2.0 Pre-Release |
| 3 | Component Kit (`ratatui_ruby-kit`) | Planned |
| 4 | Syntax DSL (`glimmer-dsl-ratatui`) | Planned |
| 5 | Framework (`ratatui_ruby-framework`) | Planned |
| 6 | Premium Components | Future |

Development proceeds inside-out: core first, then runtime, then syntax. Work on Tea has begun to help validate the Engine. Work on the Component Kit will begin once Tea reaches stability, ensuring the foundation is complete and ready before any layer reaches v1.0.0.

---

## Competitive Positioning

RatatuiRuby respects the TUI ecosystem. Each tool has strengths.

| Approach | Strengths | Trade-offs |
|----------|-----------|------------|
| **CharmRuby** | Elm Architecture, Go ecosystem | Two runtimes (Ruby + Go) |
| **Rust (Ratatui)** | Maximum performance | Steeper learning curve for UI |
| **Go (Bubble Tea)** | Fast compilation, great tooling | Separate language |
| **RatatuiRuby** | Ruby expressiveness, Rust performance | Rust build dependency |

RatatuiRuby does not denigrate competitors. It differentiates on architecture.

---

## Governance

Performance matters: if a feature slows the render loop, it does not go in. Ergonomics matter: if an API feels clunky, it gets rewritten. Kindness matters: contributors are partners, and users are colleagues.

### Documentation

All public code follows the [Alexandrian Form](https://wiki.c2.com/?AlexandrianForm), or an inline version: Context, Problem, Solution. The documentation explains *why*, not just *what*.

### Versioning

Semantic versioning. RatatuiRuby breaks APIs only for correctness (or upstream alignment in the core gem). Deprecation warnings precede removal.

---

## Resources

- [Source Code](https://git.sr.ht/~kerrick/ratatui_ruby)
- [Documentation: stable](https://git.sr.ht/~kerrick/ratatui_ruby/tree/stable/item/doc/index.md) • [trunk](https://git.sr.ht/~kerrick/ratatui_ruby/tree/trunk/item/doc/index.md)
- [Troubleshooting & FAQ](./troubleshooting.md)
- [Contributing Guide](./contributing.md)
- [Code of Conduct](./code_of_conduct.md)
- [Branching and Releases](./branching_and_releases.md)
- [Documentation Guide](./documentation_guide.md)
- [Governance](./governance.md)
- [Project History](./history/index.md)
- [Bug Tracker](https://todo.sr.ht/~kerrick/ratatui_ruby)
- Mailing Lists: [Discussion](https://lists.sr.ht/~kerrick/ratatui_ruby-discuss) • [Development](https://lists.sr.ht/~kerrick/ratatui_ruby-devel) • [Announcements](https://lists.sr.ht/~kerrick/ratatui_ruby-announce)
