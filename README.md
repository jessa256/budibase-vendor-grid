# Wedding Vendor Grid Component (Simplified)

<div align="center">

**🎊 A clean, responsive wedding vendor management component for Budibase**

*Streamlined vendor display with dynamic layouts and beautiful card designs*

[![Version](https://img.shields.io/badge/version-1.1.0-blue.svg)](https://github.com/jessa256/budibase-vendor-grid/releases)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Budibase](https://img.shields.io/badge/Budibase-Compatible-purple.svg)](https://budibase.com)
[![Supabase](https://img.shields.io/badge/Supabase-Ready-orange.svg)](https://supabase.com)

[✨ Features](#features) • [🚀 Quick Start](#quick-start) • [📖 Documentation](#documentation) • [🤝 Contributing](#contributing)

</div>

---

## 🎯 Overview

The **Wedding Vendor Grid Component v1.1** is a simplified, responsive Budibase component designed for modern wedding planning applications. It displays vendor information in a clean, dynamic grid layout that automatically adapts to content and screen size.

### ✨ Key Highlights

- 🎨 **Clean, Minimal Design**: Simplified card layout focusing on essential vendor information
- 📱 **Fully Responsive**: Dynamic grid that adapts to any screen size
- ⚡ **Performance Optimized**: Streamlined component for faster loading
- ♿ **Accessibility First**: Full keyboard navigation and screen reader support
- 🔌 **Supabase Ready**: Direct integration with your vendor database

---

## ✨ Features

### 🎨 **Visual Design**
- **Dynamic Card Heights**: Cards automatically adjust based on content
- **Responsive Grid Layout**: Auto-fit grid with intelligent spacing
- **Three Card Styles**: Elevated, Outlined, or Flat design variants
- **Profile Picture Display**: Beautiful vendor photos with fallback handling
- **Booking Status Indicators**: Clear visual indicators for booked vendors

### 📱 **Responsive & Mobile**
- **Mobile-First Design**: Optimized for wedding planning on-the-go
- **Automatic Layout Adaptation**: From desktop grids to mobile single-column
- **Touch-Friendly Interface**: Perfect for tablet and mobile use
- **Consistent Spacing**: Proper whitespace management across all devices

### 🔗 **Data Integration**
- **Supabase REST API**: Direct integration with your vendor database
- **Real-Time Updates**: Reactive data binding with loading states
- **Error Handling**: Graceful error display for network issues
- **Flexible Schema**: Works with standard vendor information structure

### ♿ **Accessibility**
- **WCAG 2.1 AA Compliant**: Professional accessibility standards
- **Keyboard Navigation**: Full tab and enter key support
- **Screen Reader Friendly**: Semantic HTML with ARIA labels
- **High Contrast Support**: Automatic adaptation for accessibility needs

---

## 🚀 Quick Start

### 1. **Install in Budibase**

1. Download the latest `.tar.gz` file from [Releases](https://github.com/jessa256/budibase-vendor-grid/releases)
2. In Budibase: **Settings** → **Plugins** → **Add Plugin**
3. Upload the component file
4. The component appears in your component library as **"Wedding Vendor Grid (Simplified)"**

### 2. **Set Up Your Data**

```sql
-- Supabase Table: vendorInformation
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
  is_booked BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### 3. **Add to Your Screen**

1. **Drag the component** onto your Budibase screen
2. **Configure the data provider** to point to your vendor query
3. **Choose your preferred card style** (Elevated, Outlined, or Flat)
4. **Set card height** (Small, Medium, or Large)
5. **Add default image URL** for vendors without photos

### 4. **Basic Configuration**

```javascript
// Component Settings in Budibase
{
  "dataProvider": "{{ Your Vendor Query }}",
  "cardHeight": "medium",
  "cardStyle": "elevated",
  "defaultImage": "https://your-domain.com/default-vendor.jpg"
}
```

---

## 📖 Documentation

### 🔧 **Component Props**

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `dataProvider` | DataProvider | *required* | Budibase data provider with vendor information |
| `cardHeight` | String | `"medium"` | Card height: `"small"`, `"medium"`, `"large"` |
| `cardStyle` | String | `"elevated"` | Visual style: `"elevated"`, `"outlined"`, `"flat"` |
| `defaultImage` | String | `""` | Fallback image URL for vendors without photos |

### 📊 **Expected Data Structure**

```typescript
interface VendorData {
  id: string;
  vendor_name: string;          // Required: Business name
  contact_name?: string;        // Contact person
  vendor_type?: string;         // Service category
  phone_number?: string;        // Contact phone
  email?: string;              // Contact email
  quoted_amount?: number;       // Service price
  image_url?: string;          // Profile picture URL
  is_booked?: boolean;         // Booking status
}
```

### 🎨 **Card Styles**

- **Elevated**: Material Design shadow elevation (default)
- **Outlined**: Clean border with minimal shadow
- **Flat**: No borders or shadows for minimal design

### 📏 **Card Heights**

- **Small**: `200-280px` - Compact display
- **Medium**: `240-350px` - Balanced layout (default)
- **Large**: `280-400px` - Spacious cards with room for content

---

## 🔌 Integration Examples

### **Basic Wedding Vendor Display**

```javascript
// Simple 4-column vendor grid
{
  "component": "wedding-vendor-grid",
  "props": {
    "dataProvider": "{{ All Vendors Query }}",
    "cardHeight": "medium",
    "cardStyle": "elevated"
  }
}
```

### **Mobile-Optimized View**

```javascript
// Single column for mobile wedding planning
{
  "component": "wedding-vendor-grid", 
  "props": {
    "dataProvider": "{{ Vendors Query }}",
    "cardHeight": "small",
    "cardStyle": "flat",
    "defaultImage": "https://your-cdn.com/mobile-default.jpg"
  }
}
```

### **Vendor Portfolio Display**

```javascript
// Showcase booked vendors for clients
{
  "component": "wedding-vendor-grid",
  "props": {
    "dataProvider": "{{ Booked Vendors Query }}", // is_booked = true
    "cardHeight": "large", 
    "cardStyle": "elevated"
  }
}
```

---

## 🔧 Development

### **Local Development Setup**

```bash
# Clone the repository
git clone https://github.com/jessa256/budibase-vendor-grid.git
cd budibase-vendor-grid

# Install dependencies
npm install

# Start development with hot reload
npm run watch

# Build for production
npm run build

# Package for Budibase
npm run package
```

### **Project Structure**

```
wedding-vendor-grid/
├── src/
│   ├── Component.svelte      # Main simplified component
│   └── index.js             # Component entry point
├── dist/                    # Build output
├── schema.json             # Budibase component configuration
├── package.json            # Project dependencies
├── rollup.config.js        # Build configuration
└── README.md               # This documentation
```

### **Development Commands**

| Command | Purpose |
|---------|---------|
| `npm run watch` | Development with hot reload |
| `npm run build` | Production build |
| `npm run package` | Create Budibase plugin package |
| `npm test` | Run component tests |
| `npm run lint` | Code quality checks |

---

## 🔄 Migration from v1.0.0

### **What Changed in v1.1.0**

- ✅ **Simplified Design**: Cleaner, more professional appearance
- ✅ **Dynamic Layouts**: Cards automatically adjust to content
- ✅ **Better Mobile Experience**: Improved responsive behavior  
- ❌ **Removed Features**: Add vendor button, manual column control, detailed contact info

### **Migration Steps**

1. **Update Component**: Replace with v1.1.0 package
2. **Remove Old Props**: Delete `columns`, `showAddButton`, `addButtonText` settings
3. **Update Data Field**: Ensure you're using `is_booked` (not `booked`) in your database
4. **Test Responsive**: Verify layout on different screen sizes

---

## 🤝 Contributing

We welcome contributions to make this component even better!

### **How to Contribute**

1. 🍴 **Fork** the repository
2. 🌿 **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. 💾 **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. 📤 **Push** to the branch (`git push origin feature/amazing-feature`)
5. 🎯 **Open** a Pull Request

### **Development Guidelines**

- Follow existing code style and conventions
- Add tests for new functionality
- Update documentation for any API changes
- Ensure accessibility standards are maintained

---

## 🆘 Support & Community

| Resource | Link |
|----------|------|
| 📖 **Documentation** | [Component Wiki](https://github.com/jessa256/budibase-vendor-grid/wiki) |
| 🐛 **Bug Reports** | [GitHub Issues](https://github.com/jessa256/budibase-vendor-grid/issues) |
| 💬 **Discussions** | [GitHub Discussions](https://github.com/jessa256/budibase-vendor-grid/discussions) |
| 📧 **Direct Support** | [jessica@example.com](mailto:jessica@example.com) |

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Budibase Team** for the excellent low-code platform
- **Supabase** for providing robust backend infrastructure  
- **Svelte** for the reactive component framework
- **Wedding Planning Community** for feature inspiration and feedback

---

<div align="center">

**Built with ❤️ for wedding planners everywhere**

*Making wedding vendor management beautiful and effortless*

⭐ **Star this repo** if it helps make your wedding planning easier! ⭐

[🚀 Get Started](#quick-start) • [📖 Documentation](#documentation) • [🤝 Contribute](#contributing)

</div>