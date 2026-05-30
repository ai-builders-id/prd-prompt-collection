
## Project
Enterprise Asset Management - Static Boilerplate
A modern EAM system for tracking physical assets, maintenance schedules, and work orders.

## Core Layout: 3-Panel EAM

1. **Left Sidebar (Navigation - 20%)**
   - Dashboard, Assets, Work Orders, Maintenance, Locations, Reports
   - Asset category tree / location hierarchy
   - Quick stats: total assets, active WO, overdue maintenance

2. **Center Panel (List/Detail - 55%)**
   - Asset table: asset code, name, category, location, status, condition, assigned to
   - Work order list: WO number, asset, type, priority, status, assigned tech, date
   - Maintenance calendar: scheduled tasks by month
   - Grid/list toggle

3. **Right Panel (Detail Sidebar - 25%)**
   - Asset detail: photo, QR code (display), specs, purchase info, warranty
   - Maintenance history timeline
   - Current work order for asset
   - Actions: Create WO, Transfer, Dispose, Edit

## Data Structure (Mock Only)

```typescript
type AssetStatus = 'active' | 'under_maintenance' | 'retired' | 'transferred' | 'lost'
type AssetCondition = 'excellent' | 'good' | 'fair' | 'poor' | 'damaged'
type WOPriority = 'low' | 'medium' | 'high' | 'emergency'
type WOStatus = 'open' | 'assigned' | 'in_progress' | 'completed' | 'cancelled'
type MaintenanceType = 'preventive' | 'corrective' | 'predictive' | 'emergency'

interface Asset {
  id: string
  assetCode: string
  name: string
  category: string
  subCategory: string
  brand: string
  model: string
  serialNumber: string
  location: string
  department: string
  assignedTo: string | null
  status: AssetStatus
  condition: AssetCondition
  purchaseDate: Date
  purchasePrice: number
  currentValue: number
  warrantyExpiry: Date | null
  expectedLife: number // years
  image: string | null
  notes: string
  createdAt: Date
  updatedAt: Date
}

interface WorkOrder {
  id: string
  woNumber: string
  assetId: string
  title: string
  description: string
  type: MaintenanceType
  priority: WOPriority
  status: WOStatus
  assignedTo: string | null
  scheduledDate: Date
  completedDate: Date | null
  estimatedHours: number
  actualHours: number | null
  parts: PartUsed[]
  cost: number
  notes: string
  createdAt: Date
}

interface PartUsed {
  name: string
  quantity: number
  unitPrice: number
  totalPrice: number
}

interface MaintenanceSchedule {
  id: string
  assetId: string
  type: MaintenanceType
  frequency: 'daily' | 'weekly' | 'monthly' | 'quarterly' | 'yearly'
  lastPerformed: Date | null
  nextDue: Date
  assignedTeam: string
  instructions: string
}

interface AssetCategory {
  id: string
  name: string
  parentId: string | null
  icon: string
  assetCount: number
}

interface AssetLocation {
  id: string
  name: string
  parentId: string | null
  type: 'building' | 'floor' | 'room' | 'outdoor'
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Icons:** Lucide React
- **Components:** shadcn/ui
- **Animations:** Framer Motion
- **Date Utils:** date-fns (with `id` locale)
- **Charts:** Recharts for maintenance metrics

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Industrial, robust, enterprise
- **Color Palette:**
  - Primary: Teal (reliability) - `teal-600`
  - Accent: Orange (maintenance alerts) - `orange-500`
  - Condition: Excellent (green), Good (blue), Fair (amber), Poor (orange), Damaged (red)
  - Neutral: Stone

## UI Components Specification

**Asset Row:**
```
┌──────────────────────────────────────────────────────────────────┐
│ 🔧 AST-001  Server Dell PowerEdge R750                        │
│ Kategori: IT Server    Lokasi: DC-01 / Rack A3                 │
│ Status: ✅ Active     Kondisi: 🟢 Excellent                    │
│ Assigned: Budi Santoso (IT Dept)                               │
│ [WO] [Transfer] [Edit] [⋮]                                     │
└──────────────────────────────────────────────────────────────────┘
```

**Asset Detail:**
```
┌──────────────────────────────────────────────────────────────────┐
│ 🔧 Server Dell PowerEdge R750          Kode: AST-001          │
│ ┌────────────┐                                                  │
│ │   🖼️      │  Brand: Dell       Model: PowerEdge R750        │
│ │  Asset     │  SN: DL12345ABC    Tahun: 2024                  │
│ │  Image     │                                                  │
│ └────────────┘  ┌──────────────────────────────────────┐       │
│                 │ ████████████████████▨░░░ QR Code     │       │
│                 │ (display only, visual mock)          │       │
│                 └──────────────────────────────────────┘       │
│                                                              │
│ Purchase: 15 Jan 2024  |  Rp 150.000.000                    │
│ Current Value: Rp 120.000.000  |  Depreciation: 20%        │
│ Warranty: 15 Jan 2027  |  Est. Life: 5 tahun               │
│                                                              │
│ ─── Maintenance History ───                                  │
│ 10 Dec 2024  Preventive     ✅ Completed                     │
│ 05 Sep 2024  Corrective     ✅ Completed (PSU replacement)   │
│ 15 Jun 2024  Preventive     ✅ Completed                     │
│                                                              │
│ [Buat Work Order] [Transfer] [Dispose] [Edit]                │
└──────────────────────────────────────────────────────────────────┘
```

**Work Order Card:**
```
┌──────────────────────────────────────────────────────────────────┐
│ WO-2025-001   Preventive Maintenance — Server R750             │
│ Asset: AST-001  Server Dell PowerEdge                         │
│ Priority: 🟡 Medium      Status: 🔵 In Progress                │
│ Teknisi: Ahmad Fauzi    Schedule: 15 Jan 2025                  │
│ Est: 4 jam    Biaya: Rp 500.000                                │
│ [Complete] [Reschedule] [Add Parts]                            │
└──────────────────────────────────────────────────────────────────┘
```

## Interactions & States
- Asset lifecycle: Active → Maintenance, Transfer, Retire
- WO workflow: Open → Assigned → In Progress → Completed
- Schedule maintenance: calendar-based
- Asset QR code display (visual mock)

**Empty States:**
- No assets: "Belum ada aset terdaftar."
- No work orders: "Tidak ada work order."
- No schedule: "Belum ada jadwal maintenance."

## Example Mock Data
```typescript
const mockAssets: Asset[] = [
  { id: 'a1', assetCode: 'AST-001', name: 'Server Dell PowerEdge R750', category: 'IT', subCategory: 'Server', brand: 'Dell', model: 'PowerEdge R750', serialNumber: 'DL12345ABC', location: 'DC-01 / Rack A3', department: 'IT', assignedTo: 'Budi Santoso', status: 'active', condition: 'excellent', purchaseDate: new Date('2024-01-15'), purchasePrice: 150000000, currentValue: 120000000, warrantyExpiry: new Date('2027-01-15'), expectedLife: 5, image: null, notes: 'Production server', createdAt: new Date('2024-01-15'), updatedAt: new Date('2025-01-10') },
]

const mockWorkOrders: WorkOrder[] = [
  { id: 'wo1', woNumber: 'WO-2025-001', assetId: 'a1', title: 'Preventive Maintenance Q1 2025', description: 'Routine preventive maintenance including cleaning, diagnostics, and firmware update.', type: 'preventive', priority: 'medium', status: 'in_progress', assignedTo: 'Ahmad Fauzi', scheduledDate: new Date('2025-01-15'), completedDate: null, estimatedHours: 4, actualHours: null, parts: [{ name: 'Thermal Paste', quantity: 1, unitPrice: 50000, totalPrice: 50000 }], cost: 500000, notes: '', createdAt: new Date('2025-01-05') },
]
```

## Language & Localization
- Bahasa Indonesia: "Aset", "Work Order", "Maintenance", "Lokasi"
- Asset status: "Aktif", "Maintenance", "Ditarik", "Dipindahtangankan", "Hilang"
- Condition: "Sangat Baik", "Baik", "Cukup", "Buruk", "Rusak"
- Maintenance: "Preventif", "Korektif", "Prediktif", "Darurat"

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Asset CRUD with full lifecycle tracking
- ✅ Work order management with assignment
- ✅ Maintenance schedule calendar
- ✅ Asset history timeline
- ✅ Location/category hierarchy
- ✅ Search and filter assets/WOs
- ✅ Responsive across desktop/tablet
- ✅ NO backend calls, NO API integration
