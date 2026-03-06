# 每日一寄 (Daily Note)

## Project Overview

This is a minimalist daily journaling/note-taking web application implemented as a single HTML file. It features a dark-themed UI with soft aesthetics, designed for quick text recording throughout the day.

**File**: `每日一寄.html`

**Key Features**:
- Daily journaling with morning and afternoon sections
- Custom panels for additional note categories (addable, renamable, deletable)
- Drag-and-drop reordering for custom panels
- Timeline view showing all records for the current year (2026)
- Auto-save to browser localStorage
- Responsive design for both desktop and mobile

## Technology Stack

- **HTML5**: Single-file application with embedded CSS and JavaScript
- **CSS3**: Custom properties (CSS variables), flexbox, grid layout, backdrop filters
- **Vanilla JavaScript**: No frameworks, direct DOM manipulation
- **Storage**: Browser localStorage for data persistence
- **Fonts**: JetBrains Mono (via Google Fonts)

## Architecture

The entire application is contained in one HTML file with three main sections:

```
每日一寄.html
├── <head>
│   ├── Google Fonts preconnect and stylesheet
│   └── <style> (lines 13-490) - All CSS styling
├── <body>
│   ├── Header with date display and save indicator
│   ├── 2026 Timeline Panel (collapsible)
│   ├── Today Panel (morning/afternoon textareas)
│   ├── Yesterday Panel (read-only view)
│   ├── Custom Panels Container (dynamic)
│   └── Add Panel Button
└── <script> (lines 574-842) - All JavaScript logic
```

### CSS Structure

Uses CSS custom properties (design tokens) defined in `:root`:
- `--bg`, `--surface`: Background colors (dark theme)
- `--text`, `--text-muted`, `--text-dim`: Text colors
- `--accent`: Sky blue (#38bdf8) for highlights
- `--danger`: Rose (#f43f5e) for delete actions
- `--border`, `--border-hi`: Border colors with opacity

Key components:
- `.panel`: Main content blocks
- `.sub-panels`: Grid layout for morning/afternoon
- `.custom-panel`: User-created panels with drag handles
- `.timeline-list`: Collapsible year overview

### JavaScript Structure

**Data Functions**:
- `getRecords()` / `saveRecord()`: Read/write daily records to localStorage (`meiri_records`)
- `getCustomPanels()` / `saveCustomPanels()`: Read/write custom panels to localStorage (`meiri_custom`)

**Render Functions**:
- `renderHeader()`: Update header with current date
- `renderToday()`: Load today's data into textareas
- `renderYesterday()`: Display yesterday's records (read-only)
- `render2026()`: Build timeline of all 2026 records
- `renderCustomPanels()`: Rebuild custom panels from storage

**Auto-save System**:
- `scheduleAutoSave()`: Debounced save trigger (800ms delay)
- `flushSave()`: Immediate save and UI update

**Custom Panel Management**:
- `addCustomPanel()`: Create new panel with default title
- `removePanel()`: Delete panel with fade animation
- `createPanelNode()`: Build DOM element with drag-drop handlers
- Drag-and-drop using native HTML5 DnD API

**Utility Functions**:
- `fmtKey()`, `fmtShort()`, `fmtFull()`: Date formatting helpers
- `escHtml()`: XSS protection for user input display

## Data Storage Schema

localStorage keys:

```javascript
// Daily records
meiri_records: {
  "2026-03-06": {
    morning: "string",
    afternoon: "string"
  },
  // ... more dates
}

// Custom panels
meiri_custom: [
  { id: "string", title: "string", content: "string" },
  // ... more panels
]
```

## Usage Instructions

### Running the Application

Simply open the HTML file in any modern web browser:

```bash
open 每日一寄.html
# or
python -m http.server 8000  # if serving via HTTP
```

No build step, dependencies, or server required.

### User Workflow

1. **Today's Entry**: Type in morning/afternoon textareas - auto-saves after 800ms of inactivity
2. **Yesterday's View**: Read-only display of previous day's entries
3. **Timeline**: Click "展开全部" to see all 2026 records; click rows to expand details
4. **Custom Panels**: Click "+ 新增板块" to add; drag handles to reorder; edit titles inline

### Development Guidelines

**Code Style**:
- CSS uses BEM-like naming with kebab-case
- JavaScript uses camelCase for functions/variables
- Comments in Chinese, following existing convention

**When Modifying**:
1. **Styling**: Update CSS custom properties in `:root` for theme changes
2. **Storage**: Maintain backward compatibility when changing data structures
3. **Panels**: The drag-drop logic uses `data-drag-handle` attribute for grab handles
4. **Date handling**: All dates use local time; format with `fmtKey()` for storage keys

**Adding New Features**:
- For new UI sections: add HTML in `<body>`, CSS in `<style>`, logic in `<script>`
- For new data fields: extend the record object structure and update save/load functions
- For new storage: use separate localStorage keys to avoid conflicts

## Testing

Manual testing checklist:
- [ ] Morning/afternoon entries save and persist after refresh
- [ ] Yesterday's panel shows correct data from previous day
- [ ] Timeline shows all 2026 records sorted newest first
- [ ] Custom panels: add, edit title, edit content, delete, reorder
- [ ] Drag-drop reordering works smoothly
- [ ] Save indicator shows "保存中…" then "已保存"
- [ ] Mobile responsive: panels stack vertically, timeline adapts

## Browser Compatibility

Requires modern browsers supporting:
- CSS custom properties
- `backdrop-filter` (with `-webkit-` prefix fallback)
- HTML5 Drag and Drop API
- `localStorage`
- `Intl.DateTimeFormat` (not used, manual formatting instead)

Tested: Chrome, Firefox, Safari, Edge (latest versions)

## Security Considerations

- **XSS Protection**: `escHtml()` function escapes HTML entities when displaying stored content
- **No authentication**: Data is stored locally in browser; clearing localStorage removes all data
- **No server**: All data stays on user's device

## File Size

- Total: ~24KB (single HTML file)
- No external dependencies except Google Fonts CDN

## Future Enhancement Ideas

- Export/import functionality for data backup
- Year selector for timeline (currently hardcoded to 2026)
- Search/filter in timeline
- Dark/light theme toggle
- Markdown support in entries
