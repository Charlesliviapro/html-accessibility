# Web Accessibility Notes

## Table of Contents
1. [What is Web Accessibility?](#what-is-web-accessibility)
2. [Alt Text Best Practices](#alt-text-best-practices)
3. [Skip-to-Content Links](#skip-to-content-links)
4. [ARIA Roles and Attributes](#aria-roles-and-attributes)
5. [Keyboard Navigation](#keyboard-navigation)
6. [Focus States](#focus-states)
7. [Additional Best Practices](#additional-best-practices)
8. [Testing Tools](#testing-tools)

---

## What is Web Accessibility?

Web accessibility means that websites, tools, and technologies are designed and developed so that people with disabilities can use them. This includes people with:
- Visual impairments (blindness, low vision, color blindness)
- Auditory impairments (deafness, hard of hearing)
- Motor impairments (difficulty using a mouse, limited mobility)
- Cognitive impairments (learning disabilities, distractibility)

Following accessibility guidelines helps **everyone**, not just users with disabilities. It improves usability, SEO, and overall user experience.

---

## Alt Text Best Practices

Alt text (alternative text) provides a textual alternative for images, making visual content accessible to screen reader users and serving as a fallback when images fail to load.

### Guidelines for Writing Alt Text:

1. **Be Descriptive and Concise**
   - Describe the content and function of the image
   - Keep it under 125 characters when possible
   - Example: `alt="Red apple on a wooden table"`

2. **Provide Context**
   - Consider why the image is there and what information it conveys
   - Bad: `alt="image123.jpg"`
   - Good: `alt="Bar chart showing 30% increase in sales during Q4"`

3. **Decorative Images**
   - Use empty alt text (`alt=""`) for purely decorative images
   - Or use `role="presentation"` or `aria-hidden="true"`
   - Example: `<img src="decorative-border.png" alt="" role="presentation">`

4. **Functional Images**
   - For images that are links or buttons, describe the action/destination
   - Example: `alt="Search" for a search button icon`
   - Example: `alt="Go to homepage" for a logo link`

5. **Complex Images**
   - For charts, graphs, or diagrams, provide a summary in alt text
   - Use `aria-describedby` to link to longer descriptions
   - Consider providing a text alternative or data table nearby

6. **Don't Use**
   - Don't start with "image of" or "picture of" (screen readers already announce it's an image)
   - Don't include redundant information already in surrounding text
   - Don't use alt text for CSS background images (use ARIA labels if needed)

### Examples:

```html
<!-- Informative image -->
<img src="chart.png" alt="Monthly revenue chart showing steady growth from January to December">

<!-- Decorative image -->
<img src="border-decoration.png" alt="">

<!-- Functional image (button) -->
<button>
  <img src="search-icon.png" alt="Search">
</button>

<!-- Logo as a link -->
<a href="/">
  <img src="logo.png" alt="Company Name - Home">
</a>

<!-- Complex image with long description -->
<img src="complex-diagram.png" alt="System architecture diagram" aria-describedby="diagram-desc">
<div id="diagram-desc">
  Detailed description of the diagram components...
</div>
```

---

## Skip-to-Content Links

Skip-to-content links (also called skip links or skip navigation links) allow keyboard and screen reader users to bypass repetitive navigation and jump directly to the main content.

### Why They're Important:

- Keyboard users don't have to tab through dozens of navigation links on every page
- Screen reader users save time by skipping to relevant content
- Improves efficiency and reduces frustration
- Required by WCAG 2.1 guidelines (Success Criterion 2.4.1)

### Implementation:

```html
<!-- Skip link should be the first focusable element -->
<a href="#main-content" class="skip-link">Skip to main content</a>

<!-- CSS to hide until focused -->
<style>
  .skip-link {
    position: absolute;
    top: -40px;
    left: 0;
    background: #000;
    color: #fff;
    padding: 8px;
    text-decoration: none;
    z-index: 100;
  }
  
  .skip-link:focus {
    top: 0;
  }
</style>

<!-- Main content with matching ID -->
<main id="main-content">
  <!-- Your main content here -->
</main>
```

### Best Practices:

1. Place the skip link as the **first focusable element** on the page
2. Make it **visible when focused** (don't use `display: none` or `visibility: hidden`)
3. Use clear, descriptive text like "Skip to main content"
4. Ensure the target element receives focus (may need `tabindex="-1"` on the target)
5. Style it so it stands out when visible
6. Consider multiple skip links for complex pages (skip to navigation, skip to search, etc.)

---

## ARIA Roles and Attributes

ARIA (Accessible Rich Internet Applications) provides a way to make web content and applications more accessible by adding semantic information that assistive technologies can understand.

### Key Principle: **Use Semantic HTML First**

Before using ARIA, ask yourself: "Can I use a native HTML element instead?" Semantic HTML elements have built-in accessibility support.

- Use `<nav>` instead of `<div role="navigation">`
- Use `<button>` instead of `<div role="button">`
- Use `<main>` instead of `<div role="main">`

### Common ARIA Roles:

#### Landmark Roles
Landmarks help users navigate and understand page structure:

- `role="banner"` - Site header (usually `<header>`)
- `role="navigation"` - Navigation sections (usually `<nav>`)
- `role="main"` - Main content area (usually `<main>`)
- `role="complementary"` - Supporting content (usually `<aside>`)
- `role="contentinfo"` - Footer information (usually `<footer>`)
- `role="search"` - Search functionality
- `role="form"` - Form region (usually `<form>`)

#### Widget Roles
For interactive components:

- `role="button"` - Clickable button
- `role="tab"` / `role="tablist"` / `role="tabpanel"` - Tab interfaces
- `role="dialog"` - Modal dialogs
- `role="alert"` - Important messages
- `role="menu"` / `role="menuitem"` - Application menus
- `role="slider"` - Slider controls
- `role="switch"` - Toggle switches

#### Document Structure Roles
For organizing content:

- `role="article"` - Self-contained content
- `role="list"` / `role="listitem"` - Lists
- `role="heading"` - Headings
- `role="img"` - Images (with complex graphics)

### Important ARIA Attributes:

#### Labels and Descriptions

```html
<!-- aria-label: Provides a text label -->
<button aria-label="Close dialog">
  <span aria-hidden="true">&times;</span>
</button>

<!-- aria-labelledby: References another element for label -->
<h2 id="dialog-title">Confirm Action</h2>
<div role="dialog" aria-labelledby="dialog-title">
  <!-- Dialog content -->
</div>

<!-- aria-describedby: References another element for description -->
<input type="text" id="username" aria-describedby="username-help">
<span id="username-help">Must be 6-20 characters</span>
```

#### State and Property Attributes

```html
<!-- aria-hidden: Hides content from assistive technologies -->
<span aria-hidden="true">★★★★☆</span>
<span class="sr-only">4 out of 5 stars</span>

<!-- aria-expanded: Indicates if element is expanded -->
<button aria-expanded="false" aria-controls="submenu">
  Menu
</button>

<!-- aria-selected: Indicates selection state -->
<button role="tab" aria-selected="true">Tab 1</button>

<!-- aria-checked: Checkbox/radio state -->
<div role="checkbox" aria-checked="true">Option 1</div>

<!-- aria-disabled: Disabled state -->
<button aria-disabled="true">Unavailable Action</button>

<!-- aria-required: Required form fields -->
<input type="text" aria-required="true">

<!-- aria-invalid: Validation state -->
<input type="email" aria-invalid="true" aria-describedby="error-msg">
<span id="error-msg">Please enter a valid email</span>
```

#### Live Regions
For dynamic content updates:

```html
<!-- aria-live: Announces changes to screen readers -->
<div aria-live="polite" role="status">
  <!-- Dynamic content updates here -->
</div>

<!-- aria-atomic: Whether to announce entire region or just changes -->
<div aria-live="assertive" aria-atomic="true">
  Loading progress: 75%
</div>

<!-- aria-relevant: What changes to announce -->
<div aria-live="polite" aria-relevant="additions removals">
  <ul id="message-list">
    <!-- Messages added/removed dynamically -->
  </ul>
</div>
```

**Live Region Values:**
- `aria-live="off"` - Default, no announcements
- `aria-live="polite"` - Announce when user is idle
- `aria-live="assertive"` - Announce immediately (use sparingly)

### ARIA Best Practices:

1. **First Rule of ARIA**: Don't use ARIA if you can use semantic HTML
2. **Don't override native semantics**: `<button role="heading">` is bad
3. **All interactive elements must be keyboard accessible**
4. **Don't use `role="presentation"` or `aria-hidden="true"` on focusable elements**
5. **Validate**: Use ARIA correctly according to specifications
6. **Test with screen readers**: JAWS, NVDA, VoiceOver
7. **Keep it simple**: Overusing ARIA can make things worse

### Common Mistakes to Avoid:

```html
<!-- BAD: Using ARIA instead of semantic HTML -->
<div role="button" tabindex="0">Click me</div>

<!-- GOOD: Use native button -->
<button>Click me</button>

<!-- BAD: Hiding focusable content -->
<button aria-hidden="true">Don't do this</button>

<!-- BAD: Redundant ARIA -->
<nav role="navigation"> <!-- nav already has navigation role -->

<!-- GOOD: Just use semantic HTML -->
<nav>
  <!-- navigation content -->
</nav>
```

---

## Keyboard Navigation

All functionality must be accessible via keyboard for users who cannot use a mouse.

### Essential Keyboard Controls:

- **Tab** - Move focus forward
- **Shift + Tab** - Move focus backward
- **Enter** - Activate links and buttons
- **Space** - Activate buttons, toggle checkboxes
- **Arrow keys** - Navigate within components (menus, tabs, radio groups)
- **Escape** - Close dialogs/menus, cancel operations
- **Home/End** - Jump to first/last item in lists

### Focusable Elements:

By default, these elements are keyboard accessible:
- `<a>` with href
- `<button>`
- `<input>`
- `<select>`
- `<textarea>`
- Elements with `tabindex="0"`

### Making Custom Elements Focusable:

```html
<!-- Add tabindex="0" to make an element focusable -->
<div role="button" tabindex="0" onclick="doAction()">
  Custom Button
</div>

<!-- Add keyboard event handlers -->
<script>
  element.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      doAction();
    }
  });
</script>
```

### Tab Order:

```html
<!-- Natural tab order (follows DOM order) - PREFERRED -->
<button>First</button>
<button>Second</button>
<button>Third</button>

<!-- Using tabindex to control order - AVOID if possible -->
<button tabindex="3">Third</button>
<button tabindex="1">First</button>
<button tabindex="2">Second</button>

<!-- Remove from tab order -->
<div tabindex="-1">Not in tab order, but can be focused programmatically</div>

<!-- Never use positive tabindex values (1, 2, 3...) -->
<!-- They create maintenance nightmares and confuse users -->
```

### Component-Specific Keyboard Patterns:

#### Tab Interface
```html
<div role="tablist">
  <button role="tab" tabindex="0">Tab 1</button>
  <button role="tab" tabindex="-1">Tab 2</button>
  <button role="tab" tabindex="-1">Tab 3</button>
</div>
```
- **Tab** to focus the tab list
- **Left/Right arrows** to navigate between tabs
- **Enter/Space** to activate a tab
- Only one tab in the tab order at a time (`tabindex="0"` on active, `-1` on others)

#### Menu
- **Tab** to focus menu button
- **Enter/Space** to open menu
- **Arrow keys** to navigate menu items
- **Enter** to select item
- **Escape** to close menu

#### Modal Dialog
- **Tab** should cycle within the modal (focus trap)
- **Escape** to close modal
- Focus should return to trigger element when closed

### Keyboard Navigation Best Practices:

1. **Logical tab order**: Follow visual flow (left to right, top to bottom)
2. **Visible focus indicators**: Always show where focus is
3. **Skip links**: Allow bypassing repetitive content
4. **Focus management**: Move focus appropriately (e.g., to dialogs when opened)
5. **No keyboard traps**: Users must always be able to navigate away
6. **Consistent patterns**: Follow established conventions (e.g., arrow keys for tabs)
7. **Document custom interactions**: Provide instructions for complex widgets

### Testing Keyboard Accessibility:

1. Unplug your mouse
2. Navigate the entire site using only the keyboard
3. Can you reach all interactive elements?
4. Can you activate all functionality?
5. Is the focus indicator always visible?
6. Is the tab order logical?
7. Can you escape from all components?

---

## Focus States

Focus indicators show users where they are on a page when navigating with a keyboard. They are essential for keyboard accessibility.

### Why Focus States Matter:

- Help keyboard users know their current position
- Required by WCAG 2.1 (Success Criterion 2.4.7)
- Improve usability for everyone
- Show which element will be activated on Enter/Space

### Default Browser Focus Styles:

Browsers provide default focus indicators (usually an outline), but they're often too subtle or don't match your design.

```css
/* Browser default (don't rely on this alone) */
:focus {
  outline: 2px solid blue; /* varies by browser */
}
```

### Custom Focus Styles:

```css
/* Enhanced focus styles - MORE visible */
a:focus,
button:focus,
input:focus,
select:focus,
textarea:focus {
  outline: 3px solid #4A90E2;
  outline-offset: 2px;
}

/* Alternative with box-shadow (for rounded elements) */
button:focus {
  outline: none; /* Only remove if replacing with something better */
  box-shadow: 0 0 0 3px rgba(74, 144, 226, 0.5);
}

/* Visible on dark backgrounds */
.dark-theme a:focus {
  outline: 3px solid #FFD700;
}

/* Different style for different elements */
input:focus,
textarea:focus {
  outline: 2px solid #4A90E2;
  outline-offset: 0;
  border-color: #4A90E2;
}
```

### Focus Styles Best Practices:

1. **Never remove focus styles without replacement**
   ```css
   /* BAD - makes keyboard navigation impossible */
   *:focus {
     outline: none;
   }
   
   /* GOOD - custom style that's visible */
   *:focus {
     outline: 3px solid #4A90E2;
     outline-offset: 2px;
   }
   ```

2. **High contrast**: Ensure at least 3:1 contrast ratio with background
3. **Consistent**: Use similar focus styles throughout your site
4. **Distinctive**: Make focus clearly different from hover state
5. **Not just color**: Consider using thickness, patterns, or animations
6. **Account for all states**: Focus, focus + hover, focus + active

### :focus vs :focus-visible:

Modern CSS provides `:focus-visible` to show focus only when needed:

```css
/* Shows focus only for keyboard navigation */
button:focus-visible {
  outline: 3px solid #4A90E2;
}

/* Removes default focus for mouse clicks */
button:focus:not(:focus-visible) {
  outline: none;
}
```

Browser support is good (2021+), but provide fallback:

```css
/* Fallback for older browsers */
button:focus {
  outline: 3px solid #4A90E2;
}

/* Enhanced for modern browsers */
@supports selector(:focus-visible) {
  button:focus:not(:focus-visible) {
    outline: none;
  }
  button:focus-visible {
    outline: 3px solid #4A90E2;
  }
}
```

### Focus Management in JavaScript:

Sometimes you need to move focus programmatically:

```javascript
// Focus an element
element.focus();

// Focus with options
element.focus({ preventScroll: true });

// Store and restore focus
const previousFocus = document.activeElement;
// ... do something ...
previousFocus.focus();

// Focus first element in a modal
const modal = document.getElementById('modal');
const firstFocusable = modal.querySelector('button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])');
firstFocusable.focus();
```

### Skip Links and Focus:

When skip links are used, ensure the target receives focus:

```javascript
// Skip link click handler
skipLink.addEventListener('click', (e) => {
  e.preventDefault();
  const target = document.querySelector(skipLink.getAttribute('href'));
  
  // Set tabindex if needed
  if (!target.hasAttribute('tabindex')) {
    target.setAttribute('tabindex', '-1');
  }
  
  target.focus();
  target.scrollIntoView();
});
```

### Testing Focus States:

1. Tab through your entire site
2. Is focus always visible?
3. Does focus move logically?
4. Can you see where you are on every page?
5. Do custom focus styles work on all interactive elements?
6. Test in different browsers (focus styles vary)
7. Test with high contrast mode enabled
8. Use browser DevTools to force :focus state

---

## Additional Best Practices

### Semantic HTML

Use the right HTML element for the job:

```html
<!-- Use headings in order -->
<h1>Page Title</h1>
  <h2>Section</h2>
    <h3>Subsection</h3>

<!-- Use lists for lists -->
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>

<!-- Use buttons for actions -->
<button onclick="save()">Save</button>

<!-- Use links for navigation -->
<a href="/page">Go to page</a>
```

### Color Contrast

Ensure sufficient color contrast (WCAG 2.1 requirements):
- **Normal text**: 4.5:1 contrast ratio minimum
- **Large text** (18pt or 14pt bold): 3:1 minimum
- **UI components and graphics**: 3:1 minimum

### Forms Accessibility

```html
<form>
  <!-- Always use labels -->
  <label for="email">Email</label>
  <input type="email" id="email" name="email">
  
  <!-- Group related fields -->
  <fieldset>
    <legend>Shipping Address</legend>
    <!-- address fields -->
  </fieldset>
  
  <!-- Indicate required fields -->
  <label for="name">
    Name <abbr title="required" aria-label="required">*</abbr>
  </label>
  <input type="text" id="name" required aria-required="true">
  
  <!-- Provide helpful error messages -->
  <input type="email" id="email-2" aria-describedby="email-error">
  <span id="email-error" role="alert">Please enter a valid email address</span>
</form>
```

### Media Accessibility

```html
<!-- Videos need captions -->
<video controls>
  <source src="video.mp4" type="video/mp4">
  <track kind="captions" src="captions.vtt" srclang="en" label="English">
</video>

<!-- Audio needs transcripts -->
<audio controls>
  <source src="podcast.mp3" type="audio/mpeg">
</audio>
<a href="transcript.html">Read transcript</a>
```

### Responsive and Mobile

- Minimum touch target size: 44x44 pixels
- Support both portrait and landscape
- Allow zooming (don't use `user-scalable=no`)
- Test with screen magnification

---

## Testing Tools

### Automated Testing Tools:

1. **Browser Extensions**
   - axe DevTools (Chrome, Firefox)
   - WAVE (Chrome, Firefox)
   - Lighthouse (built into Chrome DevTools)

2. **Command Line Tools**
   - axe-core
   - pa11y
   - jest-axe (for React/Jest)

3. **Online Tools**
   - WAVE Web Accessibility Evaluation Tool
   - WebAIM Color Contrast Checker
   - AChecker

### Manual Testing:

1. **Keyboard navigation**
   - Tab through entire site
   - Test all interactive elements
   - Verify logical tab order

2. **Screen readers**
   - NVDA (Windows, free)
   - JAWS (Windows, paid)
   - VoiceOver (Mac/iOS, built-in)
   - TalkBack (Android, built-in)

3. **Browser zoom**
   - Test at 200% zoom
   - Verify text doesn't overlap
   - Check mobile responsive design

4. **High contrast mode**
   - Windows High Contrast mode
   - macOS Increase Contrast
   - Browser extensions

5. **Color blindness**
   - Use color blindness simulators
   - Don't rely on color alone

### Testing Checklist:

- [ ] All images have appropriate alt text
- [ ] Headings are in logical order (h1, h2, h3)
- [ ] Page has a descriptive title
- [ ] Skip-to-content link is present and works
- [ ] All functionality available via keyboard
- [ ] Focus is always visible
- [ ] Color contrast meets WCAG standards
- [ ] Forms have proper labels and error messages
- [ ] ARIA is used correctly (or not at all)
- [ ] Content is readable at 200% zoom
- [ ] Page is tested with screen reader
- [ ] Dynamic content changes are announced
- [ ] No keyboard traps

---

## Resources

### Guidelines and Standards
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [WAI-ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
- [Section 508 Standards](https://www.section508.gov/)

### Learning Resources
- [WebAIM](https://webaim.org/)
- [A11y Project](https://www.a11yproject.com/)
- [MDN Web Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
- [Inclusive Components](https://inclusive-components.design/)

### Tools and Testing
- [axe DevTools](https://www.deque.com/axe/devtools/)
- [WAVE](https://wave.webaim.org/)
- [Color Contrast Analyzer](https://www.tpgi.com/color-contrast-checker/)

---

Remember: **Accessibility is not a feature, it's a fundamental aspect of web development.** Building accessible websites from the start is easier than retrofitting accessibility later. When in doubt, test with real users, including people who use assistive technologies.
