## Project
Enterprise Resource Planning - Static Boilerplate
An integrated ERP dashboard providing unified access to Finance, HR, Inventory, Sales, and Procurement modules.

## Core Layout: Modular ERP

1. **Global Navigation (Top Bar)**
   - ERP logo + company name
   - Module switcher (dropdown or mega-menu): Finance, HR, Inventory, Sales, Procurement, Production
   - Global search (⌘K)
   - User profile, notifications, settings

2. **Module-Specific Left Sidebar (20%)**
   - Module-dependent navigation (e.g., in Finance: Chart of Accounts, Journal, Reports)
   - Quick actions relevant to current module
   - Module dashboard link

3. **Main Content Area (55%)**
   - Module dashboard or selected view
   - Unified data tables with standardized interactions
   - Drill-down capability across modules

4. **Context Panel (Right - 25%)**
   - Contextual detail based on selected item
   - Cross-module references (e.g., see customer's sales orders AND payments)
   - Quick edit / actions

## Data Structure (Mock Only)
Reuses schemas from sub-modules (Accounting, HR, SCM, CRM, Procurement) with cross-references:

```typescript
// Shared master data
interface Company {
  id: string
  name: string
  taxId: string
  address: string
  phone: string
  email: string
  fiscalYearStart: number // month
  currency: string
  logo: string | null
}

interface ErpUser {
  id: string
  name: string
  email: string
  role: string
  permissions: ErpPermission[]
  modules: string[] // accessible modules
  avatar: string | null
}

interface ErpPermission {
  module: string
  access: 'view' | 'create' | 'edit' | 'delete' | 'approve'
}

// Cross-module notifications / activities
interface ErpActivity {
  id: string
  type: 'journal_posted' | 'invoice_created' | 'po_approved' | 'payment_received' | 'inventory_adjusted' | 'leave_approved'
  module: string
  reference: string
  description: string
  userId: string
  createdAt: Date
}

// Module-level dashboard aggregations
interface ModuleDashboard {
  finance: {
    cashBalance: number
    accountsReceivable: number
    accountsPayable: number
    monthlyRevenue: number
    monthlyExpense: number
    recentTransactions: JournalEntry[]
  }
  hr: {
    totalEmployees: number
    presentToday: number
    pendingLeaves: number
    upcomingBirthdays: Employee[]
    payrollDue: number
  }
  inventory: {
    totalItems: number
    lowStockItems: number
    pendingReceipts: number
    warehouseUtilization: number
    recentMovements: StockMovement[]
  }
  sales: {
    totalOrders: number
    pendingOrders: number
    monthlyRevenue: number
    topCustomers: CrmContact[]
    recentDeals: Deal[]
  }
  procurement: {
    pendingPOs: number
    activeVendors: number
    monthlySpend: number
    pendingApprovals: number
  }
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Routing:** React Router v6 (nested routes per module)
- **Icons:** Lucide React (module icons + action icons)
- **Components:** shadcn/ui (Radix-based, copy-paste components)
- **Animations:** Framer Motion for smooth transitions
- **Date Utils:** date-fns (with `id` locale)
- **Charts:** Recharts for dashboard metrics
- **State:** Zustand for global module state + localStorage mock

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Enterprise-grade, unified, professional
- **Color Palette:**
  - Primary: Indigo (corporate identity) - `indigo-600`
  - Module colors:
    - Finance: Emerald - `emerald-600`
    - HR: Pink - `pink-600`
    - Inventory: Cyan - `cyan-600`
    - Sales/Sales: Blue - `blue-600`
    - Procurement: Amber - `amber-600`
  - Neutral: Slate - `slate-50` to `slate-900`
  - Status indicators: consistent across modules

- **Typography:**
  - Font: Inter
  - Module heading: text-xl font-semibold with module color
  - Data tables: text-sm
  - Metrics: text-2xl font-bold tabular-nums

- **Spacing & Layout:**
  - Container: max-w-full
  - Module dashboard: grid grid-cols-2 lg:grid-cols-4 gap-4
  - Data tables: rounded-xl border shadow-sm
  - Consistent card, button, form styles across modules

## UI Components Specification

**Global Module Switcher:**
```
┌──────────────────────────────────────────────────────────────────┐
│ [ERP Logo]  🏢 PT Maju Jaya                                   │
│                                                                  │
│ [💹 Finance] [👥 HR] [📦 Inventory] [🤝 Sales] [📋 Procurement]│
│                                                                  │
│ [🔍 Cari invoice, karyawan, produk...]     [🔔 3] 👤 Admin    │
└──────────────────────────────────────────────────────────────────┘
```
- Module icons with colors
- Active module: filled
- Notifications badge per module

**Unified Dashboard (Home):**
```
┌──────────────────────────────────────────────────────────────────┐
│ 📊 ERP Dashboard                          Period: [Jan 2025 ▼] │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐               │
│ │ Finance │ │ HR      │ │Inventory│ │ Sales   │               │
│ │ Kas:    │ │Karyawan:│ │ Item:   │ │ Orders: │               │
│ │ Rp 500jt│ │ 120 org │ │ 1.250   │ │ 45 bulan│               │
│ │ AR: 200 │ │Cuti: 5  │ │Habisk: 8│ │ Revenue │               │
│ │ AP: 150 │ │Payroll  │ │         │ │Rp 2M   │               │
│ └─────────┘ └─────────┘ └─────────┘ └─────────┘               │
│                                                                  │
│ Recent Activities                          Pending Approvals     │
│ ┌────────────────────────┐  ┌────────────────────────┐          │
│ │ ✅ Jurnal diposting    │  │ ⏳ PO-2025-001         │          │
│ │ 📦 Barang diterima     │  │ ⏳ Cuti: Budi (5 hari)│          │
│ │ 💰 Pembayaran masuk   │  │ ⏳ Invoice: PT ABC    │          │
│ │ 👤 Karyawan baru      │  │ [Lihat Semua →]        │          │
│ └────────────────────────┘  └────────────────────────┘          │
└──────────────────────────────────────────────────────────────────┘
```

**Module Dashboard (Finance Example):**
```
┌──────────────────────────────────────────────────────────────────┐
│ 💹 Finance                         [Buku Besar] [Jurnal] [Laporan]│
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐               │
│ │ Kas     │ │Piutang  │ │ Hutang   │ │ Revenue │               │
│ │ Rp 500jt│ │Rp 200jt │ │ Rp 150jt │ │ Rp 2M  │               │
│ │ +15%    │ │ -5%     │ │ +10%     │ │ +25%   │               │
│ └─────────┘ └─────────┘ └─────────┘ └─────────┘               │
│                                                                  │
│ Revenue Trend                    AR Aging                        │
│ ┌────────────────────┐          ┌────────────────────┐          │
│ │ ██▇▆▅▄▃▂▁         │          │ 0-30: 60%         │          │
│ │ ││││││││││││││  │          │ 31-60: 25%        │          │
│ └────────────────────┘          │ 61+: 15%          │          │
│                                  └────────────────────┘          │
└──────────────────────────────────────────────────────────────────┘
```

**Cross-Module Context Panel:**
```
┌──────────────────────────────────────────────────────────────────┐
│ PT ABC Corporation                   [Customer]                  │
│ ──────────────────────────────────────────────────────────────── │
│ Contacts:                                                       │
│ 👤 Budi Santoso (CEO) — budi@abc.com — 0812-3456-7890         │
│                                                                  │
│ Sales Orders (3 active):                                        │
│ 🔵 SO-001  Server Package     Rp 450 jt    ✅ Delivered        │
│ 🔵 SO-002  Maintenance        Rp 50 jt     🟡 In Progress      │
│ 🔵 SO-003  Software License   Rp 120 jt    📄 Draft             │
│                                                                  │
│ Invoices:                                                       │
│ 📄 INV-001  Rp 450 jt    💰 Paid                               │
│ 📄 INV-002  Rp 50 jt     ⏳ Overdue 15 days                    │
│                                                                  │
│ Support Tickets (2 open):                                       │
│ 🎫 TKT-045  Server down     🟡 In Progress                     │
│ 🎫 TKT-052  Billing issue   🔴 High                            │
│                                                                  │
│ [Buat Invoice] [Create Order] [Log Activity]                    │
└──────────────────────────────────────────────────────────────────┘
```

## Interactions & States

**Module Navigation:**
- Click module icon → load module dashboard
- Back/Forward browser navigation works
- Global search across all modules

**Cross-Module Features:**
- Customer/Contact appears in Sales, Support, Finance
- Product appears in Inventory, Sales, Procurement
- Employee appears in HR, Payroll, Attendance
- Unified search bar (⌘K) searches all modules

**State Management:**
- Zustand store per module
- Shared store for auth, navigation, global search
- Data is mock + localStorage for persistence

**Animations:**
- Module transition: fade + slide
- Dashboard metrics: count-up animation
- Global search: modal overlay with results
- Context panel: slide from right

**Empty States:**
- Per-module empty states (as defined in individual PRDs)
- Global: "Selamat datang di ERP. Mulai dengan mengisi data master."

## Example Mock Data
```typescript
const mockCompany: Company = {
  id: 'comp-1',
  name: 'PT Maju Jaya',
  taxId: '01.234.567.8-901.000',
  address: 'Jl. Sudirman No. 123, Jakarta',
  phone: '021-1234567',
  email: 'info@majujaya.com',
  fiscalYearStart: 1,
  currency: 'IDR',
  logo: null,
}

const mockErpUsers: ErpUser[] = [
  { id: 'u1', name: 'Admin', email: 'admin@majujaya.com', role: 'Super Admin', permissions: [{ module: '*', access: 'approve' }], modules: ['finance', 'hr', 'inventory', 'sales', 'procurement'], avatar: null },
]

const mockErpDashboard: ModuleDashboard = {
  finance: {
    cashBalance: 500000000,
    accountsReceivable: 200000000,
    accountsPayable: 150000000,
    monthlyRevenue: 2000000000,
    monthlyExpense: 1500000000,
    recentTransactions: [],
  },
  hr: {
    totalEmployees: 120,
    presentToday: 108,
    pendingLeaves: 5,
    upcomingBirthdays: [],
    payrollDue: 750000000,
  },
  inventory: {
    totalItems: 1250,
    lowStockItems: 8,
    pendingReceipts: 3,
    warehouseUtilization: 72,
    recentMovements: [],
  },
  sales: {
    totalOrders: 45,
    pendingOrders: 12,
    monthlyRevenue: 2000000000,
    topCustomers: [],
    recentDeals: [],
  },
  procurement: {
    pendingPOs: 6,
    activeVendors: 24,
    monthlySpend: 500000000,
    pendingApprovals: 3,
  },
}
```

## Language & Localization
- Bahasa Indonesia across all modules (consistent with sub-modules)
- Module names: "Keuangan", "SDM", "Inventory", "Penjualan", "Pembelian"
- Global: "Dashboard", "Pencarian", "Notifikasi", "Pengaturan"
- Consistent date, currency, number formatting

## Success Criteria
- ✅ Global navigation with module switcher
- ✅ Unified landing dashboard with KPI cards per module
- ✅ Each module dashboard renders with its specific metrics
- ✅ Cross-module search (⌘K)
- ✅ Context panel shows related entities across modules
- ✅ Global notification center
- ✅ Module-level CRUD (via each module's components)
- ✅ Consistent UI/UX across all modules
- ✅ Mock data demonstrating cross-module relationships
- ✅ Responsive across desktop/tablet
- ✅ NO backend calls, NO API integration
