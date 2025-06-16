# Wedding Vendor Grid Component API Documentation

## Overview

The Wedding Vendor Grid Component is a Budibase custom component that displays wedding vendor information in a responsive grid layout. This document provides comprehensive API documentation for developers implementing and customizing the component.

## Table of Contents

- [Component Props](#component-props)
- [Data Provider Schema](#data-provider-schema)
- [Event System](#event-system)
- [CSS Custom Properties](#css-custom-properties)
- [Accessibility API](#accessibility-api)
- [Integration Examples](#integration-examples)
- [Error Handling](#error-handling)

## Component Props

### Required Props

#### `dataProvider`
- **Type**: `DataProvider`
- **Required**: `true`
- **Description**: Budibase data provider containing vendor information from Supabase
- **Example**:
```javascript
{
  rows: Array<VendorObject>,
  loading: boolean,
  error: string | null
}
```

### Optional Props

#### `columns`
- **Type**: `number`
- **Default**: `4`
- **Range**: `1-6`
- **Description**: Number of columns in the grid layout
- **Responsive Behavior**: Automatically reduces on smaller screens

#### `cardHeight`
- **Type**: `string`
- **Default**: `"medium"`
- **Options**: `"small" | "medium" | "large"`
- **Description**: Controls the height of vendor cards
- **Measurements**:
  - `small`: 180px
  - `medium`: 220px
  - `large`: 280px

#### `cardStyle`
- **Type**: `string`
- **Default**: `"elevated"`
- **Options**: `"elevated" | "outlined" | "flat"`
- **Description**: Visual styling variant for vendor cards
- **Details**:
  - `elevated`: Material Design shadow elevation
  - `outlined`: Clean border with no shadow
  - `flat`: Minimal styling with no borders or shadows

#### `showAddButton`
- **Type**: `boolean`
- **Default**: `true`
- **Description**: Controls visibility of the "Add New Vendor" button

#### `addButtonText`
- **Type**: `string`
- **Default**: `"Add New Vendor"`
- **Description**: Customizable text for the add vendor button

#### `showProfilePics`
- **Type**: `boolean`
- **Default**: `true`
- **Description**: Controls visibility of vendor profile pictures

#### `defaultImage`
- **Type**: `string`
- **Default**: `""`
- **Description**: Fallback image URL for vendors without profile pictures

## Data Provider Schema

### VendorObject Interface

```typescript
interface VendorObject {
  id: string;                    // Unique identifier (UUID)
  vendor_name: string;           // Business/vendor name
  contact_name?: string;         // Primary contact person
  vendor_type?: string;          // Service category
  phone_number?: string;         // Contact phone number
  email?: string;                // Contact email address
  website?: string;              // Vendor website URL
  notes?: string;                // Additional notes
  quoted_amount?: number;        // Quoted service price
  image_url?: string;            // Profile picture URL
  booked?: boolean;              // Booking status
  created_at?: string;           // ISO timestamp
}
```

### DataProvider Interface

```typescript
interface DataProvider {
  rows: VendorObject[];          // Array of vendor objects
  loading: boolean;              // Loading state indicator
  error: string | null;          // Error message if any
}
```

### Supabase Query Example

```sql
SELECT 
  id,
  vendor_name,
  contact_name,
  vendor_type,
  phone_number,
  email,
  website,
  notes,
  quoted_amount,
  image_url,
  booked,
  created_at
FROM vendorInformation
WHERE deleted_at IS NULL
ORDER BY 
  booked DESC,
  vendor_name ASC;
```

## Event System

### Event Handlers

#### `vendorClick`
- **Trigger**: When a vendor card is clicked
- **Payload**: `{ vendor: VendorObject }`
- **Usage**: Navigate to vendor details or open modal
- **Example**:
```javascript
function handleVendorClick(event) {
  const vendor = event.detail.vendor;
  // Navigate to vendor detail page
  navigate(`/vendors/${vendor.id}`);
}
```

#### `addVendor`
- **Trigger**: When add vendor button is clicked
- **Payload**: `{}`
- **Usage**: Open add vendor form or navigate to creation page
- **Example**:
```javascript
function handleAddVendor(event) {
  // Open add vendor modal or navigate
  openModal('add-vendor-form');
}
```

### Budibase Event Binding

In Budibase, bind events using the component settings:

1. **On Vendor Click**: Select "Navigate To" action
   - **Screen**: Vendor Detail Screen
   - **Parameters**: Pass `{{ Component.vendorClick.vendor.id }}`

2. **On Add Vendor**: Select "Open Modal" or "Navigate To" action
   - **Modal**: Add Vendor Form
   - **Screen**: Add Vendor Screen

## CSS Custom Properties

### Theme Variables

```css
.vendor-grid-container {
  /* Grid Layout */
  --grid-gap: 1rem;
  --grid-padding: 1rem;
  
  /* Card Styling */
  --card-border-radius: 8px;
  --card-background: #ffffff;
  --card-border: 1px solid #e5e7eb;
  
  /* Shadows */
  --card-shadow: 0 2px 8px rgba(0,0,0,0.1);
  --card-hover-shadow: 0 4px 16px rgba(0,0,0,0.15);
  --card-elevated-shadow: 0 4px 16px rgba(0,0,0,0.12);
  
  /* Colors */
  --primary-color: #6366f1;
  --success-color: #10b981;
  --error-color: #ef4444;
  --warning-color: #f59e0b;
  
  /* Text Colors */
  --text-primary: #111827;
  --text-secondary: #6b7280;
  --text-tertiary: #9ca3af;
  
  /* Interactive States */
  --hover-background: #f9fafb;
  --focus-outline: 2px solid #6366f1;
  --focus-outline-offset: 2px;
  
  /* Transitions */
  --transition-duration: 0.2s;
  --transition-easing: ease-in-out;
}
```

### Card Size Variables

```css
/* Small Cards */
.vendor-card.small {
  --card-height: 180px;
  --card-padding: 0.75rem;
  --image-size: 48px;
  --font-size-title: 0.875rem;
  --font-size-subtitle: 0.75rem;
}

/* Medium Cards */
.vendor-card.medium {
  --card-height: 220px;
  --card-padding: 1rem;
  --image-size: 64px;
  --font-size-title: 1rem;
  --font-size-subtitle: 0.875rem;
}

/* Large Cards */
.vendor-card.large {
  --card-height: 280px;
  --card-padding: 1.25rem;
  --image-size: 80px;
  --font-size-title: 1.125rem;
  --font-size-subtitle: 1rem;
}
```

### Responsive Breakpoints

```css
/* Mobile */
@media (max-width: 640px) {
  .vendor-grid {
    grid-template-columns: 1fr;
    --grid-gap: 0.75rem;
    --grid-padding: 0.75rem;
  }
}

/* Tablet */
@media (min-width: 641px) and (max-width: 1024px) {
  .vendor-grid {
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  }
}

/* Desktop */
@media (min-width: 1025px) {
  .vendor-grid {
    grid-template-columns: repeat(var(--grid-columns, 4), 1fr);
  }
}
```

## Accessibility API

### ARIA Attributes

The component implements comprehensive ARIA support:

```html
<!-- Vendor Card -->
<div 
  role="button"
  tabindex="0"
  aria-label="View details for Amazing Photography"
  aria-describedby="vendor-123-description"
>
  <!-- Card Content -->
  <div id="vendor-123-description" class="sr-only">
    Amazing Photography, contact John Smith, 
    quoted $2500, currently booked
  </div>
</div>

<!-- Add Vendor Button -->
<div 
  role="button"
  tabindex="0"
  aria-label="Add new vendor to your wedding planning list"
>
  Add New Vendor
</div>
```

### Keyboard Navigation

| Key | Action |
|-----|---------|
| `Tab` | Navigate between vendor cards |
| `Shift + Tab` | Navigate backwards |
| `Enter` | Activate selected vendor card |
| `Space` | Alternative activation key |
| `Escape` | Cancel any modal or overlay |

### Screen Reader Support

- **Card Announcements**: "Button, Amazing Photography vendor card, contact John Smith, $2500 quoted, booked"
- **Status Updates**: "Loading vendors..." / "Error loading vendors" / "X vendors loaded"
- **Navigation**: "Vendor 1 of 12" announcements

## Integration Examples

### Basic Implementation

```svelte
<script>
  import { onMount } from 'svelte';
  
  let vendors = [];
  let loading = true;
  let error = null;
  
  onMount(async () => {
    try {
      const response = await fetch('/api/vendors');
      vendors = await response.json();
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  });
  
  function handleVendorClick(event) {
    const vendor = event.detail.vendor;
    window.location.href = `/vendors/${vendor.id}`;
  }
  
  function handleAddVendor() {
    window.location.href = '/vendors/new';
  }
  
  $: dataProvider = { rows: vendors, loading, error };
</script>

<WeddingVendorGrid
  {dataProvider}
  columns={4}
  cardHeight="medium"
  cardStyle="elevated"
  showAddButton={true}
  on:vendorClick={handleVendorClick}
  on:addVendor={handleAddVendor}
/>
```

### Advanced Configuration

```svelte
<script>
  import { writable } from 'svelte/store';
  
  const vendorStore = writable([]);
  const settingsStore = writable({
    columns: 3,
    cardHeight: 'large',
    cardStyle: 'outlined',
    showProfilePics: true,
    defaultImage: 'https://cdn.example.com/default-vendor.jpg'
  });
  
  let searchFilter = '';
  let typeFilter = 'all';
  
  $: filteredVendors = $vendorStore.filter(vendor => {
    const matchesSearch = vendor.vendor_name
      .toLowerCase()
      .includes(searchFilter.toLowerCase());
    const matchesType = typeFilter === 'all' || 
      vendor.vendor_type === typeFilter;
    return matchesSearch && matchesType;
  });
  
  $: dataProvider = { 
    rows: filteredVendors, 
    loading: false, 
    error: null 
  };
</script>

<!-- Filter Controls -->
<div class="filter-controls">
  <input 
    bind:value={searchFilter}
    placeholder="Search vendors..."
    type="text"
  />
  <select bind:value={typeFilter}>
    <option value="all">All Types</option>
    <option value="PHOTOGRAPHER">Photography</option>
    <option value="FOOD & BEVERAGE">Catering</option>
    <option value="ENTERTAINMENT">Entertainment</option>
  </select>
</div>

<!-- Vendor Grid -->
<WeddingVendorGrid
  {dataProvider}
  columns={$settingsStore.columns}
  cardHeight={$settingsStore.cardHeight}
  cardStyle={$settingsStore.cardStyle}
  showProfilePics={$settingsStore.showProfilePics}
  defaultImage={$settingsStore.defaultImage}
  on:vendorClick={handleVendorClick}
  on:addVendor={handleAddVendor}
/>
```

### Supabase Integration

```javascript
// Supabase client configuration
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_ANON_KEY
);

// Fetch vendors with real-time updates
async function fetchVendors() {
  const { data, error } = await supabase
    .from('vendorInformation')
    .select('*')
    .order('vendor_name', { ascending: true });
    
  if (error) throw error;
  return data;
}

// Real-time subscription
function subscribeToVendors(callback) {
  return supabase
    .channel('vendors')
    .on('postgres_changes', 
      { event: '*', schema: 'public', table: 'vendorInformation' },
      callback
    )
    .subscribe();
}
```

## Error Handling

### Error Types

The component handles various error scenarios:

#### Network Errors
```javascript
{
  error: "Failed to fetch vendor data",
  type: "NETWORK_ERROR",
  retry: true
}
```

#### Data Validation Errors
```javascript
{
  error: "Invalid vendor data format",
  type: "VALIDATION_ERROR",
  retry: false
}
```

#### Authentication Errors
```javascript
{
  error: "Unauthorized access to vendor data",
  type: "AUTH_ERROR", 
  retry: false
}
```

### Error Display

The component displays errors in a user-friendly format:

```html
<div class="error-container" role="alert">
  <h3>Unable to Load Vendors</h3>
  <p>We're having trouble loading your vendor information. Please try again.</p>
  <button onclick="retry()">Retry</button>
</div>
```

### Error Recovery

Implement retry logic for transient errors:

```javascript
async function loadVendorsWithRetry(maxRetries = 3) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      return await fetchVendors();
    } catch (error) {
      if (attempt === maxRetries) throw error;
      await new Promise(resolve => 
        setTimeout(resolve, 1000 * attempt)
      );
    }
  }
}
```

## Performance Optimization

### Virtualization for Large Lists

For vendors lists exceeding 100 items, consider implementing virtualization:

```javascript
// Virtual scrolling configuration
const virtualConfig = {
  itemHeight: 220, // Match card height
  bufferSize: 5,   // Extra items to render
  threshold: 100   // When to enable virtualization
};
```

### Image Optimization

Optimize vendor profile images:

```javascript
// Image loading strategy
const imageConfig = {
  lazy: true,           // Lazy load images
  placeholder: true,    // Show placeholder while loading
  srcset: true,         // Responsive image sizes
  webp: true           // Use WebP when supported
};
```

### Bundle Optimization

Component build optimizations:

```javascript
// Rollup configuration for optimal bundle size
export default {
  plugins: [
    terser({
      compress: {
        drop_console: true,
        drop_debugger: true
      }
    })
  ]
};
```

---

## Version Compatibility

| Component Version | Budibase Version | Svelte Version |
|------------------|------------------|----------------|
| 1.0.0            | 2.0+             | 4.0+           |

## Support

For API-related questions or issues:

- 📖 [Full Documentation](https://github.com/jessa256/budibase-vendor-grid/wiki)
- 🐛 [Report Issues](https://github.com/jessa256/budibase-vendor-grid/issues)
- 💬 [Community Discussions](https://github.com/jessa256/budibase-vendor-grid/discussions)

---

*Last Updated: June 15, 2025*