# UpdateProject Component Documentation

## Overview

The `UpdateProject` component provides a form interface for updating existing project details. It implements a responsive form with validation, dark mode support, and real-time error handling.

## Component Features

- Project data fetching and population
- Form validation
- Dark mode support
- Responsive layout
- Error handling
- Loading states
- Navigation integration

## Data Types

### ProjectFormData Interface

```typescript
interface ProjectFormData {
  name: string; // Project name
  project_key: string; // Unique project identifier (immutable)
  status: string; // Project status (DRAFT/ACTIVE/ARCHIVED/COMPLETED)
  summary: string; // Project description
  latitude: number; // Geographical latitude
  longitude: number; // Geographical longitude
  useful_life?: number; // Project lifespan in years (optional)
  boundary?: number; // Project area in acres (optional)
  country?: string; // Project location - country (optional)
  state?: string; // Project location - state (optional)
  county?: string; // Project location - county (optional)
}
```

## State Management

### Component States

```typescript
const [isLoading, setIsLoading] = useState(true); // Loading state for initial data fetch
const [error, setError] = useState<string | null>(null); // Error state for API failures
const [form] = Form.useForm(); // Ant Design form instance
```

## Core Functions

### `fetchProject`

**Purpose**: Fetches existing project data and populates the form
**Trigger**: Automatically on component mount
**Logic**:

- Makes GET request to project detail endpoint
- Updates form fields with received data
- Handles loading and error states
- Uses axiosInstance for authenticated requests

```typescript
const fetchProject = async () => {
  try {
    setIsLoading(true);
    const response = await axiosInstance.get(
      API_ENDPOINTS.PROJECTS.DETAIL(projectId)
    );
    form.setFieldsValue(response.data);
  } catch (error) {
    handleError(error);
  } finally {
    setIsLoading(false);
  }
};
```

### `onFinish`

**Purpose**: Handles form submission
**Trigger**: Form submit event
**Logic**:

- Validates form data
- Makes PATCH request to update project
- Handles success/error messages
- Navigates on success
- Uses axios for API request

## Form Structure

### Required Fields

1. **Project Key** (Disabled)

   - Type: Input
   - Validation: Required
   - State: Immutable

2. **Project Name**

   - Type: Input
   - Validation: Required
   - State: Mutable

3. **Status**

   - Type: Select
   - Options: DRAFT, ACTIVE, ARCHIVED, COMPLETED
   - Validation: Required
   - State: Mutable

4. **Summary**

   - Type: TextArea
   - Validation: Required
   - State: Mutable

5. **Coordinates**
   - Type: InputNumber
   - Fields: latitude, longitude
   - Validation: Required
   - State: Mutable

### Optional Fields

1. **Project Metrics**

   - Fields: useful_life, boundary
   - Type: InputNumber
   - Validation: Optional
   - State: Mutable

2. **Location Details**
   - Fields: country, state, county
   - Type: Input
   - Validation: Optional
   - State: Mutable

## API Integration

### Fetch Project Details

```typescript
GET: API_ENDPOINTS.PROJECTS.DETAIL(projectId)
Headers: {
  Authorization: Bearer ${token}
}
Response: ProjectFormData
```

### Update Project

```typescript
PATCH: API_ENDPOINTS.PROJECTS.UPDATE(projectId)
Headers: {
  Authorization: Bearer ${token}
  Content-Type: application/json
}
Body: ProjectFormData
Response: Updated project data
```

## Error Handling

### API Error Handling

- Uses axios error interceptors
- Displays user-friendly error messages
- Logs detailed errors for debugging
- Handles network errors
- Manages authentication errors

### Form Validation

- Required field validation
- Field-specific error messages
- Form-level validation
- Real-time validation feedback

## UI/UX Features

### Loading States

- Initial data loading indicator
- Form submission loading state
- Error state display
- Success feedback

### Navigation

- Back button to project overview
- Cancel button functionality
- Success navigation
- Error state handling

### Responsive Design

- Grid layout system
- Mobile-friendly input fields
- Responsive form layout
- Adaptive spacing

### Dark Mode Support

- Custom dark mode styles
- Theme-aware components
- Consistent color scheme
- Accessible contrast ratios

## Usage Example

```typescript
import UpdateProject from "../components/UpdateProject";

function App() {
  return (
    <Route path="/project/:projectId/edit">
      <UpdateProject />
    </Route>
  );
}
```

## Form Layout Structure

```jsx
<Form layout="vertical">
  <Grid cols={2}>
    <ProjectKey /> // Disabled, shows project identifier
    <ProjectName /> // Editable project name
  </Grid>
  <Status /> // Project status selector
  <Summary /> // Project description
  <Grid cols={2}>
    <Coordinates /> // Latitude/Longitude
  </Grid>
  <Grid cols={2}>
    <Metrics /> // Useful life and boundary
  </Grid>
  <Grid cols={3}>
    <Location /> // Country, state, county
  </Grid>
  <ActionButtons /> // Update and Cancel buttons
</Form>
```

## Best Practices Implemented

1. Form field validation
2. Error boundary implementation
3. Loading state management
4. Responsive design
5. Dark mode support
6. Type safety with TypeScript
7. Proper error handling
8. User feedback mechanisms
9. Clean form layout
10. Secure API integration

## Dependencies

- React Router (navigation)
- Ant Design (form components)
- Axios (API requests)
- Lucide React (icons)
- TailwindCSS (styling)
