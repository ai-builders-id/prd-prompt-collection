# PRD: Vehicle Fleet Management - Operational Excellence
**Project:** Fleet Tracker & Manager
**Aesthetic:** Rugged, Functional, Technical, High Contrast.

## Core Layout
- **Sidebar:** Live Map, Vehicles, Maintenance, Drivers, Fuel, Alerts.
- **Main View (Live Map):** 
    - Full-screen map with moving vehicle markers (Mocked).
    - Side panel with vehicle status list.
- **Vehicle Detail View:** 
    - Odometer, Fuel level, Engine Health.
    - Service history and upcoming maintenance.
- **Alerts Feed:** Critical notifications (Speeding, Low Fuel, Geo-fence breach).

## Data Structure (Mock Only)
```typescript
interface Vehicle {
  id: string;
  plateNumber: string;
  model: string;
  status: 'In Motion' | 'Parked' | 'Maintenance' | 'Emergency';
  location: { lat: number; lng: number };
  fuelLevel: number; // 0-100
  odometer: number;
}

interface Driver {
  id: string;
  name: string;
  license: string;
  assignedVehicle: string;
  rating: number;
}
```

## Implementation Details
- **Frontend:** React with TypeScript.
- **Maps:** `react-leaflet` or `google-map-react`.
- **Real-time Simulation:** Mocked location updates using `setInterval`.
- **Icons:** Lucide React (Truck, Map, AlertTriangle, Droplet).

## Styling & Theming
- **Palette:** Industrial Orange (#F97316), Slate (#475569), Caution Yellow (#FBBF24).
- **Typography:** Heavy Sans-serif (e.g., Ubuntu or Barlow) for a rugged look.
- **Components:** Compact tables and list items for dense information.

## UI Components Specification
- **Live Marker:** Vehicle icon on map that changes color based on status (Moving = Green, Stopped = Red).
- **Fuel Gauge:** Circular or bar-based indicator with color transition (Green -> Yellow -> Red).
- **Alert Toast:** Sticky notification for high-priority fleet events.
- **Maintenance Timeline:** Horizontal timeline showing past and future service dates.

## Interactions & States
- **Vehicle Tracking:** Click a vehicle in the list to center the map on it.
- **Status Filtering:** Filter map markers by status (e.g., only show "Maintenance" vehicles).
- **History Playback (Mock):** A slider that "replays" a vehicle's mock path on the map.

## Language & Localization
- **Language:** English (US) primary.
- **Units:** Imperial (Miles/Gallons) or Metric (KM/Liters).

## Example Mock Data
- Fleet of 10 delivery trucks in a metropolitan area.
- Maintenance logs for "Truck-01" showing oil change and tire rotation.

## Success Criteria
- ✅ Interactive live map with status-aware vehicle markers.
- ✅ Detailed vehicle health and maintenance dashboard.
- ✅ Real-time alert simulation and notification system.
- ✅ Driver assignment and performance overview.
