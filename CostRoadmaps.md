# CostRoadmaps Component Documentation

## Overview

The `CostRoadmaps` component manages cost roadmap files for different asset types (BESS, PV, HV) stored in S3. It provides a user interface for viewing, filtering, and editing spreadsheet files using the EditableDataTable component.

## Component Features

- File categorization by asset type (BESS, PV, HV)
- Asset type filtering
- Spreadsheet viewing and editing
- S3 file management (download/upload)
- Real-time file list updates

## State Management

The component manages several states:

- `costRoadmaps`: Stores file information categorized by asset type
- `loading`: Tracks data fetching status
- `error`: Manages error states
- `selectedFileUrl`: URL for the currently selected file
- `selectedFileName`: Name of the current file
- `selectedFileAssetType`: Asset type of current file
- `selectedFileBlob`: Binary data of the selected file
- `activeType`: Current filter selection (HV/BESS/PV/ALL)

## Core Functions

### `fetchCostRoadmaps`

**Purpose**: Retrieves list of cost roadmap files from S3
**Usage**: Called on component mount and after successful file operations
**Logic**:

- Makes API call to get file list
- Updates state with categorized file data
- Handles loading and error states

### `handleFileSelect`

**Purpose**: Manages file selection and download
**Usage**: Called when user clicks on a file card
**Logic**:

- Downloads file from S3
- Creates blob URL for file preview
- Updates relevant states for file display
- Prepares file for EditableDataTable

### `checkFileNameExists`

**Purpose**: Validates file name uniqueness
**Usage**: Used during file save operations
**Logic**:

- Checks if filename exists in current asset type
- Prevents duplicate file names
- Memoized for performance

### `formatDate`

**Purpose**: Formats file modification dates
**Usage**: Used in file card display
**Logic**:

- Converts ISO date string to localized format
- Displays date in user-friendly format

### `getAssetIcon`

**Purpose**: Provides icons for asset types
**Usage**: Used in file cards and filter buttons
**Logic**:

- Maps asset types to corresponding icons
- Returns appropriate Lucide icon component

### `renderFileCards`

**Purpose**: Renders file list UI
**Usage**: Used to display files for each asset type
**Logic**:

- Generates card components for each file
- Handles empty states
- Implements file selection UI

### `getFilteredAssetTypes`

**Purpose**: Filters displayed asset types
**Usage**: Used when rendering file sections
**Logic**:

- Returns all types if filter is 'ALL'
- Returns specific type based on active filter

## API Integration

### File List API

**Endpoint**: `GET_S3_FILELIST`
**Purpose**: Fetch available cost roadmap files
**Authentication**: Bearer token required

### File Download API

**Endpoint**: `DOWNLOAD_FILE_FROM_S3`
**Purpose**: Download specific file content
**Authentication**: Bearer token required

## Data Types

### FileInfo

```typescript
interface FileInfo {
  name: string; // File name
  key: string; // S3 key
  size: number; // File size
  lastModified: string; // Last modified date
}
```

### CostRoadmapData

```typescript
interface CostRoadmapData {
  [assetType: string]: FileInfo[]; // Files grouped by asset type
}
```

## Error Handling

- API error handling with user feedback
- Loading state management
- Empty state handling
- File operation error handling

## Usage

```typescript
import CostRoadmaps from "../components/CostRoadmaps/CostRoadmaps";

function ParentComponent() {
  return <CostRoadmaps />;
}
```

## Dependencies

- React
- Ant Design
- Axios
- Lucide React
- Styled Components
- EditableDataTable
