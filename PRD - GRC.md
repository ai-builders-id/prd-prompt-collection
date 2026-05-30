## Project
Governance, Risk, and Compliance - Static Boilerplate
A modern GRC platform for managing policies, risk assessments, audits, and compliance tracking.

## Core Layout: 3-Panel GRC

1. **Left Sidebar (Modules - 20%)**
   - Dashboard, Policies, Risks, Audits, Compliance, Incidents, Reports
   - Quick stats: open risks, pending audits, compliance score

2. **Center Panel (List/Detail - 55%)**
   - Policy list: title, version, status, owner, effective date, review date
   - Risk register: risk ID, description, category, likelihood, impact, score, owner
   - Audit schedule: planned vs completed audits
   - Compliance checklist with status

3. **Right Panel (Detail Sidebar - 25%)**
   - Policy detail: document viewer, version history, approval flow
   - Risk detail: assessment, controls, mitigation plan
   - Audit detail: findings, evidence, corrective actions
   - Actions: Approve, Review, Add Finding, Close

## Data Structure (Mock Only)

```typescript
type PolicyStatus = 'draft' | 'under_review' | 'approved' | 'active' | 'expired' | 'replaced'
type RiskLevel = 'low' | 'medium' | 'high' | 'critical'
type RiskStatus = 'identified' | 'assessed' | 'mitigated' | 'monitored' | 'accepted'
type AuditStatus = 'planned' | 'in_progress' | 'completed' | 'overdue'
type FindingSeverity = 'low' | 'medium' | 'high' | 'critical'
type FindingStatus = 'open' | 'in_progress' | 'resolved' | 'accepted'
type ComplianceStatus = 'compliant' | 'non_compliant' | 'partially_compliant' | 'not_applicable'

interface Policy {
  id: string
  title: string
  code: string // e.g. "POL-001"
  description: string
  category: 'it_security' | 'data_privacy' | 'hr' | 'finance' | 'operational'
  status: PolicyStatus
  version: number
  content: string // markdown
  owner: string
  effectiveDate: Date
  reviewDate: Date
  tags: string[]
  attachments: string[]
  createdAt: Date
  updatedAt: Date
}

interface Risk {
  id: string
  code: string // e.g. "RSK-001"
  title: string
  description: string
  category: 'strategic' | 'operational' | 'financial' | 'compliance' | 'reputational'
  likelihood: number // 1-5
  impact: number // 1-5
  riskScore: number // likelihood * impact
  riskLevel: RiskLevel
  status: RiskStatus
  owner: string
  controls: RiskControl[]
  mitigationPlan: string
  targetDate: Date | null
  createdAt: Date
  updatedAt: Date
}

interface RiskControl {
  id: string
  name: string
  description: string
  type: 'preventive' | 'detective' | 'corrective'
  effectiveness: 'high' | 'medium' | 'low'
  owner: string
  reviewDate: Date
}

interface Audit {
  id: string
  title: string
  scope: string
  auditor: string
  auditee: string
  startDate: Date
  endDate: Date
  status: AuditStatus
  findings: AuditFinding[]
  overallRating: 'satisfactory' | 'needs_improvement' | 'unsatisfactory'
  createdAt: Date
}

interface AuditFinding {
  id: string
  auditId: string
  title: string
  description: string
  severity: FindingSeverity
  status: FindingStatus
  remediationPlan: string
  targetDate: Date
  resolvedDate: Date | null
  evidence: string[]
}

interface ComplianceRequirement {
  id: string
  regulation: string // e.g. "UU PDP", "ISO 27001", "PCI DSS"
  clause: string
  description: string
  status: ComplianceStatus
  evidence: string
  owner: string
  reviewDate: Date
}

interface Incident {
  id: string
  title: string
  description: string
  category: 'security_breach' | 'data_leak' | 'fraud' | 'hr_violation' | 'operational'
  severity: 'low' | 'medium' | 'high' | 'critical'
  status: 'reported' | 'investigating' | 'resolved' | 'closed'
  reporter: string
  assignedTo: string
  resolution: string | null
  createdAt: Date
  resolvedAt: Date | null
}
```

## Implementation Details
- **Tech Stack:** Vite + React + TypeScript + Tailwind CSS
- **Icons:** Lucide React
- **Components:** shadcn/ui
- **Animations:** Framer Motion
- **Date Utils:** date-fns (with `id` locale)
- **Charts:** Recharts for risk heatmap, compliance score

## Styling & Theming

**Visual Identity:**
- **Aesthetic:** Corporate governance, serious, structured
- **Color Palette:**
  - Primary: Navy (authority) - `blue-900`
  - Accent: Red (risk, audit findings) - `red-600`
  - Risk: Low (green), Medium (amber), High (orange), Critical (red)
  - Compliance: Compliant (green), Non-compliant (red), Partial (amber)
  - Neutral: Slate

## UI Components Specification

**Risk Heatmap:**
```
┌──────────────────────────────────────────────────────────────────┐
│ Risk Heatmap                              [By Category ▼]      │
│                                                                  │
│ Impact                                                            │
│ 5 │ 🟡  │ 🟠  │ 🔴  │ 🔴  │ 🔴  │                             │
│ 4 │ 🟡  │ 🟠  │ 🟠  │ 🔴  │ 🔴  │                             │
│ 3 │ 🟢  │ 🟡  │ 🟠  │ 🟠  │ 🔴  │                             │
│ 2 │ 🟢  │ 🟢  │ 🟡  │ 🟠  │ 🟠  │                             │
│ 1 │ 🟢  │ 🟢  │ 🟢  │ 🟡  │ 🟠  │                             │
│   ─────────────────────────────────────                           │
│     1    2    3    4    5  Likelihood                            │
└──────────────────────────────────────────────────────────────────┘
```

**Policy Card:**
```
┌──────────────────────────────────────────────────────────────────┐
│ POL-001  Kebijakan Keamanan Informasi          v2.1             │
│ Category: IT Security    Owner: Budi Santoso                    │
│ Status: ✅ Active      Effective: 01 Jan 2025                  │
│ Review: 01 Jan 2026    ⏳ 11 months remaining                  │
│ [View] [Review] [Revise] [Archive]                              │
└──────────────────────────────────────────────────────────────────┘
```

**Audit Finding:**
```
┌──────────────────────────────────────────────────────────────────┐
│ 🔴 Critical  Finding: Password policy not enforced             │
│ Audit: ISO 27001 Internal Audit 2025                           │
│ ─────────────────────────────────────────────────────────────── │
│ Deskripsi:                                                      │
│ Ditemukan 15 akun dengan password default yang belum diubah.   │
│                                                                  │
│ Remediasi:                                                      │
│ 1. Reset semua password default                                 │
│ 2. Implementasikan kebijakan password kompleks                  │
│ 3. Audit bulanan                                                │
│                                                                  │
│ Target: 01 Feb 2025    Status: 🔵 In Progress                   │
│ Evidence: [📎 Password Audit Report.xlsx]                       │
│ [Resolve] [Accept Risk] [Add Evidence]                          │
└──────────────────────────────────────────────────────────────────┘
```

**Compliance Dashboard:**
```
┌──────────────────────────────────────────────────────────────────┐
│ 📊 Compliance Overview                                          │
│ ┌──────────┐  ┌──────────┐  ┌──────────┐                       │
│ │ Overall  │  │ Compliant│  │Non-Compl.│                       │
│ │  78%     │  │    34    │  │    6     │                       │
│ └──────────┘  └──────────┘  └──────────┘                       │
│                                                                  │
│ By Regulation:                                                   │
│ UU PDP      ████████████████░░ 82%    ✅                        │
│ ISO 27001   ██████████████░░░░ 70%    ⚠️                        │
│ PCI DSS     ███████████████░░░ 75%    ⚠️                        │
│ SOX         ██████████████████ 90%    ✅                        │
└──────────────────────────────────────────────────────────────────┘
```

## Interactions & States
- Risk assessment: auto-calculate score from likelihood × impact
- Policy lifecycle: Draft → Review → Approved → Active → Review
- Audit workflow: Planned → In Progress → Completed
- Finding management: Open → In Progress → Resolved

**Empty States:**
- No policies: "Belum ada kebijakan. Buat kebijakan baru!"
- No risks: "Belum ada risk register."
- No audits: "Belum ada jadwal audit."

## Example Mock Data
```typescript
const mockPolicies: Policy[] = [
  { id: 'p1', title: 'Kebijakan Keamanan Informasi', code: 'POL-001', description: 'Kebijakan keamanan informasi perusahaan', category: 'it_security', status: 'active', version: 2, content: '# Kebijakan Keamanan Informasi\n\n## 1. Tujuan...', owner: 'Budi Santoso', effectiveDate: new Date('2025-01-01'), reviewDate: new Date('2026-01-01'), tags: ['security', 'IT', 'ISO27001'], attachments: [], createdAt: new Date('2024-06-01'), updatedAt: new Date('2025-01-01') },
]

const mockRisks: Risk[] = [
  { id: 'r1', code: 'RSK-001', title: 'Data breach via third-party vendor', description: 'Vendor memiliki akses ke data pelanggan tanpa enkripsi', category: 'compliance', likelihood: 3, impact: 5, riskScore: 15, riskLevel: 'high', status: 'mitigated', owner: 'Siti Rahayu', controls: [{ id: 'rc1', name: 'Vendor Security Assessment', description: 'Assess keamanan vendor setiap 6 bulan', type: 'preventive', effectiveness: 'medium', owner: 'Siti Rahayu', reviewDate: new Date('2025-06-01') }], mitigationPlan: 'Implement mandatory encryption for all vendor data access', targetDate: new Date('2025-03-01'), createdAt: new Date('2024-11-01'), updatedAt: new Date('2025-01-15') },
]

const mockAudits: Audit[] = [
  { id: 'a1', title: 'ISO 27001 Internal Audit 2025', scope: 'All IT systems and processes', auditor: 'Audit Firm X', auditee: 'IT Department', startDate: new Date('2025-01-10'), endDate: new Date('2025-01-20'), status: 'in_progress', findings: [{ id: 'af1', auditId: 'a1', title: 'Password policy not enforced', description: '15 accounts with default passwords', severity: 'critical', status: 'in_progress', remediationPlan: 'Reset passwords, enforce policy', targetDate: new Date('2025-02-01'), resolvedDate: null, evidence: [] }], overallRating: 'needs_improvement', createdAt: new Date('2024-12-01') },
]
```

## Language & Localization
- Bahasa Indonesia: "Kebijakan", "Risiko", "Audit", "Kepatuhan", "Insiden"
- Risk level: "Rendah", "Sedang", "Tinggi", "Kritis"
- Audit rating: "Memuaskan", "Perlu Perbaikan", "Tidak Memuaskan"
- Compliance: "Patuh", "Tidak Patuh", "Patuh Sebagian"

## Success Criteria
- ✅ All UI components render with mock data
- ✅ Policy management with versioning
- ✅ Risk register with heatmap visualization
- ✅ Risk scoring (likelihood × impact)
- ✅ Audit management with findings tracking
- ✅ Compliance dashboard by regulation
- ✅ Policy lifecycle workflow
- ✅ Search and filter across all modules
- ✅ Responsive across desktop/tablet
- ✅ NO backend calls, NO API integration
