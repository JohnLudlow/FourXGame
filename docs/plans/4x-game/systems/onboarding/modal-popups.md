# Modal Popup Tutorial System

## Overview

Modal popups are blocking message boxes used sparingly for critical first-time actions and important game moments. They pause gameplay to ensure players don't miss crucial information.

## Design Principles

- **Use Sparingly**: Maximum 3 modal popups in first 30 minutes for beginners
- **Respect Experience Level**: Minimize for intermediate, disable for expert (except major new features)
- **Always Skippable**: Include "Don't show this again" option
- **Rich Content**: Support images, icons, formatted text, and progress indicators

## Use Cases

### When to Use Modal Popups

✅ **Critical First-Time Actions** (Beginners only)

- First city founding
- First combat encounter
- Major strategic decisions with irreversible consequences
- Game-changing discoveries

✅ **Major Feature Introductions**

- New expansion content for returning players
- Significant UI changes in updates
- Important patch notes or changes

❌ **When NOT to Use Modal Popups**

- Basic controls (use side panels instead)
- Intermediate/expert players for standard mechanics
- Information that can wait
- Anything that interrupts flow excessively

## UI Specification

### Layout Structure

```plain
┌────────────────────────────────────────┐
│  [Icon] Tutorial Title            [X]  │
├────────────────────────────────────────┤
│                                        │
│  Tutorial content text with rich       │
│  formatting, optional image, and       │
│  clear instructions.                   │
│                                        │
│  [Optional Image/Diagram]              │
│                                        │
│  Step 1 of 3                           │
│  ▓▓▓▓░░░░░░                            │
│                                        │
├────────────────────────────────────────┤
│  [ ] Don't show this again             │
│                                        │
│  [Learn More]  [Skip]  [Next/Got It]   │
└────────────────────────────────────────┘
```

### Visual Design

- **Size**: 400-600px wide, height adjusts to content (max 80% viewport height)
- **Backdrop**: 80% opacity black overlay, blurs game view if supported
- **Border**: 2px accent color matching game theme
- **Shadow**: Strong drop shadow for depth (0 4px 20px rgba(0,0,0,0.5))
- **Animation**: Fade in 200ms, scale from 0.95 to 1.0

### Components

#### Title Bar

- Icon (32x32px) related to tutorial topic
- Clear, concise title (max 50 characters)
- Close button (X) in top-right

#### Content Area

- Rich text with markdown-style formatting support:
  - **Bold**, *italic*, bullet lists
  - Colored text for emphasis
  - Code-style highlighting for key terms
- Optional image (max 400x300px, centered)
- Optional animated GIF demonstrations
- Clear call-to-action instructions

#### Progress Indicator

- Shown for multi-step tutorials only
- "Step X of Y" text
- Progress bar visualization
- Current step highlighted

#### Footer

- "Don't show this again" checkbox (saves to player preferences)
- Action buttons:
  - **Learn More**: Opens contextual help (optional)
  - **Skip**: Dismisses popup and marks tutorial as skipped
  - **Next/Got It**: Advances to next step or closes

## Behavior Specification

### Display Triggers

```csharp
// Example trigger conditions
TriggerModalPopup(
    tutorialId: "first_city_founding",
    condition: () => player.ExperienceLevel == Beginner && 
                     player.CitiesFounded == 0 &&
                     player.SelectedUnit?.CanFoundCity == true,
    priority: High
);
```

### Timing Rules

- **Delay on Trigger**: Wait 500ms after trigger condition to avoid jarring appearance
- **Minimum Time Between Popups**: 2 minutes minimum between modal popups
- **Queue Management**: If multiple popups triggered, queue and show sequentially
- **Session Limits**: Max 5 modals per session for beginners, 2 for intermediate, 0 for expert

### Input Handling

- **Escape Key**: Closes popup (equivalent to clicking X)
- **Enter Key**: Advances to next step or closes (equivalent to Next/Got It)
- **Tab Navigation**: Cycles through buttons and checkbox
- **Click Outside**: Disabled (must use buttons to dismiss)

### Pause Behavior

- **Game State**: Pause game simulation when modal is displayed
- **Time Continues**: Game time continues (not frozen)
- **Multiplayer**: In multiplayer, only pause for local player (others continue)

## Multi-Step Tutorials

### Step Progression

Modal popups support multi-step tutorial sequences:

1. **Step 1**: Initial concept introduction
2. **Step 2**: Detailed explanation or demonstration
3. **Step 3**: Call to action and practice

Example: **First City Founding Tutorial**

```text
Step 1/3: "Welcome to City Building!"
- Introduce concept of cities
- Show image of prosperous city
- Progress: ▓▓▓░░░░░░

Step 2/3: "Choosing a Location"
- Explain city placement considerations
- Highlight terrain bonuses
- Progress: ▓▓▓▓▓▓░░░

Step 3/3: "Found Your City"
- Instruct player to click "Found City" button
- UI highlight activates after clicking Next
- Progress: ▓▓▓▓▓▓▓▓▓
```

Note that founding the first city might not need a tutorial. A more representative
example might be:

Example: **Welcome to 4xGame**

```text
Step 1/3: "Basic controls"
- Show controls for moving the map
- Show animated image of moving the map
- Progress: ▓▓▓░░░░░░

Step 2/3: "Moving units"
- Show controls for moving units
- Show animated image of moving units
- Progress: ▓▓▓▓▓▓░░░

Step 3/3: "Getting help"
- Highlight the help button
- Show animated image of clicking the help button and opening the encyclopedia
- Progress: ▓▓▓▓▓▓▓▓▓
```

### State Management

```csharp
public class ModalTutorialState
{
    public string TutorialId { get; set; }
    public int CurrentStep { get; set; }
    public int TotalSteps { get; set; }
    public bool DontShowAgain { get; set; }
    public DateTime DisplayTime { get; set; }
    public bool IsComplete { get; set; }
}
```

## Content Examples

### Example 1: First City Founding (Beginner)

```markdown
---
topic_id: tutorial_first_city_founding
title: "First City Founding"
tutorial_id: first_city_founding
experience_level: beginner
keywords:
  - tutorial
  - modal
  - beginner
trigger:
  event: unit_selected
  condition: can_found_city AND cities_founded == 0
---

# Found Your First City!

Cities are the heart of your civilization. They produce resources, train units, and grow your population.

## Step: Choosing the Right Spot

- Look for locations with:
  - 🌾 Food resources (for growth)
  - ⚒️ Production resources (for buildings)
  - 💰 Luxury resources (for happiness)

**Rivers provide +1 food to adjacent tiles**

## Step: Found Your City

Click the **Found City** button to establish your first settlement! After founding, we'll show you how to manage your new city.

---
```

### Example 2: Major Feature Update (All Experience Levels)

```markdown
---
topic_id: tutorial_new_expansion_welcome
title: "Welcome to the New Expansion"
tutorial_id: new_expansion_welcome
experience_level: all
keywords:
  - expansion
  - announcement
  - modal
trigger:
  event: game_start
  condition: expansion_installed AND not_seen_welcome
---

# Welcome to the New Expansion!

**The Ancient Empires expansion is now active!**

New features include:

- 3 new civilizations
- Religion system
- Archaeological discoveries
- 15+ new units and buildings

Check the in-game help for details on each feature.

---
```

## Technical Requirements

### Component API

```csharp
public interface IModalPopupSystem
{
    // Display a modal tutorial popup
    void ShowModal(ModalTutorialConfig config);
    
    // Check if modal can be shown (respects timing rules, experience level)
    bool CanShowModal(string tutorialId);
    
    // Dismiss current modal
    void DismissModal();
    
    // Queue modal for later display
    void QueueModal(string tutorialId, int priority);
    
    // Check if player has dismissed tutorial permanently
    bool IsPermanentlyDismissed(string tutorialId);
}

public class ModalTutorialConfig
{
    public string TutorialId { get; set; }
    public string Title { get; set; }
    public string IconPath { get; set; }
    public List<ModalTutorialStep> Steps { get; set; }
    public ExperienceLevel MinimumLevel { get; set; }
    public bool AllowDontShowAgain { get; set; }
}
```

### Analytics Events

```csharp
// Track modal popup interactions
analytics.Track("ModalTutorialDisplayed", new {
    tutorial_id,
    experience_level,
    step_number,
    session_popup_count
});

analytics.Track("ModalTutorialButtonClicked", new {
    tutorial_id,
    step_number,
    button_type, // next, skip, learn_more, got_it
    time_displayed_seconds
});

analytics.Track("ModalTutorialCompleted", new {
    tutorial_id,
    total_steps,
    total_time_seconds
});

analytics.Track("ModalTutorialDismissed", new {
    tutorial_id,
    step_number,
    dont_show_again_selected
});
```

## Accessibility

### Screen Reader Support

- Title announced as heading level 1
- Content read in document order
- Progress announced as "Step X of Y"
- Buttons clearly labeled with ARIA labels
- "Don't show again" checkbox state announced

### Keyboard Navigation

- **Tab**: Move to next interactive element
- **Shift+Tab**: Move to previous interactive element
- **Enter**: Activate focused button
- **Space**: Toggle checkbox when focused
- **Escape**: Close modal

### Visual Accessibility

- **Color Contrast**: 4.5:1 minimum for text (WCAG AA)
- **Focus Indicators**: 2px solid outline on focused elements
- **Scalability**: Text scales up to 200% without breaking layout
- **Motion**: Animations can be disabled via accessibility settings

## Success Metrics

### Engagement Metrics

- **Completion Rate**: >= 75% of displayed modals reach final step
- **Skip Rate**: <= 20% of modals skipped before completion
- **Don't Show Again Rate**: <= 10% (indicates annoyance)
- **Learn More Click Rate**: >= 30% (indicates relevant help linking)

### Timing Metrics

- **Average Time Per Step**: 10-20 seconds (sweet spot)
- **Total Tutorial Duration**: < 2 minutes for multi-step modals
- **Time Between Modals**: Average >= 5 minutes (confirms not overwhelming)

### Quality Metrics

- **Clarity Score**: >= 4.5/5.0 in surveys
- **Usefulness Score**: >= 4.0/5.0 in surveys
- **Annoyance Score**: <= 2.0/5.0 in surveys

## Testing Requirements

### Unit Tests

- Modal display triggers correctly based on conditions
- Experience level filtering works (beginner sees all, expert sees none)
- "Don't show again" persists across sessions
- Multi-step progression advances correctly
- Queue system respects priority and timing rules

### Integration Tests

- Modal appears on correct trigger events
- Game pauses when modal displayed
- Analytics events fire correctly
- Buttons perform expected actions
- Learn More opens correct help topics

### UI Tests

- Modal renders correctly at different resolutions
- Backdrop overlay covers entire game view
- Images load and display properly
- Progress bar updates with step changes
- Animations are smooth (60 FPS)

### Accessibility Tests

- Screen reader announces all content correctly
- Keyboard navigation reaches all interactive elements
- Focus indicators visible and clear
- Color contrast meets WCAG AA standards
- Works with 200% text zoom

## Localization

### Text Assets

Per tutorial:

- Title (max 50 characters)
- 1-3 content paragraphs per step
- 3-5 button labels
- 1 checkbox label

### Image Assets

- Images with embedded text require localized versions
- Diagrams with annotations require translation
- GIFs with text overlays need per-language variants
- Icon-only images are language-neutral

### Layout Considerations

- **Text Expansion**: German can be 40% longer than English
- **RTL Languages**: Right-to-left mirror for Arabic, Hebrew
- **Vertical Scripts**: May need different layout for Asian languages
- **Font Support**: Ensure font includes all required character sets

## Future Enhancements

### Post-MVP Features

- **Video Tutorials**: Embed short video clips in modals
- **Interactive Demos**: Simulated mini-environment within modal for practice
- **Branching Paths**: Different next steps based on player choices
- **Voice-Over**: Optional narration for tutorial content
- **Theming**: Match modal style to player's selected UI theme

### Advanced Analytics

- **Heatmap Tracking**: Where players look within modal (eye tracking simulation)
- **A/B Testing**: Test different content, layouts, button labels
- **Predictive Dismissal**: Detect players likely to skip and adjust content
- **Personalization**: Customize content based on player behavior patterns
