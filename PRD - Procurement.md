
## Project
Procurement & Spend Management - Static Boilerplate
A modern procurement system for managing purchase orders, vendor management, and budget tracking.

## Core Layout: 3-Panel Procurement

1. **Left Sidebar (Navigation - 20%)**
   - Dashboard, Purchase Orders, Vendors, Budget, Approvals, Reports
   - Quick stats: pending PO, total budget, vendors
   - Approval queue badge

2. **Center Panel (List/Detail - 55%)**
   - PO table: PO number, vendor, amount, status, date, department
   - Vendor list: name, contact, category, rating, status
   - Budget tracker: department budgets with spending bars
   - Tab view: All, Pending Approval, Draft, Completed

3. **Right Panel (Detail Sidebar - 25%)**
   - PO detail: items list, amounts, vendor info
   - Approval flow: who approved, timestamps
   - Budget impact: remaining budget for department
   - Actions: Submit for Approval, Approve, Reject, Receive

## Data Structure (Mock Only)

```typescript
type POStatus = 'draft' | 'pending_approval' | 'approved' | 'rejected' | 'ordered' | 'partially_received' | 'completed' | 'cancelled'
type VendorStatus = 'active' | 'inactive' | 'blacklisted'

interface PurchaseOrder {
  id: string
  poNumber: string
  vendorId: string
  department: string
  items: POItem[]
  subtotal: number
  tax: number
  shipping: number
  total: number
  status: POStatus
  requestedBy: string
  approvedBy: string | null
  notes: string
  deliveryDate: Date | null
  createdAt: Date
  updatedAt: Date
}

interface POItem {
  id: string
  productName: string
  description: string
  quantity: number
  unit: string
  unitPrice: number
  totalPrice: number
  receivedQuantity: number
}

interface Vendor {
  id: string
  name: string
  contactPerson: string
  email: string
  phone: string
  address: string
  category: string[]
  rating: number
  status: VendorStatus
  totalPO: number
  totalSpent: number
  notes: string
  createdAt: Date
}

interface BudgetItem {
  id: string
  department: string
  fiscalYear: number
  totalBudget: number
  spent: number
  remaining: number
  encumbered: number // committed but not yet invoiced
}

interface ApprovalStep {
  id: string
  poId: string
  approver: string
  status: 'pending' | 'approved' | 'rejected'
  comment: string
  createdAt: Date
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Icons:** Lucide React
- **Components:** shadcn/ui
- **Animations:** Framer Motion
- **Date Utils:** date-fns (with `id` locale)
- **Charts:** Recharts for budget visualization

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Corporate procurement, clean and structured
- **Color Palette:**
  - Primary: Blue-gray - `blue-gray-700`
  - Accent: Amber (approvals attention) - `amber-500`
  - Status: Draft (gray), Pending (amber), Approved (green), Rejected (red)
  - Neutral: Slate

## UI Components Specification

**PO Table Row:**
```
┌──────────────────────────────────────────────────────────────────┐
│ PO-2025-001     PT Sumber Makmur            Rp 45.000.000      │
│ Engineering     Budi Santoso          🟡 Pending Approval      │
│ Permintaan: 15 Jan 2025                                        │
│ [Approve] [Reject] [View] [⋮]                                  │
└──────────────────────────────────────────────────────────────────┘
```

**PO Detail:**
```
┌──────────────────────────────────────────────────────────────────┐
│ PO Number: PO-2025-001                     Status: Pending      │
│ Vendor: PT Sumber Makmur                                       │
│ Department: Engineering                                        │
│ ─────────────────────────────────────────────────────────────── │
│ Items:                                                         │
│ #  Item                Qty   Unit   Harga     Total            │
│ 1  Server Dell         2     Unit   15.000    30.000.000       │
│ 2  RAM 32GB           10     Pcs    800.000   8.000.000        │
│ 3  SSD 1TB             5     Pcs    1.400.000 7.000.000        │
│ ─────────────────────────────────────────────────────────────── │
│ Subtotal: Rp 45.000.000                                        │
│ PPN 11%:  Rp 4.950.000                                        │
│ Total:    Rp 49.950.000                                        │
│                                                               │
│ Approval Flow:                                                 │
│ ┌───┐    ┌───┐    ┌───┐                                       │
│ │R1 │ →  │R2 │ →  │R3 │                                       │
│ │✅ │    │⏳ │    │⬜ │                                       │
│ └───┘    └───┘    └───┘                                       │
│ Budi     Siti     Ani                                         │
└──────────────────────────────────────────────────────────────────┘
```

**Vendor Card:**
```
┌──────────────────────────────────────────────────────────────────┐
│ PT Sumber Makmur                               ⭐ 4.5/5        │
│ Kontak: Budi (budi@sumbermakmur.com)                           │
│ Kategori: [IT Hardware] [Aksesoris]                            │
│ Total PO: 24   Total: Rp 1,2M                                  │
│ [Buat PO] [Edit] [Lihat Riwayat]                               │
└──────────────────────────────────────────────────────────────────┘
```

## Interactions & States
- PO workflow: Draft → Submit → Approve/Reject → Order → Receive
- Multi-level approval chain
- Budget check before approval
- Partial receipt of items

**Empty States:**
- No POs: "Belum ada purchase order."
- Pending approvals: "Tidak ada PO yang perlu disetujui."
- No vendors: "Belum ada vendor terdaftar."

## Example Mock Data
```typescript
const mockVendors: Vendor[] = [
  { id: 'v1', name: 'PT Sumber Makmur', contactPerson: 'Budi Hartono', email: 'budi@sumbermakmur.com', phone: '021-1234567', address: 'Jl. Industri Raya No. 45, Jakarta', category: ['IT Hardware', 'Aksesoris'], rating: 4.5, status: 'active', totalPO: 24, totalSpent: 1200000000, notes: 'Vendor utama IT', createdAt: new Date('2023-01-15') },
]

const mockPOs: PurchaseOrder[] = [
  { id: 'po1', poNumber: 'PO-2025-001', vendorId: 'v1', department: 'Engineering', items: [{ id: 'item1', productName: 'Server Dell PowerEdge', description: 'Rack server 2U', quantity: 2, unit: 'Unit', unitPrice: 15000000, totalPrice: 30000000, receivedQuantity: 0 }, { id: 'item2', productName: 'RAM 32GB DDR5', description: 'Server memory', quantity: 10, unit: 'Pcs', unitPrice: 800000, totalPrice: 8000000, receivedQuantity: 0 }], subtotal: 45000000, tax: 4950000, shipping: 0, total: 49950000, status: 'pending_approval', requestedBy: 'Budi Santoso', approvedBy: null, notes: 'Urgent untuk server baru', deliveryDate: new Date('2025-02-15'), createdAt: new Date('2025-01-15'), updatedAt: new Date('2025-01-15') },
]
```

## Language & Localization
- Bahasa Indonesia: "Pembelian", "Vendor", "Anggaran", "Persetujuan"
- PO Status: "Draft", "Menunggu", "Disetujui", "Ditolak", "Dipesan", "Selesai"
- Actions: "Buat PO", "Setujui", "Tolak", "Terima Barang"

## Success Criteria
- ✅ All UI components render with mock data
- ✅ PO CRUD with line items
- ✅ Multi-step approval workflow
- ✅ Vendor management with rating
- ✅ Budget tracking vs spending
- ✅ Partial receipt of items
- ✅ Search and filter POs/vendors
- ✅ Responsive across desktop/tablet
- ✅ NO backend calls, NO API integration
