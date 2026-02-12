# Web Accessibility Notes

## Table of Contents
1. [What is Web Accessibility?](#what-is-web-accessibility)
2. [Alt Text for Images](#alt-text-for-images)
3. [Skip Navigation Links](#skip-navigation-links)
4. [ARIA Roles and Attributes](#aria-roles-and-attributes)
5. [Keyboard Navigation](#keyboard-navigation)
6. [Focus Management](#focus-management)
7. [Best Practices](#best-practices)
8. [Testing Your Accessibility](#testing-your-accessibility)
9. [Resources](#resources)

---

## What is Web Accessibility?

Web accessibility means that websites, tools, and technologies are designed and developed so that people with disabilities can use them. More specifically, people can:

- **Perceive** - Information and user interface components must be presentable to users in ways they can perceive
- **Operate** - User interface components and navigation must be operable
- **Understand** - Information and the operation of user interface must be understandable
- **Robust** - Content must be robust enough that it can be interpreted by a wide variety of user agents, including assistive technologies

### Who Benefits from Accessibility?

- People with visual impairments (screen reader users)
- People with hearing impairments (requiring captions/transcripts)
- People with motor disabilities (keyboard-only users)
- People with cognitive disabilities
- People using mobile devices
- People with temporary disabilities
- Older adults with changing abilities
- Everyone in various situations (bright sunlight, noisy environments, etc.)

---

## Alt Text for Images

Alt text (alternative text) is a textual description of an image that is read by screen readers and displayed when images fail to load.

### Why Alt Text Matters

- Provides context for users who cannot see images
- Improves SEO (search engines use alt text)
- Displays when images don't load
- Required for WCAG compliance

### How to Write Good Alt Text

**DO:**
- Be descriptive and specific
- Keep it concise (typically under 125 characters)
- Describe the purpose or content of the image
- Include relevant text that appears in the image
- Consider the context in which the image appears

**DON'T:**
- Start with "image of" or "picture of" (screen readers already announce it's an image)
- Include redundant information already in surrounding text
- Use alt text for decorative images (use `alt=""` instead)
- Be overly verbose or include irrelevant details

### Examples

```html
<!-- Good: Descriptive and specific -->
<img src="dog.jpg" alt="Golden retriever puppy playing with a red ball in a grassy park">

<!-- Bad: Too vague -->
<img src="dog.jpg" alt="Dog">

<!-- Decorative image: Empty alt text -->
<img src="divider.png" alt="" role="presentation">

<!-- Informative image: Include text -->
<img src="sale-banner.jpg" alt="50% off all items - Sale ends Sunday">

<!-- Functional image (e.g., button): Describe the function -->
<img src="search-icon.png" alt="Search">
```

---

## Skip Navigation Links

Skip navigation links allow keyboard users to bypass repetitive navigation and jump directly to the main content of a page.

### Why Skip Links Matter

- Saves time for keyboard and screen reader users
- Reduces frustration on sites with long navigation menus
- Improves overall user experience
- Required for WCAG 2.1 Level A compliance

### Implementation

```html
<!-- Skip link (typically placed as the first element in <body>) -->
<a href="#main-content" class="skip-to-content">Skip to main content</a>

<!-- Main content with corresponding ID -->
<main id="main-content">
    <!-- Your main content here -->
</main>
```

### CSS for Skip Links

```css
/* Hidden by default, visible on keyboard focus */
.skip-to-content {
    position: absolute;
    top: -40px;
    left: 0;
    background: #000;
    color: #fff;
    padding: 8px 16px;
    text-decoration: none;
    z-index: 100;
}

.skip-to-content:focus {
    top: 0;
}
```

---

## ARIA Roles and Attributes

ARIA (Accessible Rich Internet Applications) provides additional semantics to make web content and applications more accessible.

### Important Note

**Use semantic HTML first!** Only use ARIA when semantic HTML doesn't provide the necessary semantics.

> "No ARIA is better than bad ARIA" - First Rule of ARIA

### Common ARIA Roles

#### Landmark Roles

Landmark roles help users navigate to different sections of a page:

```html
<!-- Header -->
<header role="banner">
    <!-- Site header content -->
</header>

<!-- Main navigation -->
<nav role="navigation" aria-label="Main navigation">
    <!-- Navigation links -->
</nav>

<!-- Main content -->
<main role="main">
    <!-- Primary content -->
</main>

<!-- Complementary content -->
<aside role="complementary">
    <!-- Related content, sidebar -->
</aside>

<!-- Footer -->
<footer role="contentinfo">
    <!-- Footer content -->
</footer>

<!-- Search -->
<form role="search">
    <!-- Search form -->
</form>
```

**Note:** Modern semantic HTML5 elements have implicit ARIA roles, so adding them is often redundant but harmless.

#### Widget Roles

For custom interactive components:

```html
<!-- Button -->
<div role="button" tabindex="0">Click me</div>
<!-- Better: Use <button> element instead -->

<!-- Alert -->
<div role="alert">
    This is an important message!
</div>

<!-- Dialog/Modal -->
<div role="dialog" aria-labelledby="dialog-title" aria-modal="true">
    <h2 id="dialog-title">Dialog Title</h2>
    <!-- Dialog content -->
</div>
```

### Common ARIA Attributes

#### aria-label

Provides an accessible name for an element:

```html
<button aria-label="Close dialog">
    <span aria-hidden="true">&times;</span>
</button>

<nav aria-label="Main navigation">
    <!-- Navigation content -->
</nav>
```

#### aria-labelledby

References another element's ID for labeling:

```html
<section aria-labelledby="section-heading">
    <h2 id="section-heading">Section Title</h2>
    <!-- Section content -->
</section>
```

#### aria-describedby

Provides additional descriptive text:

```html
<input type="text" 
       id="username" 
       aria-describedby="username-hint">
<small id="username-hint">Must be 5-20 characters</small>
```

#### aria-required

Indicates a required form field:

```html
<input type="text" 
       id="email" 
       aria-required="true" 
       required>
```

#### aria-expanded

Indicates if a collapsible element is expanded or collapsed:

```html
<button aria-expanded="false" aria-controls="menu">
    Menu
</button>
<div id="menu" hidden>
    <!-- Menu content -->
</div>
```

#### aria-live

Announces dynamic content changes:

```html
<!-- Polite: Wait for user to finish current task -->
<div aria-live="polite" role="status">
    Item added to cart
</div>

<!-- Assertive: Interrupt immediately -->
<div aria-live="assertive" role="alert">
    Error: Please fix the form errors
</div>
```

#### aria-hidden

Hides content from assistive technologies:

```html
<!-- Hide decorative icons -->
<span aria-hidden="true" class="icon">★</span>
<span class="sr-only">Favorite</span>
```

---

## Keyboard Navigation

Keyboard accessibility ensures all functionality is available via keyboard alone, without requiring a mouse.

### Why Keyboard Navigation Matters

- Essential for motor disability users
- Required for screen reader users
- Helps power users work more efficiently
- Required for WCAG 2.1 Level A compliance

### Key Concepts

#### Tab Order

- **Tab**: Move forward through interactive elements
- **Shift + Tab**: Move backward through interactive elements
- **Enter/Space**: Activate buttons and links
- **Arrow keys**: Navigate within components (menus, radio groups, tabs)
- **Esc**: Close dialogs or cancel operations

#### Focusable Elements

By default, these elements are focusable:
- `<a>` with `href` attribute
- `<button>`
- `<input>`, `<select>`, `<textarea>`
- `<area>` with `href` attribute

Make custom elements focusable with `tabindex`:

```html
<!-- Make element focusable -->
<div tabindex="0" role="button">Custom Button</div>

<!-- Remove from tab order (still programmatically focusable) -->
<div tabindex="-1">Not in tab order</div>

<!-- Bad: Don't use positive values -->
<div tabindex="1"><!-- Don't do this --></div>
```

### Best Practices

1. **Maintain logical tab order** - Follows visual flow (top to bottom, left to right)
2. **Don't trap keyboard focus** - Users should always be able to navigate away
3. **Provide keyboard shortcuts** - For frequently used actions (document them!)
4. **Skip repetitive content** - Use skip links
5. **Use native elements** - They have built-in keyboard support

### Example: Accessible Custom Button

```html
<div class="custom-button" 
     role="button" 
     tabindex="0"
     onclick="handleClick()"
     onkeydown="handleKeyDown(event)">
    Click Me
</div>

<script>
function handleKeyDown(event) {
    // Activate on Enter or Space
    if (event.key === 'Enter' || event.key === ' ') {
        event.preventDefault();
        handleClick();
    }
}
</script>
```

**Better: Use a real button:**

```html
<button onclick="handleClick()">Click Me</button>
```

---

## Focus Management

Proper focus management ensures users always know where they are on the page and can navigate efficiently.

### Why Focus Management Matters

- Helps users understand their current location
- Essential for keyboard and screen reader users
- Improves overall user experience
- Required for WCAG compliance

### Visible Focus Indicators

All interactive elements should have a visible focus indicator:

```css
/* Good: Clear, contrasting focus indicator */
a:focus,
button:focus,
input:focus {
    outline: 3px solid #007bff;
    outline-offset: 2px;
}

/* Enhanced with box-shadow for better visibility */
button:focus {
    outline: 3px solid #007bff;
    outline-offset: 2px;
    box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.2);
}

/* Bad: Never remove focus indicators without replacement */
button:focus {
    outline: none; /* Don't do this! */
}
```

### Focus Styling Requirements

Per WCAG 2.4.7 (Level AA):
- Focus indicators must be visible
- Must have sufficient color contrast (at least 3:1)
- Should be at least 2 CSS pixels thick

### Managing Focus Programmatically

#### When Opening Dialogs

```javascript
function openDialog() {
    const dialog = document.getElementById('dialog');
    const firstFocusable = dialog.querySelector('button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])');
    
    dialog.hidden = false;
    firstFocusable.focus();
}
```

#### When Closing Dialogs

```javascript
function closeDialog(returnFocusTo) {
    const dialog = document.getElementById('dialog');
    dialog.hidden = true;
    
    // Return focus to the element that opened the dialog
    if (returnFocusTo) {
        returnFocusTo.focus();
    }
}
```

#### After Deleting Content

```javascript
function deleteItem(item) {
    const nextFocusable = findNextFocusableElement(item);
    item.remove();
    
    if (nextFocusable) {
        nextFocusable.focus();
    }
}
```

### Screen Reader Only Text

Sometimes you need to provide text for screen readers that shouldn't be visually displayed:

```css
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border-width: 0;
}
```

```html
<button>
    <span aria-hidden="true">×</span>
    <span class="sr-only">Close dialog</span>
</button>
```

---

## Best Practices

### Semantic HTML

Use the right HTML element for the job:

```html
<!-- Good: Semantic HTML -->
<button>Click me</button>
<nav>...</nav>
<main>...</main>
<article>...</article>

<!-- Bad: Div soup -->
<div onclick="...">Click me</div>
<div class="nav">...</div>
<div class="main">...</div>
```

### Form Accessibility

```html
<!-- Always associate labels with inputs -->
<label for="email">Email:</label>
<input type="email" id="email" name="email">

<!-- Group related inputs -->
<fieldset>
    <legend>Shipping Address</legend>
    <!-- Address fields -->
</fieldset>

<!-- Indicate required fields -->
<label for="name">
    Name <span aria-label="required">*</span>
</label>
<input type="text" id="name" required aria-required="true">

<!-- Provide helpful error messages -->
<input type="email" 
       id="email" 
       aria-describedby="email-error"
       aria-invalid="true">
<span id="email-error" role="alert">
    Please enter a valid email address
</span>
```

### Color and Contrast

- **Don't rely on color alone** to convey information
- **Ensure sufficient contrast**: 
  - Normal text: 4.5:1 minimum
  - Large text (18pt+ or 14pt+ bold): 3:1 minimum
- **Test with color blindness simulators**

```html
<!-- Bad: Color only -->
<span style="color: red;">Error</span>

<!-- Good: Icon + color + text -->
<span class="error">
    <span aria-hidden="true">⚠</span>
    Error: Invalid input
</span>
```

### Headings

- **Use proper heading hierarchy** (h1 → h2 → h3, don't skip levels)
- **One h1 per page** (typically the page title)
- **Use headings for structure**, not just styling

```html
<!-- Good -->
<h1>Page Title</h1>
<h2>Section Title</h2>
<h3>Subsection Title</h3>
<h3>Another Subsection</h3>
<h2>Another Section</h2>

<!-- Bad: Skipping levels -->
<h1>Page Title</h1>
<h4>Section Title</h4>
```

### Links

- **Write descriptive link text** (avoid "click here")
- **Make link purpose clear** from the text alone

```html
<!-- Bad -->
<a href="...">Click here</a> to read our privacy policy

<!-- Good -->
<a href="...">Read our privacy policy</a>

<!-- Good: Context provided -->
<a href="..." aria-label="Download Q4 2023 financial report">
    Download Report
</a>
```

### Responsive and Mobile

- **Ensure touch targets are large enough** (minimum 44×44 pixels)
- **Test with zoom** (up to 200%)
- **Support portrait and landscape** orientations
- **Don't disable zoom** (`user-scalable=no` is bad)

---

## Testing Your Accessibility

### Automated Testing Tools

- **Browser Extensions:**
  - [axe DevTools](https://www.deque.com/axe/devtools/)
  - [WAVE](https://wave.webaim.org/extension/)
  - [Lighthouse](https://developers.google.com/web/tools/lighthouse) (built into Chrome DevTools)

- **Command Line:**
  - [pa11y](https://pa11y.org/)
  - [axe-core](https://github.com/dequelabs/axe-core)

### Manual Testing

1. **Keyboard Navigation**
   - Can you reach all interactive elements with Tab?
   - Can you activate them with Enter/Space?
   - Are focus indicators visible?
   - Can you see what has focus at all times?

2. **Screen Reader Testing**
   - Windows: NVDA (free), JAWS (paid)
   - macOS: VoiceOver (built-in)
   - Linux: Orca (built-in)
   - Mobile: iOS VoiceOver, Android TalkBack

3. **Zoom Testing**
   - Zoom to 200% - is everything still usable?
   - Text should reflow, not be cut off

4. **Color Contrast**
   - Use browser DevTools or online checkers
   - Test all text against its background
   - Check focus indicators

### Testing Checklist

- [ ] All images have appropriate alt text
- [ ] Skip navigation link works
- [ ] Proper heading hierarchy (h1-h6)
- [ ] All form inputs have associated labels
- [ ] Keyboard navigation works for all interactive elements
- [ ] Focus indicators are clearly visible
- [ ] Color contrast meets WCAG AA standards (4.5:1 for normal text)
- [ ] No content flashes more than 3 times per second
- [ ] Page works with screen reader
- [ ] Page works at 200% zoom
- [ ] ARIA attributes are used correctly
- [ ] No keyboard traps

---

## Resources

### Official Guidelines

- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/) - Web Content Accessibility Guidelines
- [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/) - Patterns and widgets
- [Section 508](https://www.section508.gov/) - U.S. federal accessibility requirements

### Learning Resources

- [WebAIM](https://webaim.org/) - Web Accessibility In Mind
- [A11ycasts with Rob Dodson](https://www.youtube.com/playlist?list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g) - Video series
- [The A11Y Project](https://www.a11yproject.com/) - Community-driven accessibility resources
- [MDN Web Docs - Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)

### Tools

- [Color Contrast Analyzer](https://www.tpgi.com/color-contrast-checker/) - Check contrast ratios
- [WAVE Browser Extension](https://wave.webaim.org/extension/) - Accessibility evaluation
- [axe DevTools](https://www.deque.com/axe/devtools/) - Accessibility testing
- [Screen Reader Reference](https://dequeuniversity.com/screenreaders/) - Keyboard shortcuts

### Testing Tools

- [Lighthouse](https://developers.google.com/web/tools/lighthouse) - Automated auditing
- [axe-core](https://github.com/dequelabs/axe-core) - Accessibility testing engine
- [pa11y](https://pa11y.org/) - Automated accessibility testing
- [Accessibility Insights](https://accessibilityinsights.io/) - Microsoft's testing tool

### Browser Extensions

- [axe DevTools](https://chrome.google.com/webstore/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd)
- [WAVE](https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh)
- [Accessibility Insights for Web](https://chrome.google.com/webstore/detail/accessibility-insights-fo/pbjjkligggfmakdaogkfomddhfmpjeni)

### Communities

- [WebAIM Mailing List](https://webaim.org/discussion/)
- [A11y Slack](https://web-a11y.slack.com/)
- [#a11y on Twitter](https://twitter.com/hashtag/a11y)

---

## Summary

Web accessibility is not optional—it's a fundamental requirement for creating inclusive web experiences. By following these practices:

1. **Use semantic HTML** as your foundation
2. **Provide text alternatives** for non-text content
3. **Ensure keyboard accessibility** for all functionality
4. **Make focus visible** and manage it properly
5. **Use ARIA wisely** when HTML semantics aren't enough
6. **Test thoroughly** with both automated tools and manual testing
7. **Include people with disabilities** in your testing process

Remember: **Accessibility benefits everyone, not just people with disabilities.** Good accessibility practices lead to better usability, improved SEO, and broader reach for your web content.

---

*Last updated: 2024*
