# PRD: Social Media Analytics - Data-Driven Engagement
**Project:** Social Media Dashboard
**Aesthetic:** Vibrant, Data-Dense, Dark Mode optimized, Actionable.

## Core Layout
- **Sidebar:** Overview, Platforms (Instagram, Twitter, LinkedIn), Audience, Reports, Scheduler.
- **Main View (Overview):** 
    - Key Performance Indicators (KPIs): Total Reach, Engagement Rate, Follower Growth.
    - Combined multi-platform line chart.
- **Platform Specific View:**
    - Top performing posts (Grid).
    - Engagement breakdown (Likes, Comments, Shares).
- **Scheduler View:** Calendar showing planned posts.

## Data Structure (Mock Only)
```typescript
interface PlatformStats {
  platform: 'Instagram' | 'Twitter' | 'LinkedIn';
  followers: number;
  engagementRate: number;
  reach: number;
  history: { date: string; value: number }[];
}

interface SocialPost {
  id: string;
  platform: string;
  content: string;
  mediaUrl: string;
  stats: { likes: number; comments: number; shares: number };
  scheduledTime?: string;
}
```

## Implementation Details
- **Frontend:** React with TypeScript.
- **Charts:** `recharts` or `chart.js` for complex multi-axis charts.
- **Theming:** Full support for Dark Mode.
- **Icons:** Lucide React + Brand icons (simple SVG/Font).

## Styling & Theming
- **Palette:** Electric Purple (#8B5CF6), Pink (#EC4899), Deep Space (#0F172A - Dark Background).
- **Typography:** Modern Sans-serif (e.g., Montserrat or Inter).
- **Visuals:** Gradients for chart fills and progress bars.

## UI Components Specification
- **KPI Card:** Large number display with "percentage change" indicator (green up/red down).
- **Engagement Radar:** Radar chart showing audience demographics or post performance.
- **Post Feed Card:** Shows image/video thumbnail with overlay stats.
- **Sentiment Indicator:** Simple emoji or color bar representing "Positive", "Neutral", "Negative".

## Interactions & States
- **Platform Toggle:** Switch the entire dashboard context between different social platforms.
- **Date Range Picker:** Filter all data by Last 7 Days, 30 Days, or Custom Range.
- **Hover Insights:** Interactive tooltips on charts showing exact data points.

## Language & Localization
- **Language:** English (US) primary.

## Example Mock Data
- Growth stats for "Brand X" over the last quarter.
- Sample posts with high and low engagement for comparison.

## Success Criteria
- ✅ High-impact data visualizations (KPIs, line charts, radars).
- ✅ Multi-platform context switching simulation.
- ✅ Top-performing post gallery with metrics.
- ✅ Responsive and modern dark mode UI.
