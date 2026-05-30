
## Project
Document Management System - Static Boilerplate
A modern DMS for organizing, storing, and retrieving documents with folder hierarchy and version control.

## Core Layout: 3-Panel Explorer

1. **Left Sidebar (Folder Tree - 20%)**
   - Expandable folder tree with collapse/expand all
   - Root folders: My Documents, Shared, Recent, Trash
   - Breadcrumb path at top
   - Folder context menu (rename, move, delete)
   - New folder button

2. **Center Panel (Document List - 55%)**
   - Toggle: List view (detailed) / Grid view (thumbnails)
   - Columns: Name, Type, Size, Modified, Status
   - Search bar with advanced filters
   - Sort by name, date, size, type
   - Drag-and-drop upload area

3. **Right Panel (Document Preview/Metadata - 25%)**
   - Document preview (PDF viewer, image, or file icon)
   - Metadata panel: name, type, size, created, modified
   - Version history list
   - Tags editor
   - Download / Share / Delete actions

## Data Structure (Mock Only)
Use hardcoded mock data arrays - NO backend/API:

```typescript
interface DmsFolder {
  id: string
  name: string
  parentId: string | null
  path: string // full path e.g. "/My Documents/Projects"
  color: string | null // folder color label
  documentCount: number
  createdAt: Date
  updatedAt: Date
}

interface Document {
  id: string
  name: string
  type: 'pdf' | 'docx' | 'xlsx' | 'pptx' | 'txt' | 'image' | 'other'
  mimeType: string
  size: number // bytes
  extension: string
  folderId: string
  tags: string[]
  status: 'active' | 'archived' | 'deleted'
  currentVersion: number
  uploadedBy: string
  description: string
  isStarred: boolean
  createdAt: Date
  updatedAt: Date
}

interface DocumentVersion {
  id: string
  documentId: string
  versionNumber: number
  fileUrl: string
  size: number
  notes: string
  uploadedBy: string
  createdAt: Date
}

interface DmsTag {
  id: string
  name: string
  color: string
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **PDF Preview:** react-pdf (PDF.js based)
- **File Icons:** Lucide React (file-type icons)
- **Components:** shadcn/ui (Radix-based, copy-paste components)
- **Animations:** Framer Motion for smooth transitions
- **Date Utils:** date-fns (with `id` locale)
- **Drag & Drop:** @hello-pangea/dnd (successor of react-beautiful-dnd)

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Professional, clean file manager aesthetic
- **Color Palette:**
  - Primary: Indigo (enterprise trust) - `indigo-600`
  - Accent: Teal (file management) - `teal-500`
  - Neutral: Slate - `slate-50` to `slate-900`
  - File type colors: PDF (red), DOCX (blue), XLSX (green), PPTX (orange)

- **Typography:**
  - Font: Inter (clean, highly readable)
  - File names: font-medium, text-sm
  - Metadata: text-xs text-slate-500
  - Breadcrumb: text-sm with slash separator

- **Spacing & Layout:**
  - Container: max-w-full
  - File card: rounded-lg border border-slate-200 p-3
  - Preview panel: sticky, overflow-y-auto

## UI Components Specification

**File Row (List View):**
```
┌──────────────────────────────────────────────────────────────────┐
│ 📄 laporan-keuangan-2024.pdf   PDF   2.4 MB  12 Jan 2025  ★    │
│ Folder: Finance    Tags: [laporan] [tahunan]                    │
│ [Download] [Share] [⋮]                                          │
└──────────────────────────────────────────────────────────────────┘
```
- File icon colored by type
- Star toggle for favorites
- Progress bar if uploading

**File Grid (Grid View):**
```
┌──────────┐  ┌──────────┐  ┌──────────┐
│ 📁       │  │ 📄       │  │ 📊       │
│ Project  │  │ Laporan  │  │ Anggaran │
│ Proposal │  │ Q4.pdf   │  │ 2025    │
│ 15 items │  │ 2.4 MB   │  │ 1.1 MB   │
└──────────┘  └──────────┘  └──────────┘
```
- Folder: large folder icon
- File: thumbnail or type icon
- Selection checkbox on hover

**Folder Tree:**
```
▼ My Documents
  ▼ Projects (12)
    ▶ Web App (5)
    ▶ Mobile App (3)
  ▼ Finance (8)
  ▼ HR (4)
▶ Shared with Me (23)
▶ Recent (15)
🗑 Trash (3)
```
- Indentation per level (16px)
- Right-click context menu

**Upload Drop Zone:**
```
┌───────────────────────────────────────┐
│                                       │
│   📂 Drop files here or click to     │
│      upload                           │
│                                       │
│   Supported: PDF, DOCX, XLSX, PPTX,   │
│   JPG, PNG (max 50MB)                 │
│                                       │
└───────────────────────────────────────┘
```
- Full-page overlay on drag
- Progress per file
- Success/failure toast

## Interactions & States

**Folder Navigation:**
- Double-click folder to enter
- Breadcrumb click to navigate up
- Back/Forward navigation (history stack)
- Right-click context menu: New Folder, Rename, Move, Delete

**Document Actions:**
- Single click: select row
- Double click: open preview
- Drag file to folder in tree to move
- Version history: timeline view

**Search & Filter:**
- Full-text search across filenames
- Filter by: file type, date range, tags, folder
- Sort: name, size, modified date

**Animations:**
- Folder expand: smooth height transition
- File list: stagger fade-in
- Upload progress: animated bar
- Preview panel: slide from right

**Empty States:**
- Empty folder: "Folder ini kosong. Upload dokumen atau buat folder baru."
- No search results: "Tidak ditemukan. Coba filter atau kata kunci lain."
- No starred: "Belum ada dokumen yang dibintangi."

## Example Mock Data
```typescript
const mockFolders: DmsFolder[] = [
  { id: '1', name: 'My Documents', parentId: null, path: '/My Documents', color: 'blue', documentCount: 24, createdAt: new Date('2024-01-01'), updatedAt: new Date('2025-01-15') },
  { id: '2', name: 'Projects', parentId: '1', path: '/My Documents/Projects', color: 'violet', documentCount: 12, createdAt: new Date('2024-01-01'), updatedAt: new Date('2025-01-15') },
  // ...
]

const mockDocuments: Document[] = [
  {
    id: 'doc-1',
    name: 'Laporan Keuangan Q4 2024',
    type: 'pdf',
    mimeType: 'application/pdf',
    size: 2450000,
    extension: 'pdf',
    folderId: '3',
    tags: ['laporan', 'keuangan', 'q4'],
    status: 'active',
    currentVersion: 3,
    uploadedBy: 'Admin',
    description: 'Laporan keuangan kuartal keempat tahun 2024',
    isStarred: true,
    createdAt: new Date('2025-01-10'),
    updatedAt: new Date('2025-01-15')
  },
  // ... 15-20 more entries
]
```

## Language & Localization
- UI Labels: Bahasa Indonesia
- File types in Indonesian: "Dokumen", "Spreadsheet", "Presentasi", "PDF"
- Dates: "12 Jan 2025" format
- Actions: "Unduh", "Bagikan", "Pindahkan", "Ganti Nama", "Hapus"
- Empty states: Casual Indonesian ("Belum ada file nih, upload dulu yuk!")

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Folder tree with expand/collapse and navigation
- ✅ File list/grid toggle view
- ✅ Document upload with drag-and-drop (mock)
- ✅ File preview panel with metadata
- ✅ Full-text search and advanced filters
- ✅ Version history timeline
- ✅ Responsive across desktop/tablet
- ✅ Smooth animations and micro-interactions
- ✅ NO backend calls, NO API integration
- ✅ Ready to connect to real API later (clear data layer separation)
