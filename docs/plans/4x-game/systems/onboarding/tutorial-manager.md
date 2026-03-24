# Tutorial Manager and Scripting System

## Overview

The tutorial manager is the core orchestration system that drives all tutorial content delivery. It listens to game events, tracks player progress, manages tutorial state, triggers appropriate delivery mechanisms (modals, side panels, highlights), and integrates analytics to measure effectiveness.

## Table of contents

- [Overview](#overview)
- [Design Principles](#design-principles)
- [System Architecture](#system-architecture)
- [Tutorial Scripting Format](#tutorial-scripting-format)
- [State Management](#state-management)
- [Event System Integration](#event-system-integration)
- [Delivery Mechanism Integration](#delivery-mechanism-integration)
- [Analytics Integration](#analytics-integration)

## Feature status

Not started

> Allowed statuses: Not started, In discovery, In design, In development, In test, In review, Completed, Abandoned, Blocked — update this field to the current status for tracking.

## Definition of terms

- Tutorial: a guided, interactive lesson introducing a feature.
- Onboarding: a sequence of introductory tutorials, tips and UI highlights for new players.

## Design Principles

### User Experience Principles

- **Optional**: The player can skip the tutorial, on the game screen or at any time
- **Interactive**: The tutorial interacts with the player to impart knowledge about the game's systems
- **In-game**: All knowledge needed to play the game will be within the game, with no need to refer to
  an external wiki.

### Technical Principles

- **Event-Driven**: React to game events rather than polling game state
- **Declarative**: Tutorial content defined in data files, not code
- **Transparent**: Tutorial content defined in clear markdown files, not a
  config language with header metadata and defined and readable patterns to
  mark content. [Ink](https://github.com/inkle/ink) would also be a suitable
  format
- **Stateful**: Track progress, save/resume capability
- **Modular**: Pluggable delivery mechanisms
- **Observable**: Comprehensive analytics integration

## System Architecture

### Core Components

```plain
┌─────────────────────────────────────────────────┐
│           Tutorial Manager Core                 │
│                                                 │
│  ┌──────────────┐      ┌──────────────┐         │
│  │ Event        │      │ State        │         │
│  │ Dispatcher   │◄────►│ Manager      │         │
│  └──────────────┘      └──────────────┘         │
│         ▲                      ▲                │
│         │                      │                │
│         ▼                      ▼                │
│  ┌──────────────┐      ┌──────────────┐         │
│  │ Tutorial     │      │ Progress     │         │
│  │ Script       │      │ Tracker      │         │
│  │ Engine       │      └──────────────┘         │
│  └──────────────┘                               │
│         │                                       │
└─────────┼───────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────┐
│        Delivery Mechanism Layer                 │
│                                                 │
│  ┌─────────┐  ┌─────────┐  ┌──────────┐         │
│  │ Modal   │  │  Side   │  │    UI    │         │
│  │ Popups  │  │  Panel  │  │Highlights│  ...    │
│  └─────────┘  └─────────┘  └──────────┘         │
└─────────────────────────────────────────────────┘
```

### Data Flow

1. **Game Event** occurs (e.g., city founded, unit selected)
2. **Event Dispatcher** broadcasts event to registered listeners
3. **Tutorial Script Engine** evaluates trigger conditions
4. **State Manager** checks if tutorial already shown/completed
5. **Tutorial Manager** selects appropriate delivery mechanism
6. **Delivery Mechanism** displays tutorial content
7. **Progress Tracker** updates state on completion
8. **Analytics** logs all interactions

## Tutorial Scripting Format

### Markdown-First Tutorial Definition

Per the protected Design Principles, tutorial content should be authored primarily as human-readable Markdown files (optionally using a small, well-defined front-matter for metadata). This keeps content transparent to designers and localizers while allowing structured trigger/sequence data to be embedded or referenced.

Example using front-matter + markdown body (recommended):

```markdown
---
tutorial_id: exploration_basics
experience_level: beginner
category: exploration
priority: high
version: 1.0
author: tutorial_team
last_updated: 2026-02-01
---

# Explore the World

Use your scout to reveal the map and discover resources.

## Triggers
- event: game_started
  conditions:
    - player.experience_level == beginner
  delay: 3

## Sequence
- id: camera_controls
  delivery: side_panel
  title: "Navigate the Map"
  content: |
    Use **WASD** or arrow keys to pan the camera.
    Use the **mouse wheel** to zoom in and out.
    Try moving the camera around!
  completion:
    event: camera_moved
    params:
      distance_threshold: 100

- id: select_scout
  delivery: ui_highlight
  target: player_scout_unit
  style: pulsing_glow
  tooltip: "This is your scout. Click to select it."
  parallel: side_panel
---
```

Notes:

- The front-matter contains searchable metadata and simple routing fields.
- The body is human-readable content with structured blocks for triggers and steps.
- Designers may use a simple YAML-like block inside the markdown body for structured step definitions (as above) or use a small companion JSON/YAML file per tutorial when necessary.

This approach preserves the Design Principle that tutorial content remains author-friendly and readable in plain Markdown while allowing machines to parse structured pieces.

### Tutorial Script Schema

Schema guidance (for front-matter + structured blocks inside markdown):

- `tutorial_id`: string (required, unique identifier)
- `experience_level`: enum (beginner|intermediate|expert|all)
- `category`: string (for organization)
- `priority`: enum (high|medium|low)
- `version`, `author`, `last_updated`: metadata fields

Structured blocks in the markdown body should include `triggers` and a `sequence` of `steps` where each step defines:

- `id` (string)
- `delivery` (modal|side_panel|ui_highlight|help_topic)
- `content` (markdown or body reference)
- `completion` (event, params, optional timeout)
- `parallel` (optional other delivery)

Design note: prefer readable, shallow structure in the markdown body. If a tutorial requires very large structured data, a companion `.yaml` or `.json` file may be used, but markdown-first files must remain the authoritative, human-readable source.

## State Management

### Tutorial State Model

```csharp
public class TutorialState
{
    public string TutorialId { get; set; }
    public TutorialStatus Status { get; set; }
    public int CurrentStep { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime? CompletionTime { get; set; }
    public Dictionary<string, object> CustomState { get; set; }
    public List<StepState> StepHistory { get; set; }
}

public enum TutorialStatus
{
    NotStarted,
    InProgress,
    Paused,
    Completed,
    Skipped,
    Failed
}

public class StepState
{
    public string StepId { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime? CompletionTime { get; set; }
    public int AttemptsCount { get; set; }
    public bool WasSkipped { get; set; }
    public Dictionary<string, object> Metadata { get; set; }
}
```

### Persistence

Tutorial state persisted to player save file:

```json
{
  "player_id": "player_123",
  "tutorial_preferences": {
    "experience_level": "beginner",
    "enabled_categories": ["exploration", "cities", "combat"],
    "disabled_tutorials": [],
    "dont_show_again": ["first_game_welcome"]
  },
  "tutorial_progress": {
    "exploration_basics": {
      "status": "completed",
      "current_step": 4,
      "start_time": "2026-02-06T10:30:00Z",
      "completion_time": "2026-02-06T10:35:23Z",
      "steps_completed": 4,
      "was_skipped": false
    },
    "city_management_basics": {
      "status": "in_progress",
      "current_step": 2,
      "start_time": "2026-02-06T10:36:00Z",
      "completion_time": null,
      "steps_completed": 1,
      "was_skipped": false
    }
  }
}
```

## Event System Integration

### Game Event Registration

Tutorial manager listens to comprehensive game events:

```csharp
public interface ITutorialEventSystem
{
    // Register event handlers
    void RegisterHandler(string eventName, Func<EventData, bool> handler);
    
    // Unregister handlers
    void UnregisterHandler(string eventName, Func<EventData, bool> handler);
    
    // Emit game event
    void EmitEvent(string eventName, EventData data);
    
    // Query recent events
    List<GameEvent> GetRecentEvents(TimeSpan window);
}

// Example game events
public static class GameEvents
{
    public const string GameStarted = "game_started";
    public const string TurnEnded = "turn_ended";
    public const string CityFounded = "city_founded";
    public const string UnitSelected = "unit_selected";
    public const string UnitMoved = "unit_moved";
    public const string BuildingConstructed = "building_constructed";
    public const string TechResearched = "tech_researched";
    public const string CombatEngaged = "combat_engaged";
    public const string ResourceDiscovered = "resource_discovered";
    public const string DiplomacyInitiated = "diplomacy_initiated";
    // ... 50+ more events
}
```

### Trigger Condition Evaluation

Tutorial triggers use expression evaluator for conditions:

```csharp
public interface IConditionEvaluator
{
    // Evaluate boolean expression with game context
    bool Evaluate(string expression, GameContext context);
}

// Example expressions
"player.experience_level == beginner"
"player.turn_number <= 10"
"player.cities.count == 0"
"player.units.any(u => u.type == 'scout')"
"!tutorial_progress.exploration_completed"
"game.difficulty == 'easy'"
```

### Event Priority Queue

Multiple tutorials may trigger on same event; priority system determines order:

```csharp
public class TutorialTriggerQueue
{
    private PriorityQueue<TutorialTrigger> _queue;
    
    public void Enqueue(TutorialTrigger trigger, int priority)
    {
        _queue.Enqueue(trigger, priority);
    }
    
    public TutorialTrigger Dequeue()
    {
        return _queue.Dequeue();
    }
    
    // Respect timing rules (max frequency, cooldowns)
    public bool CanTriggerNow(TutorialTrigger trigger)
    {
        var lastTriggerTime = GetLastTriggerTime();
        var minimumInterval = GetMinimumInterval(player.ExperienceLevel);
        
        return DateTime.Now - lastTriggerTime >= minimumInterval;
    }
}
```

## Delivery Mechanism Integration

### Pluggable Delivery System

Tutorial manager doesn't know about specific delivery mechanisms:

```csharp
public interface IDeliveryMechanism
{
    string Name { get; }  // "modal", "side_panel", "ui_highlight", etc.
    
    // Display tutorial content
    Task Display(DeliveryConfig config);
    
    // Check if can display now (e.g., no conflicts)
    bool CanDisplay(DeliveryConfig config);
    
    // Dismiss current display
    void Dismiss();
    
    // Check if currently displaying
    bool IsActive { get; }
}

// Tutorial manager delegates to appropriate mechanism
public class TutorialManager
{
    private Dictionary<string, IDeliveryMechanism> _mechanisms;
    
    public async Task ExecuteStep(TutorialStep step)
    {
        var mechanism = _mechanisms[step.Delivery];
        
        if (mechanism.CanDisplay(step.Config))
        {
            await mechanism.Display(step.Config);
            TrackStepDisplayed(step);
        }
        else
        {
            // Queue for later or use fallback
            QueueStep(step);
        }
    }
}
```

### Delivery Configuration Mapping

Tutorial scripts map to delivery-specific configurations:

```csharp
// YAML config
delivery: modal
config:
  title: "Welcome"
  content: "Tutorial content..."
  buttons:
    - label: "Next"
      action: advance

// Maps to
var modalConfig = new ModalPopupConfig {
    TutorialId = tutorialId,
    Title = config["title"],
    Steps = new List<ModalTutorialStep> {
        new ModalTutorialStep {
            Content = config["content"],
            Buttons = ParseButtons(config["buttons"])
        }
    }
};
```

## Analytics Integration

### Comprehensive Event Tracking

Tutorial manager tracks all interactions for optimization:

```csharp
public interface ITutorialAnalytics
{
    // Tutorial lifecycle events
    void TrackTutorialStarted(string tutorialId, TutorialContext context);
    void TrackTutorialCompleted(string tutorialId, TimeSpan duration, int stepsCompleted);
    void TrackTutorialSkipped(string tutorialId, int currentStep, string reason);
    void TrackTutorialPaused(string tutorialId, int currentStep);
    void TrackTutorialResumed(string tutorialId, int currentStep);
    
    // Step-level events
    void TrackStepDisplayed(string tutorialId, string stepId, string delivery);
    void TrackStepCompleted(string tutorialId, string stepId, TimeSpan duration);
    void TrackStepTimeout(string tutorialId, string stepId);
    void TrackStepRetry(string tutorialId, string stepId, int attemptNumber);
    
    // Interaction events
    void TrackButtonClicked(string tutorialId, string stepId, string buttonLabel);
    void TrackLearnMoreClicked(string tutorialId, string stepId, string helpTopic);
    void TrackDismissed(string tutorialId, string stepId, string dismissMethod);
    
    // Aggregate queries
    TutorialMetrics GetTutorialMetrics(string tutorialId);
    TutorialMetrics GetTutorialMetrics(string tutorialId, TimeSpan period);
    Dictionary<string, float> GetCompletionRates();
    Dictionary<string, TimeSpan> GetAverageCompletionTimes();
    List<TutorialDropoffPoint> GetDropoffPoints(string tutorialId);
}

public class TutorialMetrics
{
    public int TotalStarts { get; set; }
    public int TotalCompletions { get; set; }
    public float CompletionRate => (float)TotalCompletions / TotalStarts;
    public int TotalSkips { get; set; }
    public TimeSpan AverageCompletionTime { get; set; }
    public Dictionary<string, StepMetrics> StepMetrics { get; set; }
}
```

### A/B Testing Support

Framework for testing different tutorial approaches:

```csharp
public class TutorialVariant
{
    public string VariantId { get; set; }
    public string TutorialId { get; set; }
    public string Description { get; set; }
    public float TrafficPercent { get; set; }  // % of players to show this variant
    public Dictionary<string, object> Overrides { get; set; }
}

// Example: Test different tutorial timing
var variants = new List<TutorialVariant> {
    new TutorialVariant {
        VariantId = "immediate",
        TrafficPercent = 0.5,
        Overrides = new { delay = 0 }
    },
    new TutorialVariant {
        VariantId = "delayed",
        TrafficPercent = 0.5,
        Overrides = new { delay = 10 }
    }
};
```

## Technical Requirements

### Core API

```csharp
public interface ITutorialManager
{
    // Lifecycle management
    void Initialize(TutorialConfig config);
    void Shutdown();
    
    // Tutorial control
    void StartTutorial(string tutorialId);
    void PauseTutorial(string tutorialId);
    void ResumeTutorial(string tutorialId);
    void SkipTutorial(string tutorialId);
    void ResetTutorial(string tutorialId);
    
    // State queries
    TutorialState GetTutorialState(string tutorialId);
    bool IsTutorialActive();
    string GetActiveTutorialId();
    
    // Configuration
    void SetExperienceLevel(ExperienceLevel level);
    void EnableCategory(string category);
    void DisableCategory(string category);
    void SetTutorialEnabled(string tutorialId, bool enabled);
    
    // Tutorial registration (for dynamic content)
    void RegisterTutorial(TutorialDefinition definition);
    void UnregisterTutorial(string tutorialId);
}
```

### Tutorial Loading

Tutorials loaded from content files at startup:

```csharp
public class TutorialLoader
{
    public List<TutorialDefinition> LoadTutorials(string contentPath)
    {
        var tutorials = new List<TutorialDefinition>();
        
        foreach (var file in Directory.GetFiles(contentPath, "*.tutorial.md"))
        {
          var md = File.ReadAllText(file);
          // Parse front-matter (YAML) and body; use a front-matter parser to extract metadata and structured blocks
          var tutorial = MarkdownFrontMatterParser.Parse<TutorialDefinition>(md);

          ValidateTutorial(tutorial);
          tutorials.Add(tutorial);
        }
        
        return tutorials;
    }
    
    private void ValidateTutorial(TutorialDefinition tutorial)
    {
        // Validate required fields
        // Check for circular dependencies
        // Verify event names exist
        // Validate delivery configurations
        // Check localization coverage
    }
}
```

## Performance Considerations

### Event Processing Optimization

- **Batch Processing**: Group multiple events and process in batch
- **Lazy Evaluation**: Only evaluate conditions when event relevant
- **Caching**: Cache parsed expressions and compiled conditions
- **Throttling**: Limit event processing rate to avoid frame drops

### Memory Management

- **Lazy Loading**: Load tutorial definitions on-demand
- **Unloading**: Unload completed tutorial content
- **State Compression**: Compress tutorial history for long sessions
- **Resource Pooling**: Reuse UI components rather than recreating

## Accessibility

### Tutorial Manager Responsibilities

- Announce tutorial start/end to screen readers
- Provide keyboard shortcuts for tutorial control
- Support high contrast delivery mechanisms
- Enable/disable animations based on preferences
- Ensure all tutorial content has text alternatives

## Success Metrics

### System Performance

- **Event Processing Time**: < 1ms per event
- **Condition Evaluation**: < 0.1ms per condition
- **State Save Time**: < 100ms
- **Tutorial Load Time**: < 500ms for all tutorials

### Content Quality

- **Tutorial Completion Rate**: >= 70% across all tutorials
- **Average Drop-off Point**: After step 3 or later (indicates good pacing)
- **Skip Rate**: <= 15% overall
- **Resume Rate**: >= 60% of paused tutorials resumed

## Testing Requirements

### Unit Tests

- Event trigger condition evaluation
- State transitions (not started → in progress → completed)
- Persistence (save/load state correctly)
- Tutorial sequencing (steps advance correctly)
- Priority queue (highest priority triggers first)

### Integration Tests

- End-to-end tutorial flow (start to completion)
- Multiple concurrent tutorials (queue correctly)
- Save/load during active tutorial
- Tutorial resume after game restart
- Analytics events fire correctly

### Performance Tests

- 1000+ events per second processing
- 100+ active tutorial definitions loaded
- Memory usage stays under 100MB
- No frame rate impact from tutorial system

### Content Validation Tests

- All tutorial YAML files parse correctly
- All referenced events exist
- All delivery configurations valid
- All help topics exist
- No circular tutorial dependencies

## Localization

### Content Separation

- Tutorial content in language-specific YAML files
- Tutorial logic/structure shared across languages
- Validation ensures all languages have same tutorials
- Language-specific trigger conditions (if needed)

### File Organization

```plain
content/
  tutorials/
    en/
      exploration.tutorial.yaml
      cities.tutorial.yaml
      ...
    de/
      exploration.tutorial.yaml
      cities.tutorial.yaml
      ...
    shared/
      tutorial_metadata.yaml
      event_definitions.yaml
```

## Future Enhancements

### Post-MVP Features

- **Dynamic Tutorial Generation**: AI creates tutorials for mods/custom content
- **Tutorial Editor**: Visual tool for designers to create tutorials
- **Player-Created Tutorials**: Community tutorial sharing
- **Tutorial Speedrun Mode**: Gamified tutorial completion
- **Adaptive Difficulty**: Tutorials adjust based on player performance

### Advanced Analytics

- **ML-Driven Optimization**: AI optimizes tutorial timing and content
- **Predictive Triggering**: Predict when player needs help before they struggle
- **Personalization Engine**: Customize tutorials to individual player style
- **Anomaly Detection**: Identify struggling players automatically
- **Content Gap Analysis**: Find missing tutorials based on player behavior
