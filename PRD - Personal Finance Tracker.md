
## Project
Personal Finance Tracker - Static Boilerplate
A clean, modern financial tracking dashboard with mock data visualization.

## Core Layout: 3-Panel Dashboard

1. **Left Sidebar (Categories & Filters - 25%)**
   - Income/Expense toggle
   - Category filters (collapsible sections)
   - Date range selector (daily/weekly/monthly)
   
2. **Center Panel (Transaction Feed - 45%)**
   - Transaction cards with amount, note, category, date
   - Add transaction button (floating action button)
   - Empty state illustration when no data

3. **Right Sidebar (Insights & Charts - 30%)**
   - Summary cards (total income, expense, balance)
   - Pie chart (expense by category)
   - Bar chart (income vs expense trend)

## Data Structure (Mock Only)
Use hardcoded mock data arrays - NO backend/API:

**Income Categories:**
- Utama: Gaji, Pemberian, Hadiah
- Sekunder: Investasi, Keuntungan Bisnis

**Expense Categories:**  
- Primer: Makan, Baju, Sekolah, Rumah, Listrik, Internet, Bensin, Popok, Susu
- Sekunder: Rekreasi, Nabung, Investasi, Bisnis Sampingan
- Tersier: Restoran, Kafe, Asesoris, Hobi

**Mock Transaction Schema:**
```typescript
{
  id: string,
  type: 'income' | 'expense',
  amount: number, // IDR
  note: string,
  category: string,
  date: Date
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Charts:** Recharts (tree-shakeable, responsive)
- **Icons:** Lucide React
- **Components:** shadcn/ui (Radix-based, copy-paste components)
- **Animations:** Framer Motion for smooth transitions
- **Date Utils:** date-fns (for formatting dates in Bahasa Indonesia)

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Financial SaaS meets Indonesian warmth
- **Color Palette:**
  - Primary: Emerald (wealth, growth) - `emerald-600`
  - Accent: Amber (caution, balance) - `amber-500`  
  - Neutral: Slate (professional) - `slate-50` to `slate-900`
  - Success: Green-500, Danger: Red-500
  
- **Typography:**
  - Font: Plus Jakarta Sans (modern Indonesian vibe)
  - Headings: font-bold, text-slate-900
  - Body: font-normal, text-slate-600
  - Numbers: font-semibold, tabular-nums (aligned decimals)

- **Spacing & Layout:**
  - Container: max-w-7xl mx-auto
  - Card: rounded-xl border border-slate-200 shadow-sm
  - Padding: p-6 for cards, p-4 for dense lists

## UI Components Specification

**Transaction Card:**
```
┌─────────────────────────────────────┐
│ 🍔 Makan Siang                      │
│ Rp 45.000           Primer · Makan  │
│ 15 Jan 2025                         │
└─────────────────────────────────────┘
```
- Icon based on category
- Amount: text-lg font-semibold (red for expense, green for income)
- Hover: scale-[1.02] transition

**Summary Card:**
```
┌──────────────┐
│ Total Keluar │
│ Rp 1.250.000 │
│ ↑ 12% vs...  │
└──────────────┘
```
- Trend indicator with arrow icon
- Animated counter on mount

**Charts:**
- Pie Chart: Show top 5 expense categories, group rest as "Lainnya"
- Bar Chart: Last 7 days income vs expense
- Tooltip: Custom styled with Tailwind, show formatted IDR

## Interactions & States

**Form Behavior (Modal/Drawer):**
- Input amount: Auto-format to IDR (separate thousands with dot)
- Category: Searchable dropdown with icons
- Note: Optional, placeholder "Untuk apa?"
- Date: Default today, can pick past dates

**Animations:**
- Page load: Stagger fade-in for cards (Framer Motion)
- Chart update: Smooth transition (Recharts built-in)
- Add transaction: Success toast notification

**Empty States:**
- Illustration (use Lucide icons composed as graphic)
- CTA: "Mulai Catat Transaksi"

## Example Mock Data
```typescript
const mockTransactions = [
  {
    id: '1',
    type: 'expense',
    amount: 45000,
    note: 'Makan siang',
    category: 'Makan',
    date: new Date('2025-01-15')
  },
  // ... 15-20 more realistic entries
]
```

## Language & Localization
- UI Labels: Bahasa Indonesia
- Currency: IDR with proper formatting (Rp 1.250.000)
- Dates: "15 Jan 2025" format using date-fns with `id` locale
- Empty states & tooltips: Casual Indonesian ("Belum ada transaksi nih!")

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Charts display correctly with dummy numbers  
- ✅ Responsive across desktop/tablet/mobile
- ✅ Forms have proper validation (client-side only)
- ✅ Smooth animations and micro-interactions
- ✅ NO backend calls, NO API integration
- ✅ Ready to connect to real API later (clear data layer separation)
