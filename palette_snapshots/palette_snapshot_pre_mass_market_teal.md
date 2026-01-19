# LITHOS Palette Snapshot: Pre Mass-Market Teal Migration
**Date:** 2026-01-16
**Version:** Pre-Migration (Amber Still Primary in Theme.kt)
**Purpose:** Checkpoint before mass-market Teal palette enforcement

---

## 1. COLOR TOKENS (Color.kt)

### 1A. Teal System (v2026.6 - TARGET PRIMARY)
```
LithosTeal           = #3FA9B5  (Line 180)
```

### 1B. Sleep Mode
```
SleepBlue            = #6B9FD4  (Line 187)
SleepBlueMuted       = #5A7FA8  (Line 188)
SleepBlueGlow        = #8BB8E8  (Line 189)
```

### 1C. Completion Metals - Platinum (Dark Mode)
```
PlatinumBright       = #E2E2E5  (Line 196)
PlatinumMid          = #C8C8CB  (Line 197)
PlatinumShadow       = #8E8E93  (Line 198)
PlatinumShimmer      = #FFFFFF  (Line 199)
```

### 1D. Completion Metals - Titanium (Light Mode)
```
TitaniumBright       = #8E8E93  (Line 202)
TitaniumMid          = #636366  (Line 203)
TitaniumShadow       = #48484A  (Line 204)
```

### 1E. Amber/Copper (LEGACY - TO BE RESTRICTED)
```
LithosAmber          = #C87F3F  (Line 73)
LithosAmberLight     = #DA9555  (Line 74)
LithosAmberDark      = #A66830  (Line 75)
LithosAmberSubtle    = #26C87F3F (Line 76) - 15% alpha
MoltenCopper         = #D68840  (Line 57)
SaddleCognac         = #B35A1F  (Line 34)
```

### 1F. Progress Tracks
```
LithosProgressTrack      = #14FFFFFF (Line 254) - 8% white
LithosProgressTrackLight = #141C1C1E (Line 255) - 8% slate
```

---

## 2. THEME.KT MAPPINGS (CURRENT - PROBLEM)

### LithosDarkColorScheme (Lines 21-39)
```kotlin
primary = LithosAmber           // <-- PROBLEM: Should be LithosTeal
primaryContainer = LithosAmberDark
secondary = LithosMoss
tertiary = LithosAmberLight     // <-- PROBLEM: Should be LithosTeal
background = LithosSlate
surface = LithosSurfaceDark
```

### LithosLightColorScheme (Lines 41-59)
```kotlin
primary = LithosAmberDark       // <-- PROBLEM: Should be LithosTeal
primaryContainer = LithosAmber
secondary = LithosMossDark
tertiary = LithosAmber          // <-- PROBLEM: Should be LithosTeal
background = LithosOat
surface = LithosSurfaceLight
```

---

## 3. LITHOSTHEME.KT MAPPINGS (PARTIALLY MIGRATED)

### Dark Branch colorScheme (Lines 223-241)
```kotlin
primary = LithosTeal            // OK - already migrated
primaryContainer = LithosTeal.copy(alpha = 0.3f)  // OK
tertiary = LithosTeal           // OK
```

### Light Branch colorScheme (Lines 242-261)
```kotlin
primary = SaddleCognac          // <-- NEEDS REVIEW for mass-market
tertiary = SaddleCognac         // <-- NEEDS REVIEW
```

### LithosThemeData Instances
```kotlin
LithosDarkTheme.progressFill  = LithosTeal   // OK (Line 116)
LithosLightTheme.progressFill = LithosTeal   // OK (Line 152)
LithosOLEDTheme.progressFill  = LithosTeal   // OK (Line 182)
```

---

## 4. GLASSTHEME.KT MAPPINGS (MIGRATED)

### GlassColors Object (Lines 55-67)
```kotlin
Interactive = LithosTeal           // OK
InteractivePressed = LithosTeal.copy(alpha = 0.80f)  // OK
LithosAccent = LithosTeal          // OK
LithosAccentPressed = LithosTeal.copy(alpha = 0.80f) // OK
LithosAccentSubtle = LithosTeal.copy(alpha = 0.15f)  // OK
```

### LithosAccents Object (Lines 296-300)
```kotlin
Primary = LithosTeal               // OK
PrimaryLight = LithosTeal          // OK
PrimaryVibrant = LithosTeal        // OK
```

### LithosUI Object (Line 345)
```kotlin
ActiveTrack = LithosTeal           // OK
```

### GlassThemeData Instances
```kotlin
DarkGlassTheme.interactive = LithosTeal       // OK (Line 517)
LightGlassTheme.interactive = LithosTeal      // OK (Line 531)
LithosOLEDGlassTheme.interactive = LithosTeal // OK (Line 546)
```

---

## 5. SCREEN-LOCAL OVERRIDES (VIOLATIONS)

### AddToSeriesSheet.kt (Line 28)
```kotlin
private val LithosAmber = Color(0xFFD9943A)  // LOCAL OVERRIDE - REMOVE
```

### BookmarksScreen.kt (Lines 35-37)
```kotlin
private val LithosAmber = Color(0xFFD9943A)        // LOCAL OVERRIDE - REMOVE
private val LithosSlate = Color(0xFF1A1D21)        // LOCAL OVERRIDE - REMOVE
private val LithosGlassBackground = Color(0xD91A1D21)  // LOCAL - REMOVE
```

### BookmarksScreen.kt (Line 309 - inside BookmarkCard)
```kotlin
val SleepBlue = Color(0xFF6B9FD4)  // LOCAL OVERRIDE - REMOVE
```

### ChapterListScreen.kt (Line 35)
```kotlin
private val LithosAmber = Color(0xFFD9943A)  // LOCAL OVERRIDE - REMOVE
```

### DownloadsScreen.kt (Line 58)
```kotlin
private val LithosAmber = Color(0xFFD9943A)  // LOCAL OVERRIDE - REMOVE
```

### SeriesDetailScreen.kt (Line 60)
```kotlin
private val LithosAmber = Color(0xFFD9943A)  // LOCAL OVERRIDE - REMOVE
```

### SplitBookScreen.kt (Line 55)
```kotlin
private val LithosAmber = Color(0xFFD9943A)  // LOCAL OVERRIDE - REMOVE
```

---

## 6. SUMMARY OF REQUIRED CHANGES

### Theme.kt
- Change primary from LithosAmber to LithosTeal
- Change tertiary from LithosAmberLight to LithosTeal

### LithosTheme.kt Light Branch
- Change primary from SaddleCognac to LithosTeal (for mass-market)
- Change tertiary from SaddleCognac to LithosTeal

### Screen Files (Remove Local Overrides)
1. AddToSeriesSheet.kt - Remove local LithosAmber
2. BookmarksScreen.kt - Remove local LithosAmber, LithosSlate, SleepBlue
3. ChapterListScreen.kt - Remove local LithosAmber
4. DownloadsScreen.kt - Remove local LithosAmber
5. SeriesDetailScreen.kt - Remove local LithosAmber
6. SplitBookScreen.kt - Remove local LithosAmber

### Screen Usage Migration
- Replace Amber usages with LithosTeal for interactive/selected states
- Keep SleepBlue ONLY for sleep-related UI
- Keep Platinum/Titanium ONLY for completion

---

*End of Pre-Migration Snapshot*
