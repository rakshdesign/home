

# README — Plastic Bag Interactive Site

## Overview

Interactive single-page site where:

* A central **plastic bag acts as controller**
* Clicking it toggles a **state (open/closed)**
* 5 images emerge in an **arc/fountain motion**
* Images are **draggable**
* 4 images are **hyperlinks**
* 1 image (tennis ball) has **physics simulation**
* Background includes **grain + music tied to state**

---

## File Structure

```
/project
 ├── index.html
 ├── style (inside HTML)
 ├── script (inside HTML)
 ├── topcover.png        (bag)
 ├── 1.png
 ├── 2.png               (tennis ball)
 ├── 3.png
 ├── 4.png
 ├── 5.png
 └── music.mp3
```

---

## Core System

### 1. State System

Single boolean state:

```
OPEN / CLOSED
```

Controlled by:

```
gallery.classList.toggle("show")
```

---

### 2. Layout System

* Fullscreen container (`100vh`)
* Centered using flexbox
* All interactive elements are **absolutely positioned**

```
.container (centered)
 ├── bag (z-index: 2)
 └── gallery (absolute)
      └── items (absolute)
```

---

### 3. Animation Logic

#### Closed State

* images invisible
* scale small
* stacked at center

```
opacity: 0
scale: 0.2
```

#### Open State

* images visible
* move outward in arc
* scale to full

```
.show .item → translate(var(--x), var(--y)) scale(1)
```

---

### 4. Arc System (Fountain Effect)

Each image has custom variables:

```
--x → horizontal spread
--y → vertical height
```

Example:

```
i1 → (-200, -150)
i3 → (0, -250)
i5 → (200, -150)
```

This creates:

> center → upward → spread = fountain motion

---

### 5. Layering Trick (Important)

Initial:

```
bag: z-index 2
images: z-index 1
```

After opening:

```
images → z-index 5 (after delay)
```

Effect:

* images appear from behind
* then move above bag

---

### 6. Floating Behavior

```
.floating → animation active
.stop-float → animation removed
```

Rules:

* OPEN → stop floating
* CLOSED → resume floating

---

### 7. Drag System (Pointer Events)

Uses unified input:

```
pointerdown / pointermove / pointerup
```

Logic:

```
start position → store
move → calculate delta
apply → base position + delta
save → final offset
```

Transform formula:

```
translate(calc(var(--x) + dx), calc(var(--y) + dy))
```

Important:

```
touch-action: none;
```

Prevents mobile scroll interference.

---

### 8. Hyperlink System

Structure:

```
<a href="https://linkX.com">
    <img src="X.png">
</a>
```

Exception:

```
2.png → NOT wrapped (used for physics)
```

---

### 9. Tennis Ball Physics

Independent system.

#### Variables

```
x, y → position
vx, vy → velocity
gravity
bounce count
```

#### Loop

```
x += vx
y += vy
vy += gravity
```

#### Collisions

Walls:

```
if (x <= 0 || x + width >= screenWidth)
    vx *= -1
```

Top:

```
if (y <= 0)
    vy *= -1
```

Ground:

```
if (y + height >= screenHeight)
    vy *= -0.7
```

#### Stop Rule

```
after 6 bounces → stop bouncing → fall naturally
```

#### Responsiveness

Uses:

```
window.innerWidth
window.innerHeight
```

---

### 10. Audio System

```
audio.volume = 0.6
```

Behavior:

```
OPEN → play()
CLOSED → pause()
```

Bag acts as:

> play / pause toggle

---

### 11. Grain / Noise System

Canvas overlay:

```
position: fixed
pointer-events: none
```

Loop:

```
generate random pixels
draw every frame
```

Creates animated noise texture.

---

## Interaction Flow

1. Load → bag floating
2. Click bag:

   * images emerge (arc + scale)
   * z-index shifts
   * music starts
3. User interactions:

   * drag images
   * click links
   * click tennis ball → physics
4. Click bag again:

   * images collapse
   * positions reset
   * music pauses
   * floating resumes

---

## Reset Logic (Important)

On CLOSE:

```
reset transform
reset drag offsets
reset z-index
stop animations
```

Prevents broken state on reopen.

---

## Key Dependencies (Conceptual)

No libraries used.

Relies on:

* CSS transforms
* CSS variables
* pointer events API
* requestAnimationFrame
* HTML audio API
* canvas API

---

## Critical Implementation Details

* Only **one transform chain per element**
* Drag must **override transition**
* Physics must **lock interaction during animation**
* Z-index must change **after animation delay**
* Canvas must **resize on window resize**

---

## Rebuild Order (strict)

1. Center bag
2. Add hidden images
3. Add `.show` toggle
4. Add arc transforms
5. Add scale animation
6. Add z-index transition
7. Add drag system
8. Add audio control
9. Add tennis ball physics
10. Add grain overlay

---

## End State

A system with:

* controlled state transitions
* layered animation illusion
* multi-input interaction (mouse + touch)
* hybrid UI + physics behavior

---


