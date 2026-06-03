# PRD: AI Chatbot - Intelligent Assistant with Custom Knowledge
**Project:** AI Chatbot with Knowledge Base
**Aesthetic:** Futurist, Conversational, Sleek, Dark/Light Mode support.

## Core Layout
- **Sidebar:** Conversation History, Knowledge Base Manager, Settings.
- **Main View (Chat Interface):** 
    - Message list (User & AI bubbles).
    - Input area with "Attach" icon for knowledge injection.
- **Knowledge Base View:**
    - List of uploaded documents/sources.
    - "Add Knowledge" button (Modal or Drawer).
    - Status of document indexing (Mocked).

## Data Structure (Mock Only)
```typescript
interface ChatMessage {
  id: string;
  sender: 'user' | 'assistant';
  content: string;
  timestamp: string;
  sources?: string[]; // IDs of knowledge sources used
}

interface KnowledgeSource {
  id: string;
  name: string;
  type: 'file' | 'url' | 'text';
  status: 'Indexed' | 'Processing' | 'Error';
  contentSnippet: string;
  uploadedAt: string;
}
```

## Implementation Details
- **Frontend:** React with TypeScript.
- **Styling:** CSS Modules or Tailwind-like utility classes.
- **Icons:** Lucide React or Phosphor Icons.
- **Animation:** `framer-motion` for message entry and smooth transitions.

## Styling & Theming
- **Palette:** Deep Indigo (#4F46E5), Emerald Green for "Active" states, Ghost White for bubbles.
- **Typography:** System-native sans-serif (San Francisco / Segoe UI).
- **Bubble Style:** Rounded corners with slight shadow; distinct colors for User vs AI.

## UI Components Specification
- **Message Bubble:** Support for markdown rendering (bold, lists, code blocks).
- **Input Area:** Textarea that expands vertically; "Send" button that activates on text entry.
- **Knowledge Card:** Mini-card showing file type icon, name, and "Delete" option.
- **Source Citations:** Small clickable pills at the bottom of AI messages that show which knowledge was used.

## Interactions & States
- **Chat Flow:** Type message -> Enter -> Show "AI is thinking..." typing indicator -> Display AI response.
- **Knowledge Upload:** Click "Add Knowledge" -> Select File -> Show upload progress -> File appears in sidebar list.
- **Contextual Chat:** AI responses should mock using the uploaded knowledge sources.

## Language & Localization
- **Language:** English (US) primary.

## Example Mock Data
- Conversations about "Company Policy", "Project Specs", and "Technical Docs".
- Mocked sources: `employee_handbook.pdf`, `api_v2_spec.txt`.

## Success Criteria
- ✅ Functional chat interface with history management.
- ✅ Ability to "upload" and manage multiple knowledge sources.
- ✅ AI messages that simulate "grounding" in provided knowledge.
- ✅ Responsive design for desktop and mobile.
