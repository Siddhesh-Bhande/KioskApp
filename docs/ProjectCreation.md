# Project Creation Documentation

## Overview

Project creation in the application is handled through multiple components and integrates with the project management system. The creation flow is accessible from the main Projects dashboard and provides a comprehensive form for setting up new projects.

## Access Points

### Primary Access

- Located in the Projects dashboard (`/projects`)
- Accessed via "New project" button in the top navigation
- Direct URL access through `/projects/create`

### Navigation Flow

1. User clicks "New project" from Projects dashboard
2. System navigates to project creation form
3. Form submission redirects to new project's overview page

## Data Structure

### CreateProjectForm Interface

```typescript
interface CreateProjectForm {
  name: string; // Project name
  project_key: string; // Unique identifier
  status: string; // Project status
  summary: string; // Project description
  latitude: number; // Geographical coordinates
  longitude: number; // Geographical coordinates
  useful_life?: number; // Optional project lifespan
  boundary?: number; // Optional project area
  country?: string; // Optional location details
  state?: string; // Optional location details
  county?: string; // Optional location details
}
```

## Form Structure

### Required Fields

1. **Project Key**

   - Type: Input
   - Validation: Required
   - Format: Unique identifier
   - Example: "PROJ-001"

2. **Project Name**

   - Type: Input
   - Validation: Required
   - Purpose: Display name for project

3. **Status**

   - Type: Select
   - Options:
     - DRAFT
     - ACTIVE
     - ARCHIVED
     - COMPLETED
   - Default: DRAFT

4. **Project Summary**

   - Type: TextArea
   - Validation: Required
   - Purpose: Project description

5. **Location Coordinates**
   - Type: InputNumber
   - Fields: latitude, longitude
   - Validation: Required
   - Purpose: Project geographical location

### Optional Fields

1. **Project Metrics**

   - Useful Life (years)
   - Boundary (acres)
   - Purpose: Project scope definition

2. **Location Details**
   - Country
   - State
   - County
   - Purpose: Additional location context

## API Integration

### Create Project Endpoint

```typescript
API_ENDPOINTS.PROJECTS.CREATE
Method: POST
Headers: {
  'Content-Type': 'application/json',
  'Authorization': `Bearer ${token}`
}
Body: CreateProjectForm
```

### Response Handling

```typescript
interface CreateProjectResponse {
  id: number;
  project_key: string;
  name: string;
  status: string;
  // ... other project fields
}
```

## Implementation Details

### Form Validation

```typescript
const validationRules = {
  project_key: [{ required: true, message: "Please enter project key" }],
  name: [{ required: true, message: "Please enter project name" }],
  status: [{ required: true, message: "Please select status" }],
  summary: [{ required: true, message: "Please enter project summary" }],
  latitude: [{ required: true, message: "Please enter latitude" }],
  longitude: [{ required: true, message: "Please enter longitude" }],
};
```

### Form Submission

```typescript
const handleCreateProject = async (values: CreateProjectForm) => {
  try {
    const response = await axios.post(API_ENDPOINTS.PROJECTS.CREATE, values, {
      headers: {
        Authorization: `Bearer ${token}`,
        "Content-Type": "application/json",
      },
    });

    // Navigate to new project on success
    navigate(`/project/${response.data.id}/overview`);
  } catch (error) {
    handleError(error);
  }
};
```

## State Management

### Form State

```typescript
const [form] = Form.useForm<CreateProjectForm>();
const [isSubmitting, setIsSubmitting] = useState(false);
const [error, setError] = useState<string | null>(null);
```

### Form Reset

```typescript
const resetForm = () => {
  form.resetFields();
  setError(null);
};
```

## User Interface Components

### Form Layout

```jsx
<Form layout="vertical">
  <Grid cols={2}>
    <ProjectKey /> // Project identifier input
    <ProjectName /> // Project name input
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
  <ActionButtons /> // Submit and Cancel buttons
</Form>
```

### Responsive Design

- Grid-based layout
- Mobile-friendly input fields
- Adaptive spacing
- Flexible form sections

## Error Handling

### Validation Errors

- Field-level validation messages
- Form-level validation
- Required field indicators
- Real-time validation feedback

### API Errors

- Error message display
- Network error handling
- Validation error handling
- User feedback mechanisms

## Security

### Access Control

- Role-based access
- Token validation
- Permission checking
- Secure form submission

### Data Validation

- Input sanitization
- Type checking
- Format validation
- Security headers

## Integration Points

### Project List Update

- Updates projects list on creation
- Refreshes dashboard data
- Updates navigation
- Updates recent projects

### Team Management

- Initial team setup
- Role assignment
- Access control
- Collaboration setup

## Usage Guidelines

### Creating a New Project

1. Navigate to Projects dashboard
2. Click "New project" button
3. Fill required fields
4. Add optional information
5. Submit form
6. Review created project

### Modifying Project Details

1. Access project overview
2. Click edit button
3. Modify desired fields
4. Save changes
5. Review updates

## Best Practices

### Form Handling

1. Validate inputs client-side
2. Provide clear error messages
3. Maintain form state
4. Handle submission gracefully

### Data Management

1. Sanitize inputs
2. Validate data types
3. Handle optional fields
4. Maintain data consistency

## Future Improvements

### Technical Enhancements

1. **Form Optimization**

   - Dynamic field validation
   - Auto-save functionality
   - Form state persistence
   - Field dependencies

2. **Data Validation**
   - Advanced validation rules
   - Custom validators
   - Format checking
   - Data normalization

### Feature Additions

1. **Project Templates**

   - Predefined templates
   - Custom templates
   - Template management
   - Default values

2. **Location Services**

   - Map integration
   - Address lookup
   - Coordinate validation
   - Location suggestions

3. **Team Integration**
   - Team templates
   - Role presets
   - Access patterns
   - Collaboration settings

### UI Improvements

1. **Form Experience**

   - Step-by-step wizard
   - Progress indicators
   - Context help
   - Field suggestions

2. **Accessibility**

   - Screen reader support
   - Keyboard navigation
   - Focus management
   - Error announcements

3. **Mobile Experience**
   - Touch-friendly inputs
   - Responsive layouts
   - Mobile optimization
   - Gesture support
