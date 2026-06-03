# PRD: Healthcare EMR - Patient-Centric Medical Records
**Project:** Electronic Medical Record (EMR) System
**Aesthetic:** Clinical, Sterile, Highly Readable, Accessible.

## Core Layout
- **Sidebar:** Patient Search, Appointments, Medical Records, Prescriptions, Lab Results.
- **Main View (Patient Dashboard):** 
    - Vital signs overview (Charts).
    - Recent visits and upcoming appointments.
    - Allergies and active medications (High Visibility).
- **Medical Entry View:** 
    - SOAP note format (Subjective, Objective, Assessment, Plan).
    - Digital signature area.

## Data Structure (Mock Only)
```typescript
interface Patient {
  id: string;
  name: string;
  dob: string;
  gender: string;
  bloodType: string;
  allergies: string[];
  vitalsHistory: {
    date: string;
    bp: string;
    heartRate: number;
    temp: number;
  }[];
}

interface Appointment {
  id: string;
  patientId: string;
  date: string;
  doctor: string;
  reason: string;
  status: 'Scheduled' | 'Completed' | 'Cancelled';
}
```

## Implementation Details
- **Frontend:** React with TypeScript.
- **Accessibility:** High contrast ratios, ARIA labels.
- **Charts:** `recharts` for vital signs history.
- **Icons:** Lucide React (Stethoscope, Activity, ClipboardList, Pill).

## Styling & Theming
- **Palette:** Clinical Blue (#2563EB), Hospital Green (#10B981), Warning Red (#EF4444), Plain White.
- **Typography:** Sans-serif fonts optimized for reading dense data (e.g., Roboto or Inter).
- **Layout:** Grid-heavy for rapid data scanning.

## UI Components Specification
- **Vital Signs Chart:** Line chart showing trends in Blood Pressure/Heart Rate.
- **Allergy Alert:** High-contrast red badge in the header of the patient view.
- **Prescription List:** Table with Dosage, Frequency, and Refill status.
- **Lab Result Card:** Shows value vs. normal range with visual indicator (In Range / Out of Range).

## Interactions & States
- **Patient Search:** Global search bar with autocomplete by name or ID.
- **Appointment Scheduling:** Drag-and-drop calendar for booking visits.
- **Form Validation:** Strict validation for medical data entry to prevent errors.

## Language & Localization
- **Language:** English (US) primary.
- **Units:** Metric system by default (Celsius, kg).

## Example Mock Data
- Patient "John Doe" with hypertension history.
- Appointment schedule for "Dr. Sarah Smith".

## Success Criteria
- ✅ Clear, organized patient summary view.
- ✅ Visualization of vital signs history.
- ✅ Prescription management interface.
- ✅ Accessible design for healthcare professionals.
