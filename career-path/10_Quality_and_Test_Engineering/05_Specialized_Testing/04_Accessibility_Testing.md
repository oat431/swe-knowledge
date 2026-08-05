---
title: Accessibility Testing
parent: Specialized Testing
topic: Testing system usability for people with disabilities
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - accessibility
  - a11y
  - wcag
---

# Accessibility Testing

> **Core Principle:** Accessibility testing verifies that the system is usable by people with disabilities, ensuring equal access and legal compliance.

## What Accessibility Testing Is

Accessibility testing (a11y testing) verifies:
- **Perceivable:** Content can be perceived by all users
- **Operable:** Interface can be operated by all users
- **Understandable:** Content and interface are understandable
- **Robust:** Content works with assistive technologies

Based on **WCAG** (Web Content Accessibility Guidelines) principles: POUR

## Why Accessibility Matters

**Legal requirements:**
- ADA (Americans with Disabilities Act)
- Section 508 (US federal government)
- EN 301 549 (European Union)
- Various national laws

**Business benefits:**
- Larger audience (15% of world population has disabilities)
- Better SEO (accessibility improves search rankings)
- Better UX for everyone (curb cut effect)
- Reduced legal risk
- Social responsibility

**Types of disabilities:**
- Visual (blindness, low vision, color blindness)
- Auditory (deafness, hard of hearing)
- Motor (limited mobility, tremors, paralysis)
- Cognitive (learning disabilities, attention disorders)
- Temporary (broken arm, eye surgery)
- Situational (bright sunlight, noisy environment)

## WCAG 2.1 Guidelines

### Level A (Minimum)

**1.1.1 Non-text Content:**
```
Requirement: All non-text content has text alternatives

Test cases:
- Images have alt text describing content
- Decorative images have empty alt=""
- Complex images have long descriptions
- Icons have accessible labels
- Videos have captions

Example:
<img src="chart.png" alt="Sales increased 25% from Q1 to Q2">
<img src="decorative-line.png" alt="">
```

**1.3.1 Info and Relationships:**
```
Requirement: Structure conveyed through presentation can be programmatically determined

Test cases:
- Headings used correctly (h1-h6 hierarchy)
- Lists use proper markup (ul, ol, li)
- Tables have headers (th) and scope
- Form labels associated with inputs
- Landmarks used (nav, main, aside)

Example:
<h1>Page Title</h1>
<h2>Section</h2>
<nav aria-label="Main navigation">...</nav>
<label for="email">Email:</label>
<input type="email" id="email">
```

**2.1.1 Keyboard:**
```
Requirement: All functionality available from keyboard

Test cases:
- Tab through all interactive elements
- Enter/Space activate buttons
- Arrow keys navigate within components
- Escape closes modals/menus
- No keyboard traps

Test method:
- Unplug mouse
- Navigate entire site with keyboard only
- Verify all features work
```

### Level AA (Standard)

**1.4.3 Contrast (Minimum):**
```
Requirement: Text contrast ratio at least 4.5:1 (3:1 for large text)

Test cases:
- Normal text (< 18pt): 4.5:1 contrast
- Large text (≥ 18pt or 14pt bold): 3:1 contrast
- UI components: 3:1 contrast
- Graphical objects: 3:1 contrast

Tools:
- WebAIM Contrast Checker
- Chrome DevTools
- axe DevTools

Example:
Bad: Gray text (#808080) on white background (3.5:1)
Good: Dark gray (#595959) on white background (7:1)
```

**1.4.4 Resize Text:**
```
Requirement: Text can be resized up to 200% without loss of content

Test cases:
- Increase browser zoom to 200%
- Verify all text remains visible
- Verify no overlap or truncation
- Verify functionality still works
- Test at different viewport sizes

Test method:
- Set browser zoom to 200%
- Navigate all pages
- Check for horizontal scrollbars
- Verify no content hidden
```

**2.4.7 Focus Visible:**
```
Requirement: Keyboard focus indicator visible

Test cases:
- Tab through page
- Focus indicator clearly visible
- Focus indicator has sufficient contrast
- Focus order logical
- Focus not hidden by other elements

Example:
:focus {
  outline: 3px solid #005fcc;
  outline-offset: 2px;
}
```

### Level AAA (Enhanced)

**1.4.6 Contrast (Enhanced):**
```
Requirement: Text contrast ratio at least 7:1 (4.5:1 for large text)

Test cases:
- Normal text: 7:1 contrast
- Large text: 4.5:1 contrast
- Even better readability for low vision users
```

## Accessibility Testing Process

### 1. Automated Testing

**Tools:**
- **axe DevTools:** Browser extension, comprehensive checks
- **WAVE:** Web accessibility evaluation tool
- **Lighthouse:** Built into Chrome DevTools
- **Pa11y:** Command-line accessibility tester
- **eslint-plugin-jsx-a11y:** React accessibility linting

**Example (axe):**
```javascript
// Install axe-core
npm install axe-core

// Run in tests
const axe = require('axe-core');

describe('Accessibility', () => {
  it('should have no accessibility violations', async () => {
    await page.goto('https://example.com');
    const results = await page.evaluate(() => axe.run());
    
    expect(results.violations).toHaveLength(0);
    
    if (results.violations.length > 0) {
      console.log('Violations:', results.violations);
    }
  });
});
```

**Example (Pa11y):**
```bash
# Install pa11y
npm install -g pa11y

# Test a page
pa11y https://example.com

# Test with specific standard
pa11y --standard WCAG2AA https://example.com

# Generate report
pa11y --reporter html https://example.com > report.html
```

**Automated testing catches:**
- Missing alt text
- Insufficient contrast
- Missing form labels
- Invalid ARIA usage
- Missing language attribute
- Empty links or buttons

**Automated testing misses:**
- Keyboard navigation issues
- Focus management
- Screen reader compatibility
- Meaningful alt text
- Logical reading order

### 2. Manual Testing

**Keyboard testing:**
```
Test checklist:
□ Tab through entire page
□ All interactive elements reachable
□ Focus order logical
□ Focus indicator visible
□ No keyboard traps
□ Escape closes modals/menus
□ Enter/Space activate buttons
□ Arrow keys work in dropdowns
□ Skip links work

Common issues:
- Elements not focusable (missing tabindex)
- Focus trapped in component
- Focus order doesn't match visual order
- Focus indicator hidden or invisible
- Custom widgets not keyboard accessible
```

**Screen reader testing:**
```
Tools:
- NVDA (Windows, free)
- JAWS (Windows, commercial)
- VoiceOver (Mac/iOS, built-in)
- TalkBack (Android, built-in)

Test checklist:
□ Page title announced
□ Headings announced correctly
□ Images described with alt text
□ Form fields labeled
□ Error messages announced
□ Dynamic content announced
□ Landmarks navigable
□ Reading order logical

Common issues:
- Missing or poor alt text
- Form fields without labels
- Dynamic content not announced
- Incorrect heading hierarchy
- Tables without headers
```

**Color and contrast testing:**
```
Tools:
- WebAIM Contrast Checker
- Colour Contrast Analyser
- Chrome DevTools

Test checklist:
□ Text meets contrast requirements
□ UI components meet contrast requirements
□ Information not conveyed by color alone
□ Links distinguishable from text
□ Focus indicators visible
□ Error states visible without color

Common issues:
- Gray text on white background
- Red/green color coding only
- Low contrast buttons
- Invisible focus indicators
```

### 3. User Testing

**Recruit users with disabilities:**
- Work with disability organizations
- Include various disability types
- Test with assistive technologies they actually use
- Pay participants fairly
- Get informed consent

**Test scenarios:**
```
Scenario 1: Complete a purchase
- User with screen reader
- Task: Find product, add to cart, checkout
- Measure: Time, errors, satisfaction

Scenario 2: Fill out a form
- User with motor disability
- Task: Complete registration form
- Measure: Time, errors, frustration level

Scenario 3: Navigate site
- User with low vision
- Task: Find specific information
- Measure: Time, zoom level used, success rate
```

## Accessibility Testing Checklist

### Perceivable

- [ ] All images have appropriate alt text
- [ ] Videos have captions and transcripts
- [ ] Audio has transcripts
- [ ] Text contrast meets requirements (4.5:1 minimum)
- [ ] Text resizable to 200% without loss
- [ ] Content doesn't rely on color alone
- [ ] Content adaptable to different orientations
- [ ] Text spacing adjustable

### Operable

- [ ] All functionality available via keyboard
- [ ] No keyboard traps
- [ ] Focus order logical
- [ ] Focus indicator visible
- [ ] Users can pause/stop moving content
- [ ] No content flashes more than 3 times/second
- [ ] Skip links provided
- [ ] Page titles descriptive
- [ ] Links descriptive (not "click here")

### Understandable

- [ ] Page language specified
- [ ] Language of parts specified when different
- [ ] Form inputs have labels
- [ ] Error messages clear and helpful
- [ ] Consistent navigation
- [ ] Consistent identification
- [ ] Error prevention for legal/financial transactions

### Robust

- [ ] Valid HTML
- [ ] ARIA used correctly
- [ ] Name, role, value programmatically determinable
- [ ] Status messages programmatically determinable

## Accessibility Testing Tools

### Automated Tools

| Tool | Type | Best For |
|------|------|----------|
| **axe DevTools** | Browser extension | Development, CI/CD |
| **WAVE** | Web service | Quick evaluation |
| **Lighthouse** | Chrome DevTools | Performance + a11y |
| **Pa11y** | CLI | Automated testing |
| **Siteimprove** | Commercial | Enterprise monitoring |

### Manual Testing Tools

| Tool | Purpose |
|------|---------|
| **NVDA** | Free screen reader (Windows) |
| **VoiceOver** | Built-in screen reader (Mac/iOS) |
| **ChromeVox** | Chrome screen reader |
| **Color Oracle** | Color blindness simulator |
| **NoCoffee** | Vision impairment simulator |

### Assistive Technologies to Test

- Screen readers (NVDA, JAWS, VoiceOver)
- Screen magnifiers (ZoomText)
- Switch devices
- Voice control (Dragon NaturallySpeaking)
- Braille displays
- Alternative keyboards

## Common Accessibility Issues

| Issue | Impact | Solution |
|-------|--------|----------|
| **Missing alt text** | Screen readers can't describe images | Add descriptive alt text |
| **Low contrast** | Hard to read for low vision users | Increase contrast to 4.5:1 |
| **No keyboard access** | Can't use without mouse | Make all features keyboard accessible |
| **Missing labels** | Screen readers can't identify form fields | Add labels to all inputs |
| **Poor heading structure** | Hard to navigate with screen reader | Use proper h1-h6 hierarchy |
| **Auto-playing media** | Disorienting for screen reader users | Don't auto-play, or provide controls |
| **Time limits** | Not enough time for some users | Allow users to extend or disable |
| **Flashing content** | Can trigger seizures | Avoid flashing > 3 times/second |

## Accessibility Testing Best Practices

### 1. Test Early and Often

**Integrate into workflow:**
- Include accessibility in design reviews
- Test during development, not just at end
- Automate checks in CI/CD
- Manual testing each sprint

### 2. Combine Automated and Manual

**Automated testing:**
- Catches ~30% of issues
- Fast and repeatable
- Good for regression testing

**Manual testing:**
- Catches remaining ~70%
- Requires human judgment
- Essential for keyboard and screen reader testing

### 3. Learn Assistive Technologies

**Invest in learning:**
- Use screen readers regularly
- Navigate with keyboard only
- Try different assistive technologies
- Understand user experiences

### 4. Involve Users with Disabilities

**Nothing about us without us:**
- Include users in testing
- Get feedback from real users
- Understand real-world usage
- Learn from user experiences

## Key Takeaways

1. **Accessibility is a requirement:** Legal, ethical, and business imperative
2. **Test automated and manual:** Automated catches 30%, manual catches 70%
3. **Learn assistive technologies:** Use screen readers, keyboard navigation
4. **Follow WCAG guidelines:** Level AA is the standard
5. **Involve users with disabilities:** Real users provide real insights

## Related Topics

- [[01_Performance_Testing]]: Performance for all users
- [[04_Accessibility_Testing]]: Accessibility in depth
- [[06_Mobile_Testing]]: Mobile accessibility

## Existing Vault Connections

- [[software-engineering-note/05_Software_Testing/09_Accessibility_Testing]]: Accessibility testing techniques
