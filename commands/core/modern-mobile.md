---
name: modern-mobile
slug: modern-mobile
version: 1.0.0
description: Build premium, modern mobile app interfaces for iOS (SwiftUI/UIKit) and cross-platform (Ionic/Capacitor). Enforces a curated mobile design system with card patterns, navigation, typography, and motion drawn from Peapod, Relay, Denim, and Navan Edge. Replaces generic mobile UI with intentional, polished design.
license: MIT
---

This skill enforces a premium, modern mobile design system. Every decision is opinionated and concrete — no vague guidance. The output should feel like it belongs alongside Peapod, Relay, Denim, or Navan Edge.

The user provides mobile app requirements. Apply this entire system automatically. Do not ask which mood or pattern — infer from context, or default to Dark Minimal.

---

## 1. Core Philosophy

Four principles govern every mobile decision:

1. **Card-First Thinking** — Cards are the atomic unit of mobile UI. Every piece of content lives in a card. Cards group, elevate, and organize.
2. **Generous Real Estate** — Hero cards take 50-60% of the screen. Don't be timid with space. One impactful element beats three cramped ones.
3. **Dark Mode First** — Design for dark mode, adapt for light. Dark mode is the premium feel.
4. **Tactile Polish** — Every element should feel like it responds to touch. Subtle shadows, pressed states, smooth transitions. ChatGPT/Uber-level polish.

---

## 2. Decision Framework — 5 Mood Directions

Pick ONE based on context. Each maps to a palette, card style, and reference.

### Dark Minimal
- **Vibe**: Clean, premium, task-focused. Elevated cards on deep backgrounds.
- **References**: Relay dark mode, task-calendar dark
- **Palette**: Obsidian Mobile (see Section 4)
- **Card style**: Elevated cards with subtle borders, pill badges
- **Best for**: Task managers, dashboards, productivity apps, dev tools

### Light Clean
- **Vibe**: Bright, spacious, friendly. Grouped list cards on off-white.
- **References**: Relay light, task-list-tab-bar, task-calendar light
- **Palette**: Bone Mobile (see Section 4)
- **Card style**: Grouped cards with light borders, colored status pills
- **Best for**: Todo apps, calendars, habit trackers, note-taking apps

### Media Rich
- **Vibe**: Visual-first, gallery layouts, content showcase with bold imagery.
- **References**: Peapod (podcast app), Denim (gallery/covers), Navan Edge
- **Palette**: Adaptive (see Section 4)
- **Card style**: Image-heavy cards with gradient overlays, horizontal scroll rows
- **Best for**: Podcast apps, music players, galleries, media browsers, content discovery

### Health & Wellness
- **Vibe**: Warm gradients, large insight cards, calming but data-driven.
- **References**: health-goals-warm-gradient
- **Palette**: Warm Vitals (see Section 4)
- **Card style**: Large ~60% screen hero cards with warm gradient backgrounds, priority badges
- **Best for**: Health tracking, fitness apps, wellness dashboards, nutrition apps

### Techie Onboarding
- **Vibe**: Minimal, black-and-white, advanced SaaS feel. ChatGPT/Uber energy.
- **References**: location-picker-onboarding (Kingsley Orji)
- **Palette**: Mono (see Section 4)
- **Card style**: Clean white cards with subtle shadows, step indicators, map/visual integrations
- **Best for**: Onboarding flows, setup wizards, location pickers, account creation

---

## 3. Typography

### iOS / SwiftUI
Use the system type scale. Do not fight the platform.

```swift
// Heading hierarchy
.font(.largeTitle)   // Screen titles, hero text
.font(.title)        // Section headers
.font(.title2)       // Card titles
.font(.title3)       // Subsection headers
.font(.headline)     // Bold labels, card titles (alternative)
.font(.body)         // Primary content
.font(.callout)      // Secondary descriptions
.font(.subheadline)  // Metadata, timestamps
.font(.footnote)     // Tertiary info, badges
.font(.caption)      // Smallest text, fine print
.font(.caption2)     // Micro labels
```

**Weight guidelines:**
```swift
.fontWeight(.bold)      // Titles, CTAs
.fontWeight(.semibold)  // Card titles, section headers
.fontWeight(.medium)    // Labels, pill text
.fontWeight(.regular)   // Body text, descriptions
```

**Design rules:**
- `.largeTitle` for the main screen title — always bold, left-aligned
- Card titles use `.headline` or `.title3` — semibold
- Subtitles/descriptions use `.subheadline` — secondary color
- Metadata (dates, counts) use `.caption` — muted color
- Never use custom fonts unless the app has a strong brand identity that demands it

### Web / Ionic / Capacitor
Use the same curated fonts from modern-frontend but with mobile-optimized sizes:

```css
:root {
  --font-heading: 'Satoshi', -apple-system, sans-serif;
  --font-body: 'General Sans', -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  --text-xs: 0.7rem;
  --text-sm: 0.8rem;
  --text-base: 0.95rem;
  --text-lg: 1.1rem;
  --text-xl: 1.35rem;
  --text-2xl: 1.65rem;
  --text-3xl: 2rem;
  --text-display: 2.5rem;
}

body {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}
```

### BANNED FONTS (same as modern-frontend)
`Inter, Roboto, Poppins, Montserrat, Open Sans, Lato, Nunito, Raleway, system-ui (as visible font)`

---

## 4. Color Palettes — Mobile Presets

### Obsidian Mobile (Dark Minimal)
```swift
// SwiftUI
static let bgPrimary = Color(hex: "#0a0a0b")
static let bgSecondary = Color(hex: "#111113")
static let bgElevated = Color(hex: "#18181b")
static let bgCard = Color(hex: "#1c1c1f")
static let border = Color(hex: "#27272a")
static let borderSubtle = Color(hex: "#1e1e21")
static let textPrimary = Color.white
static let textSecondary = Color(hex: "#a1a1aa")
static let textMuted = Color(hex: "#52525b")
static let accent = Color(hex: "#3b82f6")
static let accentSubtle = Color(hex: "#3b82f6").opacity(0.12)
```
```css
/* CSS */
:root {
  --bg-primary: #0a0a0b;
  --bg-secondary: #111113;
  --bg-elevated: #18181b;
  --bg-card: #1c1c1f;
  --border: #27272a;
  --border-subtle: #1e1e21;
  --text-primary: #ffffff;
  --text-secondary: #a1a1aa;
  --text-muted: #52525b;
  --accent: #3b82f6;
  --accent-subtle: rgba(59, 130, 246, 0.12);
}
```

### Bone Mobile (Light Clean)
```swift
static let bgPrimary = Color(hex: "#f5f5f4")
static let bgSecondary = Color(hex: "#fafaf9")
static let bgElevated = Color.white
static let bgCard = Color.white
static let border = Color(hex: "#e7e5e4")
static let borderSubtle = Color(hex: "#f0eeec")
static let textPrimary = Color(hex: "#0c0a09")
static let textSecondary = Color(hex: "#57534e")
static let textMuted = Color(hex: "#a8a29e")
static let accent = Color(hex: "#0d9488")
static let accentSubtle = Color(hex: "#0d9488").opacity(0.08)
```
```css
:root {
  --bg-primary: #f5f5f4;
  --bg-secondary: #fafaf9;
  --bg-elevated: #ffffff;
  --bg-card: #ffffff;
  --border: #e7e5e4;
  --border-subtle: #f0eeec;
  --text-primary: #0c0a09;
  --text-secondary: #57534e;
  --text-muted: #a8a29e;
  --accent: #0d9488;
  --accent-subtle: rgba(13, 148, 136, 0.08);
}
```

### Adaptive (Media Rich)
Inherits from content — use dynamic background colors extracted from artwork/images.
```swift
// Base structure — accent adapts per content
static let bgPrimary = Color.white  // or .black for dark variant
static let bgCard = Color.white
static let textPrimary = Color(hex: "#1c1c1e")
static let textSecondary = Color(hex: "#3c3c43")
// Accent derived from content imagery at runtime
```
```css
:root {
  --bg-primary: #ffffff;
  --bg-card: #ffffff;
  --text-primary: #1c1c1e;
  --text-secondary: #3c3c43;
  --text-muted: #8e8e93;
  /* Accent varies per content — set dynamically */
  --accent: #007aff;
}
```

### Warm Vitals (Health & Wellness)
```swift
static let bgPrimary = Color(hex: "#faf8f5")
static let bgCard = Color(hex: "#ffffff")
static let gradientWarm = LinearGradient(
    colors: [Color(hex: "#8b2500"), Color(hex: "#c4501a"), Color(hex: "#d4783a")],
    startPoint: .topLeading, endPoint: .bottomTrailing
)
static let textPrimary = Color(hex: "#1a1714")
static let textSecondary = Color(hex: "#5c564e")
static let textOnGradient = Color.white
static let accent = Color(hex: "#c4501a")
```
```css
:root {
  --bg-primary: #faf8f5;
  --bg-card: #ffffff;
  --gradient-warm: linear-gradient(135deg, #8b2500, #c4501a, #d4783a);
  --text-primary: #1a1714;
  --text-secondary: #5c564e;
  --text-on-gradient: #ffffff;
  --accent: #c4501a;
}
```

### Mono (Techie Onboarding)
```swift
static let bgPrimary = Color(hex: "#f7f7f7")
static let bgCard = Color.white
static let textPrimary = Color.black
static let textSecondary = Color(hex: "#6b6b6b")
static let textMuted = Color(hex: "#999999")
static let accent = Color.black
static let accentInverse = Color.white
```
```css
:root {
  --bg-primary: #f7f7f7;
  --bg-card: #ffffff;
  --text-primary: #000000;
  --text-secondary: #6b6b6b;
  --text-muted: #999999;
  --accent: #000000;
  --accent-inverse: #ffffff;
}
```

### Color Rules (Mobile)
- **Accent in max 2 places per screen** — more focused than web
- Status pills get their own semantic colors (red for high priority, green for today, orange for flagged)
- Card backgrounds: `--bg-card` only. Never tint card backgrounds with accent colors.
- Gradient backgrounds ONLY on hero cards in Health & Wellness mood

---

## 5. Card Patterns — The Core Building Blocks

### Pattern A: Hero Card (~60% screen)
App Store-style feature card. Large, image-backed, impactful.

```swift
// SwiftUI
ZStack(alignment: .bottomLeading) {
    // Background image
    Image("feature-bg")
        .resizable()
        .aspectRatio(contentMode: .fill)

    // Gradient overlay for text readability
    LinearGradient(
        colors: [.clear, .black.opacity(0.7)],
        startPoint: .top,
        endPoint: .bottom
    )

    // Content
    VStack(alignment: .leading, spacing: 8) {
        Text("CATEGORY")
            .font(.caption)
            .fontWeight(.semibold)
            .tracking(1)
        Text("Card Title Here")
            .font(.title2)
            .fontWeight(.bold)
        Text("Supporting description text goes here.")
            .font(.subheadline)
    }
    .foregroundColor(.white)
    .padding(20)
}
.frame(height: UIScreen.main.bounds.height * 0.55)
.clipShape(RoundedRectangle(cornerRadius: 16))
```

**Rules:**
- Image fills the card, content overlays with gradient
- Gradient goes from clear to 60-70% black — enough to read, image still visible
- Category label in caps + tracking, title bold, subtitle regular weight
- Corner radius: 16px
- This is the Peapod pattern — use it for featured/hero content

### Pattern B: Stat Card (Compact, Stackable)
For wallets, dashboards, loyalty points. Multiple per row.

```swift
// SwiftUI
HStack {
    Image(systemName: "creditcard.fill")
        .foregroundColor(.accent)
    VStack(alignment: .leading, spacing: 2) {
        Text("Hilton Honors")
            .font(.subheadline)
            .fontWeight(.medium)
        Text("114,000")
            .font(.caption)
            .foregroundColor(.secondary)
    }
    Spacer()
    Text("Points")
        .font(.caption)
        .foregroundColor(.secondary)
}
.padding(16)
.background(Color.bgCard)
.clipShape(RoundedRectangle(cornerRadius: 12))
```

**Rules:**
- Horizontal layout: icon/image + text stack + trailing value
- Compact padding (12-16px)
- Multiple stat cards stack vertically with 8px gaps
- Corner radius: 12px
- This is the Navan Edge pattern

### Pattern C: List-in-Card (Grouped Items)
Multiple list items inside a single card container.

```swift
// SwiftUI
VStack(spacing: 0) {
    ForEach(items) { item in
        HStack {
            VStack(alignment: .leading, spacing: 4) {
                Text(item.title)
                    .font(.body)
                    .fontWeight(.medium)
                Text(item.subtitle)
                    .font(.caption)
                    .foregroundColor(.secondary)
                // Status pill
                HStack(spacing: 4) {
                    Image(systemName: "calendar")
                    Text("Today")
                }
                .font(.caption2)
                .foregroundColor(.green)
            }
            Spacer()
            Image(systemName: "ellipsis")
                .foregroundColor(.secondary)
        }
        .padding(.vertical, 14)
        .padding(.horizontal, 16)

        if item.id != items.last?.id {
            Divider().padding(.leading, 16)
        }
    }
}
.background(Color.bgCard)
.clipShape(RoundedRectangle(cornerRadius: 16))
```

**Rules:**
- One card wraps multiple list rows
- Rows separated by inset dividers (not full-width)
- Each row: title + subtitle + optional status pill, trailing action (ellipsis or chevron)
- Corner radius: 16px on the container card
- This is the task-list / task-calendar pattern

### Pattern D: Task Card (With Pill Badges)
Individual task/item card with metadata pills.

```swift
// SwiftUI
VStack(alignment: .leading, spacing: 12) {
    // Pill row
    HStack(spacing: 8) {
        PillBadge("To do", color: .secondary)
        PillBadge("High", color: .red)
    }

    // Content
    Text("Migrate API Auth to OAuth 2.0")
        .font(.headline)
    Text("Backend security hardening phase")
        .font(.subheadline)
        .foregroundColor(.secondary)

    // Footer
    HStack {
        AvatarStack(users: assignees)
        Spacer()
        Text("Due: Today")
            .font(.caption)
            .foregroundColor(.red)
        Text("40%")
            .font(.caption)
            .foregroundColor(.secondary)
    }
}
.padding(16)
.background(Color.bgCard)
.clipShape(RoundedRectangle(cornerRadius: 14))
```

**Pill Badge component:**
```swift
struct PillBadge: View {
    let text: String
    let color: Color

    var body: some View {
        Text(text)
            .font(.caption2)
            .fontWeight(.medium)
            .padding(.horizontal, 10)
            .padding(.vertical, 4)
            .background(color.opacity(0.12))
            .foregroundColor(color)
            .clipShape(Capsule())
    }
}
```

**Rules:**
- Pills at the top of the card for status/priority
- Title + subtitle below pills
- Footer row with avatars, due date, progress
- Corner radius: 14px
- This is the Relay pattern

### Pattern E: Gallery Row (Horizontal Scroll)
Category label + horizontally scrolling cards.

```swift
// SwiftUI
VStack(alignment: .leading, spacing: 12) {
    HStack {
        Text("New in Denim")
            .font(.headline)
        Spacer()
        Image(systemName: "chevron.right")
            .foregroundColor(.secondary)
    }
    .padding(.horizontal, 16)

    ScrollView(.horizontal, showsIndicators: false) {
        HStack(spacing: 12) {
            ForEach(items) { item in
                GalleryCard(item: item)
                    .frame(width: 140, height: 140)
            }
        }
        .padding(.horizontal, 16)
    }
}
```

**Rules:**
- Section header with title + optional chevron/see-all
- Horizontal ScrollView with consistent card sizes
- Card aspect ratios match content type (square for albums, 2:3 for books, 16:9 for videos)
- Padding on the scroll content, not the ScrollView
- 12px gaps between cards
- This is the Denim / Peapod gallery pattern

### Pattern F: Calendar/Week Strip Card
Hero image card with a week day selector below it.

```swift
// SwiftUI
VStack(spacing: 0) {
    // Hero image section
    ZStack(alignment: .bottomLeading) {
        Image("calendar-hero")
            .resizable()
            .aspectRatio(contentMode: .fill)
            .frame(height: 180)
            .clipped()
        VStack(alignment: .leading) {
            Text("Tuesday")
                .font(.subheadline)
            Text("Week 42")
                .font(.headline)
                .fontWeight(.bold)
        }
        .foregroundColor(.white)
        .padding(16)
    }

    // Week strip
    HStack(spacing: 0) {
        ForEach(weekDays) { day in
            VStack(spacing: 4) {
                Text("\(day.number)")
                    .font(.callout)
                    .fontWeight(day.isToday ? .bold : .regular)
                Text(day.abbreviation)
                    .font(.caption2)
            }
            .frame(maxWidth: .infinity)
            .padding(.vertical, 10)
            .background(day.isToday ? Color.accent : Color.clear)
            .clipShape(RoundedRectangle(cornerRadius: 8))
        }
    }
    .padding(8)
    .background(Color.bgCard)
}
.clipShape(RoundedRectangle(cornerRadius: 16))
```

---

## 6. Navigation

### Tab Bar (Compact — Preferred for all iOS apps)
```swift
// SwiftUI
TabView {
    TodayView()
        .tabItem {
            Image(systemName: "doc.text")
            Text("Today")
        }
    UpcomingView()
        .tabItem {
            Image(systemName: "calendar")
            Text("Upcoming")
        }
    HabitsView()
        .tabItem {
            Image(systemName: "figure.walk")
            Text("Habits")
        }
}
.tint(.accent)
```

**Tab bar rules:**
- Max 5 tabs — 3-4 is ideal
- Use SF Symbols for icons — always
- Active tab: accent color. Inactive: muted color.
- Include a floating "+" button for primary actions if needed (outside the tab bar)
- Tab labels are short — one word each

### Top Navigation (Segmented / Pill Tabs)
For sub-navigation within a screen (Goals | Actions pattern).

```swift
// SwiftUI
HStack(spacing: 8) {
    ForEach(tabs) { tab in
        Text(tab.title)
            .font(.subheadline)
            .fontWeight(selectedTab == tab ? .semibold : .regular)
            .foregroundColor(selectedTab == tab ? .primary : .secondary)
            .padding(.horizontal, 16)
            .padding(.vertical, 8)
            .background(
                selectedTab == tab
                    ? Color.bgElevated
                    : Color.clear
            )
            .clipShape(Capsule())
            .onTapGesture { selectedTab = tab }
    }
}
```

**Rules:**
- Pill-shaped selected state or simple text weight change
- Left-aligned or centered depending on number of tabs
- No underline tabs — use pill/capsule backgrounds instead

### Navigation Bar
```swift
// SwiftUI
.navigationTitle("Today")
.navigationBarTitleDisplayMode(.large)  // Default to large titles
.toolbar {
    ToolbarItem(placement: .topBarTrailing) {
        Button(action: {}) {
            Image(systemName: "line.3.horizontal.decrease")
        }
    }
}
```

**Rules:**
- Large titles by default (`.large` display mode)
- Trailing toolbar buttons for filters/settings — SF Symbols only
- No custom back buttons unless absolutely necessary
- Title should be one word when possible ("Today", "Goals", "Library")

---

## 7. Motion & Interaction

### Core Principles
```swift
// Standard spring animation — use for most transitions
withAnimation(.spring(response: 0.35, dampingFraction: 0.8)) {
    // state change
}

// Subtle ease for simple state changes
withAnimation(.easeOut(duration: 0.2)) {
    // state change
}
```

### Hard Motion Limits
| Property | Max Value | Reason |
|----------|-----------|--------|
| Spring response | `0.4s` | Longer feels sluggish on mobile |
| Transition duration | `0.35s` | Mobile needs snappier than web |
| Scale on press | `0.97` | Subtle press-in effect |
| Opacity on press | `0.85` | Gentle feedback |
| Card lift shadow | `8px blur` | More is gaudy |

### Press States (Tactile Feedback)
```swift
// Button press scale
.scaleEffect(isPressed ? 0.97 : 1.0)
.animation(.easeOut(duration: 0.15), value: isPressed)

// Card press
.scaleEffect(isPressed ? 0.98 : 1.0)
.opacity(isPressed ? 0.9 : 1.0)
```

### Haptic Feedback
```swift
// Light tap — for selections, toggles
UIImpactFeedbackGenerator(style: .light).impactOccurred()

// Medium — for completing actions
UIImpactFeedbackGenerator(style: .medium).impactOccurred()

// Success — for task completion
UINotificationFeedbackGenerator().notificationOccurred(.success)
```

**Rules:**
- Haptics on every primary action (tap, complete, delete)
- Light for passive interactions, medium for active, success for completions
- Never use `.heavy` — it feels aggressive
- No haptics on scroll or passive viewing

### Reduced Motion
```swift
@Environment(\.accessibilityReduceMotion) var reduceMotion

// Conditional animation
withAnimation(reduceMotion ? .none : .spring(response: 0.35)) {
    // state change
}
```

---

## 8. Spacing System

```swift
// SwiftUI spacing constants
enum Spacing {
    static let xs: CGFloat = 4
    static let sm: CGFloat = 8
    static let md: CGFloat = 12
    static let lg: CGFloat = 16
    static let xl: CGFloat = 20
    static let xxl: CGFloat = 24
    static let section: CGFloat = 32
}
```

```css
/* CSS */
:root {
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 12px;
  --space-lg: 16px;
  --space-xl: 20px;
  --space-2xl: 24px;
  --space-section: 32px;
}
```

**Usage rules:**
- Card internal padding: 16px (lg)
- Gap between cards in a list: 12px (md)
- Gap between cards in a horizontal scroll: 12px (md)
- Section spacing (between card groups): 32px (section)
- Screen edge padding: 16px (lg)
- Pill badge internal padding: 4px vertical, 10px horizontal

---

## 9. Corner Radius Scale

```swift
enum CornerRadius {
    static let sm: CGFloat = 8    // Pills, small buttons
    static let md: CGFloat = 12   // Stat cards, compact cards
    static let lg: CGFloat = 16   // Standard cards, list-in-card
    static let xl: CGFloat = 20   // Hero cards, sheets
    static let full: CGFloat = .infinity  // Pill badges (Capsule)
}
```

**Rules:**
- Pill badges: Capsule (fully rounded)
- Hero cards: 16-20px
- Standard cards: 12-16px
- Week strip day cells: 8px
- Never mix radii within the same card type — consistency is key
- All cards on the same screen should share a radius

---

## 10. Anti-Patterns — BANNED

### Banned Layouts
- Full-screen cards with no breathing room — always leave edge padding
- Cards with colored backgrounds that don't match the palette
- More than 2 card styles on a single screen
- Tab bars with more than 5 items
- Bottom sheets that cover more than 70% of the screen on first appear

### Banned Effects
- Blur on everything — backdrop blur only on navigation bars and sheets
- Drop shadows on every card — use borders in dark mode, subtle shadows in light mode only
- Bouncy spring animations (dampingFraction < 0.7)
- Skeleton loading screens with pulsing gradients — use simple opacity fade-in instead
- Parallax scroll effects within cards

### Banned Interactions
- Swipe-to-delete without confirmation
- Force touch / 3D touch as the only way to access features
- Custom gesture recognizers that conflict with system gestures
- Pull-to-refresh with custom animations (use the system one)

### Banned Visual Choices
- Gradient text on mobile (readability nightmare at small sizes)
- Card shadows in dark mode (use 1px borders instead)
- Colored card backgrounds (use `--bg-card` only, gradient hero cards are the exception)
- Icon badges/circles above text in a grid (the "generic feature grid")
- Purple accent colors (#6366f1, #8b5cf6, #a855f7)

---

## 11. Implementation Checklist

Every mobile screen must pass ALL 16 items. Binary pass/fail.

### Typography (3 items)
- [ ] Using system fonts (SwiftUI) or curated font list (web) — no banned fonts
- [ ] Heading hierarchy is clear: one `.largeTitle` per screen, `.headline` for cards
- [ ] Metadata uses `.caption` or `.footnote` with secondary/muted colors

### Color (3 items)
- [ ] Using one of the 5 palette presets — no custom hex outside the palette
- [ ] Accent color appears in max 2 places per screen
- [ ] No banned hex values anywhere

### Cards (4 items)
- [ ] Hero/feature content uses Pattern A (60% screen hero card) or Pattern F (calendar card)
- [ ] List content uses Pattern C (list-in-card) — not bare list rows
- [ ] Card corner radii are consistent across the screen
- [ ] Cards use borders in dark mode, subtle shadows in light mode — never both

### Navigation (3 items)
- [ ] Tab bar has 3-5 items with SF Symbols
- [ ] Large navigation titles by default
- [ ] Sub-navigation uses pill/capsule tabs, not underline tabs

### Motion (3 items)
- [ ] Press states on all tappable elements (scale 0.97-0.98)
- [ ] Spring animations have response <= 0.4s
- [ ] Haptic feedback on primary actions

---

## 12. Accessibility

Non-negotiable requirements:

- **Dynamic Type**: Support all system text sizes. Use system fonts and relative sizing.
```swift
.font(.body)  // Automatically scales with Dynamic Type
// Never hardcode font sizes like .system(size: 16)
```
- **VoiceOver**: All interactive elements have accessibility labels.
```swift
.accessibilityLabel("Mark task as complete")
.accessibilityHint("Double tap to complete this task")
```
- **Touch targets**: Minimum 44x44pt for all tappable elements.
- **Color contrast**: Text meets WCAG AA (4.5:1 body, 3:1 large text).
- **Reduced motion**: Checked and respected (see Section 7).
- **Dark/light mode**: Both must work. Dark mode first, but light mode must not be broken.

---

## 13. Platform-Specific Notes

### SwiftUI (Primary)
- Use `@Environment(\.colorScheme)` for dark/light adaptation
- `ScrollView` for all scrollable content — never `List` for custom card layouts
- `.scrollContentBackground(.hidden)` to remove default List backgrounds
- Group related cards in a `VStack` with consistent spacing
- Use `LazyVStack` for long lists, `LazyHStack` for horizontal galleries

### Ionic / Capacitor (Cross-platform)
- Use `ion-content` with custom CSS, not Ionic's default card components
- Override Ionic's default styles completely — same philosophy as Bootstrap override in modern-frontend
- Use CSS custom properties from the palettes above
- `ion-tab-bar` for bottom navigation, styled to match the compact tab bar pattern
- Safe area insets: always respect `env(safe-area-inset-*)` in CSS

### UIKit (Legacy)
- Same design principles apply — cards via custom `UIView` subclasses with the correct radii and padding
- Use `UICollectionViewCompositionalLayout` for complex card layouts
- `UIImpactFeedbackGenerator` for haptics

---

## 14. Maalik's Mobile Design Preferences

When Maalik says "make it look modern" for a mobile app, prioritize:

### Card Priorities
1. **Hero cards take 50-60% of the screen** — App Store style with gradient overlays (Peapod)
2. **List items grouped in a single card** — never bare rows (task-list-tab-bar)
3. **Pill badges for status/priority** — on every task/item card (Relay)
4. **Stat cards for data** — compact, stackable, horizontal layout (Navan Edge)
5. **Gallery rows with horizontal scroll** — category label + cards (Denim, Peapod)

### Navigation Priorities
1. **Compact tab bar for all iOS apps** — especially Swift apps
2. **Pill/capsule tabs** for sub-navigation, not underlines
3. **Large titles** by default

### Mood Priorities
1. **Dark mode preferred** over light when both exist
2. **Techie/minimal aesthetic** — ChatGPT, Uber level polish for onboarding
3. **Warm gradients** for health/wellness content

### Reference Screenshots
See `/Users/maalikahmadtech/Dev/modern-mobile/` for all reference images:
- `peapod-podcast-app.png` — FAVORITE: gradient overlay card + gallery rows
- `relay-task-manager-dark.jpeg` — PREFERRED: pill badges, dark card layout
- `task-calendar-light-dark-mode.jpeg` — FAVORITE LIST VIEW: list-in-card pattern
- `task-list-tab-bar.jpeg` — compact tab bar + grouped list card
- `denim-gallery-covers.png` — gallery row layout
- `navan-loyalty-wallet.png` — stat card layout
- `health-goals-warm-gradient.jpeg` — 60% hero card with warm gradient
- `location-picker-onboarding.jpeg` — techie onboarding style
- `profile-card-light-vs-dark.jpeg` — profile card variations
- `relay-task-manager-light.jpeg` — light mode alternative

### Full design preferences
See `~/.claude/projects/-Users-maalikahmadtech-Dev/memory/design-inspiration.md` for Maalik's complete web + mobile design analysis.

---

## Quick Reference

| Decision | Answer |
|----------|--------|
| Which platform first? | SwiftUI for iOS, Ionic/Capacitor for cross-platform |
| Which fonts? | System fonts (SwiftUI) or curated list (web) |
| Which colors? | One of 5 palettes from Section 4 |
| Dark or light? | Dark first, light must also work |
| Card corner radius? | 12-16px standard, 20px for hero/sheets |
| How much animation? | Spring 0.35s, press scale 0.97, haptics on actions |
| Tab bar style? | Compact, 3-5 tabs, SF Symbols |
| Card shadows? | Light mode only. Dark mode uses borders. |
| Gradient overlays? | Hero cards only, for text readability over images |
| Purple? | No. |
