# Projects and ProjectOverview Components Documentation

## Table of Contents

1. [Projects Component](#projects-component)
2. [ProjectOverview Component](#projectoverview-component)
3. [Shared Features](#shared-features)
4. [Future Improvements](#future-improvements)

## Projects Component

### Overview

The `Projects` component serves as the main dashboard for displaying and managing all projects. It provides a responsive interface with both list and grid views, search functionality, and project management capabilities.

### Core Features

- List/Grid view toggle
- Project search
- Project creation/deletion
- Project sharing
- Dark mode support
- Responsive design

### Data Types

#### Project Interface

```typescript
interface Project {
  id: number;
  project_key: string;
  name: string;
  created_at?: string;
  status?: string;
  image?: string;
  isPrivate?: boolean;
  simulationCount?: number;
  team_members?: any;
  team_member_access?: any;
}
```

### State Management

```typescript
const [projects, setProjects] = useState<Project[]>([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState<string | null>(null);
const [searchQuery, setSearchQuery] = useState("");
const [isDeleteModalVisible, setIsDeleteModalVisible] = useState(false);
const [isListView, setIsListView] = useState(true);
const [isShareModalVisible, setIsShareModalVisible] = useState(false);
const [selectedProject, setSelectedProject] = useState<Project | null>(null);
```

### Core Functions

#### `fetchProjectsWithSimulations`

**Purpose**: Fetches projects and their simulation counts
**Implementation**:

- Makes API call to get projects list
- For each project, fetches simulation count
- Updates state with enriched project data
- Handles loading and error states

#### `handleAddMember`

**Purpose**: Manages project sharing functionality
**Implementation**:

- Sends POST request to invite endpoint
- Handles success/error messages
- Updates UI accordingly

#### `handleDeleteProject`

**Purpose**: Manages project deletion
**Implementation**:

- Sends DELETE request to project endpoint
- Handles confirmation dialog
- Updates project list on success

### UI Components

#### Project Card/List Item

- Displays project information
- Shows status badge
- Provides action menu (share, edit, delete)
- Handles click navigation

#### Search Bar

- Real-time filtering of projects
- Updates searchQuery state
- Filters projects based on name

#### View Toggle

- Switches between list and grid views
- Updates isListView state
- Persists view preference

## ProjectOverview Component

### Overview

The `ProjectOverview` component provides a detailed view of a single project with multiple tabs for different aspects of the project (summary, notes, simulations, etc.).

### Core Features

- Project details display
- Interactive map
- Team member management
- Notes management
- Simulations management
- Review status management
- Dark mode support

### Data Types

#### Project Interface (Extended)

```typescript
interface Project {
  id: number;
  name: string;
  project_key: string;
  status: "DRAFT" | "ACTIVE" | "ARCHIVED" | "COMPLETED";
  summary: string;
  team_members: ProjectMember[];
  latitude: number;
  longitude: number;
  boundary: number;
  country: string;
  state: string;
  county: string;
  useful_life: number;
  created_at: string;
  updated_at: string;
  location?: ProjectLocation;
  metrics?: ProjectMetrics;
  team?: ProjectMember[];
  simulations?: SimulationType[];
}
```

### State Management

The component manages multiple states for different features:

#### Project Data

```typescript
const [project, setProject] = useState<Project | null>(null);
const [recentSimulations, setRecentSimulations] = useState<any[]>([]);
const [allSimulations, setAllSimulations] = useState<any[]>([]);
```

#### Notes Management

```typescript
const [notes, setNotes] = useState<ProjectNote[]>([]);
const [highlightedNotes, setHighlightedNotes] = useState<ProjectNote[]>([]);
const [newNote, setNewNote] = useState("");
```

#### UI States

```typescript
const [isLoading, setIsLoading] = useState(true);
const [error, setError] = useState<string | null>(null);
const [activeTab, setActiveTab] = useState("summary");
```

### Core Functions

#### Data Fetching

```typescript
const fetchAllData = async () => {
  // Fetches project details, simulations, and notes
  // Updates all relevant states
  // Handles errors and loading states
};
```

#### Notes Management

```typescript
const handleAddNote = async () => {
  // Validates note content
  // Sends POST request
  // Updates notes list
  // Handles success/error states
};

const toggleHighlightNote = async (noteId: number, currentStatus: boolean) => {
  // Toggles note highlight status
  // Updates both notes and highlighted notes lists
  // Handles API interaction
};
```

#### Simulations Management

```typescript
const handleReviewStatusChange = async (
  projectId: number,
  simulationId: number,
  newStatus: string
) => {
  // Updates simulation review status
  // Handles API interaction
  // Updates UI state
};

const toggleHighlightSimulation = async (
  projectId: number,
  simulationId: number,
  currentStatus: boolean
) => {
  // Toggles simulation highlight status
  // Updates relevant lists
  // Handles API interaction
};
```

### Tab Structure

1. **Summary Tab**

   - Project overview
   - Location map
   - Team members
   - Highlighted notes
   - Highlighted simulations

2. **Notes Tab**

   - Full notes list
   - Note management
   - Highlight functionality

3. **Simulations Tab**

   - Simulations table
   - Review status management
   - Simulation actions

4. **Additional Tabs**
   - Curves
   - Models & Roadmaps
   - Other Resources

### API Integration

#### Project APIs

```typescript
API_ENDPOINTS.PROJECTS.DETAIL(projectId); // GET project details
API_ENDPOINTS.PROJECTS.NOTES(projectId); // GET/POST project notes
API_ENDPOINTS.PROJECTS.SIMULATIONS(projectId); // GET project simulations
```

#### Simulation APIs

```typescript
API_ENDPOINTS.SIMULATIONS.UPDATE_REVIEW_STATUS; // PATCH simulation status
API_ENDPOINTS.SIMULATIONS.HIGHLIGHT; // PATCH simulation highlight
API_ENDPOINTS.SIMULATIONS.DELETE; // DELETE simulation
```

### Error Handling

- API error handling with user feedback
- Loading states
- Error boundaries
- Network error handling
- Authentication error handling

### Security Features

- Role-based access control
- Permission checking for actions
- Token-based authentication
- Secure API calls

## Shared Features

### Dark Mode Support

- Theme-aware components
- Custom dark mode styles
- Consistent color scheme
- Accessible contrast ratios

### Responsive Design

- Mobile-friendly layouts
- Adaptive grid systems
- Flexible components
- Dynamic spacing

### Performance Optimizations

- Memoized components
- Efficient state updates
- Optimized re-renders
- Cached user data

## Future Improvements

### Technical Improvements

1. **Type Safety**

   - Replace 'any' types with proper interfaces
   - Strengthen type checking
   - Add type guards

2. **Performance**

   - Implement virtual scrolling for large lists
   - Add pagination for notes
   - Optimize API calls
   - Cache frequently accessed data

3. **Error Handling**
   - Add retry mechanisms
   - Improve error messages
   - Add error boundaries
   - Implement offline support

### Feature Enhancements

1. **Project Management**

   - Bulk actions for projects
   - Project templates
   - Project categories
   - Advanced filtering

2. **Collaboration**

   - Real-time updates
   - Comment threads
   - Activity timeline
   - User mentions

3. **Data Visualization**

   - Advanced analytics
   - Custom charts
   - Export options
   - Data insights

4. **User Experience**

   - Keyboard shortcuts
   - Drag-and-drop support
   - Customizable views
   - Quick actions

5. **Integration**
   - External tool integration
   - API webhook support
   - Custom automation
   - Third-party plugins

### UI/UX Improvements

1. **Accessibility**

   - ARIA labels
   - Keyboard navigation
   - Screen reader support
   - High contrast mode

2. **Mobile Experience**

   - Touch-optimized controls
   - Mobile-specific features
   - Offline capabilities
   - Responsive images

3. **Customization**
   - User preferences
   - Custom themes
   - Layout options
   - Widget system
