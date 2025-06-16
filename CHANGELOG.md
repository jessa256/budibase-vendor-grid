# Changelog

All notable changes to the Wedding Vendor Grid Component will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2025-06-16

### 🎉 Major Simplification Release

#### ✨ Added
- **Dynamic Card Layouts**: Cards now automatically adjust height based on content
- **Responsive Grid System**: Auto-fit grid with minimum card width constraints
- **Enhanced Mobile Support**: Better responsive behavior on all screen sizes
- **Improved Text Readability**: Fixed text visibility issues across different viewport sizes
- **Flexible Component Structure**: Cards adapt to content length with proper whitespace

#### 🔄 Changed
- **Simplified Component Structure**: Streamlined from complex multi-section layout to clean, minimal design
- **Removed Manual Column Configuration**: Grid now automatically adapts to screen size
- **Simplified Schema**: Removed unnecessary configuration options for cleaner setup
- **Updated Card Heights**: More reasonable height constraints for better content display
- **Removed Add Vendor Button**: Focused on pure vendor display functionality
- **Data Field Alignment**: Updated to use `is_booked` field consistently with database schema

#### 🚀 Improved
- **Performance**: Reduced component complexity for faster rendering
- **User Experience**: Cards display vendor information more clearly and consistently
- **Accessibility**: Maintained full keyboard navigation and screen reader support
- **Cross-Browser Compatibility**: Better support across different browsers and devices

#### 🧹 Removed
- **Add Vendor Functionality**: Simplified to focus on vendor display only
- **Complex Grid Configuration**: Removed manual column settings
- **Detailed Contact Information**: Streamlined to show only essential vendor info
- **Profile Picture Toggle**: Always shows images when available
- **Multiple Card Style Variants**: Simplified styling options

### 🛠️ Technical Improvements
- **CSS Grid Optimization**: Better use of modern CSS Grid features
- **Flexbox Integration**: Improved content flow within cards
- **Responsive Breakpoints**: More intuitive mobile-first responsive design
- **Bundle Size**: Reduced overall component size through simplification

---

## [1.0.0] - 2025-06-15

### 🎉 Initial Release

#### ✨ Added

##### 🎯 Core Features
- **Responsive Grid Layout**: Configurable 1-6 column grid with automatic responsive breakpoints
- **Supabase Integration**: Direct REST API connectivity with `vendorInformation` table
- **Profile Picture Support**: Image display with fallback placeholder handling
- **Vendor Information Cards**: Display of vendor name, contact person, type, and pricing
- **Booking Status Indicators**: Visual green badges for booked vendors
- **Add Vendor Functionality**: Dedicated "Add New Vendor" card with customizable text
- **Multiple Card Styles**: Elevated, Outlined, and Flat card design variants
- **Size Options**: Small (180px), Medium (220px), Large (280px) card heights
- **Event System**: Vendor click and add vendor event dispatching

##### ♿ Accessibility Features
- **Full ARIA Support**: Comprehensive accessibility labels and descriptions
- **Keyboard Navigation**: Tab-accessible with Enter key activation
- **Screen Reader Compatibility**: Semantic HTML structure
- **Focus Management**: Visible focus indicators
- **High Contrast Mode**: Automatic adaptation for accessibility needs

##### 📱 Mobile & Responsive
- **Touch-Friendly Interface**: Optimized for mobile wedding planning
- **Responsive Breakpoints**: Automatic layout adjustments
- **Mobile-First Design**: Progressive enhancement approach

##### 🛠️ Technical Features
- **Svelte 3.x Framework**: Modern reactive component architecture
- **Budibase SDK Integration**: Native data provider and styling system support
- **CSS Grid Layout**: Modern layout system with custom property theming
- **Rollup Build System**: Optimized bundle with Terser minification
- **Performance Optimization**: Efficient rendering with minimal re-computations

##### 🎨 Customization Options
- **Configurable Columns**: 1-6 column grid layouts
- **Card Height Options**: Small, Medium, Large size variants
- **Style Variants**: Elevated, Outlined, Flat card designs
- **Profile Picture Toggle**: Show/hide vendor profile images
- **Custom Default Images**: Configurable fallback image URLs
- **Add Button Customization**: Configurable button text and visibility
- **Theme Integration**: CSS custom properties for easy theming

##### 🔧 Developer Experience
- **Hot Reload Development**: Live component updates during development
- **Comprehensive Documentation**: Detailed setup and configuration guides
- **Example Configurations**: Ready-to-use component configurations
- **TypeScript Definitions**: Full type support for enhanced development

---

## Migration Notes

### From v1.0.0 to v1.1.0

**Breaking Changes:**
- Removed `columns` prop - grid now auto-adapts
- Removed `showAddButton` and `addButtonText` props
- Removed `showProfilePics` prop - always shows images when available
- Removed `onAddVendor` event handler

**Migration Steps:**
1. Remove any `columns` configuration from your Budibase component settings
2. Remove add vendor button related code/handlers
3. Update to use `is_booked` field in your data if using custom queries
4. Test responsive behavior on different screen sizes

**Benefits of Upgrading:**
- Much cleaner, more professional appearance
- Better mobile experience
- Easier configuration and maintenance
- More reliable cross-browser compatibility