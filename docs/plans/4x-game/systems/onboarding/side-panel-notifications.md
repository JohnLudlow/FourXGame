# Side Panel Notification System

## Overview

Non-blocking side panel notifications provide contextual guidance without interrupting gameplay. Inspired by Rise of Nations, this system delivers tips, suggestions, and tutorial content at the screen edge while allowing players to continue interacting with the game.

## Table of contents

- [Overview](#overview)
- [Design Principles](#design-principles)
- [Use Cases](#use-cases)
- [UI Specification](#ui-specification)
- [Behavior Specification](#behavior-specification)
- [Content Types](#content-types)
- [Technical Requirements](#technical-requirements)

## Feature status

Not started

> Allowed statuses: Not started, In discovery, In design, In development, In test, In review, Completed, Abandoned, Blocked — update this field to the current status for tracking.

## Definition of terms

- Side Panel: A non-blocking UI panel used to present context-sensitive guidance.
- Sequence: A multi-step tutorial progression shown through side panels.

## Design Principles

- **Non-Blocking**: Game continues while panel is visible
- **Contextual**: Appears when relevant to current player actions
- **Dismissible**: Auto-dismisses or manually closeable
- **Unobtrusive**: Positioned to avoid obscuring critical UI
- **Progressive**: Supports multi-step tutorial sequences

## Use Cases

### When to Use Side Panels

✅ **Primary Tutorial Delivery**

- Step-by-step guidance for beginners
- Contextual hints for intermediate players
- Just-in-time tips triggered by player actions

✅ **Advisor Suggestions**

- Council members offering strategic advice
- Recommendations based on game state
- Warnings about negative trends

✅ **Achievement/Discovery Notifications**

- Points of interest discovered
- Milestones reached
- New features unlocked

❌ **When NOT to Use Side Panels**

- Critical information requiring immediate attention (use modals)
- Information needed for current decision (use tooltips)
- Extremely urgent warnings (use alert system)

## UI Specification

### Layout Structure

```plain
Game View                   ┌──────────────────────────┐
                            │ [!] Tutorial      [─][X] │
                            ├──────────────────────────┤
                            │                          │
                            │ 🔍 Explore the Map      │
                            │                          │
                            │ Use your scout to        │
                            │ reveal unexplored        │
                            │ areas. Look for          │
                            │ resources and ruins!     │
                            │                          │
                            │ [Show Me How]            │
                            │                          │
                            │ Step 2 of 5              │
                            │ ▓▓▓▓░░░░░░               │
                            └──────────────────────────┘
```

### Position Options

- **Right Side** (Default): Anchored to right edge, vertically centered
- **Left Side**: For RTL languages or player preference
- **Bottom Right**: For screens with UI elements on right side
- **Custom**: Player can drag to preferred position (saved in preferences)

### Visual Design

- **Size**: 300px wide, height adjusts to content (min 150px, max 400px)
- **Background**: Semi-transparent (85% opacity) matching game UI theme
- **Border**: 1px subtle border with theme accent color
- **Shadow**: Medium drop shadow (0 2px 8px rgba(0,0,0,0.3))
- **Animation**:
  - **Slide In**: 300ms ease-out from edge
  - **Slide Out**: 200ms ease-in to edge
  - **Minimize**: 150ms collapse to title bar only

### Components

#### Title Bar

- Icon indicating notification type (tutorial, advisor, achievement)
- Title text (max 30 characters)
- Minimize button (─) to collapse to title only
- Close button (X) to dismiss

#### Content Area

- Rich text with limited markdown formatting
- Optional small icon/image (64x64px max)
- Clear, concise message (max 150 words)
- Action buttons (1-2 maximum)

#### Progress Indicator

- Shown for multi-step tutorials
- Step counter text
- Visual progress bar
- Subtle animation when advancing

## Behavior Specification

### Display Triggers

```csharp
// Example trigger for exploration tutorial
TriggerSidePanel(
    panelId: "explore_with_scout",
    condition: () => player.ExperienceLevel <= Intermediate &&
                     player.TurnNumber == 1 &&
                     !player.HasMovedScout,
    delay: 10.seconds,
    priority: Medium
);
```

### Timing Rules

- **Delay on Trigger**: Configurable delay after condition met (default: 3 seconds)
- **Auto-Dismiss Timeout**: Default 30 seconds, configurable per panel
- **Stack Management**: Maximum 1 side panel visible at a time, queue others
- **Frequency Limits**:
  - Beginners: Max 1 every 2 minutes
  - Intermediate: Max 1 every 5 minutes
  - Expert: On-demand only

### Input Handling

- **Click Outside**: Panel remains visible (non-modal)
- **Escape Key**: Minimizes panel
- **Close Button (X)**: Dismisses and removes from queue
- **Minimize Button (─)**: Collapses to title bar
- **Action Buttons**: Perform specific actions, may dismiss or advance

### Auto-Dismiss Behavior

```csharp
public enum AutoDismissMode
{
    Never,              // Must be manually dismissed
    OnTimeout,          // Dismiss after configured seconds
    OnActionComplete,   // Dismiss when suggested action performed
    OnBoth             // Whichever comes first
}
```

## Multi-Step Tutorial Sequences

### Sequence Flow

Side panels support linear tutorial progressions:

1. **Trigger**: Initial condition met
2. **Display Step 1**: First panel appears with delay
3. **Action**: Player completes suggested action
4. **Advance**: Next step displays automatically
5. **Repeat**: Continue until sequence complete

### Example: Exploration Tutorial Sequence

```markdown
---
topic_id: sequence_exploration_basics
title: "Exploration Basics"
sequence_id: exploration_basics
experience_level: beginner
keywords:
  - exploration
  - side_panel
  - tutorial
---

# Exploration Basics

## Welcome to the Map

You can pan the camera using WASD keys or by dragging with the middle mouse button. Try moving the camera around!

## Your Scout

This is your scout unit. Click on it to select it, then right-click on the map to move. Scouts reveal unexplored areas.

- Action button: "Show Me How" → opens help topic `unit_movement`

## Explore the World

Move your scout around to discover points of interest like ruins, resources, and other factions.

**Tip:** Keep scouts moving to maximize exploration!

---
```

### State Management

```csharp
public class SidePanelSequenceState
{
    public string SequenceId { get; set; }
    public int CurrentStep { get; set; }
    public int TotalSteps { get; set; }
    public DateTime StepStartTime { get; set; }
    public bool IsMinimized { get; set; }
    public bool IsComplete { get; set; }
    public Dictionary<string, bool> ActionsCompleted { get; set; }
}
```

## Content Types

### Tutorial Panels

Primary educational content for onboarding.

**Characteristics:**

- Progress indicator visible
- Action-oriented instructions
- "Show Me How" links to help
- Part of tracked sequences

**Example:**

```markdown
---
topic_id: panel_first_building_construction
title: "Build Your First Building"
panel_id: first_building_construction
type: tutorial
keywords:
  - tutorial
  - side_panel
  - building
---

# Build Your First Building

Cities grow stronger with buildings. Click on your city and select the **Barracks** to train military units.

- Button: "Show Me How" → opens help topic `city_buildings`

Progress: 3 / 5

---
```

### Advisor Panels

Suggestions from council members or game AI.

**Characteristics:**

- No progress indicator
- Strategic recommendations
- Optional to follow
- Personalized to player situation

**Example:**

```markdown
---
topic_id: panel_military_advisor_warning
title: "Military Advisor"
panel_id: military_advisor_warning
type: advisor
keywords:
  - advisor
  - suggestion
---

# Military Advisor

My lord, our borders are vulnerable. I recommend recruiting more units or forming defensive alliances.

- Button: "Recruit Units" → opens `recruitment_screen`
- Button: "Dismiss" → closes panel

---
```

### Achievement Panels

Celebratory notifications for milestones.

**Characteristics:**

- Positive reinforcement
- No action required
- Auto-dismiss quickly (10 seconds)
- Optional share functionality

**Example:**

```markdown
---
topic_id: panel_achievement_first_city
title: "Achievement: City Founder"
panel_id: achievement_first_city
type: achievement
keywords:
  - achievement
  - notification
---

# Achievement: City Founder

🎉 **City Founder**

You've founded your first city! This is the beginning of a great civilization.

Auto-dismiss: 10s

---
```

## Technical Requirements

### Component API

```csharp
public interface ISidePanelSystem
{
    // Display a side panel
    void ShowPanel(SidePanelConfig config);
    
    // Show a multi-step sequence
    void StartSequence(string sequenceId);
    
    // Dismiss current panel
    void DismissPanel();
    
    // Minimize/restore panel
    void ToggleMinimize();
    
    // Advance to next step in sequence
    void AdvanceSequence();
    
    // Queue panel for later display
    void QueuePanel(string panelId, int priority);
    
    // Check if panel can be shown
    bool CanShowPanel(string panelId);
}

public class SidePanelConfig
{
    public string PanelId { get; set; }
    public PanelType Type { get; set; } // Tutorial, Advisor, Achievement
    public string Title { get; set; }
    public string IconPath { get; set; }
    public string Content { get; set; }
    public List<PanelButton> Buttons { get; set; }
    public AutoDismissMode AutoDismiss { get; set; }
    public int TimeoutSeconds { get; set; }
    public ProgressInfo Progress { get; set; }
}
```

### Analytics Events

```csharp
// Track side panel interactions
analytics.Track("SidePanelDisplayed", new {
    panel_id,
    panel_type,
    sequence_id,
    step_number,
    experience_level
});

analytics.Track("SidePanelButtonClicked", new {
    panel_id,
    button_label,
    time_displayed_seconds
});

analytics.Track("SidePanelDismissed", new {
    panel_id,
    dismiss_method, // timeout, manual_close, action_complete
    time_displayed_seconds,
    step_number
});

analytics.Track("SidePanelMinimized", new {
    panel_id,
    time_displayed_seconds
});

analytics.Track("SequenceCompleted", new {
    sequence_id,
    total_steps,
    total_time_seconds,
    steps_skipped
});
```

## Accessibility

### Screen Reader Support

- Panel announcement: "Tutorial notification" or "Advisor message"
- Content read in document order
- Progress announced: "Step 2 of 5"
- Minimize/restore state changes announced
- Buttons clearly labeled

### Keyboard Navigation

- **Tab**: Move between buttons and panel controls
- **Escape**: Minimize panel
- **Ctrl+W**: Close/dismiss panel
- **Arrow Keys**: Navigate between panels if multiple queued

### Visual Accessibility

- **High Contrast**: Panel background adjusts for high contrast themes
- **Focus Indicators**: 2px outline on focused buttons
- **Text Scaling**: Content scales up to 200%
- **Reduced Motion**: Slide animations disabled if motion sensitivity enabled

## Player Customization

### Position Preferences

```csharp
public enum PanelPosition
{
    RightCenter,    // Default
    RightTop,
    RightBottom,
    LeftCenter,
    LeftTop,
    LeftBottom,
    Custom          // Player-defined XY coordinates
}
```

### Display Preferences

- **Panel Opacity**: 70-100% adjustable
- **Auto-Dismiss Timeout**: 10-120 seconds adjustable
- **Frequency**: Configure minimum time between panels
- **Sound Effects**: Enable/disable panel appearance sound
- **Animations**: Enable/disable slide/minimize animations

## Success Metrics

### Engagement Metrics

- **Read Rate**: >= 80% of panels displayed for >= 3 seconds
- **Action Completion Rate**: >= 60% complete suggested action
- **Minimize Rate**: <= 20% (indicates good timing/relevance)
- **Manual Dismiss Rate**: <= 30% before timeout

### Timing Metrics

- **Average Display Time**: 15-30 seconds (optimal attention span)
- **Sequence Completion Time**: 5-10 minutes for typical 5-step sequence
- **Time to Action**: < 10 seconds from panel display to action start

### Quality Metrics

- **Helpfulness Score**: >= 4.0/5.0 in surveys
- **Intrusiveness Score**: <= 2.5/5.0 in surveys
- **Relevance Score**: >= 4.0/5.0 (content matches current situation)

## Testing Requirements

### Unit Tests

- Panel displays on correct trigger conditions
- Auto-dismiss timer functions correctly
- Sequence advancement logic works
- Queue management respects priority
- Minimize/restore state persists

### Integration Tests

- Panel appears at correct screen position
- Game continues while panel visible
- Button actions perform correctly
- Analytics events fire appropriately
- Multiple panels queue correctly (no overlap)

### UI Tests

- Panel renders at different resolutions
- Text wraps properly in content area
- Progress bar updates correctly
- Animations are smooth (60 FPS)
- Panel doesn't obscure critical UI

### Usability Tests

- Beginners can read and act on panel content
- Panels appear at appropriate times (not overwhelming)
- Suggested actions are clear and achievable
- Panel position doesn't interfere with gameplay

## Localization

### Text Assets

Per panel:

- Title (max 30 characters)
- Content (max 150 words)
- 1-2 button labels
- Progress text template

### Layout Considerations

- **Text Expansion**: Content area expands vertically for longer translations
- **RTL Languages**: Panel flips to left side, right-to-left text flow
- **Font Support**: Ensure readability at small sizes for complex scripts
- **Icon Localization**: Text-free icons preferred, or provide localized variants

## Future Enhancements

### Post-MVP Features

- **Pinned Panels**: Keep important panels visible permanently
- **Panel History**: Review recently dismissed panels
- **Smart Positioning**: AI adjusts position based on screen activity
- **Rich Media**: Embed small videos or GIFs in panels
- **Interactive Elements**: Sliders, dropdowns for settings within panel

### Advanced Analytics

- **Engagement Scoring**: ML model predicts panel effectiveness
- **Optimal Timing**: AI determines best moment to display each panel
- **Content Optimization**: A/B test different panel wording
- **Personalization**: Adapt panel content to player style
