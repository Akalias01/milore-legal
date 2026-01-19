# LITHOS Palette Snapshot: Pre-Teal Migration
**Date:** 2026-01-15
**Version:** Pre v2026.6 (Amber/Copper System)
**Purpose:** Checkpoint before migrating to Teal Design System

---

## 1. SOURCE FILES

| File | Path | Lines |
|------|------|-------|
| Color.kt | `.../ui/theme/Color.kt` | 1-450 |
| GlassTheme.kt | `.../ui/theme/GlassTheme.kt` | 1-632 |
| LithosTheme.kt | `.../ui/theme/LithosTheme.kt` | 1-470 |
| PlayerScreenGlass.kt | `.../ui/screens/PlayerScreenGlass.kt` | 2077-2137, 2545-2991 |

---

## 2. COLOR TOKENS (CURRENT)

### 2A. Primary Accent (Amber System)
```
LithosAmber           = #C87F3F  (Primary accent - Warm copper)
LithosAmberLight      = #DA9555  (Lighter variant)
LithosAmberDark       = #A66830  (Darker variant)
LithosAmberSubtle     = #26C87F3F (15% amber for secondary)
```

### 2B. Gunmetal & Copper (Dark Mode)
```
GunmetalBackground    = #1A1A1A  (Deep matte base)
GunmetalSurface       = #2C2C2E  (Elevated surfaces)
MoltenCopper          = #D68840  (Warm amber-copper accent)
BoneWhite             = #E1E1E1  (Soft silver text)
DeepCharcoalTrack     = #1C1C1E  (Track backgrounds)
```

### 2C. Porcelain & Saddle (Light Mode)
```
OatBackground         = #FBFBF9  (Warm matte base)
PorcelainWhite        = #FFFFFF  (Physical buttons/cards)
SaddleCognac          = #B35A1F  (Deep leather accent)
VellumTrack           = #EBEBE6  (Tab tracks, containers)
CharcoalIcon          = #1C1B1F  (High-contrast icons)
```

### 2D. Backgrounds (iOS Dark Mode)
```
LithosSlate           = #1C1C1E  (Primary background)
LithosSlateBottom     = #141415  (Gradient bottom)
LithosOat             = #F2F0E9  (Reader Light)
LithosBlack           = #000000  (OLED Night)
```

### 2E. Frosted Glass
```
LithosGlass           = #B82C2C2E (72% opacity)
LithosGlassBorder     = #14FFFFFF (8% white)
LithosGlassLight      = #D9F2F0E9 (85% opacity)
LithosGlassBorderLight= #14000000 (8% black)
```

### 2F. Text Colors
```
LithosTextPrimary     = #FFFFFF   (100% white)
LithosTextSecondary   = #8CFFFFFF (55% white)
LithosTextTertiary    = #59FFFFFF (35% white)
LithosTextPrimaryLight= #1C1C1E
LithosTextSecondaryLight= #8C1C1C1E (55% slate)
```

### 2G. Sleep Mode Colors
```
SleepBlue             = #6B9FD4  (Primary sleep accent)
SleepBlueMuted        = #5A7FA8  (Darker variant)
SleepBlueGlow         = #8BB8E8  (Soft breathing highlight)
```

### 2H. Completion Metals (Platinum/Titanium)
```
PlatinumBright        = #E2E2E5  (Dark mode highlight)
PlatinumMid           = #C8C8CB  (Mid-tone silver)
PlatinumShadow        = #8E8E93  (Etched text)
PlatinumShimmer       = #FFFFFF  (Pure white sweep)
TitaniumBright        = #8E8E93  (Light mode brightest)
TitaniumMid           = #636366  (Light mode mid)
TitaniumShadow        = #48484A  (Light mode deepest)
```

### 2I. Progress Colors
```
LithosProgressTrack   = #14FFFFFF (8% white)
LithosProgressTrackLight = #141C1C1E (8% slate)
LithosScrubberBorder  = #1C1C1E
```

### 2J. Moss (Play/Pause Button)
```
LithosMoss            = #3D4F39  (Darker moss)
LithosMossLight       = #4A5D45  (Previous main)
LithosMossDark        = #2F3D2C  (Deep forest)
```

### 2K. Semantic Colors
```
LithosSuccess         = #4A5D45  (Moss-based)
LithosWarning         = #D48C2C  (Amber)
LithosError           = #B54D4D  (Muted terracotta)
LithosInfo            = #4D6B8C  (Slate blue)
```

---

## 3. THEME MAPPINGS

### 3A. Dark Theme (LithosDarkTheme)
```kotlin
background      = LithosSlate (#1C1C1E)
surface         = LithosSurfaceDark (#22262B)
amber           = LithosAmber (#C87F3F)
progressFill    = LithosAmber (#C87F3F)
```

### 3B. Light Theme (LithosLightTheme)
```kotlin
background      = OatBackground (#FBFBF9)
surface         = PorcelainWhite (#FFFFFF)
amber           = SaddleCognac (#B35A1F)
progressFill    = SaddleCognac (#B35A1F)
```

### 3C. OLED Theme (LithosOLEDTheme)
```kotlin
background      = LithosBlack (#000000)
surface         = #0A0A0A
amber           = LithosAmber (#C87F3F)
progressFill    = LithosAmber (#C87F3F)
```

### 3D. GlassThemeData (DarkGlassTheme)
```kotlin
background      = GunmetalBackground (#1A1A1A)
interactive     = MoltenCopper (#D68840)
```

### 3E. GlassThemeData (LightGlassTheme)
```kotlin
background      = LithosOat (#F2F0E9)
interactive     = LithosAmberDark (#A66830)
```

---

## 4. SMART NAVIGATION RING

**Source:** PlayerScreenGlass.kt, lines 2545-2991

### Ring Mode Color Mappings (CURRENT)
```kotlin
// PlayerScreenGlass.kt:2581-2595
val progressColor by animateColorAsState(
    targetValue = when (mode) {
        RingMode.READING -> LithosAmber      // <-- AMBER (to be changed)
        RingMode.SLEEP -> SleepBlue          // <-- OK
        RingMode.COMPLETION -> if (isDark)
            PlatinumBright else TitaniumMid  // <-- OK
    }
)

val waveColor by animateColorAsState(
    targetValue = when (mode) {
        RingMode.READING -> LithosAmber      // <-- AMBER (to be changed)
        RingMode.SLEEP -> SleepBlueGlow      // <-- OK
        RingMode.COMPLETION -> if (isDark)
            PlatinumShimmer else TitaniumBright  // <-- OK
    }
)
```

### Ring Geometry (DO NOT CHANGE)
```kotlin
// PlayerScreenGlass.kt - SmartNavigationRing parameters
val ringSize = 46.dp           // FIXED
val ringStroke = 3.dp          // Normal stroke - FIXED
val completionStroke = 23.dp   // Completion medallion - FIXED
strokeCap = StrokeCap.Round    // FIXED
```

---

## 5. PLAYER UI ELEMENTS

### 5A. Play/Pause Button (lines 2077-2137)
```kotlin
// Icon color
val iconColor = if (isDark) MoltenCopper else SaddleCognac
// Button color
val buttonColor = when {
    !isDark -> PorcelainWhite
    isOLED -> Color.Black.copy(alpha = 0.85f)
    else -> GunmetalSurface  // #2C2C2E
}
```

### 5B. Scrubber Progress (line 2278)
```kotlin
val progressColor = LithosAmber  // #D9943A
```

---

## 6. GLASCOLORS OBJECT INTERACTIVE

**Source:** GlassTheme.kt, lines 55-89
```kotlin
// Interactive - Lithos Amber (NOT iOS blue)
val Interactive = LithosAmber
val InteractivePressed = LithosAmber.copy(alpha = 0.80f)

// Lithos Amber accent for TEXT and ICONS
val LithosAccent = LithosAmber
val LithosAccentPressed = LithosAmber.copy(alpha = 0.80f)
val LithosAccentSubtle = LithosAmber.copy(alpha = 0.15f)
```

---

## 7. LITHOSUI OBJECT

**Source:** GlassTheme.kt, lines 318-381
```kotlin
val ActiveTrack = LithosAmber  // <-- To be changed
```

---

## 8. LITHOSACCENTS OBJECT

**Source:** GlassTheme.kt, lines 296-312
```kotlin
val Primary = LithosAmber
val PrimaryLight = LithosAmberLight
val PrimaryVibrant = LithosAmber
```

---

## SUMMARY: Items Requiring Migration

1. **RingMode.READING color** -> Change from `LithosAmber` to `LithosTeal`
2. **Play/Pause icon color** -> Change from `MoltenCopper`/`SaddleCognac` to `LithosTeal`
3. **Scrubber progressColor** -> Change from `LithosAmber` to `LithosTeal`
4. **GlassColors.Interactive** -> Change from `LithosAmber` to `LithosTeal`
5. **LithosUI.ActiveTrack** -> Change from `LithosAmber` to `LithosTeal`
6. **LithosAccents.Primary** -> Change from `LithosAmber` to `LithosTeal`
7. **Theme progressFill** -> Change from `LithosAmber`/`SaddleCognac` to `LithosTeal`

**New Token Required:**
```kotlin
val LithosTeal = Color(0xFF3FA9B5)  // #3FA9B5
```

---

*End of Snapshot*
