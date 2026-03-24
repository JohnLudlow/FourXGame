# Onboarding and Tutorial System

This section describes the tutorial and onboarding process, which covers 3 main areas:

- User experience meeting expectations
- Tutorials and help content
- Teaching through the mechanics

Story time...

Many years ago, I bought and played (or at least tried to play) a game called Master of Orion 3. Now, I know this game was universally loved by everyone
(no, it wasn't) but the tutorial was an abomination. It went something like this:

- Start the game. There's a checkbox so you can choose to include a tutorial or not. Probably leave that checked since I'm new to the game
- Usual race selection and game settings screens
- Main game scre--
- Oh, a modal message box telling me it's the main game screen. Sure, whatever. Hit ok--
- Oh, a modal message box telling me about something on the main game screen. Sure, whatever. Hit ok--
- ...Many, many message boxes later...
- Finally, I get to play the game... Hmmm, let's take a look at the tech tree
- - Oh, a modal message box telling me it's the tech tree screen!
- ***Rage quit***

Compare that to a contemporary game, Rise of Nations.

- Start the game. There's a checkbox so you can choose to include a tutorial or not. Probably leave that checked since I'm new to the game
- Usual race selection and game settings screens
- Main game screen. This game is similar to Age of Empires
- I'm left to look over the UI, which is similar to AoE
- After a minute a message appears to the side, noticeable but not interrupting. It says "build a market"

The difference of experience onboarding in the second example was far superior, but many games have made the mistake of using tedious tutorials
that the player has to wade through in order to play the game, rather than a system that reacts to the player's needs.

This can lead to a negative first impression, either that the player gets bored and burnt out before getting to the fun part of the game, or
(particularly if the tutorial system makes the player try to play the game without tutorials so they don't get bored and burnt out before getting
to the fun part of the game) that the game makes no sense.

## Table of contents

- [Onboarding and Tutorial System](#onboarding-and-tutorial-system)
  - [Table of contents](#table-of-contents)
  - [Overview](#overview)
  - [Definition of terms](#definition-of-terms)
  - [Feature status](#feature-status)
  - [Implementation guide](#implementation-guide)
    - [Requirements](#requirements)
      - [Player Experience Levels](#player-experience-levels)
      - [Tutorial Delivery Mechanisms](#tutorial-delivery-mechanisms)
      - [Tutorial Resumption and Control](#tutorial-resumption-and-control)
    - [Tutorial Delivery Mechanisms](#tutorial-delivery-mechanisms-1)
    - [Core Tutorial Content Areas](#core-tutorial-content-areas)
    - [Implementation Steps](#implementation-steps)
  - [Phases](#phases)
    - [Phase 1 — Discovery](#phase-1--discovery)
    - [Phase 2 — Implementation](#phase-2--implementation)
    - [Phase 3 — Polish](#phase-3--polish)
  - [Success Metrics](#success-metrics)
    - [Primary Metrics](#primary-metrics)
    - [Secondary Metrics](#secondary-metrics)
    - [Quality Metrics](#quality-metrics)
  - [Acceptance criteria](#acceptance-criteria)
    - [Functional Requirements](#functional-requirements)
    - [Non-Functional Requirements](#non-functional-requirements)
  - [Testing](#testing)
    - [Unit Tests](#unit-tests)
    - [Integration Tests](#integration-tests)
    - [Playtesting](#playtesting)
    - [Accessibility Testing](#accessibility-testing)
    - [Performance Testing](#performance-testing)
  - [Future Enhancements](#future-enhancements)
    - [Additional Tutorial Systems (Post-MVP)](#additional-tutorial-systems-post-mvp)
    - [Advanced Features (v2.0+)](#advanced-features-v20)
    - [Analytics-Driven Improvements](#analytics-driven-improvements)
  - [Dependencies](#dependencies)
    - [Technical Dependencies](#technical-dependencies)
    - [Content Dependencies](#content-dependencies)
    - [External Dependencies](#external-dependencies)

## Overview

A progressive onboarding and tutorial system introducing core gameplay concepts: exploration, city management, armies and battles, characters and factions, and resources. Tutorials are contextual and triggered by player actions or milestones.

This section describes the tutorial and onboarding process, which covers 3 main areas:

- User experience meeting expectations
  - "I've played this kind of game before, do I know how to do basic things like moving the camera?"
    - Good: "Moving the camera holding the middle mouse button and dragging, or using WSAD keys"
    - Bad: "Moving the camera involves sticking my finger up my nose and twisting while singing We Wish You a Merry Christmas"
  - Are the standard things (camera controls, selecting items, moving units) standard?
  - Does the UI do what the user expects?
  - Does the UI present the most important information and actions to the user in any given moment?
  - Does the user understand the information presented and how to perform various actions?

- Tutorials and help content
  - "As a ***new*** player who's ***never played strategy games*** before, how quickly can I get started?"
    - Good: "When starting the game, you get some richly formatted pop-ups telling you about ***basic or important*** mechanics and making sure you know about the help. Other popups ***will*** appear as you play"
    - Bad: "When starting the game, you 400 billion popups with tedious walls of text you'll never remember"
    - Bad: "When starting the game, no popups appear at all - good luck!"

  - "As a ***new*** player who's ***played many strategy games*** before, how quickly can I get started?"
    - Good: "When starting the game, you get some richly formatted pop-ups telling you about ***important*** mechanics (skipping basic stuff) and making sure you know about the help. Other popups ***may rarely*** appear as you play"
    - Bad: "When starting the game, you have to wade through popups about basic stuff that's the same as every single other game that exists"
    - Bad: "When starting the game, no popups appear at all - good luck!"

  - "As a ***returning*** player ***who's played this strategy game*** before, how quickly can I get started?"
    - Good: "When starting the game, just play. You know where things are. The help's there if you need it"
    - Bad: "When starting the game, you have to wade through popups about basic stuff that's the same as every single other game that exists"
    - Bad: "When starting the game, you have to wade through popups about game mechanics you're already aware of"

  - "I'm not sure how trade works, how can I find out?"
    - Good: "Most screens, popups and relevant messages have a button which you can click to get information about relevant help topics"
    - Bad: "Leave the game, open your browser, goto 4xgame.fandom.com..."

  - Introductory scenarios
  - Popups and messages at the start of the game
  - In-game help content with context-sensitive links
  - "Show me how" links from a pop-up or suggestion to a help page
  - Controls hints

- Teaching through the mechanics
  - Councilors who offer suggestions and advice

- Teaching through the user experience
  - UI hints about important factors, such as a warning message and indicator if food is too low and people are starving

## Definition of terms

- Tutorial: a guided, interactive lesson introducing a feature.
- Onboarding: a sequence of introductory tutorials, tips and UI highlights for new players.

| Term       | Meaning                                                                                                                                                          | Reference |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| Tutorial   | A guided lesson introducing a topic                                                                                                                              |           |
| Onboarding | The process of introducing a complex topic to someone unfamiliar with it. In game terms, this can involve introductory scenarios, tutorials or careful UX design |           |

## Feature status

Not started

## Implementation guide

### Requirements

#### Player Experience Levels

Players must be able to select their experience level, which determines tutorial behavior:

- **Beginner** (Never played 4X games)
  - GIVEN a player starting a new game as a beginner
  - WHEN the player proceeds through the new game menu
  - THEN the player sees comprehensive tutorials including basic controls, UI navigation, and core mechanics
  - AND modal popups are used for critical first-time actions (e.g., first unit movement, first city founded)
  - AND contextual help appears frequently via non-blocking side panels

- **Intermediate** (Played other 4X games)
  - GIVEN a player starting a new game with 4X experience
  - WHEN the player proceeds through the new game menu
  - THEN the player skips basic tutorials (camera controls, unit selection, basic UI)
  - AND receives tutorials only for unique/complex mechanics specific to this game
  - AND modal popups are minimized (used only for critical unique features)
  - AND contextual help appears for game-specific features

- **Expert** (Returning player or experienced)
  - GIVEN an expert player starting a new game
  - WHEN the player proceeds through the new game menu
  - THEN minimal guidance is provided (only changelog highlights if returning player)
  - AND modal popups are disabled except for major new features
  - AND contextual help is available on-demand only

#### Tutorial Delivery Mechanisms

- **Modal Popups** (Blocking)
  - Used sparingly for critical first-time actions
  - Reserved for beginners only, except for major new features
  - Must include "Don't show this again" option
  - Maximum 5 modal popups in first 30 minutes of gameplay for beginners

- **Side Panel Notifications** (Non-blocking, Rise of Nations style)
  - Primary delivery mechanism for contextual guidance
  - Appears at relevant moments without interrupting gameplay
  - Can be minimized/expanded by player
  - Includes progress indicators for multi-step tutorials
  - Auto-dismisses after configurable timeout (default: 30 seconds)

- **UI Highlights and Tooltips**
  - Integrated into existing UI elements
  - Pulsing/glowing indicators for relevant buttons/areas
  - Enhanced tooltips with "Learn More" links to help system
  - Persistent until player completes suggested action

- **Contextual Help System**
  - In-game help accessible via "?" button on all major screens
  - Context-sensitive topics based on current screen/action
  - Search functionality for help topics
  - "Show Me How" interactive demonstrations linked from help pages

#### Tutorial Resumption and Control

- Tutorials must be resumable after game exit
  - GIVEN a player exits mid-tutorial
  - WHEN the player returns to the game
  - THEN tutorial progress is preserved and can continue from last step
  
- Tutorials must be skippable
  - GIVEN a player is viewing a tutorial
  - WHEN the player clicks "Skip" or "Skip All Tutorials"
  - THEN the current tutorial ends immediately
  - AND player preferences are updated to reflect skip choice

- Tutorial settings must be adjustable
  - GIVEN a player in-game
  - WHEN the player accesses settings
  - THEN tutorial level can be changed (beginner/intermediate/expert)
  - AND individual tutorial categories can be enabled/disabled
  - AND completed tutorials can be reset to replay

### Tutorial Delivery Mechanisms

The onboarding system uses five flexible delivery mechanisms that can be applied to any tutorial topic:

1. **[Modal Popups](onboarding/modal-popups.md)**
   - Blocking message boxes for critical first-time actions
   - Progress indicators for multi-step tutorials
   - Rich formatting with images and icons
   - "Don't show again" options

2. **[Side Panel Notifications](onboarding/side-panel-notifications.md)**
   - Non-blocking contextual guidance (Rise of Nations style)
   - Auto-dismiss with configurable timeout
   - Minimize/expand functionality
   - Progress tracking for tutorial sequences

3. **[UI Highlights and Tooltips](onboarding/ui-highlights.md)**
   - Pulsing/glowing indicators for relevant elements
   - Enhanced tooltips with "Learn More" links
   - Dimmed overlays to focus attention
   - Arrow indicators and animations

4. **[Contextual Help System](onboarding/contextual-help.md)**
   - In-game help accessible via "?" button
   - Context-sensitive topic suggestions
   - Search functionality
   - "Show Me How" interactive demonstrations

5. **[Tutorial Manager and Scripting](onboarding/tutorial-manager.md)**
   - Event-driven tutorial triggering
   - Tutorial state management and persistence
   - Scripting format for content authoring
   - Analytics integration

### Core Tutorial Content Areas

Tutorial content covers five core gameplay areas using the above delivery mechanisms:

1. **Exploration and Mapping**: Camera controls, fog of war, scout units, terrain
2. **City Management**: Founding cities, building construction, population, specialization
3. **Armies and Combat**: Unit recruitment, movement, combat resolution, tactics
4. **Characters and Factions**: Character roles, faction bonuses, council system
5. **Resources and Economy**: Resource types, trade routes, economic management

### Implementation Steps

1. **Tutorial Manager Framework**
   - Implement event-driven tutorial system listening to game state changes
   - Create tutorial scripting DSL or data format (JSON/YAML)
   - Build tutorial progression state machine
   - Implement save/load for tutorial progress

   - Develop side panel notification component
   - Create modal popup component with accessibility features
   - Implement UI highlight/pulse system for element focus
   - Build enhanced tooltip system with "Learn More" functionality

2. **Tutorial Content**

- Write tutorial scripts for 5 core systems
- Create help documentation with context-sensitive linking
- Develop tutorial progression flows based on player actions

4. **Analytics Integration**
   - Implement event tracking for tutorial starts/completions
   - Track drop-off points within tutorials
   - Monitor time-to-complete metrics
   - Record skip/dismiss actions for optimization

5. **Localization Preparation**
   - Extract all tutorial text to localization files
   - Design tutorial system to support RTL languages
   - Create screenshot/video asset pipeline for localized content

## Phases

### Phase 1 — Discovery

**Objective**: Define core tutorial topics, player flows, and success metrics

**Deliverables**:

- Player persona definitions (beginner/intermediate/expert)
- Tutorial flow diagrams for each core system
- Event trigger mapping (which game events trigger which tutorials)
- Success metrics and KPIs definition
- UI mockups for tutorial delivery mechanisms

**Duration**: 2-3 weeks

**Key Activities**:

- Playtest competitor 4X games to identify best practices
- Interview potential players to understand experience levels
- Map critical learning moments in game progression
- Define tutorial trigger conditions and prerequisites

### Phase 2 — Implementation

**Objective**: Implement tutorial manager, UI components, and core tutorial content

**Deliverables**:

- Tutorial manager framework with event system
- UI components (side panels, modals, highlights, tooltips)
- Tutorial scripts for 5 core systems
- In-game help system with context-sensitive linking
- Analytics integration for tracking

**Duration**: 6-8 weeks

**Key Activities**:

- Build tutorial state machine and progression system
- Implement all UI delivery mechanisms
- Write and integrate tutorial content
- Create help documentation and interactive demonstrations
- Add analytics hooks for metrics collection

### Phase 3 — Polish

**Objective**: Refine pacing, optimize based on data, add localization, and improve accessibility

**Deliverables**:

- Tutorial pacing optimizations based on playtest data
- Localization support for all tutorial content
- Accessibility improvements (screen reader, colorblind modes)
- Tutorial quality metrics dashboard
- Player feedback integration

**Duration**: 3-4 weeks

**Key Activities**:

- Playtest competitor 4X games to identify best practices
- Interview potential players to understand experience levels
- Map critical learning moments in game progression
- Define tutorial trigger conditions and prerequisites
- Conduct playtests with target personas
- Analyze analytics data to identify drop-off points
- Refine tutorial triggers and pacing
- Prepare localization assets
- Implement accessibility features
- Create metrics dashboard for ongoing monitoring

## Success Metrics

The onboarding system will be measured against the following KPIs:

### Primary Metrics

- **Tutorial Completion Rate**: >= 70% of beginners complete at least 4 of 5 core tutorials
- **Time to First Meaningful Action**: New players found first city within 5 minutes
- **New Player Retention**: >= 60% of tutorial-completing players return for 2nd session within 48 hours
- **Tutorial Drop-off Rate**: <= 15% of players disable tutorials before completing 3 core systems

### Secondary Metrics

- **Help System Engagement**: >= 40% of players use in-game help at least once in first session
- **Tutorial Skip Rate by Persona**:
  - Beginners: <= 10% skip rate
  - Intermediate: 20-40% skip rate (expected for basic tutorials)
  - Expert: >= 80% skip rate (confirms proper filtering)
- **Average Tutorial Completion Time**:
  - Exploration: <= 3 minutes
  - City Management: <= 5 minutes
  - Combat: <= 7 minutes
  - Characters/Factions: <= 4 minutes
  - Resources/Economy: <= 5 minutes

### Quality Metrics

- **Tutorial Clarity Score**: >= 4.0/5.0 in player surveys
- **Accessibility Compliance**: 100% WCAG 2.1 AA compliance for tutorial UI
- **Localization Coverage**: Tutorial content available in all supported languages at launch

## Acceptance criteria

### Functional Requirements

- **FR-1**: Tutorial system supports three player experience levels (beginner, intermediate, expert)
  - GIVEN a player creating a new game
  - WHEN selecting experience level
  - THEN appropriate tutorials are enabled/disabled based on selection

- **FR-2**: Tutorials can be enabled/disabled granularly in settings
  - GIVEN a player in the settings menu
  - WHEN accessing tutorial options
  - THEN each tutorial category can be individually toggled
  - AND global tutorial level can be changed
  - AND completed tutorials can be reset

- **FR-3**: Tutorials are resumable after game exit
  - GIVEN a player exits mid-tutorial
  - WHEN returning to the game
  - THEN tutorial progress is preserved
  - AND player can continue from last completed step
  - AND player can choose to restart tutorial

- **FR-4**: Tutorial manager triggers based on game events
  - GIVEN specific game events occur (e.g., first city founded)
  - WHEN player meets trigger conditions
  - THEN appropriate tutorial is displayed
  - AND tutorial is skipped if player has disabled category
  - AND previously completed tutorials are not retriggered

- **FR-5**: Multiple tutorial delivery mechanisms are supported
  - Side panel notifications for primary guidance (non-blocking)
  - Modal popups for critical first-time actions (limited for beginners only)
  - UI highlights for relevant elements during tutorials
  - Enhanced tooltips with "Learn More" links
  - In-game help system with context-sensitive content

### Non-Functional Requirements

- **NFR-1**: Tutorial UI must not impact game performance
  - Tutorial rendering must maintain >= 60 FPS on minimum spec hardware
  - Tutorial state checks must complete in < 1ms

- **NFR-2**: Tutorial content must be accessible
  - All tutorial text readable by screen readers
  - UI highlights visible to colorblind players (using patterns + color)
  - Tutorial navigation possible via keyboard only

- **NFR-3**: Tutorial system must support localization
  - All text externalized to localization files
  - UI layouts support text expansion (up to 40% for German)
  - Images/videos have localized alternatives where text appears

- **NFR-4**: Analytics must capture tutorial effectiveness
  - Tutorial start/completion events tracked
  - Drop-off points within multi-step tutorials recorded
  - Time-to-complete metrics captured
  - Skip/dismiss actions logged with context

## Testing

### Unit Tests

- **Tutorial Manager State Machine**
  - State transitions (not started → in progress → completed)
  - Event trigger conditions (prerequisites, player actions)
  - Save/load of tutorial progress
  - Tutorial reset functionality

- **Tutorial Scripting**
  - Step progression logic
  - Conditional branching based on player choices
  - Timeout and auto-dismiss behavior
  - Multi-step tutorial coordination

### Integration Tests

- **Event System Integration**
  - Game events correctly trigger tutorials
  - Tutorial triggers respect player experience level
  - Tutorial UI components display at correct times
  - Analytics events fire on tutorial interactions

- **UI Component Integration**
  - Side panels render without blocking gameplay
  - Modal popups correctly pause game (if configured)
  - UI highlights attach to correct elements
  - Enhanced tooltips display context-sensitive help

### Playtesting

- **Beginner Playtest** (15-20 participants, never played 4X games)
  - Measure tutorial completion rates
  - Identify confusing or unclear steps
  - Track time to complete each tutorial
  - Gather feedback on tutorial pacing and clarity

- **Intermediate Playtest** (10-15 participants, played other 4X games)
  - Verify basic tutorials are properly skipped
  - Validate unique mechanics are highlighted
  - Measure time to productivity (first meaningful decisions)
  - Gather feedback on tutorial relevance

- **Expert Playtest** (5-10 participants, returning players or very experienced)
  - Confirm minimal guidance mode works as intended
  - Validate tutorial disable options are effective
  - Test help system accessibility for on-demand learning

### Accessibility Testing

- **Screen Reader Testing**
  - All tutorial content readable via NVDA/JAWS
  - Tutorial navigation logical and complete
  - UI highlights announced appropriately

- **Keyboard Navigation**
  - All tutorial interactions accessible via keyboard
  - Tab order logical through tutorial UI
  - Escape key dismisses modals/panels

- **Visual Accessibility**
  - Tutorial UI readable at 200% zoom
  - Color contrast meets WCAG 2.1 AA standards
  - UI highlights visible in colorblind simulation modes

### Performance Testing

- **Rendering Performance**
  - Tutorial UI maintains 60 FPS on minimum spec hardware
  - Side panel animations smooth and non-janky
  - UI highlights don't cause frame drops

- **Memory Usage**
  - Tutorial system memory footprint < 50 MB
  - No memory leaks during extended tutorial sessions
  - Tutorial assets properly unloaded when not in use

## Future Enhancements

Beyond the initial 5 core tutorial systems, the onboarding framework is designed to expand:

### Additional Tutorial Systems (Post-MVP)

- **Diplomacy**: Treaty negotiations, alliances, espionage
- **Technology**: Research trees, tech trading, scientific advancement
- **Culture**: Cultural influence, borders, achievements
- **Events**: Random events, decision-making, consequences
- **Advanced Combat**: Flanking, terrain bonuses, combined arms

### Advanced Features (v2.0+)

- **Adaptive Tutorials**: AI-driven tutorial personalization based on player behavior
- **Community Tutorial Creator**: Allow players to create custom tutorial scenarios
- **Tutorial Speedrunning**: Leaderboards for tutorial completion times
- **Video Tutorials**: Integrated video content for visual learners
- **Interactive Scenarios**: Dedicated tutorial missions beyond main game

### Analytics-Driven Improvements

- **A/B Testing**: Test different tutorial approaches to optimize engagement
- **Heatmap Analysis**: Identify where players struggle most
- **Predictive Drop-off**: Proactively offer help before players get stuck
- **Tutorial Effectiveness Scoring**: Automatically identify low-performing tutorials

## Dependencies

### Technical Dependencies

- **Event System**: Tutorial manager requires robust game event system
- **UI Framework**: Tutorial UI components require flexible UI rendering system
- **Persistence System**: Tutorial progress requires save/load functionality
- **Analytics Pipeline**: Metrics collection requires telemetry infrastructure
- **Localization System**: Tutorial content requires string externalization

### Content Dependencies

- **Core Game Systems**: Tutorials require stable implementations of exploration, cities, combat, characters, and resources
- **Help Documentation**: Tutorial "Learn More" links require comprehensive help content
- **Visual Assets**: UI highlights and demonstrations require art/animation support

### External Dependencies

- **Analytics Service**: Cloud-based analytics platform for data collection (e.g., Unity Analytics, custom backend)
- **Localization Service**: Translation management for tutorial content
- **Accessibility Testing Tools**: Screen readers, contrast analyzers, keyboard testing frameworks
