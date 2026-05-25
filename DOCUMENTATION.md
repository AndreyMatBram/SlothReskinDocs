# SlothReSkin — Complete Documentation

> A Chrome extension that replaces Roll20's default character sheet with your own custom HTML sheet.

---

## Table of Contents

- [1. What is SlothReSkin?](#1-what-is-slothreskin)
- [2. Installing the extension](#2-installing-the-extension)
- [3. Using the popup](#3-using-the-popup)
- [4. Loading a template](#4-loading-a-template)
- [5. Creating your own template — Creator's guide](#5-creating-your-own-template--creators-guide)
  - [5.1 The `config.json` file](#51-the-configjson-file)
  - [5.2 The template HTML file](#52-the-template-html-file)
  - [5.3 Special `data-sf-*` attributes](#53-special-data-sf--attributes)
  - [5.4 Freetext sections (`freetext`)](#54-freetext-sections-freetext)
  - [5.5 Dynamic lists (`data-sf-list`)](#55-dynamic-lists-data-sf-list)
  - [5.6 Tabs and panels](#56-tabs-and-panels)
  - [5.7 Ability buttons (dice rolls)](#57-ability-buttons-dice-rolls)
  - [5.8 Images and avatar](#58-images-and-avatar)
  - [5.9 Inline field editing](#59-inline-field-editing)
  - [5.10 HP bars (`data-sf-hpbar`)](#510-hp-bars-data-sf-hpbar)
- [6. Complete `data-sf-*` attribute reference](#6-complete-data-sf--attribute-reference)
- [7. `config.json` reference](#7-configjson-reference)
- [8. How data is saved (architecture)](#8-how-data-is-saved-architecture)
- [9. FAQ](#9-faq)

---

## 1. What is SlothReSkin?

Roll20 is an online tabletop RPG platform. Its default character sheets don't always suit every GM or player. SlothReSkin is a Google Chrome extension that **visually overlays** Roll20's default sheet with your own HTML sheet, without touching anything on Roll20's servers.

### How it works in practice

1. You open a Roll20 session normally.
2. When you open a character sheet, the extension **hides the original sheet** and displays **your custom sheet** in its place.
3. All data (attributes, values, text) continues to be read from and saved directly to Roll20 — the extension only changes the appearance.
4. Dice rolls (abilities) keep working normally, because the extension uses Roll20's own rolling system behind the scenes.

### What the extension does NOT do

- Does not send any data to external servers.
- Does not attempt to hack or circumvent Roll20's API.
- Does not share your sheet with other players (everyone needs the extension installed).

---

## 2. Installing the extension

SlothReSkin is available in the official stores for major Chromium-based browsers:

| Browser | Store |
|---|---|
| Google Chrome | Chrome Web Store |
| Microsoft Edge | Edge Add-ons |
| Brave / Opera / Vivaldi | Chrome Web Store (compatible) |

### Step by step

1. Go to the SlothReSkin page in your browser's store and click **"Add to Chrome"** (or "Add to Edge").
2. Confirm the installation in the permissions window. The extension only requests access to `roll20.net`.
3. After installing, the SlothReSkin icon (the orange sloth) will appear in the browser toolbar.
4. If the icon doesn't appear automatically, click the puzzle piece icon (🧩) in the Chrome/Edge toolbar and pin SlothReSkin by clicking the pin next to its name.

---

## 3. Using the popup

Click the SlothReSkin icon in the Chrome toolbar to open the extension menu.

```
┌─────────────────────────────────────┐
│  SlothReSkin              v0.1 ● ON │
├─────────────────────────────────────┤
│   ⚠ Reload the Roll20 page to       │
│       apply changes                 │
├─────────────────────────────────────┤
│  TEMPLATE                           │
│  ✓ my-sheet.html                    │
│  [  Load HTML  ]                    │
│  [  Load CSS   ]                    │
├─────────────────────────────────────┤
│  FIELD CONFIGURATION                │
│  ✓ MySystem                         │
│  [  Load config.json  ]             │
├─────────────────────────────────────┤
│  [        Clear all        ]        │
├─────────────────────────────────────┤
│  □ Auto-refresh on change           │
└─────────────────────────────────────┘
```

### Toggle ON/OFF

- The **ON/OFF** button in the top-right corner enables or disables the extension completely.
- **Green (ON):** the extension is active and will replace sheets when Roll20 is opened.
- **Gray (OFF):** the extension is inactive; Roll20 works normally.
- Any change to the toggle requires **reloading the Roll20 page** to take effect.

### Reload warning

Whenever you change any setting (load a new template, swap the config, toggle on/off), an orange warning appears at the top of the popup asking you to reload the page. This is necessary because the extension injects the template when the page loads.

### Load HTML

Click **"Load HTML"** and select your template's `.html` file. This file defines the appearance of the sheet.

### Load CSS (optional)

If you want to separate visual styles from the HTML, click **"Load CSS"** and select a `.css` file. Styles will be applied alongside the HTML.

### Load config.json

Click **"Load config.json"** and select the configuration file for your RPG system. This file tells the extension which Roll20 fields correspond to the tags in your template.

### Clear all

The red **"Clear all"** button removes the template and configuration from the extension's storage. After clearing, sheets will show the default Roll20 sheet again (after reloading the page).

### Auto-refresh on change

When this checkbox is checked, every time you change any setting in the popup, Chrome will automatically reload the active Roll20 tab — no need to do it manually.

---

## 4. Loading a template

To use SlothReSkin you need two files: an **HTML template** (the sheet's appearance) and a **config.json** (the fields for your RPG system). These files can come from different sources:

### Where to find templates

- **SlothForge template store** — ready-made templates, with support, for the most popular RPG systems.
- **Community** — independent creators who share their templates for free or paid on RPG sites and forums.
- **Self-made** — you can create a template from scratch by following the [Creator's guide](#5-creating-your-own-template--creators-guide) in section 5.

### How to use

Regardless of the template's origin, the process is always the same:

1. Open the extension popup by clicking the SlothReSkin icon in the browser toolbar.
2. Click **"Load HTML"** and select the template's `.html` file.
3. Click **"Load config.json"** and select the corresponding `.json` file.
4. Reload the Roll20 page (F5) — the orange warning in the popup will remind you.
5. Enter a campaign and open a character sheet.

> **Note:** the `config.json` must correspond to the same system as the HTML template. Mixing files from different systems will cause fields to appear wrong or empty.

---

## 5. Creating your own template — Creator's guide

This section assumes prior knowledge of HTML, CSS, and JSON. There is no introduction to web concepts — the focus is to describe what the extension provides and how to integrate it into your HTML.

> **Generating templates with AI:** models like Claude, ChatGPT, and Gemini can create compatible templates when given this documentation as context. Pass the full section 5 (and your system's `config.json`) as context — the AI will understand the `data-sf-*` attributes, the config format, and the technical constraints (such as the tabs rule).

---

### 5.1 The `config.json` file

The `config.json` is the "dictionary" that connects the **tags** in your HTML to the **field names** in Roll20.

#### Full structure

```json
{
  "system_name": "YourSystemName",

  "fields": [
    { "roll20": "HP",       "tag": "hp"  },
    { "roll20": "Strength", "tag": "str" },
    { "roll20": "Dexterity","tag": "dex" }
  ],

  "freetext": ["skills", "inventory", "notes"],

  "abilities": [
    { "roll20": "Attack", "tag": "attack" },
    { "roll20": "Defense","tag": "defense"}
  ]
}
```

#### Field explanations

| Property | Required | Description |
|---|---|---|
| `system_name` | Yes | Name displayed in the popup when loading the config |
| `fields` | Yes | List of numeric/text Roll20 fields |
| `fields[].roll20` | Yes | **Exact name** of the attribute as it appears in Roll20 (character sheet attributes tab) |
| `fields[].tag` | Yes | Short tag you will use in the HTML (no spaces, no accents) |
| `freetext` | No | List of freetext sections (dynamic lists or text boxes) |
| `abilities` | No | List of abilities (macros) that can be rolled via buttons |
| `abilities[].roll20` | Yes | **Exact name** of the ability as it appears in Roll20's abilities tab |
| `abilities[].tag` | Yes | Tag you will use in the `data-sf-ability` attribute in the HTML |

> **Tip:** To find the exact name of a field in Roll20, open the default sheet, go to the "Attributes & Abilities" tab, and copy the field name exactly as it appears, including uppercase and special characters.

---

### 5.2 The template HTML file

The template is a normal HTML file with a few special conventions:

- **CSS styles** go inside a `<style>` tag in the `<head>`.
- **Content** goes inside the `<body>`.
- Roll20 values are inserted using **special tags** in the HTML.

#### Minimal template example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <style>
    body {
      background: #111;
      color: #eee;
      font-family: sans-serif;
      padding: 12px;
    }
    h1 { color: #ff6600; }
  </style>
</head>
<body>

  <h1 data-sf="_name">{{_name}}</h1>
  <p>HP: <strong data-sf="hp">{{hp}}</strong></p>
  <p>Strength: <strong data-sf="str">{{str}}</strong></p>

</body>
</html>
```

---

### 5.3 Special `data-sf-*` attributes

These are the HTML attributes the extension recognizes to display and sync data.

#### Displaying a value (read-only)

```html
<!-- Displays the current value of "hp" -->
<span data-sf="hp">{{hp}}</span>

<!-- Displays the max value of "hp" -->
<span data-sf-max="hp">{{hp_max}}</span>

<!-- Displays the character name (built-in field) -->
<span data-sf="_name">{{_name}}</span>
```

> The text between `{{` and `}}` is the initial value rendered when the sheet opens. The `data-sf` attribute keeps the value in sync when Roll20 updates the field in real time.

#### Built-in fields (no need to declare in config.json)

The three fields below exist automatically for every character and do not need to be declared in `config.json`.

---

##### `_name` — Character name

Synchronous read. Available immediately when the sheet opens.

```html
<!-- As plain text -->
<span data-sf="_name">{{_name}}</span>

<!-- In HTML attributes -->
<img alt="{{_name}}">
<h1>{{_name}}</h1>
```

---

##### `_avatar` — Character avatar

Synchronous read. Returns the profile image URL. **Always use `data-sf-src`** (not a direct `src`) — the extension fetches the image via its service worker to bypass Roll20's security restrictions (CORS/CSP).

```html
<img data-sf-src="_avatar" src="{{_avatar}}" alt="{{_name}}">
```

> Using `src="{{_avatar}}"` alone works on the first render but may fail depending on browser security policy. With `data-sf-src`, the image always loads correctly.

---

##### `_bio` — Character biography

**Asynchronous read.** Roll20 stores the biography as a separate blob — it is not a simple attribute like `_name`. The extension reads the content directly from the Summernote editor inside the character sheet iframe, which takes a moment after the sheet opens.

In practice: the bio appears **shortly after** the rest of the sheet, with no noticeable delay in most cases. If the iframe is still loading, the extension waits up to 3 seconds.

The bio contains **HTML** (Roll20 uses Summernote, which saves with `<p>`, `<strong>`, etc. tags). Use **`data-sf-html`** to display it, not `data-sf`:

```html
<!-- Correct: renders the bio's HTML -->
<div data-sf-html="_bio"></div>

<!-- Wrong: would display raw HTML tags as literal text -->
<div data-sf="_bio"></div>
```

> **Warning:** `data-sf-html` inserts HTML directly into the DOM. Only use it for `_bio` and other fields whose content you control — never for fields editable by unknown players.

---

#### Interpolation with `{{tag}}`

You can use `{{tag}}` anywhere in the HTML to insert a value on first render:

```html
<img src="{{_avatar}}" alt="{{_name}}">
<div class="title">{{_name}} — Level {{level}}</div>
```

---

### 5.4 Freetext sections (`freetext`)

Freetext fields are sections that **do not map to individual Roll20 attributes**. They are all serialized as JSON in a single hidden attribute called `_sf_data`, managed automatically by the extension — the template never accesses this attribute directly.

Declare the sections in `config.json`:

```json
"freetext": ["skills", "inventory", "notes"]
```

Each name becomes a tag accessible in the HTML. There are two possible uses:

| Type | Storage | How to use in HTML |
|---|---|---|
| **Single text** | String | `<textarea data-sf-input="notes">` |
| **Dynamic list** | JSON Array | `<div data-sf-list="skills">` |

The type is not declared in the config — it is determined by what the template uses. If the HTML uses `data-sf-list`, the extension treats the value as an array. If it uses `data-sf-input`, it treats it as a string. Both persist in `_sf_data` and are visible in Roll20's attributes tab as `{"skills":[...],"notes":"..."}`.

---

### 5.5 Dynamic lists (`data-sf-list`)

A dynamic list allows the user to **add, edit and remove items** during the session. Each item is free text (can have multiple lines).

#### HTML structure of a dynamic list

```html
<div data-sf-list="skills">

  <!-- Item template: cloned once per array entry -->
  <template>
    <div>
      <!-- Button that expands/collapses the item -->
      <button data-sf-item-toggle>
        <span data-sf-item-preview></span>  <!-- Auto-filled: 1st line of text -->
      </button>

      <!-- Item body (hidden by default) -->
      <div data-sf-item-body hidden>
        <textarea data-sf-item-input></textarea>   <!-- Edit field -->
        <button data-sf-item-delete>Remove</button>
      </div>
    </div>
  </template>

  <!-- Button to add a new empty item -->
  <button data-sf-list-add>+ Add</button>

</div>
```

#### How it works

1. When the sheet loads, the extension reads the saved array from Roll20 and clones the `<template>` once per item.
2. `[data-sf-item-preview]` is automatically filled with the **first line** of the item's text (max 60 characters).
3. Clicking `[data-sf-item-toggle]` shows/hides `[data-sf-item-body]`.
4. Editing `[data-sf-item-input]` and losing focus (or pressing `Ctrl+Enter`) **auto-saves** to Roll20.
5. Clicking `[data-sf-item-delete]` removes the item and saves the updated array.
6. Clicking `[data-sf-list-add]` adds an empty item and saves.

#### Available attributes inside `<template>`

| Attribute | Element | Function |
|---|---|---|
| `data-sf-item-toggle` | `<button>` | Expand/collapse the item body |
| `data-sf-item-preview` | Any | Preview: first non-empty line (≤ 60 chars) |
| `data-sf-item` | Any | Displays full item text (read-only) |
| `data-sf-item-input` | `<textarea>` | Editable field; auto-saves on blur |
| `data-sf-item-save` | `<button>` | Manually saves the item |
| `data-sf-item-delete` | `<button>` | Removes the item from the list |
| `data-sf-item-body` | Any | Expandable body; starts with `hidden`; toggle opens/closes |

> All attributes inside `<template>` are **optional** — only use the ones your interface needs.

#### Example: inventory grid

```html
<div data-sf-list="inventory" style="display:grid; grid-template-columns: repeat(3, 1fr); gap:8px">
  <template>
    <div style="background:#111; border:1px solid #333; padding:8px; border-radius:4px">
      <span data-sf-item-preview style="font-weight:bold"></span>
      <button data-sf-item-toggle>✎</button>
      <button data-sf-item-delete>✕</button>
      <div data-sf-item-body hidden>
        <textarea data-sf-item-input style="width:100%"></textarea>
      </div>
    </div>
  </template>
  <button data-sf-list-add style="grid-column: 1/-1">+ Add Item</button>
</div>
```

---

### 5.6 Tabs and panels

SlothReSkin's tab system is **100% JavaScript-controlled**, scoped per overlay. This ensures that multiple sheets open at the same time never interfere with each other.

> **⚠ Do not use CSS radio buttons** (`<input type="radio" name="...">`) for tabs. They use global `name` and `id` attributes — with two overlays on the same page, clicking a tab on one sheet deselects the tab on the other. Always use `data-sf-tab` / `data-sf-tabpanel`.

#### Basic example

```html
<!-- 1. Required CSS rule: panels are hidden by default -->
<style>
  [data-sf-tabpanel] { display: none; }

  /* Active tab style (class added automatically) */
  [data-sf-tab].sf-tab-active {
    background: #ff6600;
    color: #000;
  }
</style>

<!-- 2. Tab buttons — any element with data-sf-tab -->
<div class="my-tabs">
  <button data-sf-tab="stats">Stats</button>
  <button data-sf-tab="inv">Inventory</button>
  <button data-sf-tab="notes">Notes</button>
</div>

<!-- 3. Panels — name must match data-sf-tab exactly -->
<div data-sf-tabpanel="stats">
  <!-- stats content -->
</div>

<div data-sf-tabpanel="inv">
  <!-- inventory content -->
</div>

<div data-sf-tabpanel="notes">
  <!-- notes content -->
</div>
```

**How it works:**
- When the overlay is injected, the extension finds all `[data-sf-tab]` elements and activates the **first one** automatically.
- Clicking a tab button: adds `sf-tab-active` to it, removes from the others, and toggles panel `display`.
- Inactive panels get `style="display:none"` and the active one gets `style="display:block"` — this overrides the default CSS rule.

---

#### Nested tabs (sub-tabs)

If you need **two levels of tabs** (e.g. main tabs + sub-tabs inside a panel), use the `data-sf-tabgroup` attribute to create independent groups. Tabs and panels without `data-sf-tabgroup` belong to the default group.

```html
<style>
  [data-sf-tabpanel] { display: none; }

  /* Main active tab */
  [data-sf-tab].sf-tab-active { background: #ff6600; color: #000; }

  /* Active sub-tab */
  .sub-nav [data-sf-tab].sf-tab-active { background: #fff; color: #000; }
</style>

<!-- Main tabs (default group — no data-sf-tabgroup) -->
<div>
  <button data-sf-tab="stats">Stats</button>
  <button data-sf-tab="inv">Inventory</button>
</div>

<div data-sf-tabpanel="stats">

  <!-- Sub-tabs inside the Stats panel (group "sub") -->
  <div class="sub-nav">
    <button data-sf-tab="skills"     data-sf-tabgroup="sub">Skills</button>
    <button data-sf-tab="abilities"  data-sf-tabgroup="sub">Abilities</button>
  </div>

  <div data-sf-tabpanel="skills"    data-sf-tabgroup="sub">
    <!-- skills content -->
  </div>
  <div data-sf-tabpanel="abilities" data-sf-tabgroup="sub">
    <!-- abilities content -->
  </div>

</div>

<div data-sf-tabpanel="inv">
  <!-- inventory content -->
</div>
```

**Rule:** `data-sf-tab` and `data-sf-tabpanel` with the same `data-sf-tabgroup` form an isolated group. Clicking a tab in the `"sub"` group never affects the default group tabs, and vice versa. You can have as many groups as you need — use descriptive names (`"sub"`, `"details"`, `"combat"`, etc.).

---

#### Styling with CSS

The extension only adds/removes the `sf-tab-active` class and changes `style.display` on panels. The visual design is entirely yours:

```css
/* Base tab button */
[data-sf-tab] {
  padding: 8px 16px;
  background: #1a1a1a;
  border: none;
  color: #888;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.15s, color 0.15s;
}

/* Active tab */
[data-sf-tab].sf-tab-active {
  background: #ff6600;
  color: #000;
}

/* Hover */
[data-sf-tab]:hover { color: #eee; }
```

> **Tip:** Use `clip-path`, `border-radius`, `border-bottom`, or any CSS effect to create different tab styles — the extension imposes no visual.

---

### 5.7 Ability buttons (dice rolls)

To create buttons that roll a Roll20 ability in chat, use the `data-sf-ability` attribute:

```html
<!-- Clicking this element triggers the "Attack" ability roll -->
<button data-sf-ability="attack">
  Attack: <span data-sf="attack">{{attack}}</span>
</button>
```

> **Important:** The value in `data-sf-ability` must correspond to the `tag` defined in `abilities[]` in `config.json`, which in turn points to the **exact name** of the ability in Roll20.

When clicked, the element momentarily receives the `sf-rolling` class, which you can use to animate the button:

```css
@keyframes sf-roll-flash {
  0%   { box-shadow: 0 0 0 0   rgba(255, 102, 0, 0.7); }
  100% { box-shadow: 0 0 0 10px rgba(255, 102, 0, 0); }
}
.sf-rolling {
  animation: sf-roll-flash 0.45s ease-out;
}
```

---

### 5.8 Images and avatar

To display the character's avatar, use `data-sf-src` together with `src="{{_avatar}}"`:

```html
<img data-sf-src="_avatar" src="{{_avatar}}" alt="{{_name}}">
```

The `src="{{_avatar}}"` sets the value on first render. The `data-sf-src` makes the extension reprocess the image via its service worker — necessary to bypass Roll20's CORS/CSP restrictions. Use both together.

For images in other fields that store URLs, the same pattern applies:

```html
<img data-sf-src="item_icon" src="{{item_icon}}">
```

> See `_bio` in section 5.3 for displaying the biography (asynchronous HTML).

---

### 5.9 Inline field editing

The extension provides two inline editing mechanisms. Both are **behavioral contracts** — the visual appearance (icons, buttons, layout) is entirely yours.

---

#### `data-sf-click-edit="tag"` — click the display to edit

Placing `data-sf-click-edit` on an element makes clicking it hide it and show a sibling element with the class `.sf-click-input`. On blur or Enter, the display comes back. Escape cancels without saving.

The `.sf-click-input` must also have `data-sf-input="tag"` so the value is saved to Roll20.

```html
<span data-sf="hp" data-sf-click-edit="hp">{{hp}}</span>
<input type="number" class="sf-click-input" data-sf-input="hp"
       value="{{hp}}" style="display:none">
```

---

#### `data-sf-editable` — container with edit mode activated by a trigger

Marking a container with `data-sf-editable` activates a show/hide system between three child elements recognized by class:

| Class | Role |
|---|---|
| `.sf-edit-btn` | Trigger — clicking opens edit mode |
| `.val-display` | Shown in read mode; hidden during editing |
| `.val-input` | Shown during editing (`<input>`, `<textarea>`, or a `<div>` containing inputs) |

Clicking `.sf-edit-btn`: `.val-display` hides, `.val-input` appears and receives focus. On focusout of `.val-input` (or Enter): `.val-input` hides, `.val-display` returns. Escape cancels.

```html
<div data-sf-editable>
  <button class="sf-edit-btn" type="button">Edit</button>
  <span class="val-display" data-sf="hp">{{hp}}</span>
  <input type="number" class="val-input" data-sf-input="hp"
         value="{{hp}}" style="display:none">
</div>
```

To edit the **maximum** value of a field, use `data-sf-input-max` on the input inside `.val-input`:

```html
<input type="number" class="val-input" data-sf-input-max="hp"
       value="{{hp_max}}" style="display:none">
```

> `.val-input` can be a `<div>` containing multiple inputs (e.g. current and max together). In that case, focusout closes edit mode only when focus leaves the wrapper entirely.

---

### 5.10 HP bars (`data-sf-hpbar`)

Create visual progress bars using the CSS custom property generated by the extension:

```html
<!-- The extension will calculate --sf-hp-percent based on current/max -->
<div data-sf-hpbar="hp"
     style="width:100%; height:8px; background:#333; border-radius:4px; overflow:hidden">
  <div style="height:100%; background:red;
              width: var(--sf-hp-percent, 100%);
              transition: width 0.5s ease;"></div>
</div>
```

> For this to work, the field needs a **max value** defined in Roll20 (the "max" column in the attributes tab).

---

## 6. Complete `data-sf-*` attribute reference

| Attribute | Element | Description |
|---|---|---|
| `data-sf="tag"` | Any | Displays `current` of the field; updates in real time |
| `data-sf-max="tag"` | Any | Displays `max` of the field; updates in real time |
| `data-sf-html="tag"` | Any | Inserts the field's HTML content (only use for trusted content) |
| `data-sf-src="tag"` | `<img>` | Sets the image `src` with the field's value |
| `data-sf-input="tag"` | `<input>`, `<textarea>` | Editable field; saves `current` to Roll20 on change |
| `data-sf-input-max="tag"` | `<input>` | Editable field; saves `max` to Roll20 on change |
| `data-sf-click-edit="tag"` | `<span>` or similar | Clicking the element shows a sibling `.sf-click-input` to edit `current` inline |
| `data-sf-editable` | Container | Inline edit mode; recognizes `.sf-edit-btn` (trigger), `.val-display` (read), `.val-input` (edit) |
| `data-sf-tab="name"` | `<button>` | Tab button; activates the panel with the same name in the same group |
| `data-sf-tabgroup="group"` | `<button>` or Container | Isolates tabs into independent groups (see section 5.6) |
| `data-sf-tabpanel="name"` | Container | Tab panel; shown/hidden by the tab system |
| `data-sf-ability="tag"` | Any clickable | Rolls the Roll20 ability on click |
| `data-sf-hpbar="tag"` | Container | Calculates `current/max` and defines CSS variables `--sf-{tag}-ratio`, `--sf-{tag}-percent`, `--sf-bar-ratio`, `--sf-bar-percent` |
| `data-sf-list="section"` | Container | Dynamic list of items for the freetext section |
| `data-sf-list-add` | `<button>` | Adds an empty item to the list |
| `data-sf-item-toggle` | `<button>` inside `<template>` | Expand/collapse item; receives `aria-expanded` |
| `data-sf-item-preview` | Any inside `<template>` | Preview: first non-empty line (≤ 60 chars) |
| `data-sf-item` | Any inside `<template>` | Full item text (read-only) |
| `data-sf-item-input` | `<textarea>` inside `<template>` | Edit field; auto-saves on blur or `Ctrl+Enter` |
| `data-sf-item-save` | `<button>` inside `<template>` | Manually saves the item |
| `data-sf-item-delete` | `<button>` inside `<template>` | Removes the item from the list |
| `data-sf-item-body` | Container inside `<template>` | Expandable item body; starts with `hidden` |

---

## 7. `config.json` reference

```jsonc
{
  // System name (shown in the popup when loading)
  "system_name": "SystemName",

  // Fields that map Roll20 attributes to tags used in the HTML
  "fields": [
    {
      "roll20": "Exact name in Roll20",   // Case-sensitive!
      "tag": "tag_in_html"               // No spaces, no accents
    }
  ],

  // Freetext sections (dynamic arrays or text blocks)
  // Each name becomes a section accessible as data-sf-list="name" or data-sf-input="name"
  "freetext": ["skills", "inventory", "notes"],

  // Abilities (macros) that can be triggered via data-sf-ability
  "abilities": [
    {
      "roll20": "Exact ability name in Roll20",
      "tag": "tag_in_html"
    }
  ]
}
```

---

## 8. How data is saved (architecture)

This section is for those curious about how the extension works internally.

### Extension layers

```
Chrome Extension
├── popup/           → Popup interface (HTML + CSS + JS)
├── background/      → Service Worker (image proxy)
└── content/         → Scripts injected into Roll20
    ├── roll20_bridge.js   → Runs in the page's MAIN context (access to Roll20's JS)
    ├── mapper.js          → Attribute read/write functions
    ├── injector.js        → Injects and maintains the sheet overlay
    └── main.js            → Coordinates everything; watches character dialogs
```

### How reading works

1. When a character sheet is opened in Roll20, `main.js` detects the `.characterdialog` element.
2. `main.js` asks `roll20_bridge.js` (via `CustomEvent`) for all attributes of that character.
3. `roll20_bridge.js` accesses Roll20's Backbone.js objects (`Campaign.characters`) directly and returns the data.
4. `injector.js` uses the data to render the HTML template and insert it over the original sheet.

### How writing works

1. The user edits a field in the overlay (types in a `data-sf-input`, edits a list item, etc.).
2. `injector.js` fires an `sf-write-request` event with the field name and new value.
3. `roll20_bridge.js` receives the event and calls `attr.save({ current: value })` on Roll20's Backbone object.
4. Roll20 saves automatically to its servers, exactly as it would with the default sheet.

### Freetext storage — complete pipeline (`_sf_data`)

This is the most complex part of the extension. Understanding the full pipeline helps with debugging and extending the system.

#### Why a single attribute?

Roll20 imposes no practical limit on attributes per character, but creating one attribute per list item (e.g. `inventory_0`, `inventory_1`, ...) would have serious problems: unpredictable IDs, conflicts with other sheet systems, and no way to know how many items exist without scanning all attributes. The approach adopted is to store everything as **serialized JSON** in a single hidden attribute:

```json
// Value of the "_sf_data" attribute in Roll20:
{
  "skills":    ["Archery 15/15", "Track 10/15"],
  "inventory": ["Longsword", "3x Healing Potion"],
  "notes":     "We found the ancient temple of Zorath..."
}
```

Arrays and strings coexist in the same blob. The type (array or string) is decided by the template, not the config.

#### Reading on sheet open

When `main.js` detects a `.characterdialog` and calls `readAttributesForCharacter(characterId)`:

1. **`roll20_bridge.js` / `readAttribs()`** scans all `char.attribs` normally.
2. At the end, calls `readFreetextData(char)` → does `JSON.parse` on the `_sf_data` value (fallback `{}`).
3. For each key in the JSON, adds a `_ft_<key>` entry to the attribute map:
   ```js
   // Array → serialized back to JSON string (injector will parse it later)
   attribs['_ft_skills']    = { current: '["Archery 15/15","Track 10/15"]', max: '' };
   // String → passed through directly
   attribs['_ft_notes']     = { current: 'We found the temple...', max: '' };
   ```
4. The raw `_sf_data` attribute is **not** included in the returned map — the injector never sees it directly.

#### Rendering in the injector

In `injector.js / buildTagMap()`, each section declared in `config.freetext` generates a mapping:
```js
map['skills'] = '_ft_skills';
map['notes']  = '_ft_notes';
```

This makes freetext sections transparent to the template: `data-sf-input="notes"` works exactly like any other field — the `tagMap` resolves `notes` → `_ft_notes` → current value.

For dynamic lists, `bindLists()` scans all `[data-sf-list]`, calls `parseListItems(raw)` which does `JSON.parse` on `_ft_<section>` and then `renderList()` clones the inner `<template>` once per item.

#### Writing an edited item

When the user edits an item (blur on `[data-sf-item-input]` or clicks save):

1. `sfListSave(container, index, newValue, characterId, sectionKey)` clones `container._sfItems`, replaces the index, and calls:
   ```js
   writeAttributeForCharacter(characterId, '_ft_skills', JSON.stringify(newArray));
   ```
2. `mapper.js / writeAttributeForCharacter()` fires `sf-write-request` to the MAIN world.
3. `roll20_bridge.js / writeAttrib()` detects the `_ft_` prefix and redirects to `writeFreetext(char, 'skills', newJSON)`.
4. `writeFreetext()` reads the current `_sf_data` blob, replaces the `skills` key with the new value, and saves:
   ```js
   sfAttr.save({ current: JSON.stringify(data) });
   // or, if _sf_data doesn't exist yet:
   char.attribs.create({ name: '_sf_data', current: JSON.stringify(data) });
   ```

#### Real-time update after saving

Roll20's Backbone.js emits a `change` event on `_sf_data` after `attr.save`. `listenToChar()` intercepts it:

```js
if (name === '_sf_data') {
  // Explode the blob into individual events per section
  for (const [key, val] of Object.entries(data)) {
    window.dispatchEvent(new CustomEvent('sf-attrib-changed', {
      detail: { characterId, name: '_ft_' + key, current: String(val), max: '' }
    }));
  }
  return; // Don't forward the raw _sf_data blob
}
```

On the isolated side, `main.js / onAttribChanged()` receives `name = '_ft_skills'`, detects the `_ft_` prefix and calls `rerenderList()` to update only the affected list — without re-rendering the entire sheet.

#### Summary diagram

```
User edits item
      │
      ▼
sfListSave() [injector.js]
      │  writeAttributeForCharacter('_ft_skills', '[...]')
      ▼
sf-write-request (CustomEvent → MAIN world)
      │
      ▼
writeAttrib() [roll20_bridge.js]
      │  detects _ft_ → writeFreetext()
      ▼
_sf_data.save({ current: '{"skills":[...],...}' })
      │
      ▼  (Backbone 'change' event)
listenToChar() [roll20_bridge.js]
      │  explodes → sf-attrib-changed { name: '_ft_skills' }
      ▼
onAttribChanged() [main.js]
      │  detects _ft_ → rerenderList()
      ▼
renderList() [injector.js]  ← visual list updated
```

### Where the template is stored

The HTML template and JSON configuration are stored in **Chrome's local storage** (`chrome.storage.local`), in your browser. They are **not sent to Roll20** or to any external server.

---

## 9. FAQ

**Q: Will other players at the same table see my custom sheet?**

A: No. The extension only works in your browser. Every player who wants to use a custom sheet needs to install the extension and load the template themselves.

---

**Q: Will my custom sheet overwrite the character's data in Roll20?**

A: The extension reads and **writes** data to Roll20. Any field you edit in the ReSkin sheet (attributes, freetext, inventory) is saved back to the character's attributes in Roll20 — exactly as if you had edited it in the default sheet. If you turn off the extension, the data will still be there, visible in the original sheet.

---

**Q: Why do I need to reload the page after changing the template?**

A: The extension injects the template when the Roll20 page finishes loading. If you swap the template afterwards, the old version was already injected. Reloading (F5) ensures the new template and configuration are used from scratch.

---

**Q: Can I use the same template for different systems?**

A: Yes! The HTML template defines the appearance; the `config.json` defines the fields. You can have a generic HTML and just swap the `config.json` to adapt to another system (as long as the Roll20 fields match).

---

**Q: Do dice rolls work?**

A: Yes. Clicking an element with `data-sf-ability` makes the extension locate the corresponding ability in Roll20 and simulate a click on Roll20's original roll button, which then fires the roll in chat normally.

---

**Q: Can I use external CSS or Google Fonts?**

A: External fonts like Google Fonts work normally in your HTML's `<style>` via `@import url(...)`. For separate CSS files, use the **"Load CSS"** button in the popup.

---

**Q: Does the extension work on sites other than Roll20?**

A: No. It was built specifically for `*.roll20.net` and depends on Roll20's internal structures (Backbone objects, specific attributes, etc.).

---

**Q: How do I find the exact name of a field in Roll20?**

A: Open a character sheet in Roll20, go to the **"Attributes & Abilities"** tab and you will see the list of attributes with their exact names. Copy the name exactly as it appears in the "ATTRIBUTE NAME" column.

---

---

If SlothReSkin saved you time, consider supporting development:

[![Buy Me a Coffee](https://buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://buymeacoffee.com/SlothForge)

[buymeacoffee.com/SlothForge](https://buymeacoffee.com/SlothForge)

---

*SlothReSkin v0.1 — developed by SlothForge.*
