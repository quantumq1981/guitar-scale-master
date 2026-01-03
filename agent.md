AGENTS.md - Guitar Scale Master Project

PROJECT OVERVIEW

Guitar Scale Master is a comprehensive, interactive web application for guitarists to learn, practice, and master scales, chords, and music theory. The tool combines visual fretboard mapping with practical musical applications in an all-in-one educational platform.

Core Technology Stack:

· Vanilla JavaScript (ES6+)
· SVG for fretboard rendering
· Web Audio API for sound synthesis
· Tailwind CSS + Custom CSS for styling
· localStorage for data persistence
· No external dependencies beyond CDN libraries

Live Application: [If deployed, add URL]
Target Audience: Guitarists of all levels, music educators, self-learners

REVIEW GUIDELINES

Code Quality & Style

· JavaScript: Use ES6+ features, async/await for promises, destructuring where helpful
· SVG Rendering: Use createElementNS with proper namespace "http://www.w3.org/2000/svg"
· CSS: Follow existing Tailwind patterns; custom CSS in <style> blocks must be scoped
· Naming: camelCase for variables/functions, PascalCase for classes, kebab-case for CSS classes
· Error Handling: Never use empty catch blocks. Log errors with console.error() and provide user feedback
· Audio Context: Always check audioCtx.state before operations; handle suspended state gracefully

Performance Requirements

· Initial Load: < 2s on 3G connection
· SVG Render: < 30ms for 90+ note elements
· Audio Latency: < 50ms for educational use
· Memory: No memory leaks in audio nodes or event listeners

Testing Standards

· All new features require manual testing:
  · Desktop (Chrome, Firefox, Safari)
  · Mobile (iOS Safari, Chrome Android)
  · Tablet (iPad, Android tablets)
· Audio features must degrade gracefully when Web Audio API is unavailable
· Verify localStorage operations handle quota errors

Security & Privacy

· No PII Collection: Only store anonymous practice statistics
· Local-Only: All data stays in browser unless explicitly exported
· Audio Permissions: Request audio context only after user interaction
· XSS Protection: Sanitize any user input for export/import functionality

ARCHITECTURE RULES

Core Components Structure

1. Scale Database (scaleDB): Immutable object defining scales by intervals
2. Fretboard Renderer: Pure function that takes scale data → returns SVG
3. Audio System: Singleton AudioContext with note/chord playback
4. UI State: Centralized in DOM elements; minimal JavaScript state
5. Statistics: localStorage persistence with Set/Array conversion

Key Design Patterns

· Event Delegation: Use parent event listeners for dynamic SVG elements
· Module Pattern: Functions are scoped to avoid global pollution
· Factory Functions: For creating SVG elements with consistent attributes
· Promise-Based Audio: Audio functions return promises for async handling

Mobile-First Responsive Design

· Fretboard: 8-fret view on mobile, horizontal scroll for additional frets
· Touch Targets: Minimum 44×44px for interactive elements
· Orientation: Support both portrait and landscape on mobile
· Performance: Optimize for 60fps scrolling and animations

AGENT-SPECIFIC WORKFLOW

Before Making Changes

1. Analyze Existing Code: Read through relevant files, especially:
   · scaleDB structure in main HTML
   · SVG rendering functions (drawFretboard, highlightVoicing)
   · Audio system (initAudioContext, playNote)
   · State management (loadStats, saveStats)
2. Test Current Behavior: Open the HTML file in browser, verify core functionality
3. Check Performance: Use browser DevTools to audit current performance

Implementation Priorities

1. Bug Fixes: Audio, rendering, or data loss issues
2. Performance: SVG rendering speed, audio latency, memory usage
3. Mobile UX: Touch interactions, responsive layout, accessibility
4. Features: Scale database expansion, new practice modes

Change Validation Process

For each change:

1. Visual Test: Verify fretboard renders correctly in desktop/mobile views
2. Audio Test: Confirm notes/chords play without errors
3. Data Test: Ensure statistics persist across page reloads
4. Performance Test: Check for regression in SVG render times

Token Limit Management

When approaching context limits, generate handoff report:

```markdown
## 🚨 HANDOFF REPORT - TOKEN LIMIT REACHED

**Current Task:** [Brief description]
**Files Modified:** [List files with changes]
**Next Immediate Action:** [One specific task]
**Critical State:** [Working/Broken/Partially working]
**Blockers:** [Any issues preventing progress]
**Test Instructions:** [How to verify current changes work]
```

COMMANDS & DEVELOPMENT WORKFLOW

Local Development

```bash
# No build process - direct file editing
# Test by opening guitar_scale_master_complete.html in browser

# For performance testing:
# 1. Open Chrome DevTools (F12)
# 2. Performance tab → Record while interacting
# 3. Audit tab → Run Lighthouse
```

Testing Checklist

Before considering any task complete:

· Fretboard renders correctly in all scale selections
· Audio plays without console errors
· Mobile touch interactions work
· Statistics persist and update
· No JavaScript errors in console
· CSS responsive at 320px, 768px, 1024px, 1440px

GitHub Workflow

```
1. Create feature branch from main
2. Make focused, logical changes
3. Test thoroughly across browsers
4. Commit with descriptive message
5. Create PR with:
   - Summary of changes
   - Testing performed
   - Screenshots for UI changes
   - Performance impact assessment
```

SPECIFIC IMPLEMENTATION GUIDES

Adding New Scales

```javascript
// Template for scaleDB entry:
"Scale Name": {
  intervals: [0, 2, 4, 5, 7, 9, 11],  // Semitones from root
  degrees: ["1", "2", "3", "4", "5", "6", "7"],  // Scale degrees
  compatibleChords: ["Maj7", "Maj9"],  // Chord types
  family: "Major",  // Grouping category
  usage: "Description of typical musical use"  // Educational context
}
```

Modifying SVG Fretboard

· Use createSVGElement() helper for consistency
· Position notes: cx = margin.left + fret * fretSpacing - fretSpacing/2
· For open strings (fret 0): cx = margin.left - fretSpacing/2
· Always clear .voicing-highlight elements before adding new ones

Audio System Updates

· Never create multiple AudioContext instances
· Reuse oscillators when possible
· Always wrap in try/catch with user feedback
· Support both AudioContext and webkitAudioContext

Data Persistence

· Convert Sets to Arrays before JSON.stringify()
· Handle QuotaExceededError for localStorage
· Provide export/import for user data backup

COMMON PITFALLS TO AVOID

1. SVG Namespace: Always use createElementNS, not createElement
2. Audio Context: Requires user gesture; handle suspended state
3. Event Listeners: Remove when elements are destroyed to prevent leaks
4. Mobile Scroll: Prevent default on touch events that interact with fretboard
5. Color Contrast: Ensure WCAG AA compliance for text on colored backgrounds

ACCESSIBILITY REQUIREMENTS

· SVG: Include role="img" and aria-label describing the fretboard
· Interactive Elements: Keyboard navigable with focus states
· Color Vision: Don't rely solely on color to convey information
· Screen Readers: Ensure logical tab order and descriptive labels

PERFORMANCE BUDGETS

Metric Target Measurement Method
SVG Render < 30ms Performance panel
Audio Latency < 50ms Manual timing
DOM Nodes < 1000 Element count
Bundle Size < 500KB Network panel

CONTACT & ESCALATION

For architectural decisions or breaking changes:

1. Review existing patterns in codebase
2. Consider backward compatibility
3. Document decision in code comments
4. Update this AGENTS.md if pattern changes

---

Remember: This is an educational tool first. Every change should enhance learning without adding unnecessary complexity. When in doubt, prioritize clarity and reliability over cleverness.

Last Updated: [Date] by [Agent/Contributor Name]