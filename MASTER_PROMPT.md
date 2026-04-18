# SyncHub - Complete Master Prompt & Documentation
## A Cyber-Professional AI-Powered Productivity Dashboard

---

## 1. PROJECT OVERVIEW

### Name
**SyncHub** - An AI-powered productivity dashboard that auto-generates daily standups from GitHub activity, Google Meet summaries, and Notion integration.

### Purpose
- **Aggregate** work from multiple sources (GitHub, Google Meet, Notion)
- **Summarize** meetings using LLM AI (OpenRouter)
- **Generate** automated daily standups without manual effort
- **Sync** reports to Notion for documentation
- **Track** productivity metrics and analytics

### Core Value Proposition
"GitHub commits, Google Meet summaries, and Notion notes—all unified into one intelligent standup report. Zero configuration. Zero effort."

### Tech Stack
- **Frontend**: Next.js 14, React 18, Tailwind CSS, Framer Motion
- **Backend**: Node.js, Express.js (future implementation)
- **Chrome Extension**: Manifest V3, vanilla JavaScript
- **LLM**: OpenRouter API (GPT-4, Claude 3 Opus optional)
- **Database**: MongoDB/PostgreSQL (future)
- **Auth**: Google OAuth2, JWT tokens
- **APIs**: GitHub API, Google Calendar API, Notion API

---

## 2. DESIGN SYSTEM & COLOR PALETTE

### Cyber-Professional Aesthetic
Pure black backgrounds with electric blue accents for a high-end, modern look.

### Color Tokens (Tailwind Config)

```javascript
colors: {
  'cyber': {
    'black': '#000000',        // Pure black background
    'charcoal': '#121212',     // Dark card background
    'dark': '#0a0a0a',         // Darkest accent
    'blue': '#007AFF',         // Electric blue primary
    'blue-hover': '#0056B3',   // Blue hover state
    'blue-glow': 'rgba(0, 122, 255, 0.15)', // Glow effect
    'grey': '#808080',         // Mid grey
    'grey-light': '#A0A0A0',   // Light grey text
    'grey-lighter': '#D0D0D0', // Lightest grey
  },
}
```

### Typography
- **Font**: Inter (system fallback: -apple-system, BlinkMacSystemFont, Segoe UI)
- **Code**: Fira Code or Courier New
- **Serif**: Georgia or Times New Roman (for meeting summaries)

### Spacing System
```
0: 0
1: 4px
2: 8px
3: 12px
4: 16px
5: 20px
6: 24px
8: 32px
10: 40px
12: 48px
16: 64px
20: 80px
24: 96px
```

### Border Radius
```
sm: 8px
md: 12px
lg: 16px
full: 9999px
```

### Shadows & Glows
```
glow-blue: '0 0 20px rgba(0, 122, 255, 0.3), 0 0 40px rgba(0, 122, 255, 0.1)'
glow-blue-lg: '0 0 30px rgba(0, 122, 255, 0.4), 0 0 60px rgba(0, 122, 255, 0.2)'
```

### Animation Tokens
```
pulse-blue: 2s cubic-bezier(0.4, 0, 0.6, 1) infinite
glow: 3s ease-in-out infinite
fade-in: 0.5s ease-out
slide-up: 0.5s ease-out
slide-down: 0.3s ease-out
float: infinite (custom bounce)
```

---

## 3. ARCHITECTURE OVERVIEW

### High-Level System Design

```
┌─────────────────────────────────────────────────────┐
│ Chrome Extension (Manifest V3)                       │
│ ├─ background.js (Detect meet.google.com)          │
│ ├─ content.js (Scrape live captions)               │
│ ├─ popup.html/js (UI for recording)                │
│ └─ manifest.json (V3 configuration)                │
└────────────────┬────────────────────────────────────┘
                 │ (Message passing via chrome.runtime)
                 │
┌────────────────▼────────────────────────────────────┐
│ React Frontend (Next.js 14)                          │
│ ├─ pages/                                           │
│ │  ├─ landing.jsx (Landing page)                   │
│ │  ├─ dashboard/index.jsx (Main Bento grid)       │
│ │  ├─ meetings/index.jsx (Meeting history)        │
│ │  ├─ standups/index.jsx (Daily standups)         │
│ │  ├─ analytics/index.jsx (Productivity charts)   │
│ │  └─ settings/index.jsx (User preferences)       │
│ ├─ components/                                      │
│ │  ├─ layout/ (Sidebar, Header, AppShell)         │
│ │  ├─ dashboard/ (Cards, integrations)            │
│ │  ├─ ui/ (Button, Card, Badge, Input, Modal)    │
│ │  └─ MeetingPopup.jsx                            │
│ ├─ hooks/ (Authentication, data fetching)         │
│ ├─ services/ (API calls, extensions)              │
│ ├─ styles/ (Global CSS, Tailwind)                 │
│ └─ public/ (Assets, icons)                         │
└────────────────┬────────────────────────────────────┘
                 │ (REST/WebSocket to Node.js backend)
                 │
┌────────────────▼────────────────────────────────────┐
│ Node.js/Express Backend (Future)                    │
│ ├─ routes/                                          │
│ │  ├─ auth.js (Google OAuth2, JWT)                │
│ │  ├─ meetings.js (CRUD meetings)                 │
│ │  └─ github.js (GitHub API proxy)                │
│ ├─ services/                                        │
│ │  ├─ googleMeetService.js (Transcript fetch)    │
│ │  ├─ summarizationService.js (LLM calls)        │
│ │  ├─ githubService.js (Activity fetch)          │
│ │  └─ notionService.js (Notion sync)             │
│ ├─ models/ (Database schemas)                      │
│ ├─ middleware/ (Auth, error handling)              │
│ └─ config/ (OAuth, database, API keys)             │
└────────────────┬────────────────────────────────────┘
                 │ (External API calls)
                 │
┌────────────────▼────────────────────────────────────┐
│ External Services                                    │
│ ├─ Google OAuth2 / Calendar API                    │
│ ├─ GitHub API (GraphQL or REST)                    │
│ ├─ OpenRouter LLM API                              │
│ ├─ Notion API                                       │
│ └─ MongoDB/PostgreSQL                              │
└─────────────────────────────────────────────────────┘
```

---

## 4. UI COMPONENTS DETAILED

### 4.1 Layout Components

#### **Sidebar**
- **Location**: Fixed left side (collapsible on desktop)
- **Width**: 64px (collapsed) / 256px (expanded)
- **Background**: cyber-charcoal with electric blue borders
- **Navigation Items**:
  - Dashboard (LayoutDashboard icon)
  - Meetings (Calendar icon)
  - Standups (FileText icon)
  - Analytics (BarChart3 icon)
  - Settings (Settings icon)
- **Features**:
  - Active state indicator (blue highlight)
  - Hover effects (bg opacity increase)
  - Collapse/expand toggle button
  - Mobile responsive (hamburger menu on small screens)
  - Animated transitions (300ms)

#### **Header**
- **Location**: Fixed top, spans full width minus sidebar
- **Height**: 64px
- **Background**: cyber-charcoal/80 with backdrop blur
- **Content**:
  - Left: Page title (Dashboard, Meetings, etc.)
  - Right: 
    - Real-time sync indicator (animated blue dot)
    - User profile dropdown (User icon)
- **Profile Dropdown**:
  - "Signed in as: [Email]"
  - Settings option
  - Logout button (red text)
  - Animated slide-down entrance

#### **AppShell**
- **Wrapper**: Entire application layout
- **Provides**: Sidebar + Header + Main content area
- **Padding**: 16px-32px horizontal, 32px vertical
- **Responsive**: Adjusts margin for collapsed sidebar

### 4.2 Card Components

#### **Card (Base Component)**
```jsx
Props:
- className: string (additional Tailwind classes)
- glowing: boolean (adds glow-blue shadow)
- hover: boolean (adds hover shadow effect)
- children: ReactNode

Styling:
- Background: cyber-charcoal/40 with backdrop-blur-sm
- Border: 1px cyber-blue border-opacity-20
- Border-radius: lg (16px)
- Hover: Transitions to border-opacity-50 and glow shadow
- Padding: 24px
```

#### **GitHubFeed Card**
```jsx
Features:
- Displays last 7 days GitHub activity
- Shows commits, PRs, branches
- Icon indicators for activity type
- Hover effects on activity items
- "Generate Summary" button (primary CTA)
- Scrollable list (max-height: 384px)

Data Structure:
{
  id: number,
  type: 'commit' | 'pr' | 'branch',
  message: string,
  repo: string (format: "owner/repo"),
  time: string (relative time)
}
```

#### **MeetingSummaries Card**
```jsx
Features:
- Shows recent Google Meet summaries
- AI-generated insights and achievements
- Expandable for full details
- "Sync Meet →" button
- Max 3 meetings shown in compact view

Data Structure:
{
  id: number,
  title: string,
  summary: string,
  achievements: string,
  duration: string,
  time: string
}
```

#### **QuickStats Card**
```jsx
Features:
- Displays weekly productivity metrics
- 3 stat rows: Commits, PRs Merged, Productivity Score
- Shows change indicators (+12%, +3, ↑ 5%)
- Icons for visual context
- Animated scale-in on load
- "View Analytics →" button

Stats:
- Commits: number with % change
- PRs Merged: count with +/- change
- Productivity: percentage with trend arrow
```

#### **NotionIntegration Card**
```jsx
Features:
- Feature list with checkmarks
- Connection status indicator
- Last sync timestamp
- "Sync to Notion →" button
- Pulsing animation on status dot

Features Shown:
- Auto-generate weekly reports
- Sync GitHub activity
- Meeting summaries
```

### 4.3 UI Components

#### **Button Component**
```jsx
Props:
- variant: 'primary' | 'secondary' | 'ghost' | 'danger'
- size: 'sm' | 'md' | 'lg'
- icon: Icon component (optional)
- disabled: boolean
- children: string

Variants:
- primary: cyber-blue background, white text
- secondary: bordered, cyber-grey text
- ghost: transparent, hover bg
- danger: red background

Animations:
- whileHover: scale 1.05
- whileTap: scale 0.95
- Transition: 300ms
```

#### **Input Component**
```jsx
Props:
- type: 'text' | 'email' | 'password' (default: 'text')
- placeholder: string
- icon: Icon component (optional)
- value: string
- onChange: function
- disabled: boolean

Styling:
- Background: cyber-charcoal
- Border: 1px cyber-blue border-opacity-20
- Padding: 12px 16px
- Focus: cyan-300 text, cyber-blue ring
- Placeholder: cyber-grey
```

#### **Badge Component**
```jsx
Props:
- variant: 'success' | 'warning' | 'error' | 'info' | 'blue'
- size: 'sm' | 'md'
- children: string

Color Mapping:
- success: green background, green text
- warning: amber background
- error: red background
- info: cyan background
- blue: cyber-blue background
```

#### **Modal Component**
```jsx
Props:
- isOpen: boolean
- onClose: function
- title: string
- children: ReactNode
- size: 'sm' | 'md' | 'lg'

Features:
- Backdrop overlay with blur
- Animated entrance (fade-in-up)
- Close button (X icon)
- Keyboard dismissal (Esc key)
```

### 4.4 Special Components

#### **MeetingPopup**
```jsx
Features:
- Triggered when Google Meet is detected
- Shows live transcript from extension
- Editable transcript field
- "Summarize & Extract Actions" button
- Displays results: Summary, Achievements, Blockers, Next Steps
- Save to history button

States:
- Loading (while calling LLM)
- Success (showing summary)
- Error (with retry option)
```

#### **SyncIndicator**
```jsx
Features:
- Fixed bottom-right corner
- Animated pulse ring
- On hover: tooltip appears
- Shows sync status: "Real-time Sync", "Active"
- Clicking opens settings (future)

Animation:
- Outer ring scales 1 -> 1.2 -> 1 (2s loop)
- Main dot has glow on hover
```

---

## 5. ALL PAGES DETAILED

### 5.1 Landing Page (`/landing`)

```jsx
Structure:
├─ Navigation Bar (Fixed Top)
│  ├─ Logo & Brand (SyncHub)
│  ├─ Docs & Blog links
│  └─ (No auth button - landing only)
│
├─ Hero Section
│  ├─ Floating Badge: "🚀 The Future of Standups"
│  ├─ Main Heading: "Your Work,\nUnified" (gradient text)
│  ├─ Subheading: "GitHub commits, Google Meet summaries..."
│  ├─ CTA Buttons:
│  │  ├─ Primary: "Sign in with GitHub" (Electric Blue)
│  │  └─ Secondary: "Sign in with Google" (Bordered)
│  └─ Trust Badges:
│     ├─ OAuth 2.0 Secure
│     ├─ Real-time Sync
│     └─ Zero Setup
│
├─ Animated Background
│  ├─ Floating gradient orbs (3 total)
│  ├─ Blue opacity layers
│  └─ Infinite float animation (4-6s cycle)
│
└─ Feature Cards Section (3 columns)
   ├─ GitHub: "Auto-fetch your weekly commits"
   ├─ Google Meet: "Sync meeting transcripts"
   └─ Notion: "Push reports to Notion"

Animations:
- Navbar items: fade-in-left/right (0.6s)
- Badge: fade-in-up (0.8s)
- Heading: fade-in-up (0.8s, delay 0.1s)
- Buttons: fade-in-up (0.8s, delay 0.3s)
- Feature cards: fade-in-up on scroll (delay i*0.1s)
- Background orbs: continuous float (infinite, 4-6s)

Auth Flow:
→ Click "Sign in with GitHub"
→ Redirect to /auth/github
→ OAuth consent screen
→ Callback with token
→ Store in localStorage
→ Redirect to /dashboard
```

### 5.2 Dashboard Page (`/dashboard`)

```jsx
Layout: 3-Column Bento Grid

┌─────────────────────────────────┬─────────────────┐
│  GitHub Feed (2-col, 2-row)     │  Quick Stats    │
│  - Last 7 days activity         │  (1-col, 2-row)│
│  - Commits & PRs                │  - Commits      │
│  - "Generate Summary" button    │  - PRs Merged   │
│  - Scrollable list              │  - Productivity │
│  - View All Activity link       │                 │
├─────────────────┬───────────────┴─────────────────┤
│ Meetings (1-col)│ Notion (2-col)                  │
│ - Recent Meets  │ - Auto-sync features            │
│ - Summaries     │ - Connection status             │
│ - "Sync Meet →" │ - "Sync to Notion →"            │
└─────────────────┴─────────────────────────────────┘

Animations:
- Cards fade-in-up on load (delay 0.1-0.3s)
- Background orbs float continuously
- Stats scale-in (1.0-1.05)
- All hover effects on cards

Responsive Behavior:
- Mobile (< 640px): Single column, stacked cards
- Tablet (640-1024px): 2 columns
- Desktop (> 1024px): 3 columns with custom spanning

GitHub Activity Fetching:
- Calculates 7 days ago from today
- Queries GitHub API for user's commits
- Filters PRs and branches
- Sorts by timestamp (newest first)
- Shows relative time (e.g., "30 min ago")
- Max 5 items shown, with "Load More" option

Data Refresh:
- Initial load on page mount
- Manual refresh via "Generate Summary" button
- Auto-refresh every 5 minutes (background)
```

### 5.3 Meetings Page (`/meetings`)

```jsx
Structure:
├─ Header
│  ├─ Title: "Meeting History"
│  └─ Subtitle: "All your meeting summaries..."
│
├─ Search Bar
│  ├─ Icon: Search icon
│  ├─ Placeholder: "Search meetings..."
│  └─ Filter results in real-time
│
└─ Meetings List (Vertically Stacked Cards)
   └─ Each Card Contains:
      ├─ Header Section
      │  ├─ Video icon + Title
      │  ├─ Date & Duration
      │  └─ Action buttons (Download, Delete)
      │
      ├─ Summary Text (serif font)
      │  └─ AI-generated summary
      │
      └─ Expandable Details (Slide-down animation)
         ├─ Achievements (green checkmark)
         ├─ Blockers (amber warning)
         └─ Next Steps (blue checklist)

Empty State:
- Shows "No meetings found"
- Suggests adjusting search

Data Structure:
{
  id: number,
  title: string,
  date: 'YYYY-MM-DD',
  duration: string,
  summary: string,
  achievements: [string],
  blockers: [string],
  nextSteps: [string]
}
```

### 5.4 Standups Page (`/standups`)

```jsx
Structure:
├─ Header
│  ├─ Title: "Daily Standups"
│  └─ Subtitle: "Auto-generated from GitHub & meetings"
│
└─ Standups List (Vertically Stacked Cards)
   └─ Each Card Contains:
      ├─ Header
      │  ├─ Day of week + Date
      │  └─ Action buttons
      │     ├─ Copy to clipboard
      │     ├─ Download as PDF
      │     └─ Share button
      │
      └─ Content (Monospace Font)
         ├─ # Daily Standup - [Day], [Date]
         │
         ├─ ## Completed Yesterday
         │  ├─ Implemented feature X
         │  ├─ Merged PR Y
         │  └─ Fixed bug Z
         │
         ├─ ## Meeting Summaries
         │  └─ Auto-populated from meetings
         │
         ├─ ## Today's Focus
         │  ├─ Build feature A
         │  └─ Review code
         │
         └─ ## Blockers
            └─ Waiting on X approval

Format:
- Generated as markdown
- Human-readable formatting
- Can be copied to clipboard
- Downloadable as PDF (future)
- Shareable via link (future)
```

### 5.5 Analytics Page (`/analytics`)

```jsx
Structure:
├─ Header
│  ├─ Title: "Productivity Analytics"
│  └─ Subtitle: "Your performance metrics"
│
├─ Stats Grid (4 columns)
│  ├─ Total Commits (156, +12%)
│  ├─ Meeting Hours (28.5h, +2h)
│  ├─ Productivity Score (92%, Excellent)
│  └─ Lines of Code (12,482, +3,245)
│
├─ Weekly Activity Card
│  ├─ Commits by Day (Bar chart)
│  │  └─ 7 bars showing daily commit count
│  │
│  └─ Meeting Hours by Day (Bar chart)
│     └─ 7 bars showing meeting duration
│
└─ Key Insights Card
   ├─ "+12% commit activity increased this week"
   ├─ "28.5h in meetings - consider focus time"
   └─ "92% productivity score - excellent!"

Chart Specifications:
- Bar height: proportional to max value
- Colors:
  - Commits: cyber-blue gradient
  - Meetings: green gradient
- Min height: 8px (visible even if 0)
- X-axis: Day of week (Mon-Sun)
- Responsive: bars adjust to container width

Data Structure:
{
  day: string,
  commits: number,
  meetings: number (hours)
}
```

### 5.6 Settings Page (`/settings`)

```jsx
Structure:
├─ Header
│  ├─ Title: "Settings"
│  └─ Subtitle: "Manage your preferences"
│
├─ Account Section Card
│  ├─ Lock icon + "Account" title
│  ├─ Email input field
│  ├─ Google OAuth status badge
│  └─ "Change Password" button
│
├─ Notifications Section Card
│  ├─ Bell icon + "Notifications" title
│  ├─ Email Notifications toggle
│  ├─ Meeting Summaries toggle
│  └─ Weekly Reports toggle
│
├─ Integrations Section Card
│  ├─ Zap icon + "Integrations" title
│  ├─ GitHub (✓ Enabled)
│  ├─ Google Meet (✓ Enabled)
│  ├─ Slack (Connect button)
│  └─ Notion (✓ Enabled)
│
├─ Danger Zone Card (Red borders)
│  ├─ "Delete Account" heading
│  ├─ Warning text
│  └─ Red "Delete Account" button
│
└─ Action Buttons
   ├─ "Save Changes" (primary)
   └─ "Cancel" (secondary)

Toggle States:
- Checked: indicates enabled
- Unchecked: indicates disabled
- Smooth transition animation

Integrations Status:
- ✓ Connected: Green badge
- (blank): "Connect" button available
```

---

## 6. FEATURES & FUNCTIONALITY

### 6.1 GitHub Integration

**What it does:**
- Fetches user's GitHub commits from last 7 days
- Aggregates commits, PRs, and branches
- Displays in chronological order (newest first)
- Shows repository, commit message, time

**API Endpoint (Future):**
```
GET /api/github/activity?days=7
Response: [{id, type, message, repo, time}, ...]
```

**Implementation:**
```javascript
// Calculate date 7 days ago
const sevenDaysAgo = new Date();
sevenDaysAgo.setDate(sevenDaysAgo.getDate() - 7);

// Query GitHub API
const response = await fetch(
  `https://api.github.com/users/${username}/events?per_page=100`,
  { headers: { Authorization: `token ${githubToken}` } }
);

// Filter for commits/PRs from last 7 days
const filtered = data.filter(event => {
  const eventDate = new Date(event.created_at);
  return eventDate >= sevenDaysAgo;
});
```

### 6.2 Google Meet Integration

**What it does:**
- Chrome Extension detects when user joins meet.google.com
- Scrapes live captions/transcript
- Sends to backend for AI summarization
- Displays summary in dashboard popup

**Workflow:**
```
1. User joins Google Meet
2. Extension detects meet.google.com tab
3. Content script extracts captions every 5 seconds
4. User ends meeting or clicks "Stop Transcription"
5. Transcript sent to backend
6. Backend calls OpenRouter LLM
7. LLM returns: summary, achievements, blockers, next steps
8. Dashboard shows results in popup
9. User can save to history
10. Synced to Notion (optional)
```

**Data Flow:**
```javascript
// Extension scrapes captions
const captions = document.querySelectorAll('[data-participant-id] .caption');
const transcript = Array.from(captions).map(el => el.textContent).join(' ');

// Send to dashboard via message passing
chrome.runtime.sendMessage({
  action: 'meetingTranscript',
  transcript: transcript,
  meetingUrl: window.location.href
});

// Dashboard receives and processes
window.addEventListener('message', (event) => {
  if (event.data.type === 'meetingDetected') {
    // Show popup, send to backend for summarization
  }
});

// Backend LLM call
const summary = await callOpenRouter({
  model: 'openai/gpt-4-turbo',
  messages: [{
    role: 'system',
    content: 'Act as technical project manager...'
  }, {
    role: 'user',
    content: `Summarize this meeting:\n${transcript}`
  }]
});

// Returns
{
  summary: "...",
  achievements: ["...", "..."],
  blockers: ["..."],
  nextSteps: ["..."]
}
```

### 6.3 AI Summarization (OpenRouter LLM)

**Service Implementation:**
```javascript
// Backend service
async function summarizeMeeting(transcript) {
  const response = await fetch('https://openrouter.io/api/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${process.env.OPENROUTER_API_KEY}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      model: 'openai/gpt-4-turbo',
      messages: [{
        role: 'system',
        content: `You are a technical project manager. Extract from the meeting:
          1. Summary (2-3 sentences)
          2. Key Achievements (bullet list)
          3. Blockers (bullet list)
          4. Next Steps (bullet list)
          Format as JSON: { summary, achievements, blockers, nextSteps }`
      }, {
        role: 'user',
        content: transcript
      }],
      temperature: 0.7,
      max_tokens: 1000
    })
  });
  
  const result = await response.json();
  return JSON.parse(result.choices[0].message.content);
}
```

**Prompt Engineering:**
- System role: "Technical project manager"
- Extracts: Summary, Achievements, Blockers, Next Steps
- Format: Structured JSON
- Temperature: 0.7 (balanced creativity/accuracy)
- Max tokens: 1000

### 6.4 Notion Integration

**What it does:**
- User connects Notion account via OAuth
- Creates database for standups/meetings
- Auto-syncs new records
- Syncs weekly reports
- Updates existing records

**API Endpoints (Future):**
```
POST /api/notion/connect          (OAuth flow)
POST /api/notion/sync-standup     (Save standup)
POST /api/notion/sync-meeting     (Save meeting)
GET /api/notion/status            (Connection status)
```

**Implementation (Future):**
```javascript
// Create Notion page
async function syncToNotion(standup) {
  const response = await fetch('https://api.notion.com/v1/pages', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${notionToken}`,
      'Content-Type': 'application/json',
      'Notion-Version': '2022-06-28'
    },
    body: JSON.stringify({
      parent: { database_id: databaseId },
      properties: {
        'Date': { date: { start: new Date().toISOString() } },
        'Title': { title: [{ text: { content: standup.title } }] },
        'Status': { select: { name: 'Completed' } },
        'Content': { rich_text: [{ text: { content: standup.content } }] }
      }
    })
  });
  
  return await response.json();
}
```

### 6.5 Real-Time Sync

**Features:**
- Live indicator in header (blue dot + text)
- Background refresh every 5 minutes
- Manual refresh button
- Toast notifications on sync
- Conflict resolution (last-write-wins)

**Implementation:**
```javascript
// Auto-sync every 5 minutes
useEffect(() => {
  const interval = setInterval(async () => {
    await refreshData();
    showToast('✓ Synced with GitHub');
  }, 5 * 60 * 1000);
  
  return () => clearInterval(interval);
}, []);

// Manual refresh
const handleRefresh = async () => {
  setSyncing(true);
  try {
    await refreshData();
    showToast('✓ Data refreshed');
  } finally {
    setSyncing(false);
  }
};
```

### 6.6 Authentication Flow

**Google OAuth2:**
```
1. Click "Sign in with GitHub" or "Sign in with Google"
2. Redirect to /auth/[provider]
3. Backend initiates OAuth flow
4. User sees consent screen
5. User grants permissions
6. Callback to /auth/callback?code=...
7. Backend exchanges code for tokens
8. Tokens stored securely (httpOnly cookie + localStorage for JWT)
9. JWT returned to frontend
10. Redirect to /dashboard
11. Dashboard checks localStorage for token
12. If present, render app; if not, show /landing
```

**Logout:**
```
1. Click "Logout" in profile dropdown
2. Clear localStorage token
3. Clear httpOnly cookies
4. Redirect to /landing
```

**Protected Routes:**
```javascript
// In dashboard page
useEffect(() => {
  if (!token) {
    router.push('/landing');
  }
}, [router, token]);
```

---

## 7. CHROME EXTENSION ARCHITECTURE

### 7.1 Manifest.json (V3)

```json
{
  "manifest_version": 3,
  "name": "SyncHub - Meeting Transcriber",
  "version": "1.0.0",
  "description": "Auto-capture Google Meet transcripts and summaries",
  "permissions": ["activeTab", "tabs", "scripting"],
  "host_permissions": ["https://meet.google.com/*"],
  "background": {
    "service_worker": "background.js"
  },
  "content_scripts": [{
    "matches": ["https://meet.google.com/*"],
    "js": ["content.js"]
  }],
  "action": {
    "default_popup": "popup.html",
    "default_title": "SyncHub Meeting Capture"
  },
  "icons": {
    "16": "icons/icon16.png",
    "32": "icons/icon32.png",
    "128": "icons/icon128.png"
  },
  "oauth2": {
    "client_id": "your-google-client-id.apps.googleusercontent.com",
    "scopes": ["https://www.googleapis.com/auth/calendar.readonly"]
  }
}
```

### 7.2 Background Service Worker (background.js)

```javascript
// Detect when user navigates to meet.google.com
chrome.tabs.onUpdated.addListener((tabId, changeInfo, tab) => {
  if (changeInfo.status === 'complete' && tab.url?.includes('meet.google.com')) {
    // Inject content script
    chrome.tabs.sendMessage(tabId, {
      action: 'initializeTranscription'
    }).catch(err => console.log('Tab not ready'));
  }
});

// Listen for transcript messages from content script
chrome.runtime.onMessage.addListener((request, sender, sendResponse) => {
  if (request.action === 'meetingTranscript') {
    // Store transcript temporarily
    const meeting = {
      url: request.meetingUrl,
      transcript: request.transcript,
      timestamp: new Date(),
      tabId: sender.tab.id
    };
    
    // Send to dashboard window
    chrome.tabs.query({ url: 'http://localhost:3000/*' }, (tabs) => {
      if (tabs.length > 0) {
        chrome.tabs.sendMessage(tabs[0].id, {
          type: 'meetingTranscript',
          data: meeting
        });
      }
    });
    
    sendResponse({ success: true });
  }
});
```

### 7.3 Content Script (content.js)

```javascript
// Runs on meet.google.com
let isRecording = false;
let transcript = '';

// Listen for initialization message from background
chrome.runtime.onMessage.addListener((request, sender, sendResponse) => {
  if (request.action === 'initializeTranscription') {
    startCaptioning();
    sendResponse({ recording: true });
  }
  
  if (request.action === 'stopTranscription') {
    stopCaptioning();
    sendResponse({ recording: false });
  }
});

function startCaptioning() {
  isRecording = true;
  
  // Capture captions every 2 seconds
  setInterval(() => {
    if (!isRecording) return;
    
    // Google Meet stores captions in specific DOM structure
    const captionContainer = document.querySelector('[role="region"][aria-label*="Captions"]');
    if (captionContainer) {
      const captions = captionContainer.querySelectorAll('span[data-text]');
      const currentCaptions = Array.from(captions)
        .map(el => el.textContent)
        .join(' ');
      
      if (currentCaptions && currentCaptions !== transcript) {
        transcript += ' ' + currentCaptions;
        
        // Send to background script
        chrome.runtime.sendMessage({
          action: 'meetingTranscript',
          transcript: transcript,
          meetingUrl: window.location.href
        });
      }
    }
  }, 2000);
}

function stopCaptioning() {
  isRecording = false;
}

// Auto-start on page load
if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', () => {
    chrome.runtime.sendMessage({ action: 'extensionReady' });
  });
} else {
  chrome.runtime.sendMessage({ action: 'extensionReady' });
}
```

### 7.4 Popup UI (popup.html)

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    body {
      width: 300px;
      background: #000000;
      color: white;
      font-family: Inter, sans-serif;
      padding: 20px;
      margin: 0;
    }
    h2 { color: #007AFF; margin-top: 0; }
    button {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      background: #007AFF;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 600;
    }
    button:hover { background: #0056B3; }
    #status {
      padding: 10px;
      background: #121212;
      border-radius: 8px;
      margin: 10px 0;
      font-size: 12px;
    }
    #transcript {
      max-height: 200px;
      overflow-y: auto;
      background: #121212;
      padding: 10px;
      border-radius: 8px;
      font-size: 12px;
      margin: 10px 0;
    }
  </style>
</head>
<body>
  <h2>🎙️ SyncHub Capture</h2>
  <div id="status">
    <p>Waiting for Google Meet...</p>
  </div>
  <button id="startBtn">Start Recording</button>
  <button id="stopBtn" disabled>Stop Recording</button>
  <button id="sendBtn" disabled>Send to Dashboard</button>
  
  <h3>Transcript Preview:</h3>
  <div id="transcript"></div>
  
  <script src="popup.js"></script>
</body>
</html>
```

### 7.5 Popup Script (popup.js)

```javascript
let isRecording = false;
let currentTranscript = '';

// Check if user is on a Google Meet
chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
  const tab = tabs[0];
  if (tab.url?.includes('meet.google.com')) {
    document.getElementById('status').innerHTML = '✓ Google Meet detected';
  } else {
    document.getElementById('status').innerHTML = '✗ Open a Google Meet to use this extension';
    document.getElementById('startBtn').disabled = true;
  }
});

// Start recording button
document.getElementById('startBtn').addEventListener('click', () => {
  isRecording = true;
  currentTranscript = '';
  
  chrome.tabs.query({ active: true }, (tabs) => {
    chrome.tabs.sendMessage(tabs[0].id, { action: 'startRecording' });
  });
  
  document.getElementById('startBtn').disabled = true;
  document.getElementById('stopBtn').disabled = false;
  document.getElementById('status').innerHTML = '🔴 Recording...';
});

// Stop recording button
document.getElementById('stopBtn').addEventListener('click', () => {
  isRecording = false;
  
  chrome.tabs.query({ active: true }, (tabs) => {
    chrome.tabs.sendMessage(tabs[0].id, { action: 'stopRecording' });
  });
  
  document.getElementById('startBtn').disabled = false;
  document.getElementById('stopBtn').disabled = true;
  document.getElementById('sendBtn').disabled = false;
  document.getElementById('status').innerHTML = '⏹️ Recording stopped';
});

// Listen for transcript updates from content script
chrome.runtime.onMessage.addListener((request, sender, sendResponse) => {
  if (request.action === 'transcriptUpdate') {
    currentTranscript = request.transcript;
    document.getElementById('transcript').textContent = currentTranscript.substring(0, 500) + '...';
  }
});

// Send to dashboard button
document.getElementById('sendBtn').addEventListener('click', () => {
  // Send to dashboard backend
  fetch('http://localhost:3000/api/meetings/save-transcript', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      transcript: currentTranscript,
      meetingUrl: window.location.href,
      token: localStorage.getItem('token')
    })
  })
  .then(res => res.json())
  .then(data => {
    if (data.success) {
      document.getElementById('status').innerHTML = '✓ Sent to dashboard!';
      currentTranscript = '';
    }
  });
});
```

---

## 8. ANIMATIONS & INTERACTIONS

### 8.1 Framer Motion Presets

```javascript
// Standard entrance animations
const fadeInUp = {
  initial: { opacity: 0, y: 20 },
  animate: { opacity: 1, y: 0 },
  transition: { duration: 0.6 }
};

const fadeInLeft = {
  initial: { opacity: 0, x: -20 },
  animate: { opacity: 1, x: 0 },
  transition: { duration: 0.6 }
};

const fadeInRight = {
  initial: { opacity: 0, x: 20 },
  animate: { opacity: 1, x: 0 },
  transition: { duration: 0.6 }
};

// Hover animations
const scaleOnHover = {
  whileHover: { scale: 1.05 },
  whileTap: { scale: 0.95 }
};

const hoverGlow = {
  whileHover: {
    boxShadow: '0 0 30px rgba(0, 122, 255, 0.4)'
  }
};

// Staggered children
const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1,
      delayChildren: 0.2
    }
  }
};

const childVariants = {
  hidden: { opacity: 0, y: 10 },
  visible: { opacity: 1, y: 0 }
};
```

### 8.2 Component Animation Examples

```jsx
// Animated Card Entrance
<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.6, delay: 0.1 }}
  whileHover={{ borderColor: '#007AFF' }}
>
  <Card>...</Card>
</motion.div>

// Animated Button
<motion.button
  whileHover={{ scale: 1.05 }}
  whileTap={{ scale: 0.95 }}
  className="bg-cyber-blue px-6 py-3 rounded-lg"
>
  Click Me
</motion.button>

// Staggered List
<motion.ul
  variants={containerVariants}
  initial="hidden"
  animate="visible"
>
  {items.map((item, i) => (
    <motion.li key={i} variants={childVariants}>
      {item}
    </motion.li>
  ))}
</motion.ul>

// Animated Background
<motion.div
  animate={{ y: [0, -20, 0] }}
  transition={{ duration: 4, repeat: Infinity }}
  className="w-96 h-96 bg-cyber-blue opacity-5 rounded-full"
/>
```

### 8.3 CSS Animations (Tailwind)

```tailwind
/* Float Animation - Background Orbs */
@keyframes float
  0%, 100%: transform translateY(0px)
  50%: transform translateY(-20px)

animate-float: float 6s ease-in-out infinite

/* Gradient Shift - Text Gradients */
@keyframes gradient-shift
  0%: background-position: 0% 50%
  50%: background-position: 100% 50%
  100%: background-position: 0% 50%

animate-gradient: gradient-shift 3s ease infinite

/* Pulse Blue - Sync Indicator */
@keyframes pulse-blue
  0%, 100%: opacity: 1, box-shadow: 0 0 0 0 rgba(0, 122, 255, 0.7)
  50%: opacity: 0.8, box-shadow: 0 0 0 8px rgba(0, 122, 255, 0)

animate-pulse-blue: pulse-blue 2s infinite
```

---

## 9. DATA MODELS & DATABASE SCHEMA

### 9.1 User Model

```javascript
{
  _id: ObjectId,
  email: string (unique),
  username: string,
  avatar: string (URL),
  googleId: string (unique),
  githubId: string (unique),
  
  // OAuth Tokens
  googleAccessToken: string (encrypted),
  googleRefreshToken: string (encrypted),
  githubAccessToken: string (encrypted),
  
  // Settings
  preferences: {
    notificationsEnabled: boolean,
    emailDigest: 'daily' | 'weekly' | 'none',
    theme: 'dark' | 'light',
    timezone: string
  },
  
  // Integrations
  integrations: {
    github: { connected: boolean, connectedAt: Date },
    googleMeet: { connected: boolean, connectedAt: Date },
    notion: { connected: boolean, connectedAt: Date, databaseId: string }
  },
  
  // Metadata
  createdAt: Date,
  updatedAt: Date,
  lastLoginAt: Date
}
```

### 9.2 Meeting Model

```javascript
{
  _id: ObjectId,
  userId: ObjectId (ref: User),
  
  // Meeting Info
  title: string,
  description: string (optional),
  meetingUrl: string,
  duration: number (minutes),
  
  // Transcript & Summary
  rawTranscript: string,
  summary: {
    text: string,
    achievements: [string],
    blockers: [string],
    nextSteps: [string]
  },
  
  // Metadata
  startTime: Date,
  endTime: Date,
  recordedAt: Date,
  
  // Notion Integration
  notionPageId: string (optional),
  syncedToNotion: boolean,
  
  // Status
  status: 'draft' | 'summarized' | 'archived',
  
  createdAt: Date,
  updatedAt: Date
}
```

### 9.3 Standup Model

```javascript
{
  _id: ObjectId,
  userId: ObjectId (ref: User),
  
  // Standup Content
  date: Date,
  dayOfWeek: string,
  
  content: {
    completedYesterday: [string],
    meetingSummaries: [string],
    todaysFocus: [string],
    blockers: [string]
  },
  
  // Generated From
  githubActivityIds: [ObjectId] (ref: GitHubActivity),
  meetingIds: [ObjectId] (ref: Meeting),
  
  // Notion Integration
  notionPageId: string (optional),
  syncedToNotion: boolean,
  
  // Distribution
  sharedWith: [email],
  shareLink: string (unique, optional),
  
  createdAt: Date,
  updatedAt: Date
}
```

### 9.4 GitHubActivity Model

```javascript
{
  _id: ObjectId,
  userId: ObjectId (ref: User),
  
  // Activity Data
  type: 'commit' | 'pr' | 'push' | 'branch',
  message: string,
  repo: {
    owner: string,
    name: string,
    url: string
  },
  
  // GitHub Metadata
  githubEventId: string (unique),
  author: {
    name: string,
    avatar: string
  },
  
  // Timestamps
  eventDate: Date,
  fetchedAt: Date,
  
  // Inclusion in Standups
  includedInStandups: [ObjectId] (ref: Standup),
  
  createdAt: Date
}
```

---

## 10. API ENDPOINTS (Backend - Future Implementation)

### 10.1 Authentication Endpoints

```
POST /api/auth/google
- Initiates Google OAuth flow
- Returns: redirectUrl to consent screen

GET /api/auth/callback?code=...&state=...
- OAuth callback handler
- Returns: { token: JWT, user: { email, name, avatar } }

POST /api/auth/logout
- Clears session
- Requires: Authorization header with JWT

POST /api/auth/refresh
- Refreshes JWT token
- Returns: { token: newJWT }
```

### 10.2 GitHub Endpoints

```
GET /api/github/activity?days=7
- Fetches user's activity from last N days
- Returns: [{ id, type, message, repo, time }, ...]
- Requires: Authorization header

POST /api/github/sync
- Manually trigger sync from GitHub
- Returns: { synced: number, from: Date, to: Date }
```

### 10.3 Meeting Endpoints

```
GET /api/meetings
- List user's meetings
- Query: ?limit=10&offset=0&search=...
- Returns: [{ id, title, summary, date, duration }, ...]

POST /api/meetings/save-transcript
- Save transcript from Chrome Extension
- Body: { transcript: string, meetingUrl: string }
- Returns: { id: meetingId, summary: {...} }

POST /api/meetings/:id/summarize
- Trigger LLM summarization for meeting
- Body: { transcript: string }
- Returns: { summary, achievements, blockers, nextSteps }

GET /api/meetings/:id
- Get specific meeting details
- Returns: { id, title, transcript, summary, achievements, blockers, nextSteps }

DELETE /api/meetings/:id
- Delete a meeting record
- Returns: { success: true }
```

### 10.4 Standup Endpoints

```
GET /api/standups
- List user's standups
- Query: ?limit=10&offset=0&date=...
- Returns: [{ id, date, content, status }, ...]

POST /api/standups/generate
- Generate standup for a specific date
- Body: { date: Date }
- Returns: { id, content: {...}, meetings: [...], github: [...] }

GET /api/standups/:id
- Get specific standup
- Returns: { id, date, content, shareLink, createdAt }

POST /api/standups/:id/share
- Generate share link for standup
- Returns: { shareLink: string }

DELETE /api/standups/:id
- Delete standup
- Returns: { success: true }
```

### 10.5 Notion Integration Endpoints

```
POST /api/notion/connect
- Initiates Notion OAuth
- Returns: { redirectUrl }

GET /api/notion/callback?code=...
- Notion OAuth callback
- Returns: { token: JWT, connected: true }

POST /api/notion/sync-meeting
- Sync single meeting to Notion
- Body: { meetingId: string }
- Returns: { notionPageId: string, success: true }

POST /api/notion/sync-standup
- Sync standup to Notion
- Body: { standupId: string }
- Returns: { notionPageId: string, success: true }

GET /api/notion/status
- Check Notion connection status
- Returns: { connected: boolean, lastSync: Date }

POST /api/notion/disconnect
- Disconnect Notion account
- Returns: { success: true }
```

### 10.6 Analytics Endpoints

```
GET /api/analytics/stats
- Get productivity stats for date range
- Query: ?from=Date&to=Date
- Returns: {
    totalCommits: number,
    totalMeetings: number,
    totalMeetingHours: number,
    productivityScore: number,
    linesOfCode: number
  }

GET /api/analytics/weekly
- Get weekly breakdown of activity
- Returns: [{ day, commits, meetings, hours }, ...]

GET /api/analytics/trending
- Get trending topics/blockers/achievements
- Returns: {
    topBlockers: [string],
    topAchievements: [string],
    topTopics: [string]
  }
```

---

## 11. COMPLETE FILE STRUCTURE

```
GDE/
├── README.md
├── .gitignore
├── .env.example
│
├── frontend/
│   ├── package.json
│   ├── next.config.js
│   ├── tailwind.config.js
│   ├── postcss.config.js
│   │
│   ├── src/
│   │   ├── pages/
│   │   │   ├── _app.jsx (App wrapper with auth check)
│   │   │   ├── _document.jsx
│   │   │   ├── index.jsx (Root redirect)
│   │   │   ├── landing.jsx (Landing page)
│   │   │   ├── dashboard/
│   │   │   │   └── index.jsx (3-column Bento grid)
│   │   │   ├── meetings/
│   │   │   │   └── index.jsx (Meeting history)
│   │   │   ├── standups/
│   │   │   │   └── index.jsx (Daily standups)
│   │   │   ├── analytics/
│   │   │   │   └── index.jsx (Productivity charts)
│   │   │   └── settings/
│   │   │       └── index.jsx (User preferences)
│   │   │
│   │   ├── components/
│   │   │   ├── layout/
│   │   │   │   ├── AppShell.jsx (Main layout wrapper)
│   │   │   │   ├── Sidebar.jsx (Navigation)
│   │   │   │   └── Header.jsx (Top navigation)
│   │   │   │
│   │   │   ├── dashboard/
│   │   │   │   ├── GitHubFeed.jsx (GitHub activity)
│   │   │   │   ├── MeetingSummaries.jsx (Meet summaries)
│   │   │   │   ├── QuickStats.jsx (Weekly metrics)
│   │   │   │   ├── NotionIntegration.jsx (Notion sync)
│   │   │   │   └── SyncIndicator.jsx (Floating sync button)
│   │   │   │
│   │   │   ├── ui/
│   │   │   │   ├── Button.jsx
│   │   │   │   ├── Card.jsx
│   │   │   │   ├── Badge.jsx
│   │   │   │   ├── Input.jsx
│   │   │   │   └── Modal.jsx
│   │   │   │
│   │   │   └── MeetingPopup.jsx (Transcript editor & summarizer)
│   │   │
│   │   ├── hooks/
│   │   │   ├── useAuth.js (Authentication)
│   │   │   ├── useMeetingIntegration.js (Extension messages)
│   │   │   ├── useGitHub.js (GitHub data fetching)
│   │   │   └── useLLMSummarization.js (LLM calls)
│   │   │
│   │   ├── services/
│   │   │   ├── api.js (Base API client)
│   │   │   ├── extensionMessenger.js (Extension message passing)
│   │   │   └── storage.js (LocalStorage helpers)
│   │   │
│   │   ├── styles/
│   │   │   ├── globals.css (Global Tailwind)
│   │   │   └── variables.css (CSS variables)
│   │   │
│   │   └── utils/
│   │       ├── dateUtils.js (Date calculations)
│   │       ├── formatters.js (Text formatting)
│   │       └── validators.js (Form validation)
│   │
│   ├── public/
│   │   ├── favicon.ico
│   │   ├── logo.svg
│   │   └── icons/
│   │       ├── github.svg
│   │       ├── meet.svg
│   │       └── notion.svg
│   │
│   └── node_modules/ (Auto-generated)
│
├── backend/ (Future Implementation)
│   ├── package.json
│   ├── server.js
│   │
│   ├── config/
│   │   ├── google.js (OAuth2 config)
│   │   ├── openrouter.js (LLM config)
│   │   ├── database.js (MongoDB/PostgreSQL)
│   │   └── environment.js (Env variables)
│   │
│   ├── routes/
│   │   ├── auth.js (OAuth endpoints)
│   │   ├── meetings.js (Meeting CRUD)
│   │   ├── standups.js (Standup generation)
│   │   ├── github.js (GitHub integration)
│   │   ├── notion.js (Notion sync)
│   │   └── analytics.js (Analytics data)
│   │
│   ├── services/
│   │   ├── googleMeetService.js (Transcript handling)
│   │   ├── summarizationService.js (OpenRouter LLM)
│   │   ├── githubService.js (GitHub API proxy)
│   │   ├── notionService.js (Notion database)
│   │   ├── standupService.js (Standup generation)
│   │   └── tokenService.js (Token management)
│   │
│   ├── models/
│   │   ├── User.js
│   │   ├── Meeting.js
│   │   ├── Standup.js
│   │   └── GitHubActivity.js
│   │
│   ├── middleware/
│   │   ├── auth.js (JWT verification)
│   │   ├── errorHandler.js (Error handling)
│   │   └── rateLimiter.js (Rate limiting)
│   │
│   └── utils/
│       ├── logger.js
│       └── emailService.js
│
├── extension/
│   ├── manifest.json (V3 manifest)
│   ├── background.js (Service worker)
│   ├── content.js (Content script)
│   ├── popup.html (UI)
│   ├── popup.js (Popup logic)
│   ├── styles.css (Popup styling)
│   │
│   └── icons/
│       ├── icon16.png
│       ├── icon32.png
│       └── icon128.png
│
└── docs/
    ├── ARCHITECTURE.md
    ├── API.md
    ├── SETUP.md
    └── DEPLOYMENT.md
```

---

## 12. ENVIRONMENT VARIABLES

```bash
# Frontend (.env.local)
NEXT_PUBLIC_API_URL=http://localhost:5000
NEXT_PUBLIC_GITHUB_CLIENT_ID=your_github_client_id
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_google_client_id

# Backend (.env)
PORT=5000
NODE_ENV=development

# Database
MONGODB_URI=mongodb://localhost:27017/synchub
DATABASE_NAME=synchub

# Google OAuth2
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_secret
GOOGLE_REDIRECT_URI=http://localhost:5000/api/auth/google/callback

# GitHub OAuth2
GITHUB_CLIENT_ID=your_client_id
GITHUB_CLIENT_SECRET=your_secret
GITHUB_REDIRECT_URI=http://localhost:5000/api/auth/github/callback

# Notion
NOTION_CLIENT_ID=your_notion_client_id
NOTION_CLIENT_SECRET=your_notion_secret

# LLM (OpenRouter)
OPENROUTER_API_KEY=your_openrouter_key
OPENROUTER_MODEL=openai/gpt-4-turbo

# JWT
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRY=7d

# Email (Optional for notifications)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email
SMTP_PASS=your_password

# AWS S3 (For file storage)
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_REGION=us-east-1
AWS_S3_BUCKET=synchub-storage
```

---

## 13. SETUP & DEPLOYMENT GUIDE

### 13.1 Local Development Setup

```bash
# 1. Clone repository
git clone https://github.com/yourusername/synchub.git
cd synchub

# 2. Setup Frontend
cd frontend
npm install
cp .env.example .env.local
# Edit .env.local with your values

# 3. Run Frontend (Development)
npm run dev
# Runs on http://localhost:3000

# 4. Setup Backend (Future)
cd ../backend
npm install
cp .env.example .env
# Edit .env with your values

# 5. Run Backend
npm start
# Runs on http://localhost:5000

# 6. Setup Chrome Extension
# - Open chrome://extensions/
# - Enable "Developer mode"
# - Click "Load unpacked"
# - Select ./extension folder
# - Extension ready in Chrome toolbar
```

### 13.2 Deployment

**Frontend (Vercel - Recommended):**
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel
# Follow prompts, link to GitHub repo
# Auto-deploys on git push to main
```

**Backend (Railway, Heroku, or AWS):**
```bash
# Option 1: Railway (Simplest)
npm install -g railway
railway link
railway up

# Option 2: Heroku
heroku create synchub-backend
git push heroku main
```

**Chrome Extension (Chrome Web Store):**
```bash
# 1. Create Chrome Web Store Developer account ($5)
# 2. Prepare assets:
#    - icon128.png
#    - Screenshots (1280x800)
#    - Privacy policy
#
# 3. Zip extension folder
zip -r extension.zip extension/
#
# 4. Upload to Chrome Web Store dashboard
# 5. Submit for review (1-3 days)
# 6. Publish once approved
```

---

## 14. KEY FEATURES SUMMARY

### ✅ Completed Features
- [x] Cyber-professional UI design system
- [x] Landing page with OAuth CTAs
- [x] Bento grid dashboard layout
- [x] GitHub activity integration (7-day fetching)
- [x] Framer Motion animations throughout
- [x] Glassmorphism UI effects
- [x] Navigation (Sidebar + Header)
- [x] Meetings page with expandable details
- [x] Standups page with markdown formatting
- [x] Analytics page with charts
- [x] Settings page with integrations
- [x] Real-time sync indicator
- [x] Chrome Extension V3 scaffold

### 🚀 Future Features
- [ ] Backend API implementation
- [ ] Google Meet transcript capture
- [ ] OpenRouter LLM summarization
- [ ] Notion database sync
- [ ] PDF export for standups
- [ ] Email digest notifications
- [ ] Slack integration
- [ ] Team collaboration features
- [ ] Mobile app (React Native)
- [ ] Dark/Light theme toggle
- [ ] Advanced analytics & trending
- [ ] Chrome Extension publishing

---

## 15. TECHNOLOGY DECISIONS & RATIONALE

### Why Next.js 14?
- Built-in API routes for backend
- File-based routing (simpler structure)
- Image optimization
- Fast refresh during development
- Vercel deployment integration

### Why Tailwind CSS?
- Utility-first (no context switching)
- Custom design tokens (cyber colors)
- Responsive design (mobile-first)
- Built-in animations
- Small bundle size

### Why Framer Motion?
- Declarative animations
- No CSS keyframe complexity
- Gesture support (whileHover, whileTap)
- Smooth transitions
- Easy staggering

### Why Chrome Extension V3?
- Manifest V2 deprecated
- Service Workers instead of background pages
- Better security model
- Will be required in 2025

### Why OpenRouter instead of Direct API?
- Access to multiple LLMs (GPT-4, Claude, etc.)
- No need to manage multiple API keys
- Better pricing model
- Automatic fallback if one model fails

---

## 16. BEST PRACTICES & CONVENTIONS

### Code Style
```javascript
// Use async/await over .then()
const data = await fetchData();

// Destructure props
const { title, description } = props;

// Use const by default, let when reassigned
const name = 'SyncHub';

// Arrow functions for React components
const MyComponent = () => {
  return <div>...</div>;
};

// Component names PascalCase
export function DashboardCard() {}

// File names: lowercase, dash-separated
- dashboard-card.jsx
- github-feed.jsx
```

### Naming Conventions
```
Pages: PascalCase, folder with index.jsx
  /dashboard/index.jsx → DashboardPage

Components: PascalCase
  export function GitHubFeed() {}

Hooks: camelCase, prefix with 'use'
  export function useGitHubData() {}

Utilities: camelCase
  export const formatDate = () => {}

Constants: UPPER_SNAKE_CASE
  const MAX_RETRIES = 3;

CSS Classes: kebab-case
  className="bg-cyber-blue text-white"
```

### Accessibility Standards
- Semantic HTML (`<button>`, not `<div onClick>`)
- ARIA labels for screen readers
- Keyboard navigation (Tab, Enter, Esc)
- Color contrast ratios (WCAG AA minimum)
- Focus visible indicators
- Alt text for images

### Performance Optimization
- Image lazy loading
- Component code splitting
- Memoization for expensive calculations
- Debouncing for input events
- Virtual scrolling for long lists

---

## 17. TROUBLESHOOTING GUIDE

### Common Issues

**Issue: "synchub-blue not found" error**
```
Solution: Ensure tailwind.config.js has cyber color namespace
Check: colors: { 'cyber': { 'blue': '#007AFF', ... } }
```

**Issue: Chrome Extension not detecting Google Meet**
```
Solution: Verify manifest.json host_permissions
Check: "host_permissions": ["https://meet.google.com/*"]
Also: Ensure content.js is injected (check DevTools Console)
```

**Issue: OAuth callback not working**
```
Solution: Verify redirect URI in OAuth provider settings
GitHub: https://yourdomain.com/api/auth/github/callback
Google: https://yourdomain.com/api/auth/google/callback
```

**Issue: LLM summarization failing**
```
Solution: Check OpenRouter API key validity
Also: Verify transcript length (max ~4000 tokens)
Try: Use fallback LLM model in config
```

---

## 18. SECURITY CONSIDERATIONS

### Token Management
- **Never store tokens in localStorage for production**
- Use httpOnly cookies for sensitive tokens
- Implement token rotation/refresh logic
- Clear tokens on logout

### API Security
- CORS properly configured
- Rate limiting on endpoints
- Input validation on all requests
- SQL injection prevention (use ORMs)
- CSRF tokens for form submissions

### Data Privacy
- Encrypt sensitive data at rest
- HTTPS only (no HTTP)
- Mask PII in logs
- GDPR compliance for EU users
- Clear data retention policies

### Extension Security
- Content Security Policy headers
- No inline scripts (use popup.js)
- Validate all data from content script
- Sandbox extension from main app

---

## 19. MONITORING & ANALYTICS

### Frontend Monitoring
```javascript
// Google Analytics 4
gtag('event', 'page_view', { page_path: '/dashboard' });
gtag('event', 'generate_summary', { content_type: 'meeting' });

// Error Tracking (Sentry)
Sentry.captureException(error);

// Performance Monitoring
performance.mark('api-call-start');
// ... API call ...
performance.mark('api-call-end');
performance.measure('api-call', 'api-call-start', 'api-call-end');
```

### Backend Monitoring
```javascript
// Logging
logger.info('User logged in', { userId, timestamp });
logger.error('API error', { error, status, endpoint });

// Performance
prometheus.histogram('api_response_time', duration);
prometheus.counter('api_requests_total', { method, endpoint });

// Uptime Monitoring
// Use: UptimeRobot, Pingdom, or New Relic
```

---

## 20. VERSION HISTORY

```
v1.0.0 (Current)
- Initial launch
- Landing page
- Bento grid dashboard
- GitHub integration
- Chrome Extension scaffold
- Basic UI/UX

v1.1.0 (Planned)
- Backend implementation
- Google Meet integration
- LLM summarization
- Notion sync

v2.0.0 (Roadmap)
- Team collaboration
- Slack integration
- Mobile app
- Advanced analytics
```

---

**END OF MASTER PROMPT**

This comprehensive guide covers every aspect of the SyncHub project from design system through deployment. Use this as a reference template to rebuild on any platform or coding environment.
