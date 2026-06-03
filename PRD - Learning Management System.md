# PRD: Learning Management System (LMS) - Seamless Education & Training
**Project:** LMS Platform
**Aesthetic:** Educational, Encouraging, Intuitive, and Organized.

## Core Layout
- **Sidebar:** My Courses, Browse Catalog, Assignments, Progress Tracking, Settings.
- **Main View (Dashboard):** 
    - "Continue Learning" section with progress bars for active courses.
    - Calendar view for upcoming deadlines.
- **Course View:**
    - Sidebar with lesson list/modules.
    - Main area for Video Player, Markdown content, or Quiz interface.
- **Instructor Dashboard:** Course creation, student analytics, and gradebook.

## Data Structure (Mock Only)
```typescript
interface Course {
  id: string;
  title: string;
  instructor: string;
  thumbnail: string;
  progress: number; // 0-100
  modules: {
    id: string;
    title: string;
    lessons: {
      id: string;
      title: string;
      type: 'video' | 'text' | 'quiz';
      completed: boolean;
    }[];
  }[];
}

interface UserProgress {
  userId: string;
  enrolledCourses: string[];
  completedLessons: string[];
  certificates: { courseId: string; issueDate: string }[];
}
```

## Implementation Details
- **Frontend:** React with TypeScript.
- **Video Player:** `react-player` or mock video container.
- **Progress Visualization:** Circular progress components.
- **Icons:** Lucide React (Book, Play, CheckCircle, GraduationCap).

## Styling & Theming
- **Palette:** Sky Blue (#0EA5E9), Leaf Green (#22C55E), Soft White (#FFFFFF), Text Gray (#374151).
- **Typography:** Rounded Sans-serif (e.g., Quicksand or Inter) for a friendly feel.
- **Components:** Card-based layouts with subtle shadows.

## UI Components Specification
- **Course Card:** Shows thumbnail, title, instructor, and a progress bar.
- **Lesson List:** Accordion-style list grouped by modules.
- **Quiz Widget:** Single-choice or Multiple-choice question UI with instant feedback.
- **Certificate Preview:** A modal showing a stylized mock certificate.

## Interactions & States
- **Learning Flow:** Click lesson -> Content loads -> Mark as complete -> Progress bar updates.
- **Navigation:** Deep linking to specific modules within a course.
- **Filtering:** Search and filter the course catalog by category or difficulty.

## Language & Localization
- **Language:** English (US) primary.

## Example Mock Data
- Courses: "Introduction to React", "Advanced UI Design", "Corporate Compliance 101".

## Success Criteria
- ✅ Interactive course player with module navigation.
- ✅ Real-time progress tracking visualization.
- ✅ Functional course catalog with search/filter.
- ✅ Mobile-responsive layout for learning on the go.
