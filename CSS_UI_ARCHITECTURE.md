# POS System CSS & UI Architecture Documentation

## Table of Contents
1. [CSS Architecture Overview](#css-architecture-overview)
2. [Styling Framework Stack](#styling-framework-stack)
3. [Theme System](#theme-system)
4. [Component Library](#component-library)
5. [Layout Systems](#layout-systems)
6. [Responsive Design](#responsive-design)
7. [CSS Organization](#css-organization)
8. [UI Components](#ui-components)
9. [Animation & Transitions](#animation--transitions)
10. [Performance Optimization](#performance-optimization)

---

## CSS Architecture Overview

The POS system uses a **hybrid CSS architecture** combining multiple frameworks and custom styles for maximum flexibility and maintainability.

### CSS File Structure
```
public/css/
├── app.css              # Main application styles
├── vendor.css           # Third-party library styles
├── init.css             # Initialization styles
├── rtl.css              # Right-to-left language support
└── tailwind/            # Tailwind CSS utilities
    ├── components.css   # Custom Tailwind components
    └── utilities.css    # Custom utility classes
```

### Architecture Principles
- 📦 **Component-based**: Modular, reusable components
- 🎨 **Theme-driven**: Consistent color schemes and branding
- 📱 **Mobile-first**: Responsive design approach
- 🌐 **Multi-language**: RTL and LTR language support
- ⚡ **Performance**: Optimized loading and rendering
- 🔧 **Maintainable**: Well-organized, documented code

---

## Styling Framework Stack

### 1. **Primary Frameworks**

#### **AdminLTE 3** (Base Admin Theme)
- 🎯 **Purpose**: Admin dashboard foundation
- 📊 **Components**: Sidebar, navigation, cards, forms
- 🎨 **Themes**: Multiple color schemes
- 📱 **Responsive**: Mobile-optimized admin interface

#### **Bootstrap 4** (Grid & Components)
- 🏗️ **Grid System**: 12-column responsive grid
- 🧩 **Components**: Buttons, forms, modals, alerts
- 📱 **Utilities**: Spacing, typography, colors
- 🔧 **Customization**: SCSS variables for theming

#### **Tailwind CSS** (Utility Classes)
- ⚡ **Utility-first**: Rapid UI development
- 🎨 **Custom Design**: Flexible styling approach
- 📱 **Responsive**: Built-in responsive utilities
- 🔧 **Configuration**: Customizable design system

### 2. **Specialized Libraries**

#### **Font Awesome** (Icons)
```css
/* Icon usage throughout the application */
.fa-dashboard, .fa-products, .fa-sales, .fa-reports
```

#### **DataTables** (Table Styling)
```css
/* Enhanced table components */
.dataTable, .dataTables_wrapper, .dataTables_info
```

#### **Select2** (Enhanced Dropdowns)
```css
/* Improved select components */
.select2-container, .select2-dropdown
```

#### **SweetAlert2** (Modal Dialogs)
```css
/* Beautiful alert modals */
.swal2-popup, .swal2-title, .swal2-content
```

---

## Theme System

### 1. **Dynamic Theme Loading**
```php
// Theme detection in Blade templates
<body class="hold-transition skin-@if(!empty(session('business.theme_color')))
    {{ session('business.theme_color') }}
@else
    blue-light
@endif sidebar-mini">
```

### 2. **Available Theme Colors**
```css
/* AdminLTE Skin Variations */
.skin-blue          /* Default blue theme */
.skin-blue-light    /* Light blue variant */
.skin-yellow        /* Yellow theme */
.skin-yellow-light  /* Light yellow variant */
.skin-green         /* Green theme */
.skin-green-light   /* Light green variant */
.skin-purple        /* Purple theme */
.skin-purple-light  /* Light purple variant */
.skin-red           /* Red theme */
.skin-red-light     /* Light red variant */
.skin-black         /* Black theme */
.skin-black-light   /* Light black variant */
```

### 3. **Custom Business Branding**
```css
/* Dynamic branding variables */
:root {
    --primary-color: {{ session('business.primary_color', '#3c8dbc') }};
    --secondary-color: {{ session('business.secondary_color', '#f4f4f4') }};
    --accent-color: {{ session('business.accent_color', '#00a65a') }};
    --text-color: {{ session('business.text_color', '#333') }};
}
```

---

## Component Library

### 1. **Form Components**

#### **Input Fields**
```css
/* Base input styling */
.form-control {
    border: 1px solid #d2d6de;
    border-radius: 3px;
    box-shadow: none;
    transition: border-color 0.15s ease-in-out;
}

.form-control:focus {
    border-color: #3c8dbc;
    box-shadow: 0 0 0 0.2rem rgba(60, 141, 188, 0.25);
}

/* Error state */
.form-control.is-invalid {
    border-color: #dc3545;
}

/* Success state */
.form-control.is-valid {
    border-color: #28a745;
}
```

#### **Button Styles**
```css
/* Primary buttons */
.btn-primary {
    background-color: #3c8dbc;
    border-color: #367fa9;
    color: #fff;
}

.btn-primary:hover {
    background-color: #367fa9;
    border-color: #204d74;
}

/* Custom button sizes */
.btn-big {
    padding: 10px 30px;
    font-size: 18px;
    line-height: 1.3333333;
}

/* Icon buttons */
.btn-icon {
    padding: 8px 12px;
    font-size: 14px;
}
```

#### **Form Groups**
```css
.form-group {
    margin-bottom: 15px;
}

.form-group label {
    font-weight: 600;
    margin-bottom: 5px;
    color: #555;
}

.required::after {
    content: " *";
    color: #dd4b39;
}
```

### 2. **Card Components**

#### **Info Boxes**
```css
.info-box {
    display: block;
    min-height: 90px;
    background: #fff;
    width: 100%;
    box-shadow: 0 1px 1px rgba(0,0,0,0.1);
    border-radius: 2px;
    margin-bottom: 15px;
}

.info-box-icon {
    border-top-left-radius: 2px;
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 2px;
    display: block;
    float: left;
    height: 90px;
    width: 90px;
    text-align: center;
    font-size: 45px;
    line-height: 90px;
    background: rgba(0,0,0,0.2);
}

.info-box-content {
    padding: 5px 10px;
    margin-left: 90px;
}
```

#### **Small Boxes (Dashboard Cards)**
```css
.small-box {
    border-radius: 2px;
    position: relative;
    display: block;
    margin-bottom: 20px;
    box-shadow: 0 1px 1px rgba(0,0,0,0.1);
}

.small-box > .inner {
    padding: 15px;
}

.small-box h3 {
    font-size: 38px;
    font-weight: bold;
    margin: 0 0 10px 0;
    white-space: nowrap;
    padding: 0;
}

.small-box p {
    font-size: 15px;
}

.small-box .icon {
    -webkit-transition: all .3s linear;
    -o-transition: all .3s linear;
    transition: all .3s linear;
    position: absolute;
    top: -10px;
    right: 10px;
    z-index: 0;
    font-size: 90px;
    color: rgba(0,0,0,0.15);
}
```

### 3. **Table Components**

#### **DataTables Styling**
```css
.table-responsive {
    border: 1px solid #f4f4f4;
    border-radius: 3px;
}

.dataTable {
    margin-top: 0 !important;
    margin-bottom: 0 !important;
}

.dataTable thead {
    background-color: #f9f9f9;
}

.dataTable thead th {
    border-bottom: 1px solid #f4f4f4;
    padding: 10px;
    font-weight: 600;
}

.dataTable tbody td {
    padding: 8px 10px;
    border-top: 1px solid #f4f4f4;
}

.dataTable tbody tr:hover {
    background-color: #f5f5f5;
}
```

#### **Action Buttons in Tables**
```css
.table-actions {
    white-space: nowrap;
}

.table-actions .btn {
    margin-right: 5px;
    padding: 4px 8px;
    font-size: 12px;
}

.table-actions .btn:last-child {
    margin-right: 0;
}
```

### 4. **Navigation Components**

#### **Sidebar Navigation**
```css
.main-sidebar {
    position: fixed;
    top: 0;
    left: 0;
    padding-top: 50px;
    min-height: 100vh;
    width: 230px;
    z-index: 810;
    transition: transform 0.3s ease-in-out;
}

.sidebar-menu > li > a {
    position: relative;
    display: block;
    padding: 12px 5px 12px 15px;
    color: #b8c7ce;
    text-decoration: none;
}

.sidebar-menu > li.active > a {
    color: #fff;
    background: rgba(255,255,255,0.1);
    border-left: 3px solid #3c8dbc;
}

.sidebar-menu .treeview-menu {
    display: none;
    list-style: none;
    padding: 0;
    margin: 0;
}

.sidebar-menu .treeview.active .treeview-menu {
    display: block;
}
```

#### **Top Navigation**
```css
.main-header {
    position: relative;
    max-height: 100px;
    z-index: 1030;
}

.main-header .navbar {
    transition: margin-left 0.3s ease-in-out;
    margin-bottom: 0;
    margin-left: 230px;
    border: none;
    min-height: 50px;
    border-radius: 0;
}

.navbar-custom-menu > .nav {
    margin: 0;
    float: right;
}

.navbar-custom-menu > .nav > li > .dropdown-menu {
    position: absolute;
    right: 0;
    left: auto;
}
```

---

## Layout Systems

### 1. **Main Layout Grid**
```css
/* Application wrapper */
.wrapper {
    min-height: 100%;
    position: relative;
    overflow: hidden;
}

/* Content wrapper */
.content-wrapper {
    min-height: calc(100vh - 50px);
    z-index: 800;
    margin-left: 230px;
    transition: margin-left 0.3s ease-in-out;
}

/* Main content area */
.content {
    min-height: 250px;
    padding: 15px;
    margin-right: auto;
    margin-left: auto;
}

/* Content header */
.content-header {
    padding: 15px;
    margin-bottom: 20px;
}

.content-header h1 {
    margin: 0;
    font-size: 24px;
}
```

### 2. **POS Layout**
```css
/* POS specific layout */
.pos-container {
    display: flex;
    height: 100vh;
    overflow: hidden;
}

.pos-left-panel {
    flex: 2;
    padding: 15px;
    overflow-y: auto;
    background-color: #f4f4f4;
}

.pos-right-panel {
    flex: 1;
    padding: 15px;
    background-color: #fff;
    border-left: 1px solid #ddd;
    overflow-y: auto;
}

/* Product grid for POS */
.pos-product-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 15px;
    margin-top: 20px;
}

.pos-product-card {
    background: #fff;
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 15px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s ease;
}

.pos-product-card:hover {
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    transform: translateY(-2px);
}
```

### 3. **Responsive Breakpoints**
```css
/* Mobile First Approach */
@media (max-width: 767px) {
    .main-header .navbar-custom-menu {
        float: none !important;
        display: block !important;
    }
    
    .content-wrapper {
        margin-left: 0;
    }
    
    .main-sidebar {
        transform: translateX(-230px);
    }
    
    .sidebar-open .main-sidebar {
        transform: translateX(0);
    }
}

@media (min-width: 768px) and (max-width: 991px) {
    .pos-product-grid {
        grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
    }
}

@media (min-width: 992px) {
    .pos-product-grid {
        grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    }
}
```

---

## Responsive Design

### 1. **Mobile-First Grid System**
```css
/* Tailwind CSS Grid Classes */
.tw-grid {
    display: grid;
}

.tw-grid-cols-1 {
    grid-template-columns: repeat(1, minmax(0, 1fr));
}

@media (min-width: 768px) {
    .md\:tw-grid-cols-2 {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
}

@media (min-width: 1024px) {
    .lg\:tw-grid-cols-4 {
        grid-template-columns: repeat(4, minmax(0, 1fr));
    }
}
```

### 2. **Responsive Tables**
```css
@media (max-width: 767px) {
    .table-responsive {
        border: 0;
    }
    
    .table-responsive .table {
        margin-bottom: 0;
    }
    
    .table-responsive .table td,
    .table-responsive .table th {
        white-space: nowrap;
    }
    
    /* Stack table on mobile */
    .mobile-stack tbody,
    .mobile-stack tr,
    .mobile-stack td {
        display: block;
        width: 100%;
    }
    
    .mobile-stack tr {
        border: 1px solid #ccc;
        margin-bottom: 10px;
    }
    
    .mobile-stack td {
        border: none;
        padding: 10px;
        text-align: left;
    }
    
    .mobile-stack td:before {
        content: attr(data-label) ": ";
        font-weight: bold;
        width: 120px;
        display: inline-block;
    }
}
```

### 3. **Mobile Navigation**
```css
/* Mobile hamburger menu */
.mobile-menu-toggle {
    display: none;
    background: none;
    border: none;
    font-size: 18px;
    padding: 10px;
    color: #333;
}

@media (max-width: 767px) {
    .mobile-menu-toggle {
        display: block;
    }
    
    .desktop-menu {
        display: none;
    }
    
    .mobile-menu {
        position: fixed;
        top: 50px;
        left: 0;
        width: 100%;
        height: calc(100vh - 50px);
        background: #222d32;
        transform: translateX(-100%);
        transition: transform 0.3s ease;
        z-index: 1000;
    }
    
    .mobile-menu.active {
        transform: translateX(0);
    }
}
```

---

## CSS Organization

### 1. **File Structure & Imports**
```css
/* app.css - Main stylesheet */
@import 'vendor/bootstrap.css';
@import 'vendor/adminlte.css';
@import 'vendor/font-awesome.css';
@import 'components/forms.css';
@import 'components/tables.css';
@import 'components/modals.css';
@import 'layouts/header.css';
@import 'layouts/sidebar.css';
@import 'layouts/content.css';
@import 'pages/dashboard.css';
@import 'pages/pos.css';
@import 'utilities/helpers.css';
```

### 2. **CSS Custom Properties**
```css
:root {
    /* Color System */
    --primary-50: #eff6ff;
    --primary-100: #dbeafe;
    --primary-200: #bfdbfe;
    --primary-300: #93c5fd;
    --primary-400: #60a5fa;
    --primary-500: #3b82f6;
    --primary-600: #2563eb;
    --primary-700: #1d4ed8;
    --primary-800: #1e40af;
    --primary-900: #1e3a8a;
    
    /* Spacing System */
    --spacing-xs: 0.25rem;
    --spacing-sm: 0.5rem;
    --spacing-md: 1rem;
    --spacing-lg: 1.5rem;
    --spacing-xl: 3rem;
    
    /* Typography */
    --font-family-base: 'Source Sans Pro', sans-serif;
    --font-size-sm: 0.875rem;
    --font-size-base: 1rem;
    --font-size-lg: 1.125rem;
    --font-weight-normal: 400;
    --font-weight-bold: 600;
    
    /* Border Radius */
    --border-radius-sm: 0.125rem;
    --border-radius-base: 0.25rem;
    --border-radius-lg: 0.5rem;
    
    /* Shadows */
    --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
    --shadow-base: 0 1px 3px 0 rgba(0, 0, 0, 0.1);
    --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
}
```

### 3. **Utility Classes**
```css
/* Spacing Utilities */
.pt-0 { padding-top: 0 !important; }
.pt-1 { padding-top: var(--spacing-xs) !important; }
.pt-2 { padding-top: var(--spacing-sm) !important; }
.pt-3 { padding-top: var(--spacing-md) !important; }

.mb-0 { margin-bottom: 0 !important; }
.mb-10 { margin-bottom: 10px !important; }
.mb-20 { margin-bottom: 20px !important; }

/* Display Utilities */
.d-none { display: none !important; }
.d-block { display: block !important; }
.d-inline-block { display: inline-block !important; }
.d-flex { display: flex !important; }

/* Flexbox Utilities */
.flex-row { flex-direction: row !important; }
.flex-column { flex-direction: column !important; }
.justify-content-start { justify-content: flex-start !important; }
.justify-content-center { justify-content: center !important; }
.justify-content-between { justify-content: space-between !important; }

/* Text Utilities */
.text-left { text-align: left !important; }
.text-center { text-align: center !important; }
.text-right { text-align: right !important; }
.text-uppercase { text-transform: uppercase !important; }
.font-weight-bold { font-weight: var(--font-weight-bold) !important; }

/* Color Utilities */
.text-primary { color: var(--primary-600) !important; }
.text-success { color: #28a745 !important; }
.text-danger { color: #dc3545 !important; }
.text-warning { color: #ffc107 !important; }
.text-info { color: #17a2b8 !important; }

/* Background Utilities */
.bg-primary { background-color: var(--primary-600) !important; }
.bg-light { background-color: #f8f9fa !important; }
.bg-white { background-color: #fff !important; }
```

---

## UI Components

### 1. **Modal Dialogs**
```css
.modal {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1050;
    width: 100%;
    height: 100%;
    overflow: hidden;
    outline: 0;
}

.modal-dialog {
    position: relative;
    width: auto;
    margin: 10px;
    pointer-events: none;
}

.modal-content {
    position: relative;
    display: flex;
    flex-direction: column;
    width: 100%;
    pointer-events: auto;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid rgba(0,0,0,.2);
    border-radius: 6px;
    box-shadow: 0 3px 9px rgba(0,0,0,.5);
    outline: 0;
}

.modal-header {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    padding: 15px;
    border-bottom: 1px solid #e5e5e5;
}

.modal-title {
    margin: 0;
    line-height: 1.42857143;
}

.modal-body {
    position: relative;
    padding: 15px;
}

.modal-footer {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    padding: 15px;
    border-top: 1px solid #e5e5e5;
}
```

### 2. **Alert Components**
```css
.alert {
    padding: 15px;
    margin-bottom: 20px;
    border: 1px solid transparent;
    border-radius: 4px;
}

.alert-success {
    color: #3c763d;
    background-color: #dff0d8;
    border-color: #d6e9c6;
}

.alert-info {
    color: #31708f;
    background-color: #d9edf7;
    border-color: #bce8f1;
}

.alert-warning {
    color: #8a6d3b;
    background-color: #fcf8e3;
    border-color: #faebcc;
}

.alert-danger {
    color: #a94442;
    background-color: #f2dede;
    border-color: #ebccd1;
}

.alert-dismissible {
    padding-right: 35px;
}

.alert-dismissible .close {
    position: relative;
    top: -2px;
    right: -21px;
    color: inherit;
}
```

### 3. **Loading States**
```css
/* Loading spinner */
.loading-spinner {
    border: 3px solid #f3f3f3;
    border-top: 3px solid #3498db;
    border-radius: 50%;
    width: 30px;
    height: 30px;
    animation: spin 1s linear infinite;
    margin: 20px auto;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

/* Loading overlay */
.loading-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(255,255,255,0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
}

/* Skeleton loading */
.skeleton {
    background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
    background-size: 200% 100%;
    animation: loading 1.5s infinite;
}

@keyframes loading {
    0% { background-position: 200% 0; }
    100% { background-position: -200% 0; }
}
```

### 4. **Badge & Label Components**
```css
.badge {
    display: inline-block;
    min-width: 10px;
    padding: 3px 7px;
    font-size: 12px;
    font-weight: bold;
    line-height: 1;
    color: #fff;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    background-color: #777;
    border-radius: 10px;
}

.badge-primary { background-color: #337ab7; }
.badge-success { background-color: #5cb85c; }
.badge-info { background-color: #5bc0de; }
.badge-warning { background-color: #f0ad4e; }
.badge-danger { background-color: #d9534f; }

/* Status badges */
.status-paid { background-color: #00a65a; }
.status-pending { background-color: #f39c12; }
.status-overdue { background-color: #dd4b39; }
.status-draft { background-color: #3c8dbc; }

/* Discount badge */
.discount-badge {
    position: absolute;
    top: 6px;
    right: 10px;
    font-size: 18px;
    padding: 7px;
}

.discount-badge-small {
    position: absolute;
    top: 2px;
    right: 5px;
    font-size: 12px;
    padding: 2px 5px;
}
```

---

## Animation & Transitions

### 1. **Smooth Transitions**
```css
/* Global transition settings */
* {
    transition: all 0.3s ease-in-out;
}

/* Sidebar transitions */
.main-sidebar {
    transition: transform 0.3s ease-in-out;
}

.content-wrapper {
    transition: margin-left 0.3s ease-in-out;
}

/* Button hover effects */
.btn {
    transition: all 0.2s ease-in-out;
}

.btn:hover {
    transform: translateY(-1px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

/* Card hover effects */
.card, .box {
    transition: box-shadow 0.3s ease;
}

.card:hover, .box:hover {
    box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}
```

### 2. **Loading Animations**
```css
/* Fade in animation */
@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}

.fade-in {
    animation: fadeIn 0.5s ease-in;
}

/* Slide animations */
@keyframes slideInRight {
    from { transform: translateX(100%); }
    to { transform: translateX(0); }
}

@keyframes slideInLeft {
    from { transform: translateX(-100%); }
    to { transform: translateX(0); }
}

.slide-in-right {
    animation: slideInRight 0.3s ease-out;
}

.slide-in-left {
    animation: slideInLeft 0.3s ease-out;
}

/* Bounce effect */
@keyframes bounce {
    0%, 20%, 53%, 80%, 100% {
        transform: translate3d(0,0,0);
    }
    40%, 43% {
        transform: translate3d(0, -30px, 0);
    }
    70% {
        transform: translate3d(0, -15px, 0);
    }
    90% {
        transform: translate3d(0, -4px, 0);
    }
}

.bounce {
    animation: bounce 1s ease;
}
```

### 3. **Micro-interactions**
```css
/* Button press effect */
.btn-press:active {
    transform: scale(0.98);
}

/* Ripple effect */
.ripple {
    position: relative;
    overflow: hidden;
}

.ripple:before {
    content: "";
    position: absolute;
    top: 50%;
    left: 50%;
    width: 0;
    height: 0;
    border-radius: 50%;
    background: rgba(255,255,255,0.5);
    transform: translate(-50%, -50%);
    transition: width 0.6s, height 0.6s;
}

.ripple:active:before {
    width: 300px;
    height: 300px;
}
```

---

## Performance Optimization

### 1. **CSS Optimization Strategies**
```css
/* Use efficient selectors */
.menu-item { /* Good - class selector */ }
#header .nav li a { /* Avoid - overly specific */ }

/* Minimize repaints and reflows */
.animated-element {
    will-change: transform;
    transform: translateZ(0); /* Force hardware acceleration */
}

/* Use flexbox for layouts */
.flex-container {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}

/* Optimize images */
.optimized-image {
    max-width: 100%;
    height: auto;
    object-fit: cover;
}
```

### 2. **Critical CSS Inlining**
```html
<!-- Inline critical CSS for above-the-fold content -->
<style>
    .header, .sidebar, .main-content {
        /* Critical styles here */
    }
</style>

<!-- Load non-critical CSS asynchronously -->
<link rel="preload" href="app.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

### 3. **CSS Purging for Production**
```javascript
// Tailwind CSS purge configuration
module.exports = {
    purge: [
        './resources/**/*.blade.php',
        './resources/**/*.js',
        './resources/**/*.vue',
    ],
    theme: {
        extend: {}
    },
    variants: {},
    plugins: []
}
```

This comprehensive CSS & UI architecture documentation provides a complete understanding of the styling system, component library, and design patterns used throughout the POS system, ensuring maintainable and scalable frontend development.
