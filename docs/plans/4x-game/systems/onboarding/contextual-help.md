# Contextual Help System

## Overview

The contextual help system provides comprehensive in-game documentation accessible via "?" buttons throughout the UI. It features context-sensitive topic suggestions, search functionality, and "Show Me How" interactive demonstrations that guide players without leaving the game.

## Design Principles

- **Always Available**: "?" button on every major screen and UI panel
- **Context-Aware**: Suggest topics relevant to current screen/action
- **Comprehensive**: Cover all game mechanics and systems
- **Interactive**: "Show Me How" demonstrations beyond static text
- **Searchable**: Quick topic discovery via search

## Use Cases

### When Players Use Help

✅ **Learning New Mechanics**

- "How does trade work?"
- "What do these icons mean?"
- "How do I win a diplomatic victory?"

✅ **Refreshing Memory**

- Returning players who forgot details
- Complex mechanics not used often
- Specific stat calculations

✅ **Troubleshooting**

- "Why can't I build this?"
- "Why is my city unhappy?"
- "How do I fix low food production?"

✅ **Discovery**

- Browsing available features
- Learning advanced strategies
- Understanding faction differences

## UI Specification

### Help Panel Layout

```text
┌──────────────────────────────────────────────────┐
│ [?] In-Game Help                       [X]       │
├──────────────────────────────────────────────────┤
│ [Search: Enter keyword...]               [🔍]   │
├────────────────────┬─────────────────────────────┤
│ Topics             │ > Getting Started           │
│                    │   - Basic Controls          │
│ 📚 Categories     │   - Your First City         │
│  > Getting Started │   - Movement & Exploration  │
│  > Cities          │                             │
│  > Military        │ **Basic Controls**          │
│  > Resources       │                             │
│  > Diplomacy       │ Camera Movement:            │
│  > Technology      │ • WASD or Arrow Keys        │
│  > Victory         │ • Middle Mouse Drag         │
│                    │ • Edge Scrolling (optional) │
│ 🔥 Popular        │                             │
│  - Trade Routes    │ Unit Selection:             │
│  - Combat          │ • Left Click on unit        │
│  - Research        │ • Drag box for multiple     │
│                    │                             │
│ 💡 Suggested      │ [▶ Show Me How]             │
│  - City Growth     │                             │
│  - Food            │ Related Topics:             │
│                    │ • Map Navigation            │
│ 🕐 Recent         │ • Unit Management           │
│  - Barracks        │                             │
│  - Gold Economy    │                             │
└────────────────────┴─────────────────────────────┘
```

### Size and Position

- **Size**: 800x600px (adjustable, player can resize)
- **Position**: Centered on screen by default
- **Movable**: Player can drag to reposition
- **Persistent**: Position saved in preferences
- **Resizable**: Minimum 600x400px, maximum 1200x800px

### Components

#### Header

- Help icon and title
- Close button
- Optional minimize button (collapses to floating "?" button)

#### Search Bar

- Prominent search input
- Real-time search suggestions
- Recent searches dropdown
- Search icon button

#### Topic Navigation (Left Panel)

- **Categories**: Expandable tree of organized topics
- **Popular**: Most-viewed topics (analytics-driven)
- **Suggested**: Context-sensitive recommendations
- **Recent**: Player's recently viewed topics

#### Content Area (Right Panel)

- Rich text with markdown formatting
- Images and diagrams
- Tables for stats/comparisons
- "Show Me How" interactive demo buttons
- Related topics links
- Breadcrumb navigation

## Content Organization

### Topic Hierarchy

```text
Getting Started
├── Basic Controls
│   ├── Camera Movement
│   ├── Unit Selection
│   └── UI Navigation
├── Your First City
│   ├── Founding a City
│   ├── City Panel Overview
│   └── Building Your First Structure
└── Movement & Exploration
    ├── Moving Units
    ├── Fog of War
    └── Points of Interest

Cities
├── City Management
│   ├── Population & Growth
│   ├── Happiness
│   └── Production
├── Buildings
│   ├── Economic Buildings
│   ├── Military Buildings
│   └── Cultural Buildings
└── Specialists & Citizens
    └── Tile Assignment

Military
├── Units
│   ├── Unit Types
│   ├── Recruitment
│   └── Experience & Promotions
├── Combat
│   ├── Combat Basics
│   ├── Terrain Effects
│   └── Army Composition
└── Fortifications
    ├── Defensive Structures
    └── Siege Warfare

Resources
├── Resource Types
│   ├── Strategic Resources
│   ├── Luxury Resources
│   └── Bonus Resources
├── Trade
│   ├── Trade Routes
│   ├── Markets
│   └── Economy Management
└── Gold & Production
    ├── Income Sources
    └── Maintenance Costs

Diplomacy
├── Relations
│   ├── Building Relationships
│   ├── Alliance System
│   └── Trade Agreements
├── Espionage
│   └── Spy Operations
└── Negotiations
    └── Treaty Types

Technology
├── Research System
│   ├── Tech Tree Overview
│   ├── Research Speed
│   └── Tech Trading
└── Eras
    └── Era Progression

Victory Conditions
├── Conquest Victory
├── Cultural Victory  
├── Scientific Victory
├── Diplomatic Victory
└── Score Victory
```

### Topic Structure

Each help topic follows consistent structure:

```markdown
# Topic Title

## Summary
Brief 1-2 sentence overview of topic.

## Detailed Explanation
Comprehensive description with:
- How it works
- Why it matters
- When to use it

## Game Mechanics
Technical details:
- Formulas and calculations
- Stat modifiers
- Special conditions

## Tips & Strategies
Practical advice:
- Best practices
- Common mistakes to avoid
- Advanced techniques

## Related Topics
- [Link to Related Topic 1]
- [Link to Related Topic 2]

## Show Me How
[Interactive demonstration button]
```

## Context-Sensitive Suggestions

### How It Works

The help system analyzes current game state to suggest relevant topics:

```csharp
public interface IContextualHelpProvider
{
    // Get suggested topics based on current context
    List<HelpTopic> GetSuggestedTopics(GameContext context);
    
    // Get help topics relevant to specific UI screen
    List<HelpTopic> GetScreenTopics(string screenId);
    
    // Get help topics related to current player issue
    List<HelpTopic> GetTroubleshootingTopics(GameState state);
}

public class GameContext
{
    public string CurrentScreen { get; set; }
    public string SelectedEntity { get; set; }  // City, unit, etc.
    public List<string> RecentActions { get; set; }
    public PlayerState PlayerState { get; set; }
}
```

### Suggestion Examples

| Context | Suggested Topics |
|---------|-----------------|
| City panel open | City Management, Buildings, Population Growth |
| Low happiness in city | Happiness, Luxury Resources, Buildings |
| Unit selected | Unit Movement, Combat Basics, Promotions |
| Research screen | Technology Tree, Research Speed, Era Progression |
| Negative gold | Gold Economy, Maintenance Costs, Trade Routes |
| First combat | Combat Basics, Unit Types, Terrain Effects |

## Search Functionality

### Search Features

- **Real-time Results**: Search as you type
- **Fuzzy Matching**: Tolerates typos and partial matches
- **Synonym Support**: "money" finds "gold economy"
- **Weighted Results**: Prioritize exact matches, then partial, then related

### Search Implementation

```csharp
public interface IHelpSearchSystem
{
    // Search help topics
    List<SearchResult> Search(string query, int maxResults = 10);
    
    // Get search suggestions for autocomplete
    List<string> GetSearchSuggestions(string partialQuery, int maxSuggestions = 5);
    
    // Track search analytics
    void TrackSearch(string query, bool foundResults, string topResultId);
}

public class SearchResult
{
    public HelpTopic Topic { get; set; }
    public float Relevance { get; set; }  // 0.0 to 1.0
    public List<string> MatchedKeywords { get; set; }
    public string MatchPreview { get; set; }  // Snippet showing match context
}
```

### Search Ranking

Results ranked by:

1. **Exact title match** (100% relevance)
2. **Title contains query** (80% relevance)
3. **Keyword match** (60% relevance)
4. **Content contains query** (40% relevance)
5. **Related topic** (20% relevance)

Boosted by:

- **Popularity**: Frequently viewed topics ranked higher
- **Recency**: Recently updated content boosted
- **Context**: Topics relevant to current screen boosted

## "Show Me How" Interactive Demonstrations

### Demonstration Types

#### Highlight-Based Demo

Highlights UI elements in sequence with explanatory text.

```markdown
---
topic_id: demo_build_first_building
title: "Build First Building"
demo_id: build_first_building
type: highlight_sequence
keywords:
  - building
  - tutorial
  - highlight
steps:
  - highlight: city_panel_button
    text: "Click the city to open its panel"
    wait_for: click

  - highlight: construction_tab
    text: "Open the construction tab"
    wait_for: click

  - highlight: building_list
    text: "Choose a building from the list"
    wait_for: click

  - highlight: confirm_button
    text: "Confirm your selection"
    wait_for: click

  - complete_message: "Great! Your city is now constructing the building."
---

# Build First Building (demo)

Step-by-step highlight sequence to guide players through constructing a building.
```

#### Simulated Demo

Simplified simulation in a sandboxed environment.

```markdown
---
topic_id: demo_combat_simulation
title: "Combat Simulation"
demo_id: combat_simulation
type: sandbox
keywords:
  - combat
  - sandbox
  - tutorial
scenario:
  map_size: 5x5
  units:
    - type: swordsman
      position: [2, 2]
      player_controlled: true
    - type: archer
      position: [4, 3]
      ai_controlled: true
instructions:
  - "Attack the enemy archer with your swordsman"
  - "Notice how terrain affects combat strength"
  - "Victory! Check the combat log for details"
success_condition: enemy_units_defeated
---

# Combat Simulation (demo)

A sandboxed mini-scenario to teach combat basics.
```

#### Video/GIF Demo

Short video showing the process.

```markdown
---
topic_id: demo_trade_route_setup
title: "Trade Route Setup"
demo_id: trade_route_setup
type: video
keywords:
  - trade
  - video
  - demo
video_file: demos/trade_routes.mp4
duration: 45
captions: true
interactive_pause_points:
  - timestamp: 15
    question: "Which resource would you choose?"
    options: ["Gold", "Food", "Production"]
---

# Trade Route Setup (demo)

Short video demonstrating how to create trade routes, with interactive pauses.
```

### Demo Controls

While "Show Me How" demo active:

- **Next**: Advance to next step manually
- **Pause**: Pause automatic progression
- **Restart**: Start demo from beginning
- **Exit**: Return to help content

## Integration with Other Systems

### From Tutorials

Tutorial popups and panels include "Learn More" buttons:

```csharp
// Tutorial panel includes help link
var panel = new SidePanelConfig {
    Content = "Cities produce resources...",
    Buttons = new List<PanelButton> {
        new PanelButton {
            Label = "Learn More",
            Action = () => helpSystem.OpenTopic("city_management"),
            IconClass = "help-icon"
        }
    }
};
```

### From Enhanced Tooltips

Enhanced tooltips link to relevant help topics:

```csharp
var tooltip = new EnhancedTooltipConfig {
    Title = "Barracks",
    Description = "Trains military units",
    LearnMoreTopic = "military_buildings",
    OnLearnMoreClick = () => helpSystem.OpenTopic("military_buildings", context: "barracks")
};
```

### From Error Messages

Error messages link to troubleshooting help:

```csharp
// When player can't perform action
if (!CanBuildBuilding(building))
{
    ShowError(
        message: "Cannot build: Insufficient gold",
        helpTopic: "gold_economy",
        helpContext: "insufficient_funds"
    );
}
```

## Technical Requirements

### Help System API

```csharp
public interface IContextualHelpSystem
{
    // Open help panel to specific topic
    void OpenTopic(string topicId, string context = null);
    
    // Open help panel with search query
    void OpenSearch(string query);
    
    // Open help panel to category
    void OpenCategory(string categoryId);
    
    // Start interactive demonstration
    void StartDemo(string demoId);
    
    // Close help panel
    void Close();
    
    // Check if topic exists
    bool TopicExists(string topicId);
    
    // Get topic metadata (for link validation)
    HelpTopicMetadata GetTopicMetadata(string topicId);
}
```

### Content Format

Help content stored as structured markdown with metadata:

```yaml
---
topic_id: city_management
title: "City Management"
category: cities
keywords:
  - city
  - management
  - population
  - growth
  - happiness
related_topics:
  - city_buildings
  - population_growth
  - happiness_system
demo_id: city_management_basics
last_updated: 2026-02-01
---

# City Management

## Summary
Cities are the foundation of your civilization...

[Rest of markdown content]
```

### Analytics Events

```csharp
// Track help usage
analytics.Track("HelpPanelOpened", new {
    source,  // tutorial, tooltip, manual, error_message
    initial_topic,
    experience_level
});

analytics.Track("HelpTopicViewed", new {
    topic_id,
    view_duration_seconds,
    scrolled_to_bottom,
    came_from_search
});

analytics.Track("HelpSearchPerformed", new {
    query,
    results_count,
    clicked_result_rank,
    clicked_topic_id
});

analytics.Track("ShowMeHowStarted", new {
    demo_id,
    topic_id,
    source
});

analytics.Track("ShowMeHowCompleted", new {
    demo_id,
    completion_time_seconds,
    steps_completed
});
```

## Accessibility

### Screen Reader Support

- Help panel structure announced (navigation, content areas)
- Search results announced with count
- Topic content read in order
- Demo steps narrated
- Related links announced as links

### Keyboard Navigation

- **F1**: Open help panel
- **Ctrl+F**: Focus search box
- **Arrow Keys**: Navigate topic tree
- **Enter**: Open selected topic
- **Escape**: Close help panel
- **Tab**: Navigate between panel sections

### Visual Accessibility

- High contrast mode support
- Text scalable to 200%
- Images have alt text
- Videos have captions
- Demos work without visual cues (screen reader compatible)

## Success Metrics

### Usage Metrics

- **Help Panel Opens**: Track frequency and source
- **Topic Views**: Identify most helpful topics
- **Search Usage**: % of sessions using search
- **Demo Completions**: Track "Show Me How" engagement

### Effectiveness Metrics

- **Self-Service Rate**: % of player issues resolved without external resources
- **Return Visit Rate**: % of players using help multiple times
- **Topic Completion**: % of topics read to bottom
- **Search Success**: % of searches resulting in topic click

### Quality Metrics

- **Helpfulness Rating**: >= 4.0/5.0 per topic
- **Content Clarity**: >= 4.2/5.0 rating
- **Demo Usefulness**: >= 4.3/5.0 rating

## Testing Requirements

### Unit Tests

- Search returns correct results
- Context suggestions match current screen
- Topic links navigate correctly
- Demo progression logic works

### Integration Tests

- Help opens from all "?" buttons
- "Learn More" links from tutorials work
- Demo highlights correct UI elements
- Search index up to date with content

### Content Tests

- All topics have required sections
- All links point to existing topics
- All images load correctly
- All demos functional

### Usability Tests

- Players can find topics via search
- Suggested topics are relevant
- "Show Me How" demos clear and helpful
- Content answers common questions

## Localization

### Text Assets

- 100+ help topic articles (500-2000 words each)
- Category names and descriptions
- Search keywords and synonyms
- Demo narration text
- Error/status messages

### Media Assets

- Screenshots with UI labels (require localization)
- Diagrams with text annotations
- Demo videos with captions
- Infographics

### Technical Considerations

- Search must support language-specific stemming
- Keywords/synonyms per language
- Right-to-left layout for RTL languages
- Text expansion (German ~40% longer)

## Future Enhancements

### Post-MVP Features

- **Community Wiki**: Player-contributed tips and strategies
- **Video Library**: Full video tutorial series
- **AI Assistant**: Natural language Q&A chatbot
- **Personalized Help**: Recommendations based on play style
- **Offline Documentation**: Exportable PDF/HTML help

### Advanced Analytics

- **Topic Effectiveness Scoring**: Identify poorly written topics
- **Content Gap Analysis**: Find missing topics based on failed searches
- **Demo Optimization**: A/B test different demo formats
- **Predictive Help**: Suggest topics before player searches
