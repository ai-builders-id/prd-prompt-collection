
## Project
E-Commerce - Static Boilerplate
A modern online storefront with product catalog, shopping cart, and multi-step checkout.

## Core Layout: Multi-Page Storefront

1. **Top Navigation (Global Header)**
   - Logo + store name (left)
   - Search bar (center)
   - Icons: Akun, Wishlist (❤️), Cart with badge (🛒)
   - Category mega-menu on hover

2. **Home Page (Hero + Featured)**
   - Hero banner carousel (promotions)
   - Featured products grid
   - Category showcase cards
   - Trending section

3. **Product Listing Page (PLP)**
   - Left sidebar: filters (category, price range, rating, brand)
   - Main: product grid (3-4 columns) or list toggle
   - Sort dropdown: Terpopuler, Termurah, Termahal, Terbaru
   - Pagination or infinite scroll

4. **Product Detail Page (PDP)**
   - Left: image gallery with zoom
   - Right: product info, price, variant selector, qty, add to cart
   - Below: description, specs, reviews

5. **Cart (Slide-over Drawer)**
   - Item list with image, name, variant, qty, price
   - Promo code input
   - Subtotal + checkout button

6. **Checkout (Multi-Step Flow)**
   - Step 1: Alamat Pengiriman
   - Step 2: Metode Pengiriman (JNE, SiCepat, Gosend)
   - Step 3: Metode Pembayaran (Transfer, COD, Kartu, QRIS)
   - Step 4: Konfirmasi Pesanan

## Data Structure (Mock Only)
Use hardcoded mock data arrays - NO backend/API:

```typescript
interface EcommerceProduct {
  id: string
  name: string
  slug: string
  description: string
  price: number // IDR
  compareAtPrice: number | null // original price for discount display
  images: string[]
  categoryId: string
  tags: string[]
  rating: number
  reviewCount: number
  stock: number
  isFeatured: boolean
  variants: ProductVariant[]
  createdAt: Date
}

interface ProductVariant {
  id: string
  name: string // e.g. "36", "Merah", "250ml"
  price: number | null // null = same as base price
  stock: number
}

interface EcommerceCategory {
  id: string
  name: string
  slug: string
  image: string
  parentId: string | null
  productCount: number
}

interface CartItem {
  productId: string
  variantId: string | null
  name: string
  image: string
  price: number
  quantity: number
}

interface Order {
  id: string
  orderNumber: string
  items: CartItem[]
  subtotal: number
  shippingCost: number
  discount: number
  tax: number
  grandTotal: number
  status: 'pending' | 'confirmed' | 'packed' | 'shipped' | 'delivered' | 'cancelled'
  paymentMethod: string
  paymentStatus: 'paid' | 'unpaid' | 'refunded'
  shippingAddress: ShippingAddress
  shippingMethod: string
  trackingNumber: string | null
  notes: string
  createdAt: Date
}

interface ShippingAddress {
  name: string
  phone: string
  street: string
  city: string
  district: string
  province: string
  postalCode: string
  isDefault: boolean
}

interface ProductReview {
  id: string
  productId: string
  userName: string
  rating: number
  content: string
  images: string[]
  createdAt: Date
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Icons:** Lucide React
- **Components:** shadcn/ui (Radix-based, copy-paste components)
- **Animations:** Framer Motion for smooth transitions
- **Date Utils:** date-fns (with `id` locale)
- **Image Gallery:** Yet Another React Lightbox
- **Currency Formatter:** Custom IDR formatter

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Modern Indonesian e-commerce — clean, vibrant, trustworthy
- **Color Palette:**
  - Primary: Orange (warmth, e-commerce standard) - `orange-500`
  - Secondary: Blue (trust) - `blue-600`
  - Accent: Green (positive, stock available) - `green-500`
  - Neutral: Zinc - `zinc-50` to `zinc-900`
  - Danger: Red (sold out, discount tag)

- **Typography:**
  - Font: Inter
  - Product name: text-base font-medium
  - Price: text-xl font-bold text-orange-600
  - Discount badge: text-xs font-bold bg-red-500 text-white
  - Category: text-sm text-zinc-600

- **Spacing & Layout:**
  - Container: max-w-7xl mx-auto px-4
  - Product card: rounded-xl border shadow-sm overflow-hidden
  - Grid: grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4
  - Carousel: h-[300px] md:h-[400px]

## UI Components Specification

**Product Card:**
```
┌───────────────────┐
│ ┌───────────────┐ │
│ │    🖼️ Image   │ │
│ │               │ │
│ └───────────────┘ │
│ ⭐ 4.8 (120)     │
│ Kaos Polos Premium│
│ Rp 89.000         │
│ Rp 129.000  -31%  │
│ [🛒 + Keranjang] │
└───────────────────┘
```
- Image with aspect-ratio 1:1
- Discount badge (red, top-left corner)
- Wishlist heart (top-right corner)
- Rating stars + count
- Compare-at price strikethrough

**Product Detail Page:**
```
┌──────────────────────────────────────────────────────────────┐
│ ┌────────────┐ ┌────────────┐  Kaos Polos Premium           │
│ │   🖼️      │ │   🖼️     │  ⭐ 4.8 (120 ulasan)         │
│ │  Main      │ │  Thumb 1  │                               │
│ │  Image     │ │           │  Rp 89.000                     │
│ └────────────┘ └────────────┘  Rp 129.000  Hemat 31%       │
│   ┌────┐ ┌────┐                                               │
│   │ 🖼️│ │ 🖼️│  Ukuran: [S] [M] [L] [XL]                   │
│   └────┘ └────┘                                               │
│                                     Warna: [⬛] [⬜] [🔵]   │
│ ┌──────────────────────────────────────────────────────────┐ │
│ │ Deskripsi                                                │ │
│ │ Bahan katun 30s, nyaman dipakai...                       │ │
│ │                                                          │ │
│ │ Spesifikasi:                                             │ │
│ │ • Bahan: Katun 30s                                      │ │
│ │ • Ukuran: S-XXL                                         │ │
│ │ • Warna: 5 varian                                       │ │
│ └──────────────────────────────────────────────────────────┘ │
│                                                              │
│ Ulasan (120)                                                 │
│ ⭐⭐⭐⭐⭐ "Bagus banget, bahunya nyaman" - Budi              │
│ ⭐⭐⭐⭐ "Ukurannya pas" - Siti                              │
│                                                              │
│ ┌────────────────────┐  ┌────────────────────┐              │
│ │ ♥ Wishlist        │  │ 🛒 + Keranjang     │              │
│ └────────────────────┘  └────────────────────┘              │
└──────────────────────────────────────────────────────────────┘
```

**Cart Drawer:**
```
┌───────────────────────────────────────┐
│ 🛒 Keranjang Belanja (3)       [X]   │
│ ───────────────────────────────────── │
│ ┌─────────────────────────────────┐   │
│ │ 🖼️ Kaos Polos M x 2            │   │
│ │   Rp 89.000         Rp 178.000  │   │
│ │   [-](2)[+]               [🗑]  │   │
│ ├─────────────────────────────────┤   │
│ │ 🖼️ Celana Chino L x 1          │   │
│ │   Rp 149.000        Rp 149.000  │   │
│ │   [-](1)[+]               [🗑]  │   │
│ └─────────────────────────────────┘   │
│                                       │
│ Kode Promo: [MASUKKAN KODE] [Pakai]  │
│                                       │
│ Subtotal                    Rp 327.000│
│ Ongkir                   *Setelah alamat│
│                                       │
│ ┌────────────────────────────────┐    │
│ │  Checkout (Rp 327.000)        │    │
│ └────────────────────────────────┘    │
└───────────────────────────────────────┘
```

**Checkout Progress:**
```
● Alamat ──→ ● Pengiriman ──→ ● Pembayaran ──→ ○ Konfirmasi
```
- Completed steps: green checkmark
- Current step: blue filled circle
- Future steps: gray outline

## Interactions & States

**Shopping Flow:**
- Add to cart: item slides into cart icon
- Cart badge updates with count
- Remove item: swipe-to-delete or button
- Variant selection: update price dynamically

**Product Search:**
- Real-time autocomplete suggestions
- Filter by category, price range (range slider), rating
- Sort by popularity, price, newest

**Checkout Flow:**
- Multi-step form with validation per step
- Shipping cost estimation based on method
- Order summary updates in real-time
- Success: order confirmation page with number

**Animations:**
- Product card: hover lift effect
- Cart drawer: slide from right
- Image gallery: smooth zoom/pan
- Checkout steps: progress bar animation
- Add to cart: micro-bounce on icon

**Empty States:**
- Empty cart: "Keranjang masih kosong. Yuk belanja!"
- No search results: "Produk tidak ditemukan. Coba kata kunci lain."
- No orders: "Belum ada pesanan. Mulai belanja sekarang!"
- Empty wishlist: "Wishlist masih kosong."

## Example Mock Data
```typescript
const mockEcommerceProducts: EcommerceProduct[] = [
  {
    id: 'prod-1',
    name: 'Kaos Polos Premium',
    slug: 'kaos-polos-premium',
    description: 'Kaos katun 30s dengan bahan premium nyaman dipakai sehari-hari.',
    price: 89000,
    compareAtPrice: 129000,
    images: ['/mock/products/kaos-1.jpg', '/mock/products/kaos-2.jpg', '/mock/products/kaos-3.jpg'],
    categoryId: 'cat-1',
    tags: ['kaos', 'fashion', 'pria'],
    rating: 4.8,
    reviewCount: 120,
    stock: 50,
    isFeatured: true,
    variants: [
      { id: 'var-1', name: 'S', price: null, stock: 15 },
      { id: 'var-2', name: 'M', price: null, stock: 20 },
      { id: 'var-3', name: 'L', price: null, stock: 10 },
    ],
    createdAt: new Date('2025-01-01')
  },
  // ... 20-30 more products
]

const mockOrders: Order[] = [
  {
    id: 'ord-1',
    orderNumber: 'INV-2025-01-0001',
    items: [
      { productId: 'prod-1', variantId: 'var-2', name: 'Kaos Polos Premium (M)', image: '/mock/products/kaos-1.jpg', price: 89000, quantity: 2 },
    ],
    subtotal: 178000,
    shippingCost: 15000,
    discount: 10000,
    tax: 0,
    grandTotal: 183000,
    status: 'delivered',
    paymentMethod: 'Transfer Bank',
    paymentStatus: 'paid',
    shippingAddress: { name: 'Budi Santoso', phone: '08123456789', street: 'Jl. Merdeka No. 123', city: 'Jakarta', district: 'Menteng', province: 'DKI Jakarta', postalCode: '12345', isDefault: true },
    shippingMethod: 'JNE Regular',
    trackingNumber: 'JNE123456789',
    notes: '',
    createdAt: new Date('2025-01-10')
  },
  // ... 10-15 more orders
]
```

## Language & Localization
- UI Labels: Bahasa Indonesia
- Currency: IDR (Rp 89.000 format)
- Product status: "Tersedia", "Habis", "Pre-order"
- Actions: "Masukkan Keranjang", "Beli Sekarang", "Checkout"
- Checkout: "Alamat Pengiriman", "Metode Pengiriman", "Pembayaran", "Konfirmasi"
- Order status: "Menunggu", "Dikemas", "Dikirim", "Selesai", "Dibatalkan"
- Empty states: Casual Indonesian ("Keranjang masih kosong, yuk belanja!")

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Product catalog with grid/list toggle
- ✅ Product detail with image gallery and variants
- ✅ Cart drawer with qty controls and promo code
- ✅ Multi-step checkout flow with validation
- ✅ Order history and detail
- ✅ Search, filter, and sort functionality
- ✅ Responsive across mobile/tablet/desktop
- ✅ Smooth animations and micro-interactions
- ✅ NO backend calls, NO API integration
- ✅ Ready to connect to real API later (clear data layer separation)
