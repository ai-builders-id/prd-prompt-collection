
## Project
Business Intelligence - Static Boilerplate
A modern BI dashboard platform for data visualization, custom reports, and business analytics.

## Core Layout: 3-Panel BI

1. **Left Sidebar (Navigation - 20%)**
   - Dashboards, Reports, Data Sources, Charts Library, Settings
   - Saved dashboard list
   - Quick action: "Buat Dashboard Baru"

2. **Center Panel (Dashboard/Builder - 55%)**
   - Dashboard grid with draggable/resizable chart widgets
   - Filter bar: date range, dimensions, measures
   - Report builder: chart type selector, data config
   - Tab: View mode / Edit mode

3. **Right Panel (Chart Config - 25%)**
   - Chart type selector (bar, line, pie, area, table, metric)
   - Data field mapping (X-axis, Y-axis, group, filter)
   - Chart style options (colors, labels, legend)
   - Preview of selected chart

## Data Structure (Mock Only)

```typescript
type ChartType = 'bar' | 'line' | 'pie' | 'area' | 'donut' | 'metric' | 'table' | 'scatter'
type Aggregation = 'sum' | 'avg' | 'count' | 'min' | 'max' | 'distinct'

interface BiDashboard {
  id: string
  name: string
  description: string
  widgets: string[]
  filters: DashboardFilter[]
  isDefault: boolean
  createdAt: Date
  updatedAt: Date
}

interface DashboardWidget {
  id: string
  dashboardId: string
  title: string
  chartType: ChartType
  dataSource: string
  config: ChartConfig
  position: { x: number, y: number, w: number, h: number }
  createdAt: Date
}

interface ChartConfig {
  dimensions: FieldMapping[]
  measures: FieldMapping[]
  filters: ChartFilter[]
  sort: { field: string, order: 'asc' | 'desc' }
  limit: number
  showLegend: boolean
  showLabels: boolean
  colorScheme: string
}

interface FieldMapping {
  field: string
  label: string
  aggregation: Aggregation
}

interface DashboardFilter {
  field: string
  label: string
  type: 'date_range' | 'select' | 'multi_select' | 'number_range'
  value: any
}

interface BiDataSet {
  id: string
  name: string
  type: 'sales' | 'finance' | 'marketing' | 'operations' | 'hr'
  fields: DataField[]
  records: Record<string, any>[]
  lastUpdated: Date
}

interface DataField {
  name: string
  type: 'string' | 'number' | 'date' | 'boolean'
  label: string
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Charts:** Recharts (primary) + visx (advanced viz)
- **Icons:** Lucide React
- **Components:** shadcn/ui
- **Animations:** Framer Motion
- **Date Utils:** date-fns (with `id` locale)
- **Drag & Drop:** react-grid-layout for dashboard widgets

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Modern analytics, data-rich, clean
- **Color Palette:**
  - Primary: Violet - `violet-600`
  - Accent: Cyan (data flow) - `cyan-500`
  - Chart colors: categorical palette (12 colors)
  - Neutral: Slate

## UI Components Specification

**Dashboard Grid:**
```
┌──────────────────────────────────────────────────────────────┐
│ 📊 Dashboard: Penjualan 2025              [Edit] [Filter]   │
│ ┌──────────────────┐ ┌──────────────────┐                   │
│ │ Revenue by Month │ │ Revenue by Region│                   │
│ │ ██▇▆▅▄▃▂▁      │ │ 🟦 🟧 🟩 🟪 🟡  │                   │
│ │ │││││││││││││  │ │ Pie chart        │                   │
│ │ Bar chart        │ │                  │                   │
│ └──────────────────┘ └──────────────────┘                   │
│ ┌──────────────────────────┐ ┌──────────┐                   │
│ │ Top Products             │ │ Metrics  │                   │
│ │ #1: Product A: Rp 500jt │ │ Total    │                   │
│ │ #2: Product B: Rp 350jt │ │ Rp 2.1M  │                   │
│ │ Bar horizontal           │ │ +15%     │                   │
│ └──────────────────────────┘ └──────────┘                   │
└──────────────────────────────────────────────────────────────┘
```

**Chart Builder:**
```
┌──────────────────────────────────────────────────────────────┐
│ Chart Type: [Bar ▼]   Data: [Penjualan ▼]                   │
│                                                              │
│ Drag fields to configure:                                    │
│ ┌──────────────────┐  ┌──────────────────┐                  │
│ │ X-Axis           │  │ Y-Axis           │                  │
│ │ [Tahun ▼]        │  │ [sum: Revenue ▼] │                  │
│ └──────────────────┘  └──────────────────┘                  │
│ ┌──────────────────┐  ┌──────────────────┐                  │
│ │ Group By         │  │ Filter           │                  │
│ │ [Region ▼]       │  │ [+ Add Filter]   │                  │
│ └──────────────────┘  └──────────────────┘                  │
│                                                              │
│ Preview:                                                     │
│ ┌──────────────────────────────────────┐                    │
│ │  ██▇▆▅▄▃▂▁                         │                    │
│ │  │││││││││││││││││                 │                    │
│ └──────────────────────────────────────┘                    │
│ [Simpan Widget] [Cancel]                                    │
└──────────────────────────────────────────────────────────────┘
```

## Interactions & States
- Drag widgets to reposition on grid
- Resize widgets from bottom-right corner
- Edit mode toggle: View ↔ Configure
- Date range filter affects all widgets
- Drill-down: click chart element for detail

**Empty States:**
- No dashboards: "Belum ada dashboard. Buat dashboard baru!"
- No data: "Pilih data source untuk memulai."
- No results: "Tidak ada data untuk filter ini."

## Example Mock Data
```typescript
const mockBiDataSets: BiDataSet[] = [
  {
    id: 'ds-1',
    name: 'Penjualan',
    type: 'sales',
    fields: [
      { name: 'tanggal', type: 'date', label: 'Tanggal' },
      { name: 'produk', type: 'string', label: 'Produk' },
      { name: 'region', type: 'string', label: 'Region' },
      { name: 'revenue', type: 'number', label: 'Revenue' },
      { name: 'quantity', type: 'number', label: 'Quantity' },
    ],
    records: [
      { tanggal: '2025-01-01', produk: 'Product A', region: 'Jakarta', revenue: 50000000, quantity: 100 },
      { tanggal: '2025-01-01', produk: 'Product B', region: 'Bandung', revenue: 35000000, quantity: 75 },
    ],
    lastUpdated: new Date('2025-01-15')
  },
]

const mockDashboards: BiDashboard[] = [
  { id: 'db-1', name: 'Penjualan 2025', description: 'Dashboard monitoring penjualan', widgets: ['w-1', 'w-2', 'w-3'], filters: [{ field: 'tanggal', label: 'Periode', type: 'date_range', value: { start: '2025-01-01', end: '2025-12-31' } }], isDefault: true, createdAt: new Date('2025-01-01'), updatedAt: new Date('2025-01-15') },
]
```

## Language & Localization
- Bahasa Indonesia: "Dashboard", "Laporan", "Sumber Data", "Grafik"
- Chart types: "Batang", "Garis", "Pie", "Area", "Tabel", "Metrik"
- Actions: "Buat Dashboard", "Tambah Widget", "Ekspor", "Filter"

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Dashboard grid with draggable/resizable widgets
- ✅ Multiple chart types (bar, line, pie, area, metric, table)
- ✅ Chart builder with field mapping
- ✅ Date range and dimension filters
- ✅ Edit/View mode toggle
- ✅ Multiple mock data sets
- ✅ Responsive across desktop/tablet
- ✅ NO backend calls, NO API integration
