# LITHOS Animation Patterns

Reference document for consistent, premium animations throughout the app.

---

## Hero Card Shadow Animation

**Goal**: Shadow fades in/out smoothly as cards swipe, reaching 100% when card OVERLAPS (becomes topmost).

### CRITICAL: Parent Alpha Causes Shadow Pop

**THE FIX**: Remove parent alpha from HeroCardStack wrapper. The parent's `graphicsLayer { alpha = ... }`
affects the ENTIRE card including its shadow. When the parent alpha changes, the shadow appears to "pop"
because:
1. Non-centered cards have reduced alpha (e.g., 0.6)
2. This reduces shadow visibility to only 18% at edges
3. When card comes to front, shadow suddenly becomes fully visible

**SOLUTION**: Use scale and z-index for depth, NOT alpha changes on the parent container.

```kotlin
// In HeroCardStack - NO ALPHA on parent Box
Box(
    modifier = Modifier
        .zIndex(zIndex)
        .graphicsLayer {
            scaleX = scale
            scaleY = scale
            // NO alpha here! Was causing shadow "pop" effect
            this.translationX = translationX
        },
    contentAlignment = Alignment.Center
) {
    cardContent(item, centeredness)
}
```

### Shadow Alpha Calculation (in HeroCard)

```kotlin
// Shadow SIZE stays constant - only OPACITY changes
// Shadow reaches 100% when card overlaps the previous card (becomes topmost)
// Card is topmost at centeredness = 0.5, so 2x multiplier ensures 100% at that point
// NO animateFloatAsState - direct calculation prevents lag
val acceleratedCenteredness = (centeredness * 2f).coerceIn(0f, 1f)
val shadowAlpha = 0.3f + (0.7f * acceleratedCenteredness)

// Fixed shadow sizes - never change
val cardShadow = 20f
val coverShadow = 14f

// Apply to graphicsLayer
.graphicsLayer {
    shadowElevation = cardShadow
    ambientShadowColor = Color.Black.copy(alpha = shadowAlpha)
    spotShadowColor = Color.Black.copy(alpha = shadowAlpha)
}
```

**Key Points**:
- **NO parent alpha** - depth comes from scale and z-index only
- Shadow SIZE is constant (20dp) - never animate size
- Only change shadow OPACITY (alpha) based on centeredness
- 2x multiplier: shadow is 100% at centeredness = 0.5 (when card overlaps/becomes top)
- **NO animateFloatAsState** - direct calculation prevents animation lag
- Base alpha: 30% (back cards) → 100% (top card)

---

## Auto-Return Animation (HeroCardStack - 4 Cards)

**Goal**: Cards smoothly glide back to first card after 4 seconds of inactivity.
Each card visibly slides by during the rewind.

### CRITICAL: Why Sequential Animation is Required

HorizontalPager's `animateScrollToPage(0)` from a distant page **intentionally pre-jumps**
to skip intermediate pages (see Google Issue #267744105). This causes "skipping" artifacts.

**The ONLY way to show each card sliding by** is sequential page-by-page animation.

```kotlin
// Uses snapshotFlow with debounce for reliable idle detection
LaunchedEffect(pagerState) {
    snapshotFlow {
        Triple(
            pagerState.currentPage,
            pagerState.currentPageOffsetFraction,
            pagerState.isScrollInProgress
        )
    }
    .debounce(4000) // Wait 4 seconds after last state change
    .filter { (page, offset, scrolling) ->
        // Only proceed if not scrolling and not on page 0
        !scrolling && (page != 0 || abs(offset) > 0.01f)
    }
    .collect { (currentPage, _, _) ->
        // Sequential page-by-page animation for buttery smooth rewind
        // Each card visibly slides by - this is the only way to prevent "skipping"
        for (targetPage in (currentPage - 1) downTo 0) {
            pagerState.animateScrollToPage(
                page = targetPage,
                animationSpec = spring(
                    dampingRatio = Spring.DampingRatioNoBouncy,
                    stiffness = Spring.StiffnessMediumLow  // Smooth, not too slow
                )
            )
            // Brief pause creates visual separation between cards
            delay(80)
        }
    }
}
```

**Key Points**:
- Uses snapshotFlow + debounce for reliable detection (not while loop)
- **4 second** debounce after last user interaction
- **Sequential animation**: 4→3→2→1→0, each card visible
- **Spring animation** with `StiffnessMediumLow` for natural physics
- **80ms delay** between pages for visual separation without stopping
- Cancels pending auto-return if user interacts

**Why Spring Animation?**
- Handles interruptions smoothly (velocity continuity)
- Natural physics-based motion
- `DampingRatioNoBouncy` prevents overshoot
- `StiffnessMediumLow` is smooth without being sluggish

**Why NOT Single `animateScrollToPage(0)`?**
- HorizontalPager intentionally skips intermediate pages for performance
- Results in jarring "jump" when returning from distant pages
- Sequential animation is the only way to show each card

---

## Press Scale Animation

**Goal**: Subtle scale feedback on tap/press.

```kotlin
val scale by animateFloatAsState(
    targetValue = if (isPressed) 0.92f else 1f,
    animationSpec = tween(100),
    label = "scale"
)

.graphicsLayer {
    scaleX = scale
    scaleY = scale
}
```

**Key Points**:
- Scale to 92% when pressed
- Fast 100ms tween
- Apply via graphicsLayer for performance

---

## Standard Easing Reference

| Easing | Use Case |
|--------|----------|
| `FastOutSlowInEasing` | Enter/appear animations, auto-return |
| `LinearOutSlowInEasing` | Shadow/opacity that should complete early |
| `FastOutLinearInEasing` | Exit/dismiss animations |
| `LinearEasing` | Progress indicators, continuous motion |

---

## Duration Reference

| Duration | Use Case |
|----------|----------|
| 100ms | Shadow opacity, press feedback |
| 250ms | Quick transitions, toggles |
| 300ms | Standard element transitions |
| 500ms | Page/screen transitions, auto-return |

---

## Spring Animation Reference (When Needed)

```kotlin
spring(
    dampingRatio = Spring.DampingRatioNoBouncy,  // No overshoot
    stiffness = Spring.StiffnessMedium           // Responsive
)
```

**Avoid**: `DampingRatioMediumBouncy` for shadows - causes "pop" effect.

---

*Updated: January 2026*
