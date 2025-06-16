# 💒 Wedding Vendor Grid Component

<div align="center">

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg?style=for-the-badge)
![License](https://img.shields.io/badge/license-MIT-green.svg?style=for-the-badge)
![Budibase](https://img.shields.io/badge/platform-Budibase-6366f1.svg?style=for-the-badge)
![Supabase](https://img.shields.io/badge/backend-Supabase-3ecf8e.svg?style=for-the-badge)

**A beautiful, responsive wedding vendor management grid component for Budibase**

*Transform your wedding planning workflow with an elegant, customizable vendor display that seamlessly integrates with your Supabase backend.*

---

</div>

## ✨ Features

<table>
<tr>
<td width="50%">

### 🎨 **Design & Layout**
- 📱 **Fully Responsive** - Adapts from 1-6 columns automatically
- 🎯 **3 Card Styles** - Elevated, Outlined, or Flat designs
- 📏 **Flexible Sizing** - Small, Medium, or Large card heights
- 🖼️ **Profile Pictures** - Image display with smart fallbacks

</td>
<td width="50%">

### ⚡ **Performance & Data**
- 🔗 **Supabase Integration** - Direct REST API connectivity
- 🚀 **Optimized Rendering** - Fast loading with error handling
- 📊 **Real-time Ready** - Live data updates and synchronization
- 🔍 **Smart Filtering** - Built-in search and category filters

</td>
</tr>
<tr>
<td width="50%">

### 💼 **Vendor Management**
- 📋 **Complete Profiles** - Name, contact, type, and pricing
- ✅ **Booking Status** - Visual indicators for confirmed vendors
- 📞 **Quick Actions** - Click-to-call and email functionality
- ➕ **Easy Addition** - One-click vendor creation

</td>
<td width="50%">

### ♿ **Accessibility & UX**
- 🎯 **WCAG 2.1 Compliant** - Full accessibility support
- ⌨️ **Keyboard Navigation** - Complete keyboard accessibility
- 🎭 **Screen Reader Ready** - Comprehensive ARIA labeling
- 📱 **Touch Friendly** - Optimized for mobile devices

</td>
</tr>
</table>

## 🚀 Quick Start

### 1️⃣ Installation

```bash
# Clone the repository
git clone https://github.com/jessa256/budibase-vendor-grid.git
cd budibase-vendor-grid

# Install dependencies
npm install

# Build the component
npm run build

# Package for Budibase
npm run package
```

### 2️⃣ Supabase Setup

Create your vendor table with this schema:

```sql
CREATE TABLE vendorInformation (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  vendor_name TEXT NOT NULL,
  contact_name TEXT,
  vendor_type TEXT,
  phone_number TEXT,
  email TEXT,
  quoted_amount NUMERIC,
  image_url TEXT,
  booked BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### 3️⃣ Budibase Integration

1. **Upload the plugin** to your Budibase app
2. **Create a data source** pointing to your Supabase table
3. **Drag the component** onto your screen
4. **Configure the data provider** and customize appearance
5. **Add event handlers** for vendor interactions

> 🎉 **That's it!** Your wedding vendor grid is ready to use.

## ⚙️ Configuration

<details>
<summary><strong>📊 Component Properties</strong></summary>

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `dataProvider` | DataProvider | *required* | Supabase data source with vendor information |
| `columns` | Number (1-6) | `4` | Grid columns (auto-responsive) |
| `cardHeight` | `small` \| `medium` \| `large` | `medium` | Card height variant |
| `cardStyle` | `elevated` \| `outlined` \| `flat` | `elevated` | Visual card style |
| `showAddButton` | Boolean | `true` | Display "Add Vendor" button |
| `addButtonText` | String | `"Add New Vendor"` | Custom button text |
| `showProfilePics` | Boolean | `true` | Show vendor photos |
| `defaultImage` | String | `""` | Fallback image URL |

</details>

<details>
<summary><strong>🎯 Event Handlers</strong></summary>

| Event | Payload | Use Case |
|-------|---------|----------|
| `vendorClick` | `{ vendor }` | Navigate to vendor details or open modal |
| `addVendor` | `{}` | Open add vendor form or navigate to creation page |

**Example Event Configuration:**
```javascript
// Vendor Click → Navigate to Details
{
  "action": "Navigate To",
  "screen": "Vendor Detail",
  "parameters": { "vendorId": "{{ Component.vendorClick.vendor.id }}" }
}

// Add Vendor → Open Modal
{
  "action": "Open Modal",
  "modal": "Add Vendor Form"
}
```

</details>

<details>
<summary><strong>🗄️ Database Schema</strong></summary>

Your Supabase `vendorInformation` table should include:

```sql
CREATE TABLE vendorInformation (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  vendor_name TEXT NOT NULL,              -- Business name
  contact_name TEXT,                      -- Primary contact
  vendor_type TEXT,                       -- Category (PHOTOGRAPHER, etc.)
  phone_number TEXT,                      -- Contact phone
  email TEXT,                            -- Contact email
  website TEXT,                          -- Vendor website
  notes TEXT,                            -- Additional notes
  quoted_amount NUMERIC,                 -- Service price
  image_url TEXT,                        -- Profile picture URL
  booked BOOLEAN DEFAULT FALSE,          -- Booking status
  created_at TIMESTAMPTZ DEFAULT NOW(),  -- Creation date
  updated_at TIMESTAMPTZ DEFAULT NOW()   -- Last update
);
```

**Sample Data:**
```sql
INSERT INTO vendorInformation (vendor_name, contact_name, vendor_type, phone_number, email, quoted_amount, booked) VALUES
('Amazing Photography', 'John Smith', 'PHOTOGRAPHER', '555-123-4567', 'john@amazingphoto.com', 2500.00, true),
('Elegant Catering', 'Sarah Johnson', 'FOOD & BEVERAGE', '555-234-5678', 'sarah@elegantcatering.com', 5000.00, false);
```

</details>

## 🛠️ Development

<details>
<summary><strong>🔧 Development Setup</strong></summary>

```bash
# Clone and setup
git clone https://github.com/jessa256/budibase-vendor-grid.git
cd budibase-vendor-grid
npm install

# Development with hot reload
npm run watch

# Build for production
npm run build

# Run tests
npm test

# Package for deployment
npm run package
```

**Prerequisites:**
- Node.js 16+
- Budibase CLI
- VS Code with Svelte extension (recommended)

</details>

<details>
<summary><strong>📁 Project Structure</strong></summary>

```
wedding-vendor-grid/
├── src/
│   ├── Component.svelte     # Main component
│   └── index.js            # Entry point
├── dist/                   # Build output
├── schema.json            # Budibase configuration
├── package.json           # Dependencies
├── rollup.config.js       # Build config
├── README.md              # Documentation
├── LICENSE                # MIT license
└── CHANGELOG.md           # Version history
```

</details>

<details>
<summary><strong>🧪 Testing & Quality</strong></summary>

| Command | Purpose |
|---------|---------|
| `npm test` | Run all tests |
| `npm run test:watch` | Watch mode for development |
| `npm run test:coverage` | Generate coverage report |
| `npm run lint` | Code quality checks |
| `npm run format` | Format code with Prettier |

</details>

## 🗺️ Roadmap

### 🎯 **Phase 1: Core Enhancements** *(Next 2-4 weeks)*
- [ ] Simplified card design with minimal information
- [ ] Enhanced booking status with red/green indicators  
- [ ] Vendor details integration with navigation
- [ ] Payment plan connectivity

### 🚀 **Phase 2: Advanced Features** *(1-2 months)*
- [ ] Search and filtering system
- [ ] Bulk operations and multi-select
- [ ] Real-time updates with WebSocket
- [ ] Advanced grid features (drag-drop, sorting)

### 💼 **Phase 3: Professional Features** *(2-3 months)*
- [ ] Communication integration (email, SMS)
- [ ] Document management system
- [ ] Vendor rating and review system
- [ ] Mobile PWA support

*See our [complete roadmap](docs/ROADMAP.md) for detailed planning.*

## 🤝 Contributing

We welcome contributions! Whether you're fixing bugs, adding features, or improving documentation, your help makes this component better for everyone.

**Quick Start:**
1. 🍴 Fork the repository
2. 🌿 Create a feature branch (`git checkout -b feature/amazing-feature`)
3. 💾 Commit your changes (`git commit -m 'Add amazing feature'`)
4. 📤 Push to the branch (`git push origin feature/amazing-feature`)
5. 🎯 Open a Pull Request

See our [Contributing Guide](CONTRIBUTING.md) for detailed guidelines.

## 🆘 Support & Community

<table>
<tr>
<td align="center">
<strong>📖 Documentation</strong><br>
<a href="https://github.com/jessa256/budibase-vendor-grid/wiki">Wiki & Guides</a>
</td>
<td align="center">
<strong>🐛 Issues</strong><br>
<a href="https://github.com/jessa256/budibase-vendor-grid/issues">Bug Reports & Features</a>
</td>
<td align="center">
<strong>💬 Discussions</strong><br>
<a href="https://github.com/jessa256/budibase-vendor-grid/discussions">Community Help</a>
</td>
<td align="center">
<strong>📧 Contact</strong><br>
<a href="mailto:jessica@example.com">Direct Support</a>
</td>
</tr>
</table>

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Budibase Team** for the excellent platform and component system
- **Supabase** for providing robust backend infrastructure  
- **Svelte** for the reactive component framework
- **Wedding Planning Community** for feature inspiration and feedback

---

<div align="center">

**Built with ❤️ by [Jessica Clark](https://github.com/jessa256)**

*Making wedding planning easier, one component at a time.*

⭐ **Star this repo** if it helped you plan the perfect wedding! ⭐

</div>