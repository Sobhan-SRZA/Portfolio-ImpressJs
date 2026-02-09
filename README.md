# 🎨 Portfolio-ImpressJs Documentation

**Portfolio-ImpressJs** is a personal portfolio project built using **Impress.js**, a JavaScript library for creating dynamic, 3D-style presentations inspired by Prezi. This project demonstrates the power of CSS3 transforms, transitions, and interactive step-based navigation.

---

## 📦 Project Overview

**Project Name:** Portfolio-ImpressJs
**Technology Stack:**

* HTML5 & CSS3
* Vanilla JavaScript
* Impress.js v1.1.0

**License:** MIT
**Author:** Sobhan-SRZA

**Purpose:**

* Showcase a modern, interactive 3D portfolio.
* Demonstrate the capabilities of Impress.js for animated presentations.
* Enable smooth step transitions and swipe interactions.

---

## 🗂 Source Code Structure

```
Portfolio-ImpressJs/
│
├─ index.html               # Main entry file of the portfolio
├─ js/
│   ├─ impress.js           # Core Impress.js library (customized for portfolio)
│   └─ main.js              # Custom scripts and initialization
├─ css/
│   ├─ style.css            # Main CSS for portfolio styling
│   └─ impress-style.css    # Optional: step-specific 3D CSS
├─ assets/
│   ├─ images/              # Images for portfolio projects
│   └─ icons/               # Social icons, logos
└─ README.md                # Project documentation (this file)
```

---

## 🚀 Core Features

### ✨ Presentation Features

* 3D Step-based slides with dynamic transitions.
* Support for **rotate**, **translate**, and **scale** transforms.
* Mobile-friendly with viewport scaling and swipe gestures.
* Keyboard navigation (`next`, `prev`) and hash-based step navigation.
* Step classes for CSS animations:

  * `.future` → upcoming step
  * `.present` → active step
  * `.past` → completed step

### 🔧 Customization Options

* Configurable width, height, max/min scale, and perspective.
* Dynamic initialization of steps from HTML `data-*` attributes.
* Pre-init and pre-step-leave plugin hooks for advanced behavior.

### 📱 Interaction

* Swipe gestures for touch devices.
* Keyboard support (`ArrowLeft`, `ArrowRight`).
* Hash-based URL updates for deep-linking to steps.

---

## 📝 Key Functions & API

### ⚙️ Initialization

```javascript
const impressApi = impress('impress-root');
impressApi.init();
```

* Initializes the presentation.
* Sets viewport, canvas, and step styles.
* Fires `impress:init` event.

---

### ⏩ Navigation

#### `goto(step, duration, reason, origEvent)`

* Moves to a specific step.
* `step`: index, id, or DOM element.
* `duration`: optional transition duration (ms).
* `reason`: "next", "prev", or "goto".
* `origEvent`: optional triggering event object.
* Returns the targeted step element.

#### `next(origEvent)` / `prev(origEvent)`

* Navigate to the next or previous step.
* Automatically loops if at the first/last step.

#### `swipe(pct)`

* Animates a swipe between current and next/prev step.
* `pct`: -1.0 → next, +1.0 → prev.

#### `tear()`

* Cleans up the Impress.js instance.
* Removes steps, event listeners, and DOM changes.

---

### 🛠 Step Helpers

#### `getStep(step)`

* Retrieves a step element by index, id, or element reference.
* Returns `null` if step is invalid.

#### `initStep(el, idx)`

* Reads step data from HTML `data-*` attributes:

  * `data-x`, `data-y`, `data-z` → position
  * `data-rotate`, `data-rotateX`, `data-rotateY`, `data-rotateZ` → rotation
  * `data-scale` → zoom factor
* Applies computed CSS transforms.

#### `initAllSteps()`

* Initializes all `.step` elements in the root.
* Populates `stepsData` object for quick access.

---

### 📦 Config & Plugins

#### `impress.getConfig()`

* Returns current configuration:

```json
{
  "width": 1920,
  "height": 1080,
  "maxScale": 3,
  "minScale": 0,
  "perspective": 1000,
  "transitionDuration": 1000
}
```

#### Plugins

* `addPreInitPlugin(plugin, weight)` → called before `init()`.
* `addPreStepLeavePlugin(plugin, weight)` → called before `goto()`.

**Plugin Event Example:**

```javascript
impress.addPreStepLeavePlugin((event) => {
    console.log('Leaving step', event.target.id);
}, 10);
```

---

## 🧩 Data Structure

### Steps Data

* Stored internally in `stepsData` object.

```json
{
  "impress-step-step-1": {
    "translate": {"x":0, "y":0, "z":0},
    "rotate": {"x":0, "y":0, "z":0, "order":"xyz"},
    "scale": 1,
    "transitionDuration": 1000,
    "el": "<DOMElement>"
  }
}
```

### Root & Current State

```json
{
  "activeStep": "<DOMElement>",
  "currentState": {
    "translate": {"x":0,"y":0,"z":0},
    "rotate": {"x":0,"y":0,"z":0,"order":"xyz"},
    "scale":1
  },
  "windowScale": 1
}
```

### DOM Structure Example

```html
<div id="impress-root">
    <div class="step" data-x="0" data-y="0" data-scale="1">Welcome</div>
    <div class="step" data-x="1000" data-y="0" data-rotate="90">About Me</div>
    <div class="step" data-x="2000" data-y="0" data-rotate="180">Projects</div>
</div>
```

---

## 💡 Usage Example

```html
<body>
  <div id="impress-root">
      <div class="step" data-x="0" data-y="0">Intro</div>
      <div class="step" data-x="1000" data-y="0" data-scale="2">Work</div>
      <div class="step" data-x="2000" data-y="0" data-rotate="90">Contact</div>
  </div>
  <script src="js/impress.js"></script>
  <script>
    const api = impress('impress-root');
    api.init();
    document.addEventListener('keydown', (e) => {
        if (e.key === 'ArrowRight') api.next();
        if (e.key === 'ArrowLeft') api.prev();
    });
  </script>
</body>
```

---

## 🎨 Styling Tips

* Use `.future`, `.present`, `.past` classes for animations.
* Combine `transform`, `opacity`, and `transition` for cool effects.
* Example:

```css
.step.present {
    opacity: 1;
    transition: all 1s ease-in-out;
}
.step.future {
    opacity: 0.3;
}
.step.past {
    opacity: 0.5;
}
```