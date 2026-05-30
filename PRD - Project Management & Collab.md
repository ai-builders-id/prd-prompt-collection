
## Project
Project Management & Collab - Static Boilerplate
A modern project management tool with Kanban boards, task tracking, and team collaboration.

## Core Layout: 3-Panel Workspace

1. **Left Sidebar (Projects & Teams - 20%)**
   - Project list with color indicators
   - Team member avatars (online/offline)
   - Favorites / Recent section
   - "Buat Project Baru" button
   - Filter: My Projects, Shared, Archived

2. **Center Panel (Board/List View - 55%)**
   - Tab toggle: Board (Kanban) | List (Table) | Timeline
   - Kanban columns: To Do, In Progress, Review, Done
   - Task cards with assignee, priority, due date
   - Drag-and-drop between columns
   - List view with sortable columns

3. **Right Panel (Task Detail - 25%)**
   - Task title and description (editable)
   - Assignee picker, due date, priority, labels
   - Checklist/subtasks
   - Comments/activity feed
   - Attachments preview

## Data Structure (Mock Only)
Use hardcoded mock data arrays - NO backend/API:

```typescript
type TaskStatus = 'todo' | 'in_progress' | 'review' | 'done'
type TaskPriority = 'low' | 'medium' | 'high' | 'urgent'

interface Project {
  id: string
  name: string
  description: string
  color: string
  status: 'active' | 'archived' | 'completed'
  startDate: Date
  endDate: Date | null
  members: string[]
  taskCount: number
  completedTaskCount: number
  createdAt: Date
}

interface ProjectTask {
  id: string
  projectId: string
  title: string
  description: string
  status: TaskStatus
  priority: TaskPriority
  assigneeId: string | null
  labels: string[]
  dueDate: Date | null
  estimatedHours: number | null
  order: number
  checklist: ChecklistItem[]
  attachments: string[]
  createdAt: Date
  updatedAt: Date
}

interface ChecklistItem {
  id: string
  content: string
  isCompleted: boolean
}

interface TaskComment {
  id: string
  taskId: string
  authorId: string
  authorName: string
  authorAvatar: string | null
  content: string
  attachments: string[]
  createdAt: Date
}

interface TeamMember {
  id: string
  name: string
  email: string
  avatar: string | null
  role: 'owner' | 'manager' | 'member'
  isOnline: boolean
  projects: string[]
}

interface ProjectLabel {
  id: string
  name: string
  color: string
  projectId: string
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Icons:** Lucide React
- **Components:** shadcn/ui (Radix-based, copy-paste components)
- **Animations:** Framer Motion for smooth transitions
- **Date Utils:** date-fns (with `id` locale)
- **Drag & Drop:** @hello-pangea/dnd (Kanban board)
- **Charts:** Recharts for burndown/progress charts

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Modern, collaborative, minimal — project management SaaS feel
- **Color Palette:**
  - Primary: Violet (creativity, management) - `violet-600`
  - Accent: Cyan (communication) - `cyan-500`
  - Neutral: Slate - `slate-50` to `slate-900`
  - Priority: Urgent (red), High (orange), Medium (blue), Low (slate)
  - Project colors: user-defined palette (12 colors)

- **Typography:**
  - Font: Inter
  - Project name: text-lg font-semibold
  - Task title: text-sm font-medium
  - Comments: text-sm leading-relaxed

- **Spacing & Layout:**
  - Kanban column: min-w-[280px] max-w-[320px]
  - Task card: rounded-lg border p-3 shadow-sm
  - Board: overflow-x-auto horizontal scroll

## UI Components Specification

**Kanban Board:**
```
┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
│ To Do    │ │ In       │ │ Review   │ │ Done     │
│ (4)      │ │ Progress │ │ (2)      │ │ (8)      │
│          │ │ (3)      │ │          │ │          │
│ ┌──────┐ │ │ ┌──────┐ │ │ ┌──────┐ │ │ ┌──────┐ │
│ │T-001 │ │ │ │T-003 │ │ │ │T-006 │ │ │ │T-007 │ │
│ │Setup │ │ │ │Login │ │ │ │QA    │ │ │ │Deploy│ │
│ │🔴    │ │ │ │🟡    │ │ │ │🔵    │ │ │ │✅    │ │
│ │👤Ami │ │ │ │👤Budi│ │ │ │👤Citra│ │ │ │👤Deni│ │
│ │25 Jan│ │ │ │27 Jan│ │ │ │28 Jan│ │ │ │Selesai│ │
│ └──────┘ │ │ └──────┘ │ │ └──────┘ │ │ └──────┘ │
│ [+ Add]  │ │ [+ Add]  │ │ [+ Add]  │ │          │
└──────────┘ └──────────┘ └──────────┘ └──────────┘
```

**Task Card:**
```
┌────────────────────────────────┐
│ 🟢 T-001 Setup Database       │
│ 🔴 High                        │
│ ⬜ Checklist: 2/4             │
│ 📎 3  💬 5                    │
│ ────────────────────────────── │
│ 👤 Ami    📅 25 Jan 2025      │
└────────────────────────────────┘
```

**Task Detail Panel:**
```
┌───────────────────────────────────┐
│ Judul: [Setup Database       ]   │
│ Status: [To Do ▼]  Prioritas: High│
│ Assign: [👤 Ami ▼]  Due: 25 Jan  │
│ Label: [dev] [backend]            │
│                                   │
│ ─── Checklist ───                 │
│ ☑ Install PostgreSQL              │
│ ☐ Configure user roles            │
│ [+ Tambah item]                   │
│                                   │
│ ─── Aktivitas ───                 │
│ Budi: Sudah selesai install       │
│                                   │
│ [Tulis komentar...]         [➤]  │
└───────────────────────────────────┘
```

## Interactions & States

**Kanban:**
- Drag card to change status
- Click card to open detail panel
- Column: click "+ Add" for new task

**Task Management:**
- Create/Edit via detail panel
- Checklist: add/check/uncheck items
- Attachments: drag-and-drop files

**View Switcher:**
- Board: Kanban columns
- List: Sortable table
- Timeline: Gantt-like date bars

**Animations:**
- Card drag: smooth lift + shadow
- Detail panel: slide from right
- Status change: card color transition

**Empty States:**
- No tasks: "Belum ada tugas. Tambah tugas baru!"
- No projects: "Belum ada project. Buat project pertama!"
- No comments: "Belum ada diskusi."

## Example Mock Data
```typescript
const mockProjects: Project[] = [
  {
    id: 'proj-1',
    name: 'Website Redesign',
    description: 'Redesign company website with new branding',
    color: '#7c3aed',
    status: 'active',
    startDate: new Date('2025-01-01'),
    endDate: new Date('2025-03-31'),
    members: ['user-1', 'user-2', 'user-3'],
    taskCount: 17,
    completedTaskCount: 8,
    createdAt: new Date('2025-01-01')
  },
]

const mockTasks: ProjectTask[] = [
  {
    id: 'task-1',
    projectId: 'proj-1',
    title: 'Setup Database Schema',
    description: 'Create PostgreSQL database schema.',
    status: 'todo',
    priority: 'high',
    assigneeId: 'user-1',
    labels: ['backend', 'database'],
    dueDate: new Date('2025-01-25'),
    estimatedHours: 8,
    order: 0,
    checklist: [
      { id: 'cl-1', content: 'Install PostgreSQL', isCompleted: true },
      { id: 'cl-2', content: 'Configure roles', isCompleted: false },
    ],
    attachments: ['schema-diagram.png'],
    createdAt: new Date('2025-01-10'),
    updatedAt: new Date('2025-01-15')
  },
]
```

## Language & Localization
- UI Labels: Bahasa Indonesia
- Task status: "Belum", "Diproses", "Review", "Selesai"
- Priority: "Rendah", "Sedang", "Tinggi", "Darurat"
- Actions: "Buat Tugas", "Assign", "Tambah Checklist"
- Empty states: Casual Indonesian

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Kanban board with drag-and-drop
- ✅ Task detail with full CRUD
- ✅ Checklist with progress tracking
- ✅ Comments/activity feed
- ✅ Multiple views: Board, List, Timeline
- ✅ Project dashboard with metrics
- ✅ Responsive across desktop/tablet
- ✅ Smooth animations
- ✅ NO backend calls, NO API integration
