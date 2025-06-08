# Feature Name: Multi-Language Support for Shop Builder

**Version number:** 2.0.0  
**Date created/updated:** 2025/06/08

## 🔍 Description
This feature allows merchants to select between Arabic and English for the language of their Shop Builder site. A dropdown in the footer of the Shop Builder enables users to easily switch between the two languages. When a language is selected, the entire site language is updated accordingly. If the Arabic language is selected, the site will also switch to a Right-to-Left (RTL) layout to accommodate Arabic text direction.

## 👤 User Stories
- As a **merchant**, I want to **choose between Arabic and English languages** for my Shop Builder site, so that **I can create and manage my store in my preferred language.**
- As a **merchant**, I want to **switch the site to RTL mode** when selecting Arabic, so that **the content layout supports right-to-left text.**

## 🧠 Business Logic

### Step 1: **Accessing the Language Dropdown**
- **Logic**: The merchant enters the Shop Builder, where a dropdown is located in the footer of the page for selecting the site language.
- **Rules**: 
  - The dropdown will provide two language options: Arabic and English.
  - The dropdown is always visible to the merchant when accessing the Shop Builder.
- **Validation**: The dropdown must display the correct language options (Arabic and English).
- **Outcome**: The merchant sees the language selection dropdown in the footer of the Shop Builder.

### Step 2: **Selecting a Language**
- **Logic**: The merchant selects either Arabic or English from the dropdown.
- **Rules**: 
  - When English is selected, the site will update the content and layout to English.
  - When Arabic is selected, the site will update the content to Arabic and also change the site layout to RTL (Right-to-Left).
- **Validation**: 
  - The site must successfully switch between the two languages based on the selection.
  - The Arabic language selection must trigger the RTL layout.
- **Outcome**: The language of the site changes immediately after the merchant selects their preferred language.

### Step 3: **RTL Layout for Arabic**
- **Logic**: When Arabic is selected, the entire site layout will adjust to support Right-to-Left text direction.
- **Rules**: 
  - The text direction and content alignment will change to accommodate the RTL format.
  - This includes header, footer, content sections, and navigation.
- **Validation**: The layout must correctly reflect RTL formatting when Arabic is selected.
- **Outcome**: The site is fully displayed in RTL format with Arabic text when the language is set to Arabic.

## 📌 Change Log

| Version | Date       | Change Summary         | Author     |
|---------|------------|-------------------------|------------|
| 1.0     | 2025-06-08 | Initial version released for multi-language support | Behdad  |

## 🔗 Dependencies
- **Shop Builder**: This feature depends on the Shop Builder platform to allow merchants to access the site and change language settings.

