# FHIR ePI Viewer

A dynamic viewer for HL7 FHIR electronic Product Information (ePI) documents, specifically designed for Package Leaflets (Folhetos Informativos).

![FHIR](https://img.shields.io/badge/HL7-FHIR-red)
![License](https://img.shields.io/badge/license-MIT-blue)

## 🚀 Features

- **Drag & Drop** - Load any FHIR Bundle JSON file
- **URL Loading** - Fetch documents from remote URLs
- **Smart Formatting** - Automatic detection and styling of content types
- **Responsive Design** - Works on desktop and mobile
- **Multi-language** - Supports Portuguese and English content detection

## 📁 Files

| File | Description |
|------|-------------|
| `viewer.html` | Dynamic FHIR ePI viewer (main file) |
| `index.html` | Static example with Karvea 300mg |
| `Bundle-bundlepackageleaflet-pt-*.json` | Sample FHIR Bundle document |

## 🎯 Usage

### Basic Usage

1. Open `viewer.html` in your browser
2. Either:
   - **Drag & drop** a FHIR Bundle JSON file
   - **Click** to select a file from your computer
   - **Paste a URL** and click "Carregar"
   - Click **"Carregar ficheiro de exemplo"** to test with the sample

### URL Parameters

You can link directly to a document:

```text
viewer.html?url=https://example.com/fhir-bundle.json
viewer.html?file=local-file.json
```

---

## 🛠️ Customization Guide

The viewer automatically detects and styles content based on keywords. Here's how to add new categories or modify existing ones:

### 1. Section Icons (Main Collapsible Sections)

Located in the `<script>` section, around line 100-130:

```javascript
// Section icons mapping based on section numbers
const sectionIcons = {
    '1': { icon: 'fa-capsules', bgColor: 'bg-blue-50', textColor: 'text-blue-600' },
    '2': { icon: 'fa-triangle-exclamation', bgColor: 'bg-amber-50', textColor: 'text-amber-600' },
    '3': { icon: 'fa-pills', bgColor: 'bg-blue-50', textColor: 'text-blue-600' },
    '4': { icon: 'fa-heart-pulse', bgColor: 'bg-rose-50', textColor: 'text-rose-600' },
    '5': { icon: 'fa-temperature-low', bgColor: 'bg-cyan-50', textColor: 'text-cyan-600' },
    '6': { icon: 'fa-box', bgColor: 'bg-purple-50', textColor: 'text-purple-600' },
    // Add new section numbers here
};

// Icon keywords mapping - matches title text to icons
const iconKeywords = {
    'composição': 'fa-flask',
    'efeitos': 'fa-heart-pulse',
    'conservar': 'fa-temperature-low',
    'tomar': 'fa-pills',
    'precauções': 'fa-triangle-exclamation',
    // Add new keyword → icon mappings here
};
```

### 2. Smart Content Detection (Styled Boxes)

Located in the `applySmartFormatting()` function. Add new detection patterns:

```javascript
// Warning boxes (red)
else if (strongText.includes('não tome') || strongText.includes('do not take')) {
    wrapInWarningBox(p, 'fa-ban', strongEl.textContent);
}

// Info boxes (various colors)
else if (strongText.includes('gravidez') || strongText.includes('pregnancy')) {
    wrapInInfoBox(p, 'fa-person-pregnant', strongEl.textContent, 'pink');
}

// Dosage boxes (blue/purple)
else if (strongText.includes('doentes com') || strongText.includes('patients with')) {
    wrapInDosageBox(p, 'fa-user', strongEl.textContent);
}

// Subsection labels (gray badge)
else if (strongText.includes('advertências') || strongText.includes('warnings')) {
    wrapInSubsectionLabel(p, strongEl.textContent);
}
```

#### Adding New Detection Patterns

Example - Adding "Fertility" detection:

```javascript
else if (strongText.includes('fertilidade') || strongText.includes('fertility')) {
    wrapInInfoBox(p, 'fa-heart', strongEl.textContent, 'pink');
}
```

Example - Adding "Elderly patients" detection:

```javascript
else if (strongText.includes('idosos') || strongText.includes('elderly')) {
    wrapInInfoBox(p, 'fa-person-cane', strongEl.textContent, 'purple');
}
```

### 3. Color Schemes

In the `wrapInInfoBox()` function, available colors:

```javascript
const colorClasses = {
    blue:    { bg: 'bg-blue-50',    border: 'border-blue-200',    title: 'text-blue-800',    content: 'text-blue-700' },
    purple:  { bg: 'bg-purple-50',  border: 'border-purple-200',  title: 'text-purple-800',  content: 'text-purple-700' },
    pink:    { bg: 'bg-pink-50',    border: 'border-pink-200',    title: 'text-pink-800',    content: 'text-pink-700' },
    emerald: { bg: 'bg-emerald-50', border: 'border-emerald-200', title: 'text-emerald-800', content: 'text-emerald-700' },
    amber:   { bg: 'bg-amber-50',   border: 'border-amber-200',   title: 'text-amber-800',   content: 'text-amber-700' },
    cyan:    { bg: 'bg-cyan-50',    border: 'border-cyan-200',    title: 'text-cyan-800',    content: 'text-cyan-700' },
};
```

To add a new color (e.g., `indigo`):

```javascript
indigo: { bg: 'bg-indigo-50', border: 'border-indigo-200', title: 'text-indigo-800', content: 'text-indigo-700' },
```

### 4. Side Effects Frequencies

In the frequency detection section, add new frequency levels:

```javascript
// Detection
else if (liText.startsWith('muito raros') || liText.startsWith('very rare')) {
    styleFrequencyItem(li, 'very-rare', 'Muito raros');
}
```

Add corresponding CSS in the `<style>` section:

```css
.frequency-item.very-rare {
    background: linear-gradient(to right, #e0e7ff, #eef2ff);
}
.frequency-dot.very-rare { background-color: #6366f1; }
.frequency-label.very-rare { color: #4338ca; }
```

### 5. Storage Section Items

In the `formatStorageSection()` function, add new storage conditions:

```javascript
else if (text.includes('luz') || text.includes('light')) {
    storageItems.push({
        icon: 'fa-sun',
        title: 'Luz',
        content: p.textContent,
        bgColor: 'bg-yellow-50',
        borderColor: 'border-yellow-200',
        iconBg: 'bg-yellow-100',
        iconColor: 'text-yellow-600',
        titleColor: 'text-yellow-800',
        textColor: 'text-yellow-700'
    });
    p.remove();
}
```

---

## 📋 Available Icons

The viewer uses [Font Awesome 6](https://fontawesome.com/icons). Common icons:

| Category | Icons |
|----------|-------|
| **Medication** | `fa-pills`, `fa-capsules`, `fa-syringe`, `fa-vial`, `fa-prescription-bottle` |
| **Body/Health** | `fa-heart`, `fa-heart-pulse`, `fa-lungs`, `fa-brain` |
| **People** | `fa-person-pregnant`, `fa-baby`, `fa-child`, `fa-person-cane`, `fa-user` |
| **Warnings** | `fa-triangle-exclamation`, `fa-circle-exclamation`, `fa-ban` |
| **Storage** | `fa-temperature-high`, `fa-snowflake`, `fa-sun`, `fa-calendar-xmark` |
| **Other** | `fa-car`, `fa-utensils`, `fa-clock`, `fa-leaf`, `fa-comment-medical` |

## 🎨 Available Tailwind Colors

`blue`, `purple`, `pink`, `emerald`, `amber`, `cyan`, `red`, `orange`, `yellow`, `indigo`, `rose`, `slate`, `green`, `teal`, `sky`, `violet`, `fuchsia`

---

## 📄 FHIR Document Structure

The viewer expects a FHIR Bundle with:

```text
Bundle
└── entry[]
    └── Composition (required)
        ├── title
        ├── section[]
        │   ├── title
        │   ├── text.div (XHTML content)
        │   └── section[] (nested sections)
        └── ...
    └── Organization (optional - for metadata)
    └── MedicinalProductDefinition (optional - for metadata)
```

---

## 🤝 Contributing

Found a new pattern that should be styled? Open an issue or PR with:

1. The text pattern to detect (Portuguese and/or English)
2. Suggested icon and color
3. Example from a real ePI document

---

## 📜 License

MIT License - See [LICENSE](LICENSE) file
