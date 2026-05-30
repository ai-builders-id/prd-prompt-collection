
## Project
Point of Sales - Static Boilerplate
A modern POS system for retail transactions with product catalog, cart management, and receipt generation.

## Core Layout: 3-Panel POS

1. **Left Panel (Product Catalog - 40%)**
   - Search bar with barcode simulation
   - Category tabs/chips (Makanan, Minuman, Elektronik, dll)
   - Product grid: image, name, price, stock indicator
   - Quick-add button on each product card

2. **Center Panel (Cart/Transaction - 40%)**
   - Cart items list with qty controls (+/-)
   - Each row: product name, price, qty, subtotal, remove button
   - Discount input (percentage or nominal)
   - Notes field per item

3. **Right Panel (Payment & Summary - 20%)**
   - Subtotal, discount, tax, total
   - Payment method selector (Tunai, Kartu, QRIS, Transfer)
   - Amount paid input
   - Change calculation (auto)
   - Bayar button (large, prominent)

## Data Structure (Mock Only)
Use hardcoded mock data arrays - NO backend/API:

```typescript
interface PosProduct {
  id: string
  name: string
  sku: string
  barcode: string
  price: number // IDR
  categoryId: string
  image: string
  stock: number
  unit: string // pcs, kg, liter
  isActive: boolean
}

interface PosCategory {
  id: string
  name: string
  icon: string // emoji or lucide icon name
  color: string
}

interface CartItem {
  productId: string
  name: string
  price: number
  quantity: number
  discount: number // per-item discount
  note: string
}

interface Transaction {
  id: string
  receiptNumber: string
  items: CartItem[]
  subtotal: number
  discountTotal: number
  taxTotal: number
  grandTotal: number
  paymentMethod: 'cash' | 'card' | 'qris' | 'transfer'
  amountPaid: number
  change: number
  customerName: string
  cashierName: string
  createdAt: Date
}

interface PosCustomer {
  id: string
  name: string
  phone: string
  totalTransactions: number
  totalSpent: number
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Icons:** Lucide React
- **Components:** shadcn/ui (Radix-based, copy-paste components)
- **Animations:** Framer Motion for smooth transitions
- **Date Utils:** date-fns (with `id` locale)
- **Barcode:** Use simple visual representation (no scanner needed)

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Clean, fast, retail-ready — large touch targets
- **Color Palette:**
  - Primary: Green (money, growth) - `green-600`
  - Accent: Amber (discounts, alerts) - `amber-500`
  - Neutral: Slate - `slate-50` to `slate-900`
  - Danger: Red-500 (remove items, void)
  - Payment: Blue for card, Purple for QRIS

- **Typography:**
  - Font: Inter
  - Product name: text-sm font-medium
  - Price: text-lg font-bold tabular-nums
  - Total: text-2xl font-bold
  - Receipt: font-mono text-xs

- **Spacing & Layout:**
  - Product card: p-3 rounded-lg border
  - Cart item: p-3 border-b
  - Payment panel: sticky bottom-0 with shadow
  - Large buttons: min-h-[48px] for touch

## UI Components Specification

**Product Card:**
```
┌──────────────┐
│   🥤        │
│ Coca Cola   │
│ Rp 8.000    │
│ Stok: 45    │
│  [+ Tambah] │
└──────────────┘
```
- Category color strip on left edge
- Stock: green (≥10), amber (≤5), red (0)
- Tap card to add to cart
- Long press for product detail

**Cart Item Row:**
```
┌──────────────────────────────────────────────────────┐
│ Coca Cola                               Rp 8.000     │
│ [-](2)[+]   Catatan: [___________]   [🗑]  Rp 16.000 │
└──────────────────────────────────────────────────────┘
```
- Quantity stepper with +/- buttons
- Subtotal auto-calculated
- Remove button with confirmation

**Payment Panel:**
```
┌──────────────────────────────────────┐
│ Subtotal                  Rp 120.000 │
│ Diskon (10%)             -Rp 12.000  │
│ PPN (11%)                +Rp 13.200  │
│ ═══════════════════════════════════ │
│ Total                     Rp 121.200 │
│                                      │
│ Metode: [💵 Tunai] [💳 Kartu] [📱QRIS]│
│                                      │
│ Dibayar:   [Rp 150.000         ]    │
│ Kembali:   Rp 28.800                │
│                                      │
│ ┌────────────────────────────────┐   │
│ │        BAYAR (Rp 121.200)      │   │
│ └────────────────────────────────┘   │
└──────────────────────────────────────┘
```
- Large green Bayar button
- Auto-calculate change when amount entered
- Disabled state when amount < total

**Receipt Preview:**
```
┌──────────────────────────┐
│   TOKO MAKMUR JAYA       │
│ Jl. Merdeka No. 123      │
│                          │
│ Struk: STR-2025-01-0001  │
│ Kasir: Admin             │
│ 15 Jan 2025 14:30        │
│ ──────────────────────── │
│ Coca Cola     2x 8.000   │
│ Indomie       3x 3.500   │
│ Telur 1kg     1x 28.000  │
│ ──────────────────────── │
│ Subtotal           51.500│
│ Diskon            -5.000 │
│ Total              46.500│
│ Tunai             50.000 │
│ Kembali            3.500 │
│ ──────────────────────── │
│ Terima kasih!            │
└──────────────────────────┘
```
- Print-style layout with monospace font
- Store name and address header
- Line items with qty and price

## Interactions & States

**Cart Management:**
- Tap product to add (default qty 1)
- Swipe cart item to remove
- Combined discount or per-item discount
- Hold transaction (suspend and recall)

**Payment Flow:**
- Select payment method → enter amount paid → auto-change → confirm
- Success: receipt preview + "Transaksi Berhasil" toast
- Failed validation: highlight errors

**Search:**
- Real-time search by name or SKU
- Barcode number input (mock)
- Filter by category

**Animations:**
- Product add: item flies to cart icon
- Quantity change: spring animation
- Payment success: confetti effect
- Receipt: print-like slide animation

**Empty States:**
- No products: "Tidak ada produk. Tambahkan produk di menu Master."
- Empty cart: "Belum ada item. Pilih produk dari katalog."
- No search results: "Produk tidak ditemukan."

## Example Mock Data
```typescript
const mockPosCategories: PosCategory[] = [
  { id: 'cat-1', name: 'Makanan', icon: '🍔', color: 'orange' },
  { id: 'cat-2', name: 'Minuman', icon: '🥤', color: 'blue' },
  { id: 'cat-3', name: 'Snack', icon: '🍿', color: 'amber' },
  { id: 'cat-4', name: 'Elektronik', icon: '🔌', color: 'violet' },
]

const mockPosProducts: PosProduct[] = [
  { id: 'prod-1', name: 'Coca Cola', sku: 'MIN-001', barcode: '8991234567890', price: 8000, categoryId: 'cat-2', image: '/mock/products/coca-cola.png', stock: 45, unit: 'pcs', isActive: true },
  { id: 'prod-2', name: 'Indomie Goreng', sku: 'MKN-001', barcode: '8991234567891', price: 3500, categoryId: 'cat-1', image: '/mock/products/indomie.png', stock: 120, unit: 'pcs', isActive: true },
  // ... 20-30 more products
]

const mockTransactions: Transaction[] = [
  {
    id: 'trx-1',
    receiptNumber: 'STR-2025-01-0001',
    items: [
      { productId: 'prod-1', name: 'Coca Cola', price: 8000, quantity: 2, discount: 0, note: '' },
      { productId: 'prod-2', name: 'Indomie Goreng', price: 3500, quantity: 3, discount: 0, note: '' },
    ],
    subtotal: 26500,
    discountTotal: 0,
    taxTotal: 0,
    grandTotal: 26500,
    paymentMethod: 'cash',
    amountPaid: 50000,
    change: 23500,
    customerName: 'Umum',
    cashierName: 'Admin',
    createdAt: new Date('2025-01-15T14:30:00')
  },
  // ... 10-15 more transactions
]
```

## Language & Localization
- UI Labels: Bahasa Indonesia
- Prices: IDR with dot separator (Rp 8.000)
- Actions: "Tambah", "Bayar", "Batal", "Simpan"
- Payment: "Tunai", "Kartu", "QRIS", "Transfer"
- Receipt: "Struk", "Kasir", "Kembali", "Terima kasih!"
- Empty states: Casual Indonesian

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Product catalog with search and category filter
- ✅ Cart with qty controls and discount
- ✅ Payment panel with method selection and change calculation
- ✅ Receipt preview (mock print)
- ✅ Transaction history list
- ✅ Responsive across desktop/tablet
- ✅ Touch-friendly (min 48px targets)
- ✅ Smooth animations and micro-interactions
- ✅ NO backend calls, NO API integration
- ✅ Ready to connect to real API later (clear data layer separation)
