
## Project
Customer Support & Helpdesk - Static Boilerplate
A modern helpdesk dashboard for managing support tickets with customer management and knowledge base.

## Core Layout: 3-Panel Helpdesk

1. **Left Sidebar (Navigation - 20%)**
   - Dashboard (metrics overview)
   - Tickets (with count badges by status)
   - Customers
   - Knowledge Base
   - Reports
   - Quick action: "Buat Tiket Baru"

2. **Center Panel (Ticket List/Detail - 55%)**
   - Ticket table: ID, subject, customer, status, priority, assignee, date
   - Kanban board toggle (by status: Open, In Progress, Pending, Resolved, Closed)
   - Filter bar: status, priority, category, assignee, search
   - Ticket detail view with conversation thread

3. **Right Panel (Ticket Metadata - 25%)**
   - Customer info card (name, email, phone, company, avatar)
   - Ticket properties: category, priority, assignee, tags
   - SLA timer (if applicable)
   - Related tickets
   - Quick actions: Change status, Assign, Add tag

## Data Structure (Mock Only)
Use hardcoded mock data arrays - NO backend/API:

```typescript
type TicketStatus = 'open' | 'in_progress' | 'pending' | 'resolved' | 'closed'
type TicketPriority = 'low' | 'medium' | 'high' | 'urgent'

interface Ticket {
  id: string
  ticketNumber: string // e.g. "TKT-001"
  subject: string
  description: string
  status: TicketStatus
  priority: TicketPriority
  category: string
  customerId: string
  assigneeId: string | null
  tags: string[]
  source: 'email' | 'chat' | 'phone' | 'web'
  createdAt: Date
  updatedAt: Date
  resolvedAt: Date | null
}

interface TicketMessage {
  id: string
  ticketId: string
  sender: 'customer' | 'agent' | 'system'
  senderName: string
  senderAvatar: string | null
  content: string
  attachments: string[]
  isInternal: boolean // note only visible to agents
  createdAt: Date
}

interface Customer {
  id: string
  name: string
  email: string
  phone: string
  company: string
  avatar: string | null
  totalTickets: number
  ticketsOpen: number
  lastContact: Date
  createdAt: Date
}

interface SupportAgent {
  id: string
  name: string
  email: string
  avatar: string | null
  role: 'agent' | 'supervisor' | 'admin'
  isOnline: boolean
  assignedTickets: number
}

interface KbArticle {
  id: string
  title: string
  content: string
  category: string
  tags: string[]
  views: number
  helpful: number // upvotes
  notHelpful: number // downvotes
  createdAt: Date
  updatedAt: Date
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Rich Text:** TipTap for composing replies
- **Icons:** Lucide React
- **Components:** shadcn/ui (Radix-based, copy-paste components)
- **Animations:** Framer Motion for smooth transitions
- **Date Utils:** date-fns (with `id` locale)
- **Charts:** Recharts for dashboard metrics

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Friendly support with professional polish
- **Color Palette:**
  - Primary: Blue (trust, reliability) - `blue-600`
  - Accent: Orange (warmth, support) - `orange-500`
  - Neutral: Slate - `slate-50` to `slate-900`
  - Status: Open (blue), In Progress (amber), Pending (purple), Resolved (green), Closed (slate)
  - Priority: Urgent (red), High (orange), Medium (yellow), Low (slate)

- **Typography:**
  - Font: Inter for UI
  - Ticket subject: font-semibold, text-base
  - Conversation body: text-sm leading-relaxed
  - Status badges: text-xs font-medium

- **Spacing & Layout:**
  - Container: max-w-full
  - Conversation thread: space-y-4 between messages
  - Message bubble: max-w-[80%] rounded-2xl p-4

## UI Components Specification

**Ticket Table Row:**
```
┌───────────────────────────────────────────────────────────────────┐
│ TKT-001  🔒 Login Gagal                         🔴 Urgent  │
│          Budi Santoso                          Open              │
│          2 jam lalu                             📞 Web            │
│          [Assign] [Resolve] [⋮]                                  │
└───────────────────────────────────────────────────────────────────┘
```
- Priority indicator (colored dot or label)
- Status badge with color
- Source icon (email/chat/phone/web)
- Click row to open detail

**Conversation Thread:**
```
┌────────────────────────────────────────────┐
│ Budi Santoso        Hari ini, 09:30        │
│ ┌──────────────────────────────────────┐   │
│ │ Saya tidak bisa login. Pesan error:  │   │
│ │ "Invalid credentials" muncul terus.  │   │
│ └──────────────────────────────────────┘   │
│                                            │
│ Admin (Anda)         Hari ini, 09:45       │
│ ┌──────────────────────────────────────┐   │
│ │ Halo Budi, silakan coba reset       │   │
│ │ password melalui link "Lupa          │   │
│ │ Password" di halaman login.          │   │
│ └──────────────────────────────────────┘   │
│ ┌──────────────────────────────────────────┐│
│ │ Balas...                            [➤] ││
│ └──────────────────────────────────────────┘│
└────────────────────────────────────────────┘
```
- Customer messages: left-aligned, white bubble
- Agent messages: right-aligned, blue bubble
- Internal notes: yellow background, "Internal" badge
- Timestamp below each message

**Kanban Board:**
```
┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
│ Open     │  │ In       │  │ Pending  │  │ Resolved │
│ (5)      │  │ Progress │  │ (3)      │  │ (12)     │
│          │  │ (4)      │  │          │  │          │
│ ┌──────┐ │  │ ┌──────┐ │  │ ┌──────┐ │  │ ┌──────┐ │
│ │TKT-01│ │  │ │TKT-02│ │  │ │TKT-05│ │  │ │TKT-08│ │
│ │Login │ │  │ │Pay   │ │  │ │Req   │ │  │ │Bug   │ │
│ │issue │ │  │ │ment  │ │  │ │docs  │ │  │ │fixed │ │
│ └──────┘ │  │ └──────┘ │  │ └──────┘ │  │ └──────┘ │
└──────────┘  └──────────┘  └──────────┘  └──────────┘
```
- Drag ticket between columns to change status
- Count badge per column
- Truncated ticket preview

**Dashboard Metrics:**
```
┌─────────────────────────────────────────────────────┐
│ 📊 Dashboard Support                                │
│ ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐            │
│ │ Open │  │In Pro│  │Today │  │Avg   │            │
│ │  12  │  │  8   │  │  45  │  │ 2.5h │            │
│ │Tickets│  │gress │  │Resolv│  │Res   │            │
│ └──────┘  └──────┘  └──────┘  └──────┘            │
│                                                    │
│ Tickets per Day (chart)           By Category (pie)│
│ ┌────────────────────┐           ┌──────────────┐  │
│ │ ████▇▆▅▄▃▂▁       │           │  🟦 Tech 45% │  │
│ │ │││││││││││││││   │           │  🟧 Billing │  │
│ └────────────────────┘           │    30%       │  │
│                                   │  🟨 Account │  │
│                                   │    25%       │  │
│                                   └──────────────┘  │
└─────────────────────────────────────────────────────┘
```

## Interactions & States

**Ticket Workflow:**
- Open → In Progress → Resolved → Closed
- Pending (waiting on customer) from any non-closed state
- Bulk select tickets for batch status change
- Keyboard shortcuts: `R` reply, `S` status, `A` assign

**Reply Composer:**
- Rich text editor with formatting toolbar
- Quick replies / saved templates
- Attach file (mock)
- Toggle internal note
- Canned responses dropdown

**Customer Info:**
- Click to expand full customer profile
- Ticket history for this customer
- Quick actions: Email customer, View previous tickets

**Animations:**
- Kanban drag: smooth card transition
- New message: slide-in animation
- Status change: badge color transition
- Dashboard numbers: count-up animation

**Empty States:**
- No tickets: "Semua tiket sudah selesai! 🎉"
- No search results: "Tidak ada tiket yang cocok dengan pencarian."
- No messages: "Belum ada percakapan. Mulai dengan membalas tiket."

## Example Mock Data
```typescript
const mockTickets: Ticket[] = [
  {
    id: '1',
    ticketNumber: 'TKT-001',
    subject: 'Tidak bisa login setelah reset password',
    description: 'Saya sudah reset password tapi masih tidak bisa login...',
    status: 'open',
    priority: 'urgent',
    category: 'Teknis',
    customerId: 'cust-1',
    assigneeId: 'agent-1',
    tags: ['login', 'password'],
    source: 'web',
    createdAt: new Date('2025-01-15T08:30:00'),
    updatedAt: new Date('2025-01-15T08:30:00'),
    resolvedAt: null
  },
  // ... 15-20 more entries
]

const mockMessages: TicketMessage[] = [
  {
    id: 'msg-1',
    ticketId: '1',
    sender: 'customer',
    senderName: 'Budi Santoso',
    senderAvatar: null,
    content: 'Saya tidak bisa login. Pesan error: "Invalid credentials" muncul terus.',
    attachments: [],
    isInternal: false,
    createdAt: new Date('2025-01-15T08:30:00')
  },
  // ...
]
```

## Language & Localization
- UI Labels: Bahasa Indonesia
- Status: "Terbuka", "Diproses", "Menunggu", "Terselesaikan", "Ditutup"
- Priority: "Rendah", "Sedang", "Tinggi", "Darurat"
- Dates: "15 Jan 2025" format
- Actions: "Balas", "Assign", "Selesaikan", "Tutup", "Catatan Internal"
- Empty states: Casual Indonesian ("Semua tiket beres! 🎉")

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Ticket list in table and kanban views
- ✅ Ticket detail with conversation thread
- ✅ Reply composer with rich text and internal notes
- ✅ Customer info panel with ticket history
- ✅ Dashboard metrics and charts
- ✅ Filter and search tickets
- ✅ Drag-and-drop kanban status change
- ✅ Responsive across desktop/tablet
- ✅ Smooth animations and micro-interactions
- ✅ NO backend calls, NO API integration
- ✅ Ready to connect to real API later (clear data layer separation)
