# LUT Exam Room Utility

A simple, privacy-focused single-page web application for managing in-person exams at LUT University.

## Overview

A static website that displays exam information for a classroom setting, with support for edit and present modes. All data is stored locally in the browser and can be shared via URL encoding.

## Core Features

### 1. Finland Time Display
- Live clock showing current time in Finland (Europe/Helsinki timezone)
- Handles DST (EET/EEST) automatically
- Large, visible display suitable for classroom viewing

### 2. Exam Management
- List of exams with the following fields:
  - Course name/code
  - Start time
  - Duration (in minutes)
  - End time (auto-calculated: start + duration)
  - Color (any hex color, chosen by exam coordinator)
  - Notes (free text field for calculator rules, special instructions, etc.)
  - "Students may leave at" time (auto-calculated: start time + duration)

### 3. Display Modes
- **Edit Mode**: Full interface for adding, editing, and removing exams
- **Present Mode**: Clean, large display optimized for classroom projection
  - Hide all edit controls
  - Large fonts for readability from back of room
  - Toggle button to switch between modes

### 4. Data Persistence
- **URL Encoding**: All exam data encoded in URL hash using LZ-string compression
  - Share exams by copying the URL
  - No server required, fully portable
- **LocalStorage**: Automatic backup/restore
  - Save state on every change
  - Load from localStorage on page load if no URL data present

### 5. Privacy
- Zero telemetry
- No external API calls
- No cookies
- All data stays in browser

## UI Design

### Layout
```
┌─────────────────────────────────────────────────────────────┐
│  [Toggle Mode Button]      FINLAND TIME        [DATE]       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────────┐  ┌──────────────────┐                │
│  │ [COURSE HEADER]  │  │ [COURSE HEADER]  │  ...           │
│  │   (colored bg)   │  │   (colored bg)   │                │
│  ├──────────────────┤  ├──────────────────┤                │
│  │ Start: 09:00     │  │ Start: 13:00     │                │
│  │ End: 12:00       │  │ End: 15:00       │                │
│  │                  │  │                  │                │
│  │ May leave: 12:00 │  │ May leave: 15:00 │                │
│  │                  │  │                  │                │
│  │ Notes:           │  │ Notes:           │                │
│  │ Calc allowed     │  │ No electronics   │                │
│  └──────────────────┘  └──────────────────┘                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Edit Mode Controls
- Add new exam button
- For each exam:
  - Course name input
  - Color picker (native HTML5 color input for any hex)
  - Start time input (time picker)
  - Duration input (number field, minutes)
  - Notes textarea
  - Delete button

### Present Mode
- Hide all form inputs and buttons except mode toggle
- Large, high-contrast text
- Responsive grid layout
- Fullscreen button (optional)

## Data Model

```javascript
{
  exams: [
    {
      id: "uuid-v4",
      course: "CS-101",
      color: "#ff6b6b",
      startTime: "09:00",  // 24-hour format
      duration: 180,       // minutes
      notes: "Scientific calculator allowed. No phones."
    }
  ]
}
```

### Calculated Fields
- End time = start time + duration
- Leave time = end time (students may leave when exam ends)

## Technical Implementation

### Stack
- Single HTML file
- Vanilla JavaScript (ES6+)
- CSS in style tag (responsive)
- No build step required

### Libraries (CDN)
- LZ-string (for URL compression)
- (Optional) No other dependencies

### Key Functions
- `getFinlandTime()` - Current time in Helsinki
- `encodeData(data)` - Compress to URL-safe string
- `decodeData(string)` - Decompress from URL
- `saveToLocalStorage()` - Persist state
- `loadFromLocalStorage()` - Restore state
- `renderEditMode()` - Build edit UI
- `renderPresentMode()` - Build present UI

### Time Handling
- Use `Intl.DateTimeFormat` with `timeZone: 'Europe/Helsinki'`
- All times displayed in 24-hour format
- Today's date shown for context

### URL Structure
```
https://example.com/exam-room/#<compressed-data>
```

## File Structure

```
├── index.html          # Single file application
├── PLAN.md            # This file
└── README.md          # Usage instructions
```

## Future Enhancements (Optional)

- Export to PDF/print-friendly view
- Multiple exam sessions (save/load presets)
- Countdown timer for each exam
- Sound notification when exam ends
- Dark mode toggle

## Privacy Checklist

- [ ] No external fonts (use system fonts)
- [ ] No analytics scripts
- [ ] No external API calls
- [ ] No cookies
- [ ] LocalStorage only, no server sync
- [ ] URL data is client-side only
