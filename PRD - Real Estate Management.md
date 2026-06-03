# PRD: Real Estate Management - Property & Tenant Ecosystem
**Project:** Real Estate Portfolio Manager
**Aesthetic:** Elegant, Trustworthy, High-Quality Imagery, Sophisticated.

## Core Layout
- **Navbar:** Dashboard, Properties, Tenants, Finances, Maintenance, Documents.
- **Main View (Dashboard):** 
    - Key Metrics: Occupancy Rate, Total Revenue, Pending Maintenance.
    - Map View (Mocked) showing property locations.
- **Property Details View:**
    - Photo Gallery.
    - Tenant list and Unit availability.
    - Financial history for the specific property.
- **Tenant Portal (Mock):** Rent payment, Maintenance request form.

## Data Structure (Mock Only)
```typescript
interface Property {
  id: string;
  name: string;
  address: string;
  type: 'Residential' | 'Commercial' | 'Industrial';
  units: number;
  occupancy: number; // percentage
  images: string[];
  owner: string;
}

interface Tenant {
  id: string;
  name: string;
  propertyId: string;
  unitNumber: string;
  rentAmount: number;
  leaseEnd: string;
  status: 'Active' | 'Eviction' | 'Past';
}
```

## Implementation Details
- **Frontend:** React with TypeScript.
- **Maps:** `react-leaflet` or a stylized mock SVG map.
- **Charts:** `recharts` for occupancy and revenue trends.
- **Icons:** Lucide React (Home, Users, DollarSign, Wrench).

## Styling & Theming
- **Palette:** Navy Blue (#1E3A8A), Champagne Golden (#D4AF37), Marble White (#F5F5F5).
- **Typography:** Serif headings (e.g., Playfair Display) with Sans-serif body (e.g., Lato).
- **Spacing:** Wide gutters and breathable layouts to mimic luxury listings.

## UI Components Specification
- **Property Card:** Hover effect showing quick stats (units, price).
- **Availability Grid:** Color-coded grid showing which units are occupied (Blue) vs vacant (Green).
- **Maintenance Table:** List of issues with priority levels (High, Medium, Low).
- **Map Pin Modal:** Tooltip showing property preview when clicking a map pin.

## Interactions & States
- **Switch View:** Toggle between "Grid View" and "Map View" for properties.
- **Filter Bar:** Filter properties by type, location, or price range.
- **Lease Tracking:** Highlight tenants with leases expiring in the next 30 days.

## Language & Localization
- **Language:** English (US) primary.
- **Currency:** USD (Mocked support for others).

## Example Mock Data
- Properties: "The Grand Heights", "Silicon Valley Hub", "Greenwood Apartments".

## Success Criteria
- ✅ Integrated map visualization for property locations.
- ✅ Comprehensive dashboard with occupancy and financial metrics.
- ✅ Drill-down capability from property list to unit/tenant details.
- ✅ Clean maintenance tracking interface.
