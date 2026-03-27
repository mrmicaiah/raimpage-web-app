# Raimpage Design System

Based on research into music streaming platforms, AI product aesthetics, and creator economy apps.

---

## Brand Personality

**Raimpage should feel:**
- Fresh
- Rebellious (playful, not angry)
- Future-forward
- Community-driven
- Independent

**The vibe:** Approachable but cool. Creative. Underground meets professional.

---

## Color System

Raimpage uses a warm, energetic palette that stands out from competitors:
- Spotify = Green
- SoundCloud = Orange  
- Apple Music = Pink/Purple gradients
- Tidal = Blue
- **Raimpage = Coral + Hot Pink (warm, fun, futuristic)**

### Dark Mode (Default)

| Role | Hex | Usage |
|------|-----|-------|
| **Background** | `#0F0F1A` | Main app background |
| **Surface** | `#1A1A2E` | Cards, sidebar, player bar |
| **Surface Hover** | `#2D2D44` | Hover states, borders |
| **Primary** | `#FF6B6B` | Buttons, links, active states (Coral) |
| **Accent** | `#FF2D95` | "AI" highlight, badges, special elements (Hot Pink) |
| **Text Primary** | `#FFFFFF` | Headlines, important text |
| **Text Secondary** | `#A0A0B0` | Descriptions, metadata |
| **Border** | `#2D2D44` | Dividers, card borders |
| **Success** | `#4ADE80` | Success states |
| **Error** | `#F87171` | Error states |
| **Warning** | `#FBBF24` | Warning states |

### Light Mode

| Role | Hex | Usage |
|------|-----|-------|
| **Background** | `#FAFAFA` | Main app background |
| **Surface** | `#FFFFFF` | Cards, sidebar, player bar |
| **Surface Hover** | `#F0F0F5` | Hover states |
| **Primary** | `#E85555` | Buttons, links (darker coral for contrast) |
| **Accent** | `#D91A7A` | "AI" highlight (darker pink for contrast) |
| **Text Primary** | `#1A1A2E` | Headlines, important text |
| **Text Secondary** | `#6B6B80` | Descriptions, metadata |
| **Border** | `#E5E5EA` | Dividers, card borders |
| **Success** | `#22C55E` | Success states |
| **Error** | `#DC2626` | Error states |
| **Warning** | `#D97706` | Warning states |

---

## Logo Treatment

**Dark mode:** "R**AI**MPAGE" — White text, "AI" in Hot Pink `#FF2D95`

**Light mode:** "R**AI**MPAGE" — Dark slate `#1A1A2E` text, "AI" in Coral `#E85555`

---

## Typography

### Font Stack

| Role | Font | Weight | Usage |
|------|------|--------|-------|
| **Display** | Syne | 700, 800 | Logo, hero headlines, section titles |
| **UI** | Plus Jakarta Sans | 400, 500, 600, 700 | Body text, buttons, labels, navigation |

### Import (Google Fonts)

```html
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&family=Syne:wght@700;800&display=swap" rel="stylesheet">
```

### Scale

| Name | Size | Line Height | Usage |
|------|------|-------------|-------|
| **Display XL** | 48px / 3rem | 1.1 | Hero headlines |
| **Display** | 36px / 2.25rem | 1.2 | Page titles |
| **Heading 1** | 28px / 1.75rem | 1.3 | Section headers |
| **Heading 2** | 22px / 1.375rem | 1.4 | Card titles |
| **Heading 3** | 18px / 1.125rem | 1.4 | Subsections |
| **Body** | 16px / 1rem | 1.5 | Default text |
| **Body Small** | 14px / 0.875rem | 1.5 | Secondary text |
| **Caption** | 12px / 0.75rem | 1.4 | Metadata, timestamps |

---

## Components

### Buttons

**Primary Button**
- Background: Primary color (`#FF6B6B` dark / `#E85555` light)
- Text: White
- Border radius: 8px
- Padding: 12px 24px
- Hover: Slightly darker, subtle scale transform

**Secondary Button**
- Background: Surface color
- Border: 1px solid Border color
- Text: Text Primary
- Hover: Surface Hover background

**Ghost Button**
- Background: Transparent
- Text: Primary color
- Hover: Surface background

### Cards

**Song Card**
- Background: Surface
- Border radius: 12px
- Padding: 12px
- Hover: Subtle lift (transform + shadow)
- Cover art: 1:1 aspect ratio, 8px border radius

**Creator Card**
- Background: Surface
- Border radius: 16px
- Avatar: Circular, border in Primary color
- Hover: Border glow effect

### Player Bar

- Background: Surface with slight blur (backdrop-filter)
- Fixed to bottom
- Height: 72px (desktop), 64px (mobile)
- Progress bar: Primary color
- Controls: White icons, Primary on hover

### Navigation

**Sidebar (Desktop)**
- Width: 240px
- Background: Surface
- Active link: Primary color text, subtle Primary background tint
- Icons: 20px, match text color

**Tab Bar (Mobile)**
- Background: Surface
- Active: Primary color icon + label
- Inactive: Text Secondary

---

## Spacing

Use 4px base unit:

| Name | Size |
|------|------|
| xs | 4px |
| sm | 8px |
| md | 16px |
| lg | 24px |
| xl | 32px |
| 2xl | 48px |
| 3xl | 64px |

---

## Border Radius

| Name | Size | Usage |
|------|------|-------|
| sm | 4px | Small elements, badges |
| md | 8px | Buttons, inputs |
| lg | 12px | Cards |
| xl | 16px | Large cards, modals |
| full | 9999px | Avatars, pills |

---

## Shadows (Dark Mode)

```css
--shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.3);
--shadow-md: 0 4px 6px rgba(0, 0, 0, 0.4);
--shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.5);
--shadow-glow: 0 0 20px rgba(255, 107, 107, 0.3); /* Primary glow */
```

## Shadows (Light Mode)

```css
--shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
--shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
--shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.15);
--shadow-glow: 0 0 20px rgba(232, 85, 85, 0.2); /* Primary glow */
```

---

## Animations

**Transitions**
- Default: 150ms ease
- Hover effects: 200ms ease-out
- Page transitions: 300ms ease-in-out

**Micro-interactions**
- Button press: scale(0.98)
- Card hover: translateY(-2px) + shadow increase
- Play button: pulse animation on active

---

## Tailwind CSS Configuration

```javascript
// tailwind.config.js
module.exports = {
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        background: {
          DEFAULT: '#FAFAFA',
          dark: '#0F0F1A',
        },
        surface: {
          DEFAULT: '#FFFFFF',
          dark: '#1A1A2E',
          hover: '#F0F0F5',
          'hover-dark': '#2D2D44',
        },
        primary: {
          DEFAULT: '#E85555',
          dark: '#FF6B6B',
        },
        accent: {
          DEFAULT: '#D91A7A',
          dark: '#FF2D95',
        },
        border: {
          DEFAULT: '#E5E5EA',
          dark: '#2D2D44',
        },
      },
      fontFamily: {
        display: ['Syne', 'sans-serif'],
        sans: ['Plus Jakarta Sans', 'sans-serif'],
      },
    },
  },
};
```

---

## Implementation Notes

1. **Theme Toggle**: Store preference in localStorage, respect system preference by default
2. **Logo**: Use SVG with CSS variables for color switching
3. **Gradients**: Coral-to-Pink gradient can be used for hero sections and special CTAs
4. **Accessibility**: Ensure 4.5:1 contrast ratio for text, focus states visible
5. **Motion**: Respect `prefers-reduced-motion` for animations
