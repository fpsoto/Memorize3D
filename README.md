# Memorize

A memory card-matching game built with Unity 6. Flip cards to find all matching pairs!

## Gameplay

- Tap a card to flip it and reveal its image
- Find the matching pair for each card
- Match all 10 pairs to win
- Cards that don't match shake and flip back face-down
- Timer and failed attempts tracked in the HUD
- Personal best (time + attempts) saved between sessions

## Tech Stack

- **Engine**: Unity 6000.3.13f1
- **Target platforms**: Android (portrait), Windows/macOS
- **Language**: C# (.NET Framework 4.7.1, C# 8.0)
- **UI**: TextMeshPro + legacy UnityEngine.UI

## Project Structure

```
Assets/
  Script/
    GameManager.cs          # Singleton scene controller (DontDestroyOnLoad)
    PlayerController.cs     # Core game logic, match tracking, win condition, best score
    CardController.cs       # Per-card flip animation, shake, and click handling
    ButtonsManager.cs       # Wires UI buttons to scene transitions
    LocalizationManager.cs  # Multilanguage system, event-driven, persisted in PlayerPrefs
    LocalizedText.cs        # Component that auto-updates Text on language change
    LanguageSelector.cs     # Runtime language picker UI (Options scene)
    Music.cs                # Background music singleton
    Transitions.cs          # Animator-driven fade transitions
  Resources/
    Localization/
      en.txt               # English strings
      es.txt               # Spanish strings
  Scenes/
    Preload              # Bootstrap scene
    Start                # Main menu
    Game                 # Gameplay scene
    Options              # Options screen (Language selector + placeholder settings)
    Credits              # Credits screen
  Editor/
    LocalizationWirer.cs # Tools > Localization > Wire Current Scene
```

## Scene Flow

```
Preload → Start (main menu) → Game
                           ↘ Options
                           ↘ Credits
```

## How to Run

1. Open the project in Unity 6 (`File → Open Project`)
2. Press `Ctrl+P` or click the Play button to run in the editor
3. For Android: `File → Build Settings → Android → Build`

## How It Works

1. At game start, cards drop onto the board one by one in random order with ease-out animation
2. Timer starts and input is enabled once the last card lands
3. Click a card — it rotates 180° on the Z-axis to reveal its face
4. Click a second card — if they match, both are removed; `CardsRemaining` decrements by 2
5. If they don't match, both cards shake and flip back face-down
6. The HUD shows remaining pairs, failed attempts, current time, and personal best
7. Match all pairs (`CardsRemaining == 0`) to trigger the win screen with time, attempts, and record status

## Implemented Improvements

### Gameplay
- ✓ Visible timer while the game is active (stops on win)
- ✓ Failed attempts counter in HUD
- ✓ Personal best saved via `PlayerPrefs` — best time + best attempts shown in HUD and win screen
- ✓ Card drop animation at game start — cards fall from above in random order with cubic ease-out

### User Experience
- ✓ Shake animation on cards when a pair fails
- ✓ Multilanguage support (Spanish / English) — event-driven custom system, saved in `PlayerPrefs`
- ✓ Settings menu with 4 placeholder items: Language, SFX Volume, BGM Volume, Difficulty

### Bug Fixes
- ✓ Card matching fixed — was using array index order (non-deterministic), now uses material name

## Planned Improvements

### Gameplay
- Difficulty levels: Easy (12c / 0.9s), Medium (20c / 0.5s), Hard (20c / 0.25s), Extreme (30c / 0.2s) — requires 5 new textures for Extreme
- Countdown mode: limited time to complete the board
- Penalty for wrong matches (score reduction or limited attempts)

### User Experience
- Visual effect (particles or flash) on match before cards disappear
- Highlight the first selected card while waiting for the second
- Animated win screen entrance (fade + scale)

### Technical / Quality
- Separate audio cues for match vs. mismatch
- Migrate legacy `UnityEngine.UI.Text` to TextMeshPro
- Replace `OnMouseDown()` with `IPointerClickHandler` + `PhysicsRaycaster` for reliable Android touch
