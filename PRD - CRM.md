
## Project
Customer Relationship Management - Static Boilerplate
A modern CRM for managing contacts, deals pipeline, and customer interactions.

## Core Layout: 3-Panel CRM

1. **Left Sidebar (Navigation - 20%)**
   - Dashboard, Contacts, Deals, Pipeline, Activities, Reports
   - Quick action: "+ Tambah Kontak"
   - Contact groups / lists section
   - Favorites

2. **Center Panel (List/Detail - 55%)**
   - Contacts table: name, company, email, phone, status, last activity
   - Deals table: name, contact, value, stage, probability, close date
   - Kanban pipeline view toggle
   - Search + filters (tags, status, source, date range)

3. **Right Panel (Detail Sidebar - 25%)**
   - Contact profile: avatar, name, company, contact info
   - Deal details: value, stage, notes
   - Activity timeline: calls, emails, meetings, notes
   - Quick actions: Log activity, Create deal, Send email

## Data Structure (Mock Only)

```typescript
type DealStage = 'lead' | 'qualified' | 'proposal' | 'negotiation' | 'closed_won' | 'closed_lost'
type ContactStatus = 'active' | 'inactive' | 'lead' | 'customer'
type ActivityType = 'call' | 'email' | 'meeting' | 'note' | 'task'

interface CrmContact {
  id: string
  name: string
  company: string
  position: string
  email: string
  phone: string
  avatar: string | null
  status: ContactStatus
  source: 'referral' | 'website' | 'social_media' | 'event' | 'cold_call'
  tags: string[]
  deals: string[]
  lastActivity: Date | null
  createdAt: Date
  updatedAt: Date
}

interface Deal {
  id: string
  name: string
  contactId: string
  value: number
  stage: DealStage
  probability: number
  expectedCloseDate: Date | null
  notes: string
  products: string[]
  createdAt: Date
  updatedAt: Date
}

interface Activity {
  id: string
  contactId: string
  dealId: string | null
  type: ActivityType
  subject: string
  description: string
  outcome: string | null
  duration: number | null
  performedBy: string
  createdAt: Date
}

interface CrmPipeline {
  id: string
  name: string
  stages: DealStage[]
  deals: string[]
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Icons:** Lucide React
- **Components:** shadcn/ui (Radix-based, copy-paste components)
- **Animations:** Framer Motion
- **Date Utils:** date-fns (with `id` locale)
- **Charts:** Recharts for pipeline metrics

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Professional, relationship-focused, modern SaaS
- **Color Palette:**
  - Primary: Blue (trust, relationship) - `blue-600`
  - Accent: Green (growth, deals won) - `green-500`
  - Pipeline stages: gradient from red → yellow → green
  - Neutral: Slate

## UI Components Specification

**Pipeline Kanban:**
```
┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
│ Lead     │ │Qualified │ │ Proposal │ │Negotiat. │
│ (8)      │ │ (5)      │ │ (3)      │ │ (2)      │
│ ┌──────┐ │ │ ┌──────┐ │ │ ┌──────┐ │ │ ┌──────┐ │
│ │PT ABC│ │ │ │PT XYZ│ │ │ │PT DEF│ │ │ │PT GHI│ │
│ │Rp50jt│ │ │ │Rp120 │ │ │ │Rp200 │ │ │ │Rp350 │ │
│ │Budi  │ │ │ │Siti  │ │ │ │Ami   │ │ │ │Deni  │ │
│ └──────┘ │ │ └──────┘ │ │ └──────┘ │ │ └──────┘ │
└──────────┘ └──────────┘ └──────────┘ └──────────┘
```

**Contact Card:**
```
┌────────────────────────────────────────────────────┐
│ 👤 Budi Santoso         PT Maju Jaya              │
│   budi@maju.com         0812-3456-7890            │
│   Customer              Terakhir: 2 hari lalu      │
│   Tags: [vip] [jakarta]                            │
│   [Log Activity] [Buat Deal] [Kirim Email] [⋮]    │
└────────────────────────────────────────────────────┘
```

## Interactions & States
- Drag deal between pipeline stages
- Click contact to open detail sidebar
- Log activity modal with type selector
- Search across contacts, deals, activities

**Empty States:**
- No contacts: "Belum ada kontak. Tambah kontak baru!"
- No deals: "Belum ada deal. Buat deal dari kontak."

## Example Mock Data
```typescript
const mockContacts: CrmContact[] = [
  { id: 'c1', name: 'Budi Santoso', company: 'PT Maju Jaya', position: 'CEO', email: 'budi@maju.com', phone: '0812-3456-7890', avatar: null, status: 'customer', source: 'referral', tags: ['vip', 'jakarta'], deals: ['d1'], lastActivity: new Date('2025-01-13'), createdAt: new Date('2024-06-01'), updatedAt: new Date('2025-01-13') },
]

const mockDeals: Deal[] = [
  { id: 'd1', name: 'ERP Implementation', contactId: 'c1', value: 350000000, stage: 'negotiation', probability: 70, expectedCloseDate: new Date('2025-03-01'), notes: 'Budget approved, finalizing contract.', products: ['ERP Suite'], createdAt: new Date('2024-12-01'), updatedAt: new Date('2025-01-15') },
]
```

## Language & Localization
- Bahasa Indonesia: "Kontak", "Deal", "Pipeline", "Aktivitas"
- Activities: "Telepon", "Email", "Meeting", "Catatan", "Tugas"
- Stages: "Prospek", "Kualifikasi", "Proposal", "Negosiasi", "Menang", "Hilang"

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Contact management with full CRUD
- ✅ Deal pipeline with Kanban view
- ✅ Activity timeline per contact/deal
- ✅ Drag-and-drop stage changes
- ✅ Search and filter across entities
- ✅ Responsive across desktop/tablet
- ✅ NO backend calls, NO API integration
