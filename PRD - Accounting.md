
## Project
Accounting - Static Boilerplate
A modern accounting system with double-entry bookkeeping, chart of accounts, journals, and financial statements.

## Core Layout: Multi-Panel Accounting

1. **Left Sidebar (Modules - 20%)**
   - Dashboard, Chart of Accounts, Jurnal, Buku Besar, Neraca, Laba Rugi, Arus Kas, Settings
   - Period selector (bulan/tahun)
   - Quick action: "+ Buat Jurnal"

2. **Center Panel (Transaction/Report View - 55%)**
   - Journal entry list: date, reference, description, debit, credit, status
   - General ledger: account view with running balance
   - Financial reports: formatted statement view

3. **Right Panel (Detail/Form - 25%)**
   - Journal entry form: date, ref, line items (account, debit, credit)
   - Account detail: transactions for selected account
   - Quick calculator for amounts

## Data Structure (Mock Only)

```typescript
type AccountType = 'asset' | 'liability' | 'equity' | 'revenue' | 'expense'
type AccountGroup = 'current_asset' | 'fixed_asset' | 'current_liability' | 'long_term_liability' | 'capital' | 'retained_earnings' | 'operating_revenue' | 'operating_expense' | 'other_income' | 'other_expense'
type JournalStatus = 'draft' | 'posted' | 'reversed'

interface Account {
  id: string
  code: string // e.g. "1-1000"
  name: string
  type: AccountType
  group: AccountGroup
  normalBalance: 'debit' | 'credit'
  balance: number
  isActive: boolean
  parentId: string | null
  description: string
}

interface JournalEntry {
  id: string
  reference: string // e.g. "JRN-2025-001"
  description: string
  date: Date
  lines: JournalLine[]
  totalDebit: number
  totalCredit: number
  status: JournalStatus
  createdBy: string
  approvedBy: string | null
  createdAt: Date
  postedAt: Date | null
}

interface JournalLine {
  id: string
  accountId: string
  accountCode: string
  accountName: string
  debit: number
  credit: number
  description: string
}

interface TrialBalance {
  accountId: string
  accountCode: string
  accountName: string
  debit: number
  credit: number
}

interface FinancialStatement {
  period: string
  accounts: StatementLine[]
  total: number
}

interface StatementLine {
  accountCode: string
  accountName: string
  amount: number
  indent: number
  isTotal: boolean
}

interface FiscalPeriod {
  id: string
  month: number
  year: number
  isOpen: boolean
  isActive: boolean
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Icons:** Lucide React
- **Components:** shadcn/ui
- **Animations:** Framer Motion
- **Date Utils:** date-fns (with `id` locale)
- **Charts:** Recharts for financial trends

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Professional accounting, clean, spreadsheet-like
- **Color Palette:**
  - Primary: Blue - `blue-700`
  - Accent: Emerald (positive) / Red (negative)
  - Debit text: blue, Credit text: green
  - Neutral: Slate

## UI Components Specification

**Journal Entry Form:**
```
┌──────────────────────────────────────────────────────────────────┐
│ ✏️ Jurnal Baru                                                  │
│ Tanggal: [15/01/2025      ]  Ref: [JRN-2025-001               ]│
│ Deskripsi: [Pembelian perlengkapan kantor                     ]│
│                                                              │
│ ┌──────┬────────────────────────┬──────────┬──────────┬──────┐ │
│ │ Akun │ Deskripsi              │ Debit    │ Credit   │      │ │
│ ├──────┼────────────────────────┼──────────┼──────────┼──────┤ │
│ │1-1100│ Perlengkapan Kantor   │ 5.000.000│          │ [X] │ │
│ │1-1101│ Kas                   │          │5.000.000 │ [X] │ │
│ │      │                       │          │          │[+Ln]│ │
│ ├──────┼────────────────────────┼──────────┼──────────┼──────┤ │
│ │      │ Total                 │5.000.000 │5.000.000 │      │ │
│ └──────┴────────────────────────┴──────────┴──────────┴──────┘ │
│                                                              │
│ Catatan: Transaksi otomatis balance (debit = credit)         │
│                                                              │
│ [Simpan Draft] [Posting] [Batal]                              │
└──────────────────────────────────────────────────────────────────┘
```
- Debit and credit must balance before posting
- Account search dropdown with code + name
- Auto-format number to IDR

**General Ledger:**
```
┌──────────────────────────────────────────────────────────────────┐
│ Buku Besar: 1-1100 Kas                                         │
│ Periode: Januari 2025                                          │
│ ┌──────┬────────────┬────────────┬──────────┬──────────┬──────┐ │
│ │ Tgl  │ Ref        │ Deskripsi  │ Debit    │ Credit   │ Saldo│ │
│ ├──────┼────────────┼────────────┼──────────┼──────────┼──────┤ │
│ │01 Jan│ Saldo Awal │            │          │          │ 50jt│ │
│ │05 Jan│ JRN-001    │ Penjualan  │ 15.000.00│          │ 65jt│ │
│ │10 Jan│ JRN-002    │ Bayar sewa │          │10.000.00 │ 55jt│ │
│ │15 Jan│ JRN-003    │ Beli alat  │          │5.000.000 │ 50jt│ │
│ └──────┴────────────┴────────────┴──────────┴──────────┴──────┘ │
│ Saldo Akhir: Rp 50.000.000                                     │
└──────────────────────────────────────────────────────────────────┘
```

**Financial Statement (Income Statement):**
```
┌──────────────────────────────────────────────────────────────────┐
│ LAPORAN LABA RUGI                                               │
│ Periode: Januari 2025                                           │
│ ─────────────────────────────────────────────────────────────── │
│ PENDAPATAN                                                      │
│   4-1000  Pendapatan Jasa              Rp 150.000.000           │
│   4-2000  Pendapatan Lainnya           Rp 5.000.000             │
│           Total Pendapatan             Rp 155.000.000           │
│ ─────────────────────────────────────────────────────────────── │
│ BEBAN                                                            │
│   5-1000  Beban Gaji                   Rp 45.000.000            │
│   5-2000  Beban Sewa                   Rp 10.000.000            │
│   5-3000  Beban Operasional            Rp 5.000.000             │
│   5-4000  Beban Penyusutan             Rp 2.000.000             │
│           Total Beban                  Rp 62.000.000            │
│ ─────────────────────────────────────────────────────────────── │
│ LABA BERSIH                        Rp 93.000.000               │
└──────────────────────────────────────────────────────────────────┘
```

**Chart of Accounts (Tree):**
```
▼ AKTIVA (1)
  ▼ Aktiva Lancar (1-1000)
      1-1100  Kas                                  Rp 50 jt
      1-1200  Bank                                  Rp 150 jt
      1-1300  Piutang Usaha                         Rp 25 jt
      1-1400  Perlengkapan                          Rp 5 jt
  ▼ Aktiva Tetap (1-2000)
      1-2100  Tanah                                 Rp 500 jt
      1-2200  Bangunan                              Rp 1 M
      1-2300  Peralatan                             Rp 100 jt
▼ KEWAJIBAN (2)
...
```

## Interactions & States
- Journal entry: dynamic line addition, auto-balance check
- Posting: draft → posted (locks editing)
- Period closing logic (mock)
- Account drill-down: click account to see ledger
- Report period selector

**Empty States:**
- No journal entries: "Belum ada jurnal. Buat jurnal baru!"
- No accounts: "Chart of accounts masih kosong."

## Example Mock Data
```typescript
const mockAccounts: Account[] = [
  { id: 'a1', code: '1-1100', name: 'Kas', type: 'asset', group: 'current_asset', normalBalance: 'debit', balance: 50000000, isActive: true, parentId: null, description: 'Kas tunai perusahaan' },
  { id: 'a2', code: '1-1200', name: 'Bank BCA', type: 'asset', group: 'current_asset', normalBalance: 'debit', balance: 150000000, isActive: true, parentId: null, description: 'Rekening giro BCA' },
  { id: 'a3', code: '4-1000', name: 'Pendapatan Jasa', type: 'revenue', group: 'operating_revenue', normalBalance: 'credit', balance: 150000000, isActive: true, parentId: null, description: 'Pendapatan dari jasa konsultasi' },
  // ... full chart of accounts
]

const mockJournalEntries: JournalEntry[] = [
  { id: 'j1', reference: 'JRN-2025-001', description: 'Pendapatan jasa konsultasi', date: new Date('2025-01-05'), lines: [{ id: 'jl1', accountId: 'a2', accountCode: '1-1200', accountName: 'Bank BCA', debit: 15000000, credit: 0, description: 'Pembayaran dari PT Client' }, { id: 'jl2', accountId: 'a3', accountCode: '4-1000', accountName: 'Pendapatan Jasa', debit: 0, credit: 15000000, description: 'Pendapatan jasa konsultasi' }], totalDebit: 15000000, totalCredit: 15000000, status: 'posted', createdBy: 'Admin', approvedBy: 'Supervisor', createdAt: new Date('2025-01-05'), postedAt: new Date('2025-01-05') },
]
```

## Language & Localization
- Bahasa Indonesia: "Jurnal", "Buku Besar", "Neraca", "Laba Rugi", "Arus Kas"
- Account types: "Aktiva", "Kewajiban", "Ekuitas", "Pendapatan", "Beban"
- Actions: "Buat Jurnal", "Posting", "Void", "Lihat Buku Besar"
- Reports: "Neraca Saldo", "Laporan Laba Rugi", "Neraca", "Arus Kas"

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Chart of accounts with tree hierarchy
- ✅ Double-entry journal with auto-balance
- ✅ General ledger with running balance
- ✅ Income statement report
- ✅ Balance sheet report
- ✅ Trial balance report
- ✅ Account drill-down to transactions
- ✅ Search and filter journal entries
- ✅ Responsive across desktop/tablet
- ✅ NO backend calls, NO API integration
