# Design Guidelines: Multilingual Government Form Application

## Design Approach
**System Selected:** Material Design with government form adaptations
**Rationale:** Utility-focused government application requiring accessibility, multilingual support, trust-building elements, and form-heavy interactions. Material Design provides robust form patterns, clear hierarchy, and proven accessibility standards essential for civic applications.

## Core Design Principles
1. **Official & Trustworthy:** Clean, professional aesthetic that conveys government authority
2. **Clarity First:** Every label, instruction, and error message must be crystal clear in all languages
3. **Progressive Disclosure:** Show complexity only when needed, guide users step-by-step
4. **Accessibility Mandate:** WCAG 2.1 AA compliance minimum for all form elements

## Typography System

**Font Family:** 
- Primary: Noto Sans (excellent multilingual support for Devanagari scripts - Hindi/Marathi)
- Fallback: system-ui, -apple-system, sans-serif

**Hierarchy:**
- Page Titles: text-3xl font-semibold (forms, sections)
- Form Section Headers: text-xl font-medium 
- Field Labels: text-sm font-medium (consistent across all languages)
- Input Text: text-base font-normal
- Helper Text/Instructions: text-sm font-normal
- Error Messages: text-sm font-medium

## Layout System

**Spacing Primitives:** Tailwind units of 2, 4, 6, 8, 12
- Component gaps: gap-4 or gap-6
- Section padding: p-6 or p-8
- Form field spacing: space-y-6
- Card padding: p-6 or p-8

**Grid Structure:**
- Forms: Single column layout (max-w-4xl) for focus and accessibility
- Document preview: 2-column grid on desktop (md:grid-cols-2), stack on mobile
- Dashboard/Admin: Sidebar navigation (w-64) + main content area

## Component Library

### Navigation & Layout
- **Header:** Fixed top bar with language switcher (prominent flags/text), logo (government emblem), user menu
- **Language Switcher:** Dropdown with En | हिं | मरा - always visible, triggers full UI translation including button states
- **Sidebar (Admin):** Collapsible navigation with form categories, documents, submissions, profile

### Forms & Inputs
- **Form Container:** White card (shadow-md) with rounded-lg, clear section dividers
- **Text Fields:** Full-width inputs with floating labels (Material Design pattern), border on focus, error states with red accent
- **File Upload:** Drag-and-drop zone with camera icon, "Browse" button, file type/size limits clearly stated
- **Document Preview:** Thumbnail grid showing uploaded files with:
  - Image preview (120px x 120px thumbnails)
  - File name overlay
  - Remove/replace icons
  - Click to expand full preview modal
- **Signature Field:** Canvas drawing area (300px x 150px) with "Clear" and "Save" actions, show captured signature as image on form
- **Photo ID Display:** Show uploaded photo ID image directly on form in designated area (like actual government forms), not just filename

### Form Layout Patterns
- **Actual Government Form Rendering:** Mimic official form layouts with:
  - Photo box in top-right corner (passport-style placement)
  - Signature box at bottom
  - Fields aligned as per original form (2-column grid for related fields)
  - All labels in selected language
  - Pre-filled data from document parsing where applicable

### Buttons & Actions
- **Primary CTA:** Solid background, medium size (px-6 py-3), rounded-md
- **Secondary:** Outlined style, same padding
- **Language-Specific Text:** Ensure "Login" vs "Logout" translation keys are correctly mapped (fix: use `auth.login` and `auth.logout` translation keys, not same key)

### Feedback & Status
- **Progress Indicator:** Stepper component showing form completion stages (1. Documents → 2. Fill Form → 3. Review → 4. Submit)
- **Success/Error Messages:** Toast notifications (top-right) with icons, auto-dismiss
- **Loading States:** Skeleton screens for form loading, spinner for document processing
- **Document Processing Feedback:** Show upload progress (%), OCR extraction status ("Extracting data from Aadhaar...")

### Data Display
- **Document Gallery:** Grid layout with uploaded documents, each showing:
  - Document type badge
  - Thumbnail preview
  - Upload date
  - Verification status (if applicable via API Setu)
- **Form Preview:** Read-only view of completed form matching government format before submission

## Multilingual Implementation Strategy

**Critical Fix for Translation Bug:**
- Use separate translation keys: `navigation.login` and `navigation.logout` - never reuse same key
- Pre-login page should ONLY show `navigation.login` translation
- Post-login should ONLY show `navigation.logout` translation
- Test all button states in all three languages during implementation

**Language Support:**
- All UI text, labels, placeholders, errors, instructions in English, Hindi (Devanagari), Marathi (Devanagari)
- Form field labels: Bilingual display (English + selected Indian language) for clarity
- Error messages: Language-specific with clear actionable guidance
- Date formats: DD/MM/YYYY (Indian standard) regardless of language

## Images

**Hero Section:** Not applicable - this is a form-focused utility application, not a marketing site

**Document Images:**
- Photo ID uploads: Display actual uploaded images at 200px x 150px in designated form area
- Signature: Canvas-captured signature shown as image (actual signature rendering, not placeholder)
- Supporting documents: Thumbnail previews in 120px x 120px grid
- Form backgrounds: Subtle watermark of government seal (very light opacity) on printable form view

## API Setu Integration Visual Indicators

- **Verification Badge:** Green checkmark icon next to verified fields (e.g., "Aadhaar verified via API Setu")
- **Sync Status:** Indicator showing connection to API Setu ("Connected" with green dot, "Syncing" with spinner)
- **Submission Confirmation:** Official-looking receipt/acknowledgment with submission ID, timestamp, and government seal

## Accessibility Requirements

- High contrast text (minimum 4.5:1 ratio)
- Focus indicators on all interactive elements (2px outline)
- Screen reader labels for all form fields in all languages
- Keyboard navigation support (Tab, Enter, Esc)
- Error states announced to screen readers
- Touch targets minimum 44px x 44px