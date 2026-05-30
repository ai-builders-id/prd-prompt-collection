
## Project
Content Management System - Static Boilerplate
A clean, modern CMS for managing digital content with categories, tags, and media library.

## Core Layout: 3-Panel Dashboard

1. **Left Sidebar (Navigation - 20%)**
   - Primary nav: Content, Categories, Tags, Media, Settings
   - Collapsible menu with icons
   - Active page indicator
   - User avatar + logout at bottom

2. **Center Panel (Content List/Editor - 55%)**
   - Content table: title, status badge, category, author, date, actions
   - Search bar + filter chips (status, category, date range)
   - Rich text editor on create/edit view
   - Pagination or infinite scroll

3. **Right Panel (Preview/Metadata - 25%)**
   - Content preview (rendered HTML)
   - Metadata editor (SEO title, description, slug)
   - Publish settings (schedule publish, visibility)
   - Featured image selector

## Data Structure (Mock Only)
Use hardcoded mock data arrays - NO backend/API:

```typescript
type ContentStatus = 'draft' | 'published' | 'archived'
type ContentVisibility = 'public' | 'private' | 'password'

interface Content {
  id: string
  title: string
  slug: string
  body: string // HTML string
  excerpt: string
  featuredImage: string | null
  status: ContentStatus
  visibility: ContentVisibility
  categoryId: string
  tags: string[]
  author: string
  publishedAt: Date | null
  createdAt: Date
  updatedAt: Date
}

interface Category {
  id: string
  name: string
  slug: string
  description: string
  parentId: string | null
  order: number
  contentCount: number
  createdAt: Date
}

interface Tag {
  id: string
  name: string
  slug: string
  contentCount: number
  createdAt: Date
}

interface Media {
  id: string
  name: string
  url: string
  thumbnailUrl: string
  type: 'image' | 'document' | 'video' | 'audio'
  mimeType: string
  size: number // bytes
  alt: string
  uploadedAt: Date
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Rich Text:** TipTap (ProseMirror-based, extensible)
- **Icons:** Lucide React
- **Components:** shadcn/ui (Radix-based, copy-paste components)
- **Animations:** Framer Motion for smooth transitions
- **Date Utils:** date-fns (with `id` locale)
- **Routing:** React Router v6

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Clean editorial meets modern admin
- **Color Palette:**
  - Primary: Blue (trust, professionalism) - `blue-600`
  - Accent: Violet (creativity) - `violet-500`
  - Neutral: Gray (reading comfort) - `gray-50` to `gray-900`
  - Semantic: Emerald (published), Amber (draft), Slate (archived)

- **Typography:**
  - Font: Inter (UI) + source for editor content
  - Editor content: leading-relaxed, max-w-prose
  - Headings: font-bold, text-gray-900
  - Body: font-normal, text-gray-700

- **Spacing & Layout:**
  - Container: max-w-full px-6
  - Card: rounded-xl border border-gray-200 shadow-sm
  - Editor area: min-h-[400px] p-4 border rounded-lg

## UI Components Specification

**Content Table Row:**
```
┌──────────────────────────────────────────────────────────────┐
│ 📄 Panduan Lengkap React 2025                   │ Published │
│ Kategori: Tutorial    Penulis: Admin    12 Jan 2025         │
│ [Edit] [Preview] [⋮]                                        │
└──────────────────────────────────────────────────────────────┘
```
- Status badge: green (published), amber (draft), gray (archived)
- Row hover: bg-gray-50 transition
- Click row to open editor

**Content Editor:**
```
┌──────────────────────────────────────────────────────────────┐
│ Judul: [____________________________________________]      │
│ Slug: [/panduan-react-2025                                 ]│
│ ┌──────────────────────────────────────────────────────────┐│
│ │ H1: Panduan Lengkap React 2025                          ││
│ │                                                         ││
│ │ React terus berkembang... [B] [I] [U] [🔗] [IMG]      ││
│ │                                                         ││
│ │ ┌──────────────────────────────────────────────────────┐││
│ │ │ Paragraf pertama berisi pengantar...                 │││
│ │ └──────────────────────────────────────────────────────┘││
│ └──────────────────────────────────────────────────────────┘│
│ [Simpan Draft] [Publish] [Preview]                          │
└──────────────────────────────────────────────────────────────┘
```

**Media Library Grid:**
```
┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐
│ 🖼️   │ │ 🖼️   │ │ 🖼️   │ │ 🖼️   │
│ hero │ │ logo │ │ icon │ │ bg   │
│ 2MB  │ │ 500K │ │ 120K │ │ 4MB  │
└──────┘ └──────┘ └──────┘ └──────┘
[Upload] [Select Multiple] [Search...]
```

**Category Tree:**
```
▼ Tutorial (5)
  ▼ Frontend (3)
    ▶ React (2)
    ▶ Vue (1)
  ▶ Backend (2)
▶ Berita (8)
▶ Opini (4)
```
- Drag handle for reordering
- Badge shows content count

## Interactions & States

**Content List:**
- Bulk select with checkbox column
- Batch actions: Archive, Delete, Change Category
- Sort by: title, status, date, author
- Filter by status, category, date range

**Editor Behavior:**
- Auto-save draft every 30 seconds (localStorage)
- Unsaved changes warning on navigate away
- Image upload via drag-and-drop to editor

**Animations:**
- Page transition: fade + slide
- Table row: stagger fade-in
- Modal/Drawer: slide-up overlay
- Toast on save/publish

**Empty States:**
- No content: "Belum ada konten. Buat artikel pertama!"
- No media: "Belum ada media. Upload gambar atau dokumen."
- No search results: "Tidak ditemukan. Coba kata kunci lain."

## Example Mock Data
```typescript
const mockContents: Content[] = [
  {
    id: '1',
    title: 'Panduan Lengkap React 2025',
    slug: 'panduan-lengkap-react-2025',
    body: '<h1>Panduan Lengkap React 2025</h1><p>React terus berkembang...</p>',
    excerpt: 'Panduan komprehensif untuk belajar React di tahun 2025.',
    featuredImage: '/mock/uploads/hero-react.jpg',
    status: 'published',
    visibility: 'public',
    categoryId: 'cat-1',
    tags: ['react', 'frontend', 'javascript'],
    author: 'Admin',
    publishedAt: new Date('2025-01-12'),
    createdAt: new Date('2025-01-10'),
    updatedAt: new Date('2025-01-12')
  },
  // ... 15-20 more entries
]

const mockCategories: Category[] = [
  { id: 'cat-1', name: 'Tutorial', slug: 'tutorial', description: 'Artikel tutorial', parentId: null, order: 1, contentCount: 12, createdAt: new Date('2024-06-01') },
  { id: 'cat-2', name: 'Berita', slug: 'berita', description: 'Berita teknologi', parentId: null, order: 2, contentCount: 8, createdAt: new Date('2024-06-01') },
  // ...
]
```

## Language & Localization
- UI Labels: Bahasa Indonesia
- Status labels: "Draft", "Terbit", "Diarsipkan"
- Dates: "12 Jan 2025" format with date-fns `id` locale
- Empty states & tooltips: Casual Indonesian ("Belum ada konten nih!")
- Editor: "Simpan Draft", "Terbitkan", "Pratinjau"

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Content CRUD with form validation (client-side)
- ✅ Rich text editor functional (TipTap)
- ✅ Category/tag management with tree view
- ✅ Media library with grid/list toggle
- ✅ Search and filter by status/category/date
- ✅ Responsive across desktop/tablet
- ✅ Smooth animations and micro-interactions
- ✅ NO backend calls, NO API integration
- ✅ Ready to connect to real API later (clear data layer separation)
