# PRD: CV Extractor - Intelligent Resume Processing & Analytics
**Project:** CV Extractor & Parser
**Aesthetic:** Modern, Professional, Clean, with Data-Rich Visualizations.

## Core Layout
- **Sidebar:** Recent Uploads, Extraction History, Settings.
- **Header:** Global Search, User Profile, Notifications.
- **Main View (Dashboard/Home):** 
    - Hero section with "Drag & Drop" upload area.
    - Quick selection for extraction mode: "Raw Text" vs "AI Summary".
- **Extraction Result View:**
    - Split-screen layout: Original PDF/Image viewer on the left.
    - Extracted Data / Summary on the right.
    - Tabs for: Raw Text, AI Analysis, Skills Map, Experience Timeline.

## Data Structure (Mock Only)
```typescript
interface CVEvaluation {
  id: string;
  fileName: string;
  uploadDate: string;
  status: 'Pending' | 'Processing' | 'Completed' | 'Failed';
  mode: 'Raw' | 'AI';
  summary?: string;
  rawText?: string;
  extractedData: {
    personalInfo: {
      fullName: string;
      email: string;
      phone: string;
      location: string;
      links: string[];
    };
    education: {
      degree: string;
      institution: string;
      year: string;
    }[];
    experience: {
      title: string;
      company: string;
      period: string;
      description: string[];
    }[];
    skills: string[];
  };
}
```

## Implementation Details
- **Frontend:** React with TypeScript.
- **State Management:** React Context or simple State hooks for prototype.
- **File Handling:** `react-dropzone` for uploads.
- **PDF Viewing:** `react-pdf-viewer` or similar mock component.
- **Icons:** Lucide React.

## Styling & Theming
- **Palette:** Slate Blue (#4A90E2), Charcoal Gray (#333333), Light Gray Background (#F8F9FA).
- **Typography:** Inter or Sans-serif for readability.
- **Spacing:** Generous padding (1.5rem+) between sections.

## UI Components Specification
- **Upload Zone:** Large dashed border area with icon and upload instructions.
- **Mode Toggle:** Segmented control or clear button group to choose "Raw" vs "AI".
- **Status Badges:** Color-coded pills for "Completed", "Pending", etc.
- **Code/Raw Block:** Monospace font container for the raw text output.

## Interactions & States
- **Upload Flow:** Drag file -> Progress bar -> Redirect to Result View.
- **Extraction Simulation:** Show a "Scanning..." animation when AI mode is selected.
- **Comparison:** Allow users to toggle between Raw and AI summary view within the result page.

## Language & Localization
- **Language:** English (US) primary.

## Example Mock Data
- Mock CV for "John Doe" (Software Engineer) and "Jane Smith" (Product Designer).

## Success Criteria
- ✅ Support for PDF/DOCX upload simulation.
- ✅ Toggleable extraction modes (Raw vs AI).
- ✅ Clean, side-by-side comparison of original file and extracted data.
- ✅ Interactive skills list and experience timeline visualization.
