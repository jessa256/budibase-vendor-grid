# Wedding Vendor Grid Component - Usage Examples

This document provides practical examples for implementing the Wedding Vendor Grid Component in various scenarios and configurations.

## Table of Contents

- [Basic Implementation](#basic-implementation)
- [Budibase Setup](#budibase-setup)
- [Supabase Configuration](#supabase-configuration)
- [Common Use Cases](#common-use-cases)
- [Advanced Configurations](#advanced-configurations)
- [Customization Examples](#customization-examples)
- [Integration Patterns](#integration-patterns)

## Basic Implementation

### 1. Simple Vendor Display

The most basic implementation displays all vendors in a 4-column grid:

```javascript
// Budibase Screen Configuration
{
  "component": "wedding-vendor-grid",
  "props": {
    "dataProvider": "{{ Vendors Query }}",
    "columns": 4,
    "cardHeight": "medium",
    "cardStyle": "elevated"
  }
}
```

### 2. Minimal Configuration

For a simple, clean display:

```javascript
{
  "component": "wedding-vendor-grid",
  "props": {
    "dataProvider": "{{ Vendors Query }}",
    "columns": 3,
    "cardHeight": "small",
    "cardStyle": "flat",
    "showAddButton": false,
    "showProfilePics": false
  }
}
```

## Budibase Setup

### Step 1: Create Supabase Data Source

1. **Add Data Source**
   - Navigate to **Data** → **Data Sources**
   - Click **Add Data Source** → **REST API**
   - Name: "Supabase Vendors"
   - Base URL: `https://your-project.supabase.co/rest/v1`

2. **Configure Headers**
   ```json
   {
     "apikey": "your-supabase-anon-key",
     "Authorization": "Bearer your-supabase-anon-key",
     "Content-Type": "application/json",
     "Prefer": "return=representation"
   }
   ```

### Step 2: Create Vendor Query

```sql
-- Query Name: "Get All Vendors"
-- Method: GET
-- URL: /vendorInformation?select=*

-- With filtering (optional):
-- URL: /vendorInformation?select=*&booked=eq.false&order=vendor_name.asc
```

### Step 3: Add Component to Screen

1. **Drag Component** from the component library
2. **Configure Data Binding**:
   - Data Provider: `{{ Get All Vendors }}`
3. **Set Properties** as needed
4. **Add Event Handlers**:
   - On Vendor Click: Navigate to vendor detail screen
   - On Add Vendor: Open add vendor modal

## Supabase Configuration

### Database Setup

```sql
-- Create the vendors table
CREATE TABLE vendorInformation (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  vendor_name TEXT NOT NULL,
  contact_name TEXT,
  vendor_type TEXT,
  phone_number TEXT,
  email TEXT,
  website TEXT,
  notes TEXT,
  quoted_amount NUMERIC,
  image_url TEXT,
  booked BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Create RLS policies
ALTER TABLE vendorInformation ENABLE ROW LEVEL SECURITY;

-- Allow read access for authenticated users
CREATE POLICY "Vendors are viewable by authenticated users" 
ON vendorInformation FOR SELECT 
TO authenticated 
USING (true);

-- Allow full access for service role
CREATE POLICY "Full access for service role" 
ON vendorInformation 
FOR ALL 
TO service_role 
USING (true);
```

### Sample Data

```sql
-- Insert sample vendors
INSERT INTO vendorInformation (vendor_name, contact_name, vendor_type, phone_number, email, quoted_amount, booked) VALUES
('Amazing Photography', 'John Smith', 'PHOTOGRAPHER', '555-123-4567', 'john@amazingphoto.com', 2500.00, true),
('Elegant Catering', 'Sarah Johnson', 'FOOD & BEVERAGE', '555-234-5678', 'sarah@elegantcatering.com', 5000.00, false),
('Perfect Flowers', 'Mike Wilson', 'FLORIST', '555-345-6789', 'mike@perfectflowers.com', 800.00, true),
('Dream Venues', 'Lisa Brown', 'VENUE', '555-456-7890', 'lisa@dreamvenues.com', 8000.00, false);
```

## Common Use Cases

### 1. Wedding Planner Dashboard

Display all vendors with booking status and quick actions:

```javascript
// Component Configuration
{
  "dataProvider": "{{ All Vendors Query }}",
  "columns": 4,
  "cardHeight": "medium",
  "cardStyle": "elevated",
  "showAddButton": true,
  "addButtonText": "Add New Vendor"
}

// Event Handlers
{
  "onVendorClick": {
    "action": "Navigate To",
    "screen": "Vendor Detail",
    "parameters": {
      "vendorId": "{{ Component.vendorClick.vendor.id }}"
    }
  },
  "onAddVendor": {
    "action": "Open Modal",
    "modal": "Add Vendor Form"
  }
}
```

### 2. Vendor Comparison View

Compare multiple vendors side by side:

```javascript
// Filter for specific vendor type
{
  "dataProvider": "{{ Photographers Query }}", // Filtered query
  "columns": 3,
  "cardHeight": "large",
  "cardStyle": "outlined",
  "showAddButton": false
}

// Photographers Query:
// /vendorInformation?select=*&vendor_type=eq.PHOTOGRAPHER&order=quoted_amount.asc
```

### 3. Mobile-Optimized View

Optimized for mobile wedding planning apps:

```javascript
{
  "dataProvider": "{{ Vendors Query }}",
  "columns": 1,
  "cardHeight": "small",
  "cardStyle": "flat",
  "showProfilePics": true,
  "defaultImage": "https://your-cdn.com/default-vendor-mobile.jpg"
}
```

### 4. Vendor Portfolio Display

Showcase vendors for clients:

```javascript
{
  "dataProvider": "{{ Booked Vendors Query }}", // Only booked vendors
  "columns": 3,
  "cardHeight": "large",
  "cardStyle": "elevated",
  "showAddButton": false,
  "showProfilePics": true
}

// Booked Vendors Query:
// /vendorInformation?select=*&booked=eq.true&order=vendor_type.asc
```

## Advanced Configurations

### 1. Dynamic Column Adjustment

Adjust columns based on screen size using Budibase conditions:

```javascript
// Desktop Configuration
{
  "columns": "{{ Device.desktop ? 4 : Device.tablet ? 2 : 1 }}",
  "cardHeight": "{{ Device.mobile ? 'small' : 'medium' }}",
  "cardStyle": "elevated"
}
```

### 2. Role-Based Display

Different configurations for different user roles:

```javascript
// Wedding Planner View
{
  "dataProvider": "{{ All Vendors Query }}",
  "columns": 4,
  "showAddButton": "{{ User.role === 'planner' }}",
  "addButtonText": "Add New Vendor"
}

// Client View
{
  "dataProvider": "{{ Client Vendors Query }}", // Only their vendors
  "columns": 3,
  "showAddButton": false,
  "cardStyle": "outlined"
}
```

### 3. Search and Filter Integration

Combine with search and filter components:

```javascript
// Search Component
{
  "component": "textfield",
  "props": {
    "label": "Search Vendors",
    "placeholder": "Enter vendor name...",
    "value": "{{ State.searchTerm }}"
  },
  "events": {
    "onChange": {
      "action": "Update State",
      "key": "searchTerm",
      "value": "{{ this.value }}"
    }
  }
}

// Filter Component
{
  "component": "select",
  "props": {
    "label": "Vendor Type",
    "options": [
      { "label": "All Types", "value": "" },
      { "label": "Photographer", "value": "PHOTOGRAPHER" },
      { "label": "Catering", "value": "FOOD & BEVERAGE" },
      { "label": "Florist", "value": "FLORIST" }
    ],
    "value": "{{ State.typeFilter }}"
  }
}

// Vendor Grid with Filters
{
  "dataProvider": "{{ Filtered Vendors Query }}",
  "columns": 4
}

// Filtered Query URL:
// /vendorInformation?select=*&vendor_name=ilike.*{{ State.searchTerm }}*{{ State.typeFilter ? '&vendor_type=eq.' + State.typeFilter : '' }}
```

### 4. Real-Time Updates

Enable real-time updates using Budibase automation:

```javascript
// Auto-refresh configuration
{
  "component": "wedding-vendor-grid",
  "props": {
    "dataProvider": "{{ Live Vendors Query }}"
  },
  "refresh": {
    "interval": 30000, // Refresh every 30 seconds
    "trigger": "{{ WebSocket.vendorUpdated }}"
  }
}
```

## Customization Examples

### 1. Custom Styling with CSS

Override default styles using Budibase custom CSS:

```css
/* Custom theme for luxury weddings */
.vendor-grid-container {
  --card-background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  --card-border-radius: 12px;
  --card-shadow: 0 8px 32px rgba(0,0,0,0.1);
  --primary-color: #d4af37; /* Gold accent */
  --text-primary: #2c3e50;
}

.vendor-card.elevated {
  border: 2px solid transparent;
  background: linear-gradient(white, white) padding-box,
              linear-gradient(45deg, #d4af37, #f7dc6f) border-box;
}

.vendor-card:hover {
  transform: translateY(-4px);
  transition: transform 0.3s ease;
}
```

### 2. Custom Card Layout

Modify the display information:

```javascript
// Simplified card showing only essential info
{
  "displayFields": ["vendor_name", "contact_name", "booked"],
  "hideFields": ["vendor_type", "quoted_amount", "phone_number"],
  "showBookingStatus": true,
  "showContactDetails": false
}
```

### 3. Interactive Features

Add custom interactions:

```javascript
// Quick action buttons on cards
{
  "quickActions": [
    {
      "label": "Call",
      "icon": "phone",
      "action": "{{ 'tel:' + Component.vendor.phone_number }}"
    },
    {
      "label": "Email", 
      "icon": "email",
      "action": "{{ 'mailto:' + Component.vendor.email }}"
    },
    {
      "label": "Book",
      "icon": "calendar",
      "action": "Navigate To",
      "screen": "Booking Form",
      "condition": "{{ !Component.vendor.booked }}"
    }
  ]
}
```

## Integration Patterns

### 1. Modal Integration

Open vendor details in a modal:

```javascript
// Vendor Grid Configuration
{
  "onVendorClick": {
    "action": "Open Modal",
    "modal": "vendorDetailModal",
    "parameters": {
      "vendorData": "{{ Component.vendorClick.vendor }}"
    }
  }
}

// Modal Component
{
  "component": "modal",
  "props": {
    "title": "{{ Modal.vendorData.vendor_name }}",
    "size": "large"
  },
  "children": [
    {
      "component": "vendorDetailForm",
      "props": {
        "vendorId": "{{ Modal.vendorData.id }}",
        "readonly": true
      }
    }
  ]
}
```

### 2. Form Integration

Connect with add/edit vendor forms:

```javascript
// Add Vendor Flow
{
  "onAddVendor": {
    "action": "Navigate To",
    "screen": "Add Vendor",
    "parameters": {
      "returnUrl": "{{ Current.url }}"
    }
  }
}

// Form Submit Handler
{
  "onFormSubmit": {
    "action": "Execute Query",
    "query": "Create Vendor",
    "parameters": "{{ Form.values }}",
    "onSuccess": {
      "action": "Navigate To",
      "screen": "{{ State.returnUrl }}"
    }
  }
}
```

### 3. Data Synchronization

Keep data synchronized across components:

```javascript
// Global State Management
{
  "component": "stateProvider",
  "state": {
    "vendors": "{{ Vendors Query.data }}",
    "selectedVendor": null,
    "filters": {
      "search": "",
      "type": "",
      "booked": null
    }
  },
  "children": [
    {
      "component": "wedding-vendor-grid",
      "props": {
        "dataProvider": "{{ State.vendors }}",
        "selectedVendor": "{{ State.selectedVendor }}"
      }
    }
  ]
}
```

### 4. Multi-Screen Workflow

Create a complete vendor management workflow:

```javascript
// Screen 1: Vendor List
{
  "screens": [
    {
      "name": "vendorList",
      "components": [
        {
          "component": "wedding-vendor-grid",
          "events": {
            "onVendorClick": "Navigate → vendorDetail",
            "onAddVendor": "Navigate → addVendor"
          }
        }
      ]
    },
    
    // Screen 2: Vendor Detail
    {
      "name": "vendorDetail",
      "parameters": ["vendorId"],
      "components": [
        {
          "component": "vendorDetailView",
          "props": {
            "vendorId": "{{ URL.vendorId }}"
          }
        }
      ]
    },
    
    // Screen 3: Add Vendor
    {
      "name": "addVendor",
      "components": [
        {
          "component": "vendorForm",
          "events": {
            "onSave": "Navigate → vendorList",
            "onCancel": "Navigate → vendorList"
          }
        }
      ]
    }
  ]
}
```

## Testing Examples

### 1. Component Testing

```javascript
// Test vendor grid rendering
describe('Wedding Vendor Grid', () => {
  const mockData = {
    rows: [
      {
        id: '1',
        vendor_name: 'Test Vendor',
        contact_name: 'John Doe',
        vendor_type: 'PHOTOGRAPHER',
        booked: true
      }
    ],
    loading: false,
    error: null
  };

  test('renders vendor cards correctly', () => {
    const { getByText } = render(WeddingVendorGrid, {
      props: { dataProvider: mockData }
    });
    
    expect(getByText('Test Vendor')).toBeInTheDocument();
    expect(getByText('John Doe')).toBeInTheDocument();
  });

  test('handles vendor click events', async () => {
    const { getByRole, component } = render(WeddingVendorGrid, {
      props: { dataProvider: mockData }
    });
    
    const vendorCard = getByRole('button');
    const mockHandler = jest.fn();
    
    component.$on('vendorClick', mockHandler);
    await fireEvent.click(vendorCard);
    
    expect(mockHandler).toHaveBeenCalledWith(
      expect.objectContaining({
        detail: { vendor: mockData.rows[0] }
      })
    );
  });
});
```

### 2. Integration Testing

```javascript
// Test Budibase integration
describe('Budibase Integration', () => {
  test('data provider binding works correctly', () => {
    const screen = {
      components: [{
        type: 'wedding-vendor-grid',
        props: {
          dataProvider: '{{ Vendors Query }}'
        }
      }]
    };
    
    // Test that binding resolves correctly
    expect(resolveBinding(screen.components[0].props.dataProvider))
      .toEqual(expect.objectContaining({
        rows: expect.any(Array),
        loading: expect.any(Boolean)
      }));
  });
});
```

### 3. Accessibility Testing

```javascript
// Test accessibility compliance
describe('Accessibility', () => {
  test('has proper ARIA labels', () => {
    const { container } = render(WeddingVendorGrid, {
      props: { dataProvider: mockData }
    });
    
    const vendorCards = container.querySelectorAll('[role="button"]');
    vendorCards.forEach(card => {
      expect(card).toHaveAttribute('aria-label');
      expect(card).toHaveAttribute('tabindex', '0');
    });
  });

  test('supports keyboard navigation', async () => {
    const { container } = render(WeddingVendorGrid, {
      props: { dataProvider: mockData }
    });
    
    const firstCard = container.querySelector('[role="button"]');
    firstCard.focus();
    
    await fireEvent.keyDown(firstCard, { key: 'Enter' });
    // Verify vendor click event was triggered
  });
});
```

## Performance Examples

### 1. Large Dataset Handling

```javascript
// Pagination configuration for large vendor lists
{
  "component": "wedding-vendor-grid",
  "props": {
    "dataProvider": "{{ Paginated Vendors Query }}",
    "pagination": {
      "pageSize": 20,
      "currentPage": "{{ State.currentPage }}",
      "total": "{{ Vendors Count Query.data.count }}"
    }
  }
}

// Paginated query:
// /vendorInformation?select=*&limit=20&offset={{ (State.currentPage - 1) * 20 }}
```

### 2. Image Optimization

```javascript
// Optimized image loading
{
  "imageSettings": {
    "lazy": true,
    "placeholder": "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIHZpZXdCb3g9IjAgMCA2NCA2NCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHJlY3Qgd2lkdGg9IjY0IiBoZWlnaHQ9IjY0IiBmaWxsPSIjRjNGNEY2Ii8+CjxwYXRoIGQ9Ik0yMS4zMzMzIDQyLjY2NjdIMTAuNjY2N1YzMi41MzMzSDIxLjMzMzNWNDIuNjY2N1oiIGZpbGw9IiM5Q0E0QUIiLz4KPC9zdmc+",
    "srcset": {
      "small": "?w=64&h=64",
      "medium": "?w=128&h=128", 
      "large": "?w=256&h=256"
    }
  }
}
```

### 3. Caching Strategy

```javascript
// Client-side caching configuration
{
  "caching": {
    "enabled": true,
    "duration": 300000, // 5 minutes
    "key": "vendors-{{ User.id }}-{{ State.filters }}",
    "invalidateOn": ["vendorUpdated", "vendorAdded", "vendorDeleted"]
  }
}
```

## Deployment Examples

### 1. Environment Configuration

```javascript
// Development Environment
{
  "supabaseUrl": "https://dev-project.supabase.co",
  "supabaseKey": "dev-anon-key",
  "debug": true,
  "mockData": true
}

// Production Environment  
{
  "supabaseUrl": "https://prod-project.supabase.co",
  "supabaseKey": "prod-anon-key",
  "debug": false,
  "mockData": false,
  "analytics": true
}
```

### 2. Version Migration

```javascript
// Migration from v0.9.x to v1.0.0
const migrationConfig = {
  "oldProps": {
    "vendorData": "dataProvider",
    "gridColumns": "columns",
    "showImages": "showProfilePics"
  },
  "newDefaults": {
    "cardStyle": "elevated",
    "cardHeight": "medium"
  }
};
```

This comprehensive usage guide should help you implement the Wedding Vendor Grid Component in various scenarios. Remember to test thoroughly in your specific Budibase environment and adjust configurations based on your exact requirements.