## Project
Supply Chain Management - Static Boilerplate
A modern SCM system for managing inventory, warehouse operations, supplier orders, and logistics.

## Core Layout: 3-Panel SCM

1. **Left Sidebar (Navigation - 20%)**
   - Dashboard, Inventory, Warehouse, Orders, Suppliers, Shipments, Reports
   - Inventory category tree
   - Warehouse selector

2. **Center Panel (List/Detail - 55%)**
   - Inventory table: SKU, name, category, warehouse, stock, reserved, available
   - Warehouse map/layout view
   - Supplier orders and shipments tracking
   - Low stock alerts highlighted

3. **Right Panel (Detail Sidebar - 25%)**
   - Product detail: image, SKU, barcode, stock levels (by warehouse), supplier info
   - Stock movement history (in/out/adjustment)
   - Reorder suggestions
   - Actions: Receive Stock, Transfer, Adjust, Create PO

## Data Structure (Mock Only)

```typescript
type MovementType = 'inbound' | 'outbound' | 'transfer' | 'adjustment' | 'return'
type OrderStatus = 'pending' | 'confirmed' | 'picked' | 'packed' | 'shipped' | 'delivered' | 'cancelled'

interface Warehouse {
  id: string
  name: string
  code: string
  location: string
  capacity: number
  usedCapacity: number
  isActive: boolean
}

interface InventoryItem {
  id: string
  sku: string
  name: string
  description: string
  category: string
  unit: string
  price: number
  cost: number
  image: string | null
  minStock: number
  maxStock: number
  warehouses: InventoryWarehouse[]
  supplierId: string
  createdAt: Date
}

interface InventoryWarehouse {
  warehouseId: string
  stock: number
  reserved: number
  available: number
  binLocation: string
}

interface StockMovement {
  id: string
  itemId: string
  warehouseId: string
  type: MovementType
  quantity: number
  reference: string // PO number, SO number, etc
  notes: string
  performedBy: string
  createdAt: Date
}

interface SupplierOrder {
  id: string
  orderNumber: string
  supplierId: string
  warehouseId: string
  items: OrderLine[]
  status: OrderStatus
  expectedDate: Date
  receivedDate: Date | null
  notes: string
  createdAt: Date
}

interface OrderLine {
  itemId: string
  sku: string
  name: string
  quantity: number
  receivedQuantity: number
  unitPrice: number
  totalPrice: number
}

interface Supplier {
  id: string
  name: string
  contactPerson: string
  email: string
  phone: string
  address: string
  leadTime: number // days
  paymentTerms: string
  rating: number
  status: 'active' | 'inactive'
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Icons:** Lucide React
- **Components:** shadcn/ui
- **Animations:** Framer Motion
- **Date Utils:** date-fns (with `id` locale)
- **Charts:** Recharts for inventory metrics

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Logistics-focused, industrial, data-dense
- **Color Palette:**
  - Primary: Cyan (logistics) - `cyan-600`
  - Accent: Amber (stock alerts) - `amber-500`
  - Stock levels: High (green), Medium (blue), Low (amber), Critical (red)
  - Neutral: Slate

## UI Components Specification

**Inventory Table:**
```
┌──────────────────────────────────────────────────────────────────────┐
│ SKU        Nama           Kategori   Warehouse  Stock  Avail   Status│
│ ITM-001   Laptop Pro     Electronics WH-01     25     22     🟢    │
│ ITM-002   Monitor 27"    Electronics WH-01     12     10     🟢    │
│ ITM-003   Keyboard Mech  Aksesoris   WH-01     3      2      🟡    │
│ ITM-004   Mouse Wireless Aksesoris   WH-02     1      0      🔴    │
└──────────────────────────────────────────────────────────────────────┘
```
- Color-coded stock status row
- Click row for detail sidebar
- Sortable columns

**Warehouse Layout:**
```
┌──────────────────────────────────────────────────────────────────┐
│ Warehouse: WH-01 — Jakarta    [Capacity: 75%]                   │
│ ┌──────┬──────┬──────┬──────┬──────┬──────┐                     │
│ │ A-01 │ A-02 │ A-03 │ A-04 │ A-05 │ A-06 │                     │
│ │ Lapto│ Monit│      │      │      │      │                     │
│ │ p 25 │ or 12│      │      │      │      │                     │
│ ├──────┼──────┼──────┼──────┼──────┼──────┤                     │
│ │ B-01 │ B-02 │ B-03 │ B-04 │ B-05 │ B-06 │                     │
│ │ Keybo│ Mouse│      │      │      │      │                     │
│ │ ard 3│ 1    │      │      │      │      │                     │
│ └──────┴──────┴──────┴──────┴──────┴──────┘                     │
└──────────────────────────────────────────────────────────────────┘
```

**Stock Movement Timeline:**
```
Stock Movement — Laptop Pro (ITM-001)
┌──────────────────────────────────────────────────────────────────┐
│ ● Inbound    20 pcs    PO-2025-001    10 Jan 2025    ✅        │
│ ● Outbound   2 pcs     SO-2025-001    12 Jan 2025    📦        │
│ ● Transfer   5 pcs     WH-01 → WH-02  14 Jan 2025    🔄        │
│ ● Adjustment -1 pcs    Rusak          15 Jan 2025    ⚠️         │
└──────────────────────────────────────────────────────────────────┘
```

## Interactions & States
- Inventory adjustments with reason codes
- Receive against PO (partial/full)
- Transfer stock between warehouses
- Low stock alerts and reorder suggestions

**Empty States:**
- No inventory: "Belum ada data inventory."
- No movements: "Belum ada pergerakan stok."
- No orders: "Belum ada pesanan."

## Example Mock Data
```typescript
const mockWarehouses: Warehouse[] = [
  { id: 'wh1', name: 'Gudang Jakarta', code: 'WH-01', location: 'Jakarta Utara', capacity: 10000, usedCapacity: 7500, isActive: true },
  { id: 'wh2', name: 'Gudang Surabaya', code: 'WH-02', location: 'Surabaya', capacity: 5000, usedCapacity: 3000, isActive: true },
]

const mockInventory: InventoryItem[] = [
  { id: 'itm1', sku: 'ITM-001', name: 'Laptop Pro 15', description: 'Laptop 15 inch 16GB RAM', category: 'Electronics', unit: 'pcs', price: 15000000, cost: 12000000, image: null, minStock: 5, maxStock: 50, warehouses: [{ warehouseId: 'wh1', stock: 25, reserved: 3, available: 22, binLocation: 'A-01' }], supplierId: 's1', createdAt: new Date('2024-06-01') },
  { id: 'itm2', sku: 'ITM-002', name: 'Monitor 27 inch 4K', description: 'Monitor UHD 27 inch', category: 'Electronics', unit: 'pcs', price: 5000000, cost: 3500000, image: null, minStock: 3, maxStock: 20, warehouses: [{ warehouseId: 'wh1', stock: 12, reserved: 2, available: 10, binLocation: 'A-02' }], supplierId: 's1', createdAt: new Date('2024-06-01') },
]
```

## Language & Localization
- Bahasa Indonesia: "Inventory", "Gudang", "Pergerakan Stok", "Pemasok"
- Status: "Tersedia", "Dipesan", "Dalam Perjalanan", "Diterima"
- Movements: "Masuk", "Keluar", "Transfer", "Penyesuaian", "Retur"
- Alerts: "Stok Menipis", "Stok Kritis", "Stok Berlebih"

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Multi-warehouse inventory tracking
- ✅ Stock movement history with timeline
- ✅ Warehouse bin location system
- ✅ Supplier orders with receiving workflow
- ✅ Low stock alerts and reorder suggestions
- ✅ Transfer between warehouses
- ✅ Warehouse capacity visualization
- ✅ Search and filter inventory
- ✅ Responsive across desktop/tablet
- ✅ NO backend calls, NO API integration
