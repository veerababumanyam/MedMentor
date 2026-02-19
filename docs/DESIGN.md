# MedMentor AI — Design System

**Version:** 0.0.1
**Status:** Production Ready  
**Last Updated:** February 19, 2026

---

## 1. Brand Identity

### 1.1 Core Concept
**"The Interconnected Medical Brain"**
MedMentor AI transforms the overwhelming flood of medical information into a structured, interconnected network of knowledge. The design must feel **intelligent**, **trustworthy**, and **alive** — reflecting a system that actively learns alongside the user.

**Personalized Greeting Pattern:**
"Good Morning, [Name]. Your brain is ready for new connections." — Emphasizes the partnership between the user and the AI.

### 1.2 Design Philosophy
The visual language draws from three pillars:
*   **Liquid Glassmorphism:** Layered translucent surfaces with deep backdrop blur, creating depth and hierarchy through light and material rather than heavy borders.
*   **iOS-Grade Precision:** Apple-level attention to spacing, typography, micro-interactions, and touch targets. Every pixel is intentional.
*   **Medical Trust:** A palette grounded in deep blues and clean neutrals that conveys clinical authority while remaining warm and approachable.

### 1.3 Personality
*   **Trustworthy & Professional:** Grounded in evidence, precise, and safe.
*   **Modern & Dynamic:** Uses subtle animation and "living" UI elements (pulses, flows, glassmorphism) to convey active intelligence.
*   **Approachable & Clean:** Minimalist interfaces that reduce cognitive load, with generous whitespace and clear visual hierarchy.

### 1.4 Logo Concept
*   **Symbol:** A stylized neural network where nodes and connections form a medical cross in negative space.
*   **Metaphor:** The "spark" of connecting two concepts (synapse firing).
*   **App Icon:** Symbol on a *Trust Blue* or *Midnight Navy* background with a subtle glass-material overlay.
*   **Wordmark:** "MedMentor" in *Inter Bold*, "AI" in *Inter Light* to suggest precision.

---

## 2. Color System

The palette uses a trustworthy medical blue base with energetic teal accents to signify growth and active recall. Extended with a dark-mode glassmorphism palette for the immersive calendar and dashboard experiences.

### 2.1 Primary Brand Colors

| Color Name | Hex | RGB | Usage |
| :--- | :--- | :--- | :--- |
| **Trust Blue** | `#0056D2` | `0, 86, 210` | Core brand. Primary buttons, active states, key headers. |
| **Growth Teal** | `#00E8C6` | `0, 232, 198` | Accent. Success states, learning sparks, progress bars. |
| **Midnight Navy** | `#0F172A` | `15, 23, 42` | Primary text, deep backgrounds, dark mode base. |

### 2.2 Glass / Dark Mode Palette
Used for immersive experiences like the calendar, dashboards, and focus mode. Built on layered transparency over animated mesh backgrounds.

| Color Name | Hex / Value | Opacity | Usage |
| :--- | :--- | :--- | :--- |
| **Glass Background** | `rgba(255,255,255,0.12)` | 12% | Primary card/container surfaces |
| **Glass Hover** | `rgba(255,255,255,0.18)` | 18% | Hover state for glass containers |
| **Glass Border** | `rgba(255,255,255,0.20)` | 20% | Default border on glass surfaces |
| **Glass Border Strong** | `rgba(255,255,255,0.35)` | 35% | Active/focus state borders |
| **Glass Subtle** | `rgba(255,255,255,0.05)` | 5% | Muted backgrounds, toggle groups |
| **Deep Background** | `#0A0A1A` | 100% | Base canvas behind mesh gradients |
| **Elevated Surface** | `rgba(30,30,50,0.85)` | 85% | Dropdown panels, modal sheets |

### 2.3 iOS Accent Palette
Extended accent colors used for event categories, tags, and data visualization. Directly mapped from Apple’s Human Interface Guidelines.

| Color Name | Hex | CSS Variable | Usage |
| :--- | :--- | :--- | :--- |
| **System Blue** | `#007AFF` | `--accent` | Primary interactive, links, today indicator |
| **System Purple** | `#5856D6` | `--purple` | Secondary accent, gradient endpoints |
| **System Pink** | `#FF2D55` | `--pink` | Social events, alerts, notifications |
| **System Orange** | `#FF9500` | `--warning` | Warning states, personal events |
| **System Green** | `#34C759` | `--success` | Success, health events, completion |
| **System Red** | `#FF3B30` | `--danger` | Error states, destructive actions |
| **System Teal** | `#5AC8FA` | `--teal` | Travel, info badges, light accents |
| **System Indigo** | `#AF52DE` | `--purple` | Creative events, design reviews |

### 2.4 Neutral Scale (Slate)

| Token | Hex | Usage |
| :--- | :--- | :--- |
| **Slate 900** | `#0F172A` | Headings, Primary Text (Light Mode) |
| **Slate 700** | `#334155` | Secondary Text, Icons |
| **Slate 500** | `#64748B` | Captions, Placeholder Text, Disabled States |
| **Slate 300** | `#CBD5E1` | Subtle borders, dividers |
| **Slate 200** | `#E2E8F0` | Dividers, Borders |
| **Slate 100** | `#F1F5F9` | Page Backgrounds (Light Mode) |
| **Slate 50** | `#F8FAFC` | Card Backgrounds (Light Mode) |
| **White** | `#FFFFFF` | Surface Backgrounds, Cards, Modals |

### 2.5 Semantic Colors

| Context | Color Name | Hex | Usage |
| :--- | :--- | :--- | :--- |
| **Success** | **Emerald** | `#10B981` | Correct answers, passed quizzes, completion. |
| **Warning** | **Amber** | `#F59E0B` | Low confidence, uncertainty flags, streaks at risk. |
| **Error** | **Rose** | `#EF4444` | Incorrect answers, destructive actions, validation. |
| **Info** | **Sky** | `#0EA5E9` | Tooltips, guidance, "did you know" hints. |

### 2.6 Gradients

**Light Mode Surface Gradient**
```css
background: linear-gradient(135deg, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0.4));
```

**Dark Mode Accent Gradient**
```css
background: linear-gradient(135deg, #007AFF, #5856D6);
```

**Study Card Gradient (Cardiology)**
```css
background: linear-gradient(135deg, #1e3a8a 0%, #7c3aed 100%); /* Deep Blue to Vivid Purple */
```

**Animated Mesh Background (Calendar/Dashboard)**
```css
background:
  radial-gradient(ellipse 80% 60% at 10% 20%, rgba(88, 86, 214, 0.4) 0%, transparent 60%),
  radial-gradient(ellipse 60% 80% at 80% 80%, rgba(0, 122, 255, 0.3) 0%, transparent 60%),
  radial-gradient(ellipse 50% 50% at 50% 50%, rgba(175, 82, 222, 0.15) 0%, transparent 50%),
  #0A0A1A;
```

**Glass Shine (Liquid Effect)**
```css
background: linear-gradient(135deg,
  transparent 40%,
  rgba(255,255,255,0.04) 45%,
  rgba(255,255,255,0.08) 50%,
  rgba(255,255,255,0.04) 55%,
  transparent 60%);
animation: shineSlide 6s ease-in-out infinite;
```

---

## 3. Typography

### 3.1 Font Family
*   **Primary:** [Inter](https://fonts.google.com/specimen/Inter) — Variable weight support, highly legible, neutral but modern.
*   **Display Alternative:** [Plus Jakarta Sans](https://fonts.google.com/specimen/Plus+Jakarta+Sans) — Used in immersive/glassmorphism contexts for display headings only. Adds warmth and character.
*   **Monospace:** JetBrains Mono or SF Mono — Code snippets, medical codes, technical references.

### 3.2 Type Scale

| Style | Weight | Size (rem/px) | Line Height | Tracking | Usage |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Display H1** | ExtraBold (800) | `2.25rem` / `36px` | `1.1` | `-0.02em` | Marketing, Hero text, Calendar header |
| **H1** | Bold (700) | `1.875rem` / `30px` | `1.2` | `-0.01em` | Page Titles |
| **H2** | SemiBold (600) | `1.5rem` / `24px` | `1.3` | `-0.01em` | Section Headers, Card Titles |
| **H3** | Medium (500) | `1.25rem` / `20px` | `1.4` | `0` | Subsection Headers |
| **Body Large** | Regular (400) | `1.125rem` / `18px` | `1.6` | `0` | Introduction text, Focal content |
| **Body** | Regular (400) | `1rem` / `16px` | `1.5` | `0` | Standard paragraph text |
| **Small** | Regular (400) | `0.875rem` / `14px` | `1.5` | `0` | Metadata, Secondary info |
| **Caption** | SemiBold (600) | `0.75rem` / `12px` | `1.5` | `0.05em` | Labels, Uppercase tags, Status bar |
| **Micro** | SemiBold (600) | `0.625rem` / `10px` | `1.5` | `0.05em` | Nav labels, badge text |

### 3.3 Glass Mode Typography
In dark/glass mode, text uses opacity-based color hierarchy instead of hex values for seamless blending with translucent surfaces:

| Token | Value | Usage |
| :--- | :--- | :--- |
| `--text-primary` | `rgba(255, 255, 255, 0.95)` | Headings, active labels, key data |
| `--text-secondary` | `rgba(255, 255, 255, 0.60)` | Body text, descriptions, subtitles |
| `--text-tertiary` | `rgba(255, 255, 255, 0.35)` | Disabled, placeholder, weekday labels |

---

## 4. Spacing & Layout

### 4.1 Base Unit
**4px Grid System.** All spacing, sizing, and typography line-heights are multiples of 4.

| Token | Size | Value |
| :--- | :--- | :--- |
| `space-1` | 4px | `0.25rem` |
| `space-2` | 8px | `0.5rem` |
| `space-3` | 12px | `0.75rem` |
| `space-4` | 16px | `1rem` |
| `space-5` | 20px | `1.25rem` |
| `space-6` | 24px | `1.5rem` |
| `space-8` | 32px | `2rem` |
| `space-10` | 40px | `2.5rem` |
| `space-12` | 48px | `3rem` |
| `space-16` | 64px | `4rem` |

### 4.2 Containers
*   **Mobile:** 100% width, `16px` horizontal padding. Max calendar width: `480px`.
*   **Tablet:** Max-width `768px`, centered, `24px` padding.
*   **Desktop:** Max-width `1200px`, centered, `32px` padding.

### 4.3 Component Gap System
| Context | Gap | Notes |
| :--- | :--- | :--- |
| **Between sections** | `24px` (`space-6`) | Major content blocks |
| **Between cards** | `10–16px` | Event cards, stat cards |
| **Inside card padding** | `16–20px` | Standard glass containers |
| **Grid cell gap** | `2–3px` | Calendar day grid |
| **Icon + text** | `6–12px` | Inline label pairs |
| **Nav items** | `space-around` | Bottom navigation |

---

## 5. Glassmorphism System
The liquid glassmorphism system is the signature visual layer of MedMentor AI’s immersive interfaces. It creates a sense of depth, material, and space using layered translucency, blur, and light effects.

### 5.1 Glass Material Classes
| Class | Background | Blur | Border | Shadow |
| :--- | :--- | :--- | :--- | :--- |
| `.glass` | `rgba(255,255,255,0.12)` | `blur(40px)` | `rgba(255,255,255,0.20)` | `0 8px 32px rgba(0,0,0,0.3)` |
| `.glass-elevated` | `rgba(255,255,255,0.08)` | `blur(60px)` | `rgba(255,255,255,0.20)` | `0 20px 60px rgba(0,0,0,0.4)` |
| `.glass-subtle` | `rgba(255,255,255,0.05)` | `blur(20px)` | `rgba(255,255,255,0.08)` | None |
| `.glass-panel` (light) | `rgba(255,255,255,0.70)` | `blur(12px)` | `rgba(255,255,255,0.30)` | Level 1 |

### 5.2 Glass Shine (Liquid Reflection)
A signature animated gradient overlay that simulates light passing across a glass surface. Applied via the `::after` pseudo-element with `pointer-events: none`.
*   **Gradient angle:** 135deg diagonal sweep.
*   **Animation:** `shineSlide`, 6s ease-in-out infinite.
*   **Peak opacity:** 8% (`rgba(255,255,255,0.08)`) to maintain subtlety.

### 5.3 Animated Background Orbs
Floating radial gradient orbs create a living, breathing canvas behind glass surfaces. Three orbs with different colors, sizes, and animation timings prevent visual repetition.

| Orb | Size | Color Base | Blur | Animation Duration |
| :--- | :--- | :--- | :--- | :--- |
| **Orb 1 (Blue)** | 500px | `rgba(0,122,255,0.35)` | 80px | 20s |
| **Orb 2 (Purple)** | 400px | `rgba(175,82,222,0.30)` | 80px | 25s |
| **Orb 3 (Pink)** | 350px | `rgba(255,45,85,0.20)` | 80px | 18s |

### 5.4 CSS Implementation
```css
.glass {
  background: rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(40px);
  -webkit-backdrop-filter: blur(40px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}

.glass-elevated {
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(60px);
  -webkit-backdrop-filter: blur(60px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.4),
    0 0 0 1px rgba(255, 255, 255, 0.08);
}
```

---

## 6. Elevation & Shadows

### 6.1 Light Mode Shadows
| Level | Token | Value | Usage |
| :--- | :--- | :--- | :--- |
| **1** | `shadow-sm` | `0 1px 3px rgba(0,0,0,0.1), 0 1px 2px rgba(0,0,0,0.06)` | Cards at rest |
| **2** | `shadow-md` | `0 4px 6px rgba(0,0,0,0.1), 0 2px 4px rgba(0,0,0,0.06)` | Hover/active cards |
| **3** | `shadow-lg` | `0 20px 25px rgba(0,0,0,0.1), 0 10px 10px rgba(0,0,0,0.04)` | Modals, dropdowns |

### 6.2 Dark / Glass Mode Shadows
| Level | Token | Value | Usage |
| :--- | :--- | :--- | :--- |
| **1** | `glass-shadow` | `0 8px 32px rgba(0,0,0,0.3)` | Standard glass cards |
| **2** | `glass-shadow-elevated` | `0 20px 60px rgba(0,0,0,0.4), inset glow` | Dropdowns, modals |
| **3** | `glass-shadow-fab` | `0 8px 30px rgba(0,122,255,0.5), ring 4px` | FAB, primary CTA |
| **4** | `today-pulse` | Animated 0–30px, color-cycling | Today’s date indicator |

### 6.3 Focus Ring (Accent Glow)
All interactive elements receive a visible focus ring for keyboard navigation:
*   **Light mode:** `box-shadow: 0 0 0 3px rgba(0, 86, 210, 0.2); border-color: #0056D2;`
*   **Glass mode:** `box-shadow: 0 0 0 3px rgba(0, 122, 255, 0.2); border-color: #007AFF;`

---

## 7. Border Radius

| Token | Value | Usage |
| :--- | :--- | :--- |
| `radius-sm` | `8px` | Buttons, tags, input fields, small badges |
| `radius-md` | `12–16px` | Calendar day cells, cards (light mode), dropdown items |
| `radius-lg` | `22px` | Glass containers, event cards, month selector |
| `radius-xl` | `28px` | Calendar card wrapper, modal sheets |
| `radius-full` | `50%` / `9999px` | Icon buttons, avatar circles, FAB, color pickers |

---

## 8. UI Components

### 8.1 Buttons
| Variant | Background | Text | Border | Shadow | Hover |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Primary** | Trust Blue `#0056D2` | White | None | `shadow-sm` | Darken blue |
| **Accent (CTA)** | Gradient: `#007AFF` → `#5856D6` | White | None | `0 4px 15px rgba(0,122,255,0.4)` | `translateY(-1px)` |
| **Secondary** | White / `glass-subtle` | Slate 700 | Slate 200 | None | Slate 50 bg |
| **Ghost** | Transparent | Trust Blue | None | None | Blue-50 bg |
| **Destructive** | Rose `#EF4444` | White | None | `shadow-sm` | Darken rose |
| **Icon Button** | `rgba(255,255,255,0.1)` + blur | White | `rgba(255,255,255,0.15)` | None | `scale(1.08)` |

*All buttons: `radius-sm` (8px), min-height 44px, font-weight 600, transition 0.3s cubic-bezier(0.4, 0, 0.2, 1). Active state: `scale(0.95)` for tactile feedback.*

### 8.2 Cards
**Light Mode Cards**
*   Background: White (`#FFFFFF`)
*   Border: 1px solid Slate 200
*   Radius: 16px (`radius-md`)
*   Shadow: `shadow-sm`
*   Hover: `translateY(-2px)`, `shadow-md`, border Blue-200

**Glass Mode Cards**
*   Background: `rgba(255,255,255,0.12)` with glass-shine overlay
*   Border: 1px solid `rgba(255,255,255,0.20)`
*   Radius: 22px (`radius-lg`)
*   Shadow: `0 8px 32px rgba(0,0,0,0.3)`
*   Hover: `translateY(-2px)`, border-strong, shadow deepened

**Event Cards**
*   Layout: Horizontal flex with 4px colored accent bar on the left edge.
*   Content: Title (15px/600), Meta row with time badge and date label.
*   Avatar: 36px circle with category emoji, background matches event color at 13% opacity.
*   Active: `scale(0.98)` on press for haptic-style feedback.

### 8.3 Calendar Grid
**Day Cells**
*   Size: Square (aspect-ratio: 1), in a 7-column CSS Grid with 3px gap.
*   Default: 15px/500 weight, `--text-primary` color.
*   Other Month: `--text-tertiary` (35% opacity white).
*   Hover: `rgba(255,255,255,0.08)` background, `scale(1.08)`.
*   Selected: `rgba(0,122,255,0.2)` background with 1.5px solid accent border.
*   Today: Accent gradient background, bold white text, animated pulse shadow (0–30px, 3s infinite).
*   Event Dots: Up to 3 colored dots (5px diameter) below the date number.

**Weekday Row**
*   12px uppercase, font-weight 600, `--text-tertiary`, letter-spacing 0.5px. Consistent 8px vertical padding.

### 8.4 Dropdown Menus
Custom dropdown panels with iOS-grade presentation and clear visibility.

**Month Selector Trigger**
*   Container: Glass material with glass-shine effect, `radius-lg` (22px).
*   Layout: Month label (20px/700) + Year label (20px/400, accent-light color) + Chevron icon.
*   Chevron: Rotates 180° on open (0.4s cubic-bezier).
*   Hover: Background transitions to `glass-bg-hover` (18% white).

**Dropdown Panel**
*   Background: `rgba(30, 30, 50, 0.85)` with `blur(60px)` — opaque enough for readability.
*   Border: 1px solid `rgba(255,255,255,0.15)`.
*   Shadow: `0 25px 70px rgba(0,0,0,0.5)`, `0 0 0 1px rgba(255,255,255,0.05)`.
*   Entry animation: opacity 0→1, `translateY(-12px)`→0, `scale(0.97)`→1 over 0.35s.
*   Max height: 320px with styled scrollbar (4px track, 15% white thumb).

**Dropdown Items**
*   Padding: 12px 16px, `radius-sm` (12px).
*   Font: 15px/500, `--text-secondary`.
*   Hover: `rgba(255,255,255,0.1)` background, promote to `--text-primary`.
*   Active/Selected: Accent gradient background, white text, font-weight 600, checkmark suffix.
*   Active shadow: `0 4px 15px rgba(0,122,255,0.4)`.

**Year Navigator**
*   Centered row above the month list with ‹ / › navigation buttons. Year displayed in 17px/700 weight. Separated from month list with a 1px divider at 8% white opacity.

**Select Dropdowns (Form Context)**
*   Styling: Custom `appearance:none` with matching glass-style input treatment.
*   Background: `rgba(255,255,255,0.06)`, border `rgba(255,255,255,0.12)`.
*   Arrow: Custom SVG chevron absolutely positioned (right: 14px).
*   Focus: Accent border + 3px accent glow ring.
*   Options: Background `#1A1A30`, white text for native dropdown readability.

### 8.5 Form Inputs
**Light Mode**
*   Background: Slate 50 (`#F8FAFC`).
*   Border: 1px solid Slate 300.
*   Radius: `radius-md` (12px).
*   Focus: Trust Blue ring (3px solid, 20% opacity) + blue border.

**Glass Mode**
*   Background: `rgba(255,255,255,0.06)`.
*   Border: 1px solid `rgba(255,255,255,0.12)`.
*   Radius: `radius-sm` (12px).
*   Focus: `border-color: #007AFF`, `box-shadow: 0 0 0 3px rgba(0,122,255,0.2)`, bg promotes to 0.08.
*   Placeholder: `--text-tertiary` (35% white).
*   Text: 15px/500, `--text-primary`.

### 8.6 Modal / Bottom Sheet
*   Overlay: `rgba(0,0,0,0.5)` with `blur(8px)` backdrop.
*   Sheet: Slides up from bottom, `rgba(25,25,45,0.92)` with `blur(60px)`.
*   Handle: Centered 40px × 4px bar, `rgba(255,255,255,0.2)`, radius 4px.
*   Border radius: `radius-xl` top corners only (28px 28px 0 0).
*   Animation: `translateY(100%)`→0 over 0.4s cubic-bezier(0.4, 0, 0.2, 1).
*   Shadow: `0 -20px 60px rgba(0,0,0,0.4)`.

### 8.7 Color Picker
*   Layout: Flex row with 10px gap, wrapping.
*   Options: 32px circles with 2px transparent border.
*   Hover: `scale(1.15)`.
*   Selected: 2px white border + 3px white ring (20% opacity) + checkmark overlay.

### 8.8 View Toggle (Segmented Control)
*   Container: `glass-subtle` background, `radius-md` (16px), 4px padding.
*   Buttons: Equal flex, 10px 16px padding, `radius-sm` (12px).
*   Active: Accent gradient background, white text, 0 4px 15px blue shadow.
*   Inactive hover: `rgba(255,255,255,0.08)`, promote text to `--text-primary`.

### 8.9 Dashboard Widgets (Analytics)
**Retention Card**
*   **Visuals:** Dark glass card with a vibrant teal/green smooth curve graph.
*   **Data:** "RETENTION [XX]%" header.
*   **Context:** "Forgetting curve optimization active." (Subtle text).
*   **Accent:** Growth Teal `#00E8C6` for graph line and connection points.

**Predictor Card**
*   **Visuals:** Dark glass card containing a circular progress ring.
*   **Data:** Large central number (e.g., "245"), Label "Step 1" or "Step 2".
*   **Badge:** "ON TRACK" pill badge (Teal background, low opacity).
*   **Progress Ring:** Gradient blue to purple glow.

### 8.10 Daily Queue Card
A primary action area for the Space Repetition System (SRS).
*   **Header:** "Daily Queue" + "Space Repetition System" subtext.
*   **Stat:** Large counter "120 CARDS DUE" (White display type).
*   **Content:**
    *   **Inner Glass Card:** "Mixed Review" container with icon and 3-dot complexity indicator (Orange, Yellow, Green).
    *   **Action Button:** Full-width "Start Review" button (Primary Blue/Purple gradient).

---

## 9. Navigation

### 9.1 Bottom Tab Bar
*   **Background:** `rgba(15,15,30,0.8)` with `blur(40px)`.
*   **Border:** 1px solid `rgba(255,255,255,0.08)` top only.
*   **Layout:** 5 Icons evenly spaced:
    1.  **Home** (Grid icon) - Active styling (Teal/Cyan glow).
    2.  **Learn** (Graduation cap).
    3.  **FAB** (Center floating action button).
    4.  **Library** (Folder icon).
    5.  **Profile** (User avatar icon).
*   **Active State:** Icon color shifts to Teal (`#00E8C6`) or brand gradient + text label color change.
*   **Inactive:** `--text-tertiary` (35% white).

### 9.2 Floating Action Button (FAB)
*   **Size:** 56px circle (prominent overlap on tab bar).
*   **Background:** **Cyan/Teal Gradient** (distinct from the purple/blue brand gradient).
    *   Matches "Growth Teal" aesthetic.
*   **Shadow:** Strong Cyan Glow `0 0 20px rgba(0, 232, 198, 0.4)`.
*   **Icon:** Large White "+" (28-32px).
*   **Position:** Center docked in the Bottom Tab Bar.

### 9.3 Status Bar (iOS-Style)
Simulated iOS status bar with time (15px/700), and system icons (WiFi, battery) at 16px/white fill. Provides platform-native immersion in mobile views.

---

## 10. Animation & Motion

### 10.1 Easing Functions
| Token | Value | Usage |
| :--- | :--- | :--- |
| `ease-default` | `cubic-bezier(0.4, 0, 0.2, 1)` | General transitions (Google Material standard) |
| `ease-spring` | `cubic-bezier(0.4, 0, 0.2, 1)` | Buttons, cards, scale interactions |
| `ease-bounce` | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Playful feedback, checkmarks |

### 10.2 Transition Defaults
*   **Duration:** 0.25–0.35s for micro-interactions, 0.4s for layout shifts.
*   **Properties:** `all` (for simple elements) or explicit (`transform`, `opacity`, `background`).

### 10.3 Key Animations
| Animation | Duration | Easing | Description |
| :--- | :--- | :--- | :--- |
| `fadeInUp` | 0.6s | `cubic-bezier(0.4,0,0.2,1)` | Page load entry. Staggered 50ms per element. |
| `orbFloat1/2/3` | 18–25s | `ease-in-out` | Background orb floating. Different per orb. |
| `shineSlide` | 6s | `ease-in-out` | Glass shine sweep across containers. |
| `todayPulse` | 3s | `ease-in-out` | Glow pulse on today’s calendar cell. |
| `dropdown-enter` | 0.35s | `cubic-bezier(0.4,0,0.2,1)` | Scale + fade + translateY for menus. |
| `modal-slide` | 0.4s | `cubic-bezier(0.4,0,0.2,1)` | Bottom sheet slide up from 100%. |

### 10.4 Stagger Pattern
On page load, elements animate in sequence with 50ms delay increments (`delay-1` through `delay-6`). This creates a cascading reveal that guides the eye from top to bottom.

---

## 11. Iconography
*   **Library:** Lucide React (primary) or Heroicons Outline (alternative).
*   **Stroke Width:** 2px — matches Inter Medium/SemiBold visual weight.
*   **Style:** Rounded joins and caps for approachable feel.
*   **Sizes:** 16px (status bar), 18px (header actions), 22px (nav), 28px (FAB).
*   **Color in Glass Mode:** `currentColor`, inherited from parent — typically `--text-primary` or `--text-secondary`.
*   **Active state:** `filter: drop-shadow(0 2px 8px rgba(0,122,255,0.5))` for nav glow.

---

## 12. Accessibility (WCAG 2.2 AA)

### 12.1 Color Contrast
*   **Light mode text:** Slate 600+ on White (minimum 4.5:1 ratio). Avoid Slate 400 on White for body text.
*   **Glass mode text:** `--text-primary` (95% white) on glass surfaces ensures 7:1+ ratio against dark backgrounds.
*   **Glass mode secondary:** `--text-secondary` (60% white) maintains 4.5:1 minimum against `#0A0A1A` base.
*   **Interactive states:** All hover/active/focus states have visible contrast change.

### 12.2 Focus Management
*   All interactive elements have a visible focus ring (Trust Blue / System Blue).
*   Focus ring: 3px solid with 20% opacity glow — visible on both light and dark surfaces.
*   Tab order follows visual layout: header → selector → calendar → toggle → events → nav.
*   Modal traps focus and returns focus to trigger element on close.

### 12.3 Touch Targets
*   Minimum **44px × 44px** for all interactive elements (WCAG 2.2 AA).
*   Calendar day cells: square aspect-ratio ensures adequate tap area.
*   Icon buttons: 40px diameter with 4px invisible hit area extension.
*   Bottom nav items: 16px horizontal padding creates generous touch zones.

### 12.4 Motion & Reduced Motion
All animations respect `prefers-reduced-motion` media query:
```css
@media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
      animation-duration: 0.01ms !important;
      transition-duration: 0.01ms !important;
    }
}
```

### 12.5 Screen Reader Considerations
*   Calendar cells include `aria-label` with full date (e.g., “Thursday, February 19, 2026”).
*   Today’s cell includes `aria-current="date"`.
*   Event cards include `aria-label` combining title, time, and date.
*   Dropdown state communicated via `aria-expanded` on trigger.
*   Modal uses `role="dialog"` with `aria-labelledby` for the title.

---

## 13. CSS Custom Properties Reference
Complete token map for both light and glass/dark mode implementations:

### 13.1 Glass Mode Variables
```css
:root {
  /* Glass surfaces */
  --glass-bg: rgba(255, 255, 255, 0.12);
  --glass-bg-hover: rgba(255, 255, 255, 0.18);
  --glass-border: rgba(255, 255, 255, 0.2);
  --glass-border-strong: rgba(255, 255, 255, 0.35);
  --glass-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  --glass-shadow-elevated: 0 20px 60px rgba(0, 0, 0, 0.4);
  --glass-blur: blur(40px);
  --glass-blur-heavy: blur(60px);

    /* Accent colors */
    --accent: #007AFF;
    --accent-light: #5AC8FA;
    --accent-gradient: linear-gradient(135deg, #007AFF, #5856D6);

    /* Text hierarchy */
    --text-primary: rgba(255, 255, 255, 0.95);
    --text-secondary: rgba(255, 255, 255, 0.6);
    --text-tertiary: rgba(255, 255, 255, 0.35);

    /* Semantic */
    --danger: #FF3B30;
    --success: #34C759;
    --warning: #FF9500;
    --pink: #FF2D55;
    --purple: #AF52DE;
    --teal: #5AC8FA;

    /* Radii */
    --radius-sm: 12px;
    --radius-md: 16px;
    --radius-lg: 22px;
    --radius-xl: 28px;
}
```

### 13.2 Light Mode Variables
```css
:root[data-theme='light'] {
  --bg-page: #F1F5F9;
  --bg-card: #FFFFFF;
  --bg-input: #F8FAFC;
  --border-default: #E2E8F0;
  --border-focus: #0056D2;
  --text-primary: #0F172A;
  --text-secondary: #334155;
  --text-tertiary: #64748B;
    --accent: #0056D2;
    --accent-light: #00E8C6;
}
```

---

## 14. Implementation Checklist
Use this checklist when building new screens or components to ensure design system compliance:
1.  Font loaded: Inter (400, 500, 600, 700, 800) + Plus Jakarta Sans (700, 800).
2.  CSS custom properties defined in `:root` for both light and glass themes.
3.  All spacing on 4px grid. No arbitrary pixel values.
4.  Touch targets ≥ 44px on all interactive elements.
5.  Color contrast passes WCAG AA (4.5:1 for text, 3:1 for large text/UI).
6.  Glassmorphism includes `-webkit-backdrop-filter` prefix for Safari.
7.  Animations respect `prefers-reduced-motion`.
8.  Focus rings visible on all buttons, inputs, and interactive elements.
9.  Dropdowns use custom glass-elevated panels, not native browser selects (except in form contexts).
10. Modals use bottom-sheet pattern on mobile with drag handle.
