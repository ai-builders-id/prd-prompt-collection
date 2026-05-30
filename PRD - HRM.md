
## Project
Human Resources Management - Static Boilerplate
A modern HRM system for managing employees, attendance, leave, and payroll.

## Core Layout: 3-Panel HRM

1. **Left Sidebar (Navigation - 20%)**
   - Dashboard, Karyawan, Kehadiran, Cuti, Penggajian, Laporan
   - Quick stats: total employees, on leave, pending approvals

2. **Center Panel (List/Detail - 55%)**
   - Employee table: name, department, position, status, phone, email
   - Attendance calendar/grid view
   - Leave requests list (with approval actions)
   - Payroll summary table

3. **Right Panel (Detail Sidebar - 25%)**
   - Employee profile: photo, name, NIK, department, position, join date
   - Leave balance card
   - Attendance summary (this month)
   - Salary breakdown
   - Quick actions: Edit, Request Leave, View Payslip

## Data Structure (Mock Only)

```typescript
type EmployeeStatus = 'active' | 'resigned' | 'terminated' | 'probation'
type LeaveType = 'annual' | 'sick' | 'maternity' | 'religious' | 'emergency'
type LeaveStatus = 'pending' | 'approved' | 'rejected' | 'cancelled'

interface Employee {
  id: string
  nik: string
  name: string
  email: string
  phone: string
  avatar: string | null
  department: string
  position: string
  status: EmployeeStatus
  joinDate: Date
  birthDate: Date
  gender: 'L' | 'P'
  address: string
  bankAccount: string
  baseSalary: number
  emergencyContact: string
  createdAt: Date
}

interface Attendance {
  id: string
  employeeId: string
  date: Date
  checkIn: string // HH:mm
  checkOut: string | null
  status: 'present' | 'late' | 'absent' | 'half_day' | 'holiday'
  notes: string
}

interface LeaveRequest {
  id: string
  employeeId: string
  type: LeaveType
  startDate: Date
  endDate: Date
  totalDays: number
  reason: string
  status: LeaveStatus
  approvedBy: string | null
  createdAt: Date
  updatedAt: Date
}

interface LeaveBalance {
  employeeId: string
  annual: { total: number, used: number }
  sick: { total: number, used: number }
  maternity: { total: number, used: number }
  religious: { total: number, used: number }
}

interface Payroll {
  id: string
  employeeId: string
  period: string // "2025-01"
  baseSalary: number
  allowances: { name: string, amount: number }[]
  deductions: { name: string, amount: number }[]
  overtime: number
  tax: number
  netSalary: number
  status: 'draft' | 'approved' | 'paid'
  paidAt: Date | null
}

interface Department {
  id: string
  name: string
  headId: string
  employeeCount: number
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Icons:** Lucide React
- **Components:** shadcn/ui
- **Animations:** Framer Motion
- **Date Utils:** date-fns (with `id` locale)
- **Charts:** Recharts

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Professional, clean HR portal
- **Color Palette:**
  - Primary: Indigo - `indigo-600`
  - Accent: Pink - `pink-500`
  - Status: Present (green), Late (amber), Absent (red), Leave (blue)
  - Neutral: Slate

## UI Components Specification

**Employee Row:**
```
┌──────────────────────────────────────────────────────────────┐
│ 👤 Budi Santoso      NIK: 12710012345                      │
│ 🏢 Engineering — Senior Developer     Status: ✅ Active    │
│ 📅 Bergabung: 12 Jun 2022                                  │
│ [Profile] [Cuti] [Payslip] [⋮]                              │
└──────────────────────────────────────────────────────────────┘
```

**Attendance Calendar:**
```
┌──────────────────────────────────────────────────────────────┐
│ Januari 2025                         [<] [>]                │
│ Sen Sel Rab Kam Jum Sab Min                                 │
│        1   2   3   4   5   6                                │
│  ✓   ✓   ✓   ✓   ✗   -   -                                │
│  7   8   9  10  11  12  13                                 │
│  ✓   ⌛  ✓   ✓   ✓   -   -                                │
│ ...                                                         │
│ Legend: ✅ Hadir  ⌛ Terlambat  ✗ Absen  - Libur           │
└──────────────────────────────────────────────────────────────┘
```

**Leave Request Card:**
```
┌──────────────────────────────────────────────────────────────┐
│ 🏖️ Cuti Tahunan                    Status: ⏳ Menunggu     │
│ Budi Santoso — Engineering                                  │
│ 20 Jan 2025 — 25 Jan 2025 (6 hari)                         │
│ Alasan: Liburan keluarga                                    │
│ [Setujui] [Tolak]                                           │
└──────────────────────────────────────────────────────────────┘
```

## Interactions & States
- Leave request: Approve/Reject with confirmation
- Attendance: click date to see details
- Payroll: filter by period, export mock
- Employee: filter by department, status

**Empty States:**
- No employees: "Belum ada data karyawan."
- Pending leave: "Tidak ada permohonan cuti yang menunggu."
- No attendance: "Belum ada data kehadiran."

## Example Mock Data
```typescript
const mockEmployees: Employee[] = [
  { id: 'e1', nik: '12710012345', name: 'Budi Santoso', email: 'budi@company.com', phone: '0812-3456-7890', avatar: null, department: 'Engineering', position: 'Senior Developer', status: 'active', joinDate: new Date('2022-06-12'), birthDate: new Date('1990-03-15'), gender: 'L', address: 'Jl. Merdeka No. 123', bankAccount: 'BCA 1234567890', baseSalary: 15000000, emergencyContact: '0812-1111-2222', createdAt: new Date('2022-06-12') },
]

const mockLeaveBalances: LeaveBalance[] = [
  { employeeId: 'e1', annual: { total: 12, used: 5 }, sick: { total: 14, used: 1 }, maternity: { total: 0, used: 0 }, religious: { total: 4, used: 2 } },
]

const mockPayroll: Payroll[] = [
  { id: 'p1', employeeId: 'e1', period: '2025-01', baseSalary: 15000000, allowances: [{ name: 'Transport', amount: 500000 }, { name: 'Makan', amount: 500000 }], deductions: [{ name: 'BPJS', amount: 300000 }, { name: 'PPH21', amount: 450000 }], overtime: 750000, tax: 450000, netSalary: 16000000, status: 'paid', paidAt: new Date('2025-01-28') },
]
```

## Language & Localization
- Bahasa Indonesia: "Karyawan", "Kehadiran", "Cuti", "Penggajian"
- Leave types: "Cuti Tahunan", "Sakit", "Melahirkan", "Ibadah", "Darurat"
- Status: "Hadir", "Terlambat", "Absen", "Cuti", "Libur"
- Payroll: "Gaji Pokok", "Tunjangan", "Potongan", "Gaji Bersih"

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Employee CRUD with profile view
- ✅ Attendance calendar with color-coded days
- ✅ Leave request workflow (request → approve/reject)
- ✅ Leave balance tracking
- ✅ Payroll summary with salary breakdown
- ✅ Department filter and search
- ✅ Responsive across desktop/tablet
- ✅ NO backend calls, NO API integration
