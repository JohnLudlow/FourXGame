# UI Highlights and Enhanced Tooltips

## Overview

UI highlights and enhanced tooltips draw player attention to relevant interface elements and provide rich, contextual information. This system combines visual emphasis (glows, pulses, overlays) with informative tooltips that include "Learn More" links to the help system.

The UI can be used as a teaching tool in place of or supplementing tutorials.

An example:

- Player is playing the game
- A city is running dangerously low on food because the player hasn't understood that
  mechanic.
- A part of the UI relevant to cities (maybe the button that takes you to the list of
  cities) goes red, perhaps pulsating
- Hovering over the UI element shows a rich tooltip which should have an indication
  of the problem - a list of problem cities, broken down into their problems

Using the user interface in this way teaches the player about an aspect of the game
without needing to have them go through an explicit tutorial.

## Design Principles

- **Attention Without Interruption**: Guide focus without blocking gameplay
- **Context-Aware**: Highlight relevant elements at the right time
- **Progressive Disclosure**: Basic tooltip → Enhanced tooltip → Help system
- **Visually Distinct**: Clear differentiation from normal UI state

## Use Cases

### When to Use UI Highlights

✅ **Tutorial Guidance**

- Highlighting next action button during tutorials
- Drawing attention to specific UI elements
- Showing available actions for beginners

✅ **Feature Discovery**

- New features unlocked
- Previously unavailable options now active
- Hidden UI elements made visible

✅ **Error Prevention**

- Warning indicators on problematic choices
- Highlighting required fields
- Showing consequences of actions

❌ **When NOT to Use UI Highlights**

- Constant highlights become ignored (use sparingly)
- Non-actionable elements
- Information already in standard tooltips

## UI Highlight System

### Visual Styles

#### Pulsing Glow

Animated glow effect for actionable elements.

**Visual Spec:**

- **Glow Color**: Theme accent color (default: golden yellow)
- **Glow Size**: 4-8px blur radius
- **Animation**: Pulse from 40% to 100% opacity over 1.5 seconds, repeat
- **Border**: Optional 2px solid border in same color

**Use For:**

- Primary action buttons
- Important UI elements needing immediate attention
- Tutorial step targets

TODO: Convert to a more relevant format, such Gum styles

```css
.ui-highlight-glow {
    box-shadow: 0 0 8px 4px rgba(255, 215, 0, 0.6);
    border: 2px solid rgba(255, 215, 0, 0.8);
    animation: pulse-glow 1.5s ease-in-out infinite;
}

@keyframes pulse-glow {
    0%, 100% { box-shadow: 0 0 8px 4px rgba(255, 215, 0, 0.4); }
    50% { box-shadow: 0 0 12px 6px rgba(255, 215, 0, 1.0); }
}
```

#### Spotlight Effect

Dimmed overlay with highlighted element in focus.

**Visual Spec:**

- **Overlay**: 70% opacity black over entire game view
- **Spotlight**: Highlighted element at full opacity with padding
- **Border**: 3px solid accent color around spotlight area
- **Animation**: Fade in overlay over 300ms

**Use For:**

- Critical first-time actions (with modal tutorials)
- Multi-element highlights requiring focus
- Complex UI teaching moments

```plain
┌─────────────────────────────────────┐
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓▓▓▓▓▓┌──────────────┐▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓▓▓▓▓▓│ Highlighted  │▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓▓▓▓▓▓│   Element    │▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓▓▓▓▓▓└──────────────┘▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
└─────────────────────────────────────┘
▓ = Dimmed overlay
Clear area = Highlighted element
```

#### Arrow Indicator

Animated arrow pointing to target element.

**Visual Spec:**

- **Arrow Size**: 32x32px
- **Color**: Theme accent color with slight transparency
- **Animation**: Bounce toward target, 1 second loop
- **Placement**: Auto-positioned to avoid obscuring element

**Use For:**

- Subtle hints for beginners
- Off-screen element indicators
- Secondary emphasis (combined with other highlights)

#### Badge Indicator

Small badge icon overlaid on element.

**Visual Spec:**

- **Badge Size**: 16x16px icon
- **Position**: Top-right corner of element
- **Types**: "New" star, "!" attention, "?" help, numbered steps
- **Animation**: Optional pulse or bounce-in

**Use For:**

- New feature notifications
- Multiple highlighted elements in sequence
- Unobtrusive persistent indicators

### Highlight Priorities

When multiple highlights compete:

1. **Critical**: Spotlight effect (highest priority)
2. **High**: Pulsing glow + arrow
3. **Medium**: Pulsing glow only
4. **Low**: Badge indicator

**Rule**: Maximum 3 simultaneous highlights, prioritize most relevant.

## Enhanced Tooltip System

### Standard Tooltip vs Enhanced Tooltip

#### Standard Tooltip

Basic information on hover.

**Content:**

- Element name/title
- Brief description (1-2 sentences)
- Keyboard shortcut if applicable

**Display:**

- Appears after 500ms hover
- Small size (max 200px wide)
- Plain text only

#### Enhanced Tooltip

Rich information with additional context.

**Content:**

- Element name/title
- Detailed description (up to 1 paragraph)
- Current status/value
- Effects/consequences
- "Learn More" link to help system
- Related actions/shortcuts

**Display:**

- Appears after 800ms hover (slightly longer)
- Larger size (max 350px wide)
- Formatted text with icons
- Optional small image/diagram

### Enhanced Tooltip Layout

```text
┌─────────────────────────────────────┐
│ [Icon] Element Name                 │
├─────────────────────────────────────┤
│ Detailed description of what this   │
│ element does and why it's relevant. │
│                                     │
│ Current Status: ✓ Active            │
│                                     │
│ Effect: +2 Food per turn            │
│                                     │
│ 💡 Tip: Combine with Market for    │
│    bonus trade routes.              │
│                                     │
│ ❓ Learn More: Trade Basics          │
├─────────────────────────────────────┤
│ [Shift+Click for more options]      │
└─────────────────────────────────────┘
```

### Context-Sensitive Content

Enhanced tooltips adapt based on:

- **Player Experience**: Beginners see more explanation, experts see concise info
- **Game State**: Show relevant info (e.g., "Cannot afford" vs "Build now")
- **Tutorial Progress**: Include tutorial hints if relevant
- **Recent Actions**: Reference what player just did

#### Example: Building Tooltip

```yaml
# Beginner
title: "Barracks"
description: |
  Buildings that train military units. Barracks allow
  you to recruit basic infantry and archers.
status: "Can build (costs 100 production)"
effect: "Unlocks: Swordsman, Archer"
tip: "Build this early to defend your city!"
learn_more: "military_buildings"

# Expert  
title: "Barracks"
status: "100 production, 2 turns"
effect: "Unlocks 3 units, +1 XP to trained units"
learn_more: "military_buildings"
```

```markdown
---
topic_id: tooltip_barracks
title: "Barracks Tooltip"
keywords:
  - tooltip
  - barracks
  - building
---

## Beginner

**Barracks**

Buildings that train military units. Barracks allow you to recruit basic infantry and archers.

Status: Can build (costs 100 production)

Effect: Unlocks: Swordsman, Archer

Tip: Build this early to defend your city!

## Expert

Barracks — 100 production, 2 turns

Effect: Unlocks 3 units, +1 XP to trained units

---
```

## Technical Requirements

### Highlight API

```csharp
public interface IUIHighlightSystem
{
    // Apply highlight to UI element
    void HighlightElement(string elementId, HighlightStyle style, HighlightOptions options);
    
    // Remove highlight
    void ClearHighlight(string elementId);
    
    // Clear all highlights
    void ClearAllHighlights();
    
    // Check if element is currently highlighted
    bool IsHighlighted(string elementId);
    
    // Highlight sequence (multiple elements in order)
    void HighlightSequence(List<HighlightStep> steps);
}

public enum HighlightStyle
{
    PulsingGlow,
    Spotlight,
    ArrowIndicator,
    BadgeIndicator,
    Combined  // Multiple styles together
}

public class HighlightOptions
{
    public int Priority { get; set; }
    public int DurationSeconds { get; set; }  // 0 = indefinite
    public bool RemoveOnClick { get; set; }
    public string TooltipOverride { get; set; }  // Optional enhanced tooltip
    public Action OnClick { get; set; }  // Callback when highlighted element clicked
}
```

### Enhanced Tooltip API

```csharp
public interface IEnhancedTooltipSystem
{
    // Register enhanced tooltip for element
    void RegisterTooltip(string elementId, EnhancedTooltipConfig config);
    
    // Update tooltip content dynamically
    void UpdateTooltip(string elementId, EnhancedTooltipConfig config);
    
    // Show tooltip immediately (programmatic)
    void ShowTooltip(string elementId, Vector2 position);
    
    // Hide current tooltip
    void HideTooltip();
}

public class EnhancedTooltipConfig
{
    public string Title { get; set; }
    public string IconPath { get; set; }
    public string Description { get; set; }
    public TooltipStatus Status { get; set; }
    public List<TooltipEffect> Effects { get; set; }
    public TooltipTip Tip { get; set; }  // Optional contextual tip
    public string LearnMoreTopic { get; set; }
    public List<TooltipShortcut> Shortcuts { get; set; }
    public ExperienceLevel DetailLevel { get; set; }
}
```

### Analytics Events

```csharp
// Track highlight effectiveness
analytics.Track("UIElementHighlighted", new {
    element_id,
    highlight_style,
    tutorial_context,
    experience_level
});

analytics.Track("HighlightedElementClicked", new {
    element_id,
    time_to_click_seconds,
    tutorial_step
});

analytics.Track("HighlightDismissed", new {
    element_id,
    dismiss_method,  // timeout, manual_clear, other_action
    duration_seconds
});

// Track tooltip interactions
analytics.Track("EnhancedTooltipShown", new {
    element_id,
    tooltip_type,
    experience_level
});

analytics.Track("LearnMoreClicked", new {
    element_id,
    help_topic,
    context
});

analytics.Track("TooltipHoverDuration", new {
    element_id,
    duration_seconds
});
```

## Behavior Specification

### Highlight Lifecycle

1. **Trigger**: Tutorial step or game event activates highlight
2. **Priority Check**: Determine if highlight should display (max 3 concurrent)
3. **Animation In**: Fade/pulse effect starts (300ms)
4. **Active**: Highlight remains until dismissed or timeout
5. **Completion**: Player clicks element or timeout expires
6. **Animation Out**: Fade out (200ms)
7. **Cleanup**: Remove highlight, trigger next tutorial step if applicable

### Tooltip Behavior

**Standard Tooltip:**

- Hover delay: 500ms
- Dismiss on mouse leave or element click
- Position: Above/below element (auto-adjust to fit viewport)

**Enhanced Tooltip:**

- Hover delay: 800ms (or immediate if element highlighted)
- Persist for 200ms after mouse leaves (allows moving to tooltip)
- "Learn More" clickable while tooltip visible
- Position: Prefers right side, falls back to available space

### Interaction Rules

- **Highlighted element clicked**:
  - Trigger element's normal action
  - Clear highlight
  - Advance tutorial if part of sequence
  - Track analytics event

- **"Learn More" clicked**:
  - Open contextual help to specific topic
  - Keep tooltip visible
  - Track analytics event

- **Escape key pressed**:
  - Clear all highlights
  - Hide tooltips
  - Don't advance tutorial (player manually dismissing)

## Tutorial Integration

### Multi-Step Highlighting

Tutorial sequences often require highlighting multiple elements in order:

```yaml
tutorial_sequence: city_management_basics
steps:
  - step: 1
    highlight:
      element: city_panel_button
      style: pulsing_glow
      arrow: true
    tooltip_override: "Click here to open your city panel"
    wait_for: click
    
  - step: 2
    highlight:
      element: construction_queue
      style: spotlight
    tooltip_override: "This shows what your city is currently building"
    wait_for: timeout
    duration: 5
    
  - step: 3
    highlight:
      element: add_building_button
      style: pulsing_glow
    tooltip_override: "Click to choose a new building to construct"
    wait_for: click
```

```markdown
---
topic_id: seq_city_management_basics
title: "City Management Basics Sequence"
tutorial_sequence: city_management_basics
keywords:
  - tutorial
  - highlight
  - ui
---

# City Management Basics (sequence)

1. Highlight city panel button (pulsing glow + arrow) — wait for click
2. Highlight construction queue (spotlight) — wait for timeout (5s)
3. Highlight add building button (pulsing glow) — wait for click

---
```

### Conditional Highlighting

Highlights can appear based on game state:

```csharp
// Example: Highlight warning if player is about to make poor choice
if (player.Gold < 100 && player.IsAboutToBuild(ExpensiveBuilding))
{
    highlightSystem.HighlightElement("gold_warning", HighlightStyle.BadgeIndicator, new {
        Badge = "!",
        Color = Red,
        TooltipOverride = "Warning: Low gold! This building is expensive."
    });
}
```

## Accessibility

### Screen Reader Support

- Highlighted elements announced: "Tutorial highlight: Build Barracks button"
- Enhanced tooltip content fully readable
- "Learn More" links announced as links
- Highlight state changes announced

### Visual Accessibility

- **Colorblind Modes**: Highlights use patterns + color (diagonal stripes for colorblind)
- **High Contrast**: Glow effects increase contrast in high contrast mode
- **Reduced Motion**: Disable pulse/bounce animations, use static highlight
- **Focus Indicators**: Keyboard focus uses same highlight system

### Keyboard Navigation

- **Tab**: Navigate to highlighted elements in order
- **Enter**: Activate highlighted element
- **F1**: Open "Learn More" help topic for focused element
- **Escape**: Clear all highlights

## Success Metrics

### Effectiveness Metrics

- **Click-Through Rate**: >= 85% of highlighted elements clicked within 30 seconds
- **Learn More Usage**: >= 25% of enhanced tooltips result in "Learn More" click
- **Tooltip Read Time**: Average 3-5 seconds (indicates engagement)
- **Highlight Dismiss Rate**: <= 10% manually dismissed (indicates relevance)

### Quality Metrics

- **Clarity Score**: >= 4.3/5.0 for "Highlights helped me find actions"
- **Annoyance Score**: <= 2.0/5.0 for "Highlights were distracting"
- **Tooltip Usefulness**: >= 4.0/5.0 for enhanced tooltip ratings

## Testing Requirements

### Unit Tests

- Highlight applies correct visual style
- Multiple highlights respect priority system
- Timeout dismissal works correctly
- Click detection triggers advancement
- Enhanced tooltips format content correctly

### Integration Tests

- Highlights appear at correct screen positions
- Spotlight overlay dims non-highlighted areas
- Arrows point to correct elements
- Tooltips position to avoid viewport edges
- "Learn More" opens correct help topics

### Visual Tests

- Highlights visible against all UI backgrounds
- Colorblind simulation shows patterns correctly
- Animations smooth at 60 FPS
- Tooltips readable at different text sizes
- No z-index conflicts with other UI

### Usability Tests

- Beginners notice and click highlighted elements
- Enhanced tooltips answer common questions
- "Learn More" helps when more info needed
- Highlights don't obscure critical information

## Localization

### Text Assets

Per enhanced tooltip:

- Title (max 40 characters)
- Description (max 100 words)
- Status text (varies)
- Effects list (1-5 items)
- Tip text (max 50 words)
- Shortcuts (key combinations)

### Visual Assets

- Badge icons (language-neutral)
- Arrow indicators (language-neutral)
- Tooltip background (theme-based)
- Icon images (may need localized variants if text present)

### Layout Considerations

- **Text Expansion**: Tooltip width expands up to 400px for longer translations
- **RTL Languages**: Mirror arrow directions, flip badge positions
- **Font Scaling**: Ensure readability at default size

## Future Enhancements

### Post-MVP Features

- **Animated Tutorials**: Mini-animations within enhanced tooltips showing actions
- **Gesture Hints**: Show touch/mouse gestures in tooltips (mobile support)
- **Smart Highlights**: ML predicts which elements player needs highlighted
- **Highlight History**: Review previously highlighted elements
- **Custom Highlight Styles**: Player-selectable highlight colors/styles

### Advanced Analytics

- **Heatmap Integration**: Correlate highlights with where players look
- **Effectiveness Scoring**: Identify which highlights work best
- **Optimal Timing**: AI determines best moment to show highlights
- **A/B Testing**: Test different highlight styles and tooltip content
