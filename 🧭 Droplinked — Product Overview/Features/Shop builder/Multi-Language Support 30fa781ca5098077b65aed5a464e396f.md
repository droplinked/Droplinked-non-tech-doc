# Multi-Language Support

**Feature ID:** IAA-DASH-001

**Category:** Dashboard | Localization

**Actors:** Merchant (Logged-out), Merchant (Logged-in)

**Channel:** Web

**Status:** Writing Spec (Draft)

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Merchants from different regions need to manage their shops using the Dashboard in their preferred language. Currently, the Dashboard only supports English, which creates barriers for non-English speaking merchants. This feature allows merchants to use the Dashboard interface in their native language, improving usability and accessibility.

**Key Constraint:** This feature changes ONLY the Dashboard interface language, not the shop/storefront that customers see.

### User Stories

- As a Merchant (not logged in), I want to change the Dashboard language from the login page footer so that I can interact with the login interface in my preferred language.
- As a Merchant (logged in), I want to change my Dashboard language from settings so that my preference is saved to my profile and persists across sessions.
- As a Merchant, I want my Dashboard language preference to persist when I return so that I don't have to change it every time I log in.
- As a Merchant, I want to use the Dashboard in Arabic/Mongolian so that I can manage my shop more comfortably in my native language.

### Key User Journeys

**Journey 1: Not Logged-in Merchant Changes Language on Login Page**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens Dashboard login page | Login page loads in default language (English) |
| 2 | Merchant | Views footer | Footer is visible with language selector |
| 3 | Merchant | Clicks language selector | Dropdown opens showing available languages |
| 4 | Merchant | Selects "Arabic" | Login page reloads in Arabic |
| 5 | System | Saves preference | Language saved in browser localStorage |
| 6 | Merchant | Enters credentials and logs in | Dashboard loads in Arabic after login |

**Journey 2: Logged-in Merchant Changes Language via Settings**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Logs in to Dashboard | Dashboard loads (in previously saved language) |
| 2 | Merchant | Navigates to Settings | Settings page opens |
| 3 | Merchant | Finds "Language" section | Language dropdown visible |
| 4 | Merchant | Selects "Mongolian" | Preference saved immediately |
| 5 | System | Updates profile | Language preference saved to merchant profile |
| 6 | System | Updates interface | Dashboard interface switches to Mongolian |
| 7 | Merchant | Uses Dashboard | All menus, buttons, labels display in Mongolian |

**Journey 3: Returning Merchant with Saved Preference**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Returns to Dashboard after 3 days | Login page loads |
| 2 | System | Checks localStorage | Finds previous language preference (Arabic) |
| 3 | System | Applies language | Login page displays in Arabic automatically |
| 4 | Merchant | Logs in | Dashboard also displays in Arabic |
| 5 | Merchant | No need to change language | Preference persisted from last session |

**Journey 4: Language Preference Sync Across Sessions**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Logs in on new device | Login page loads |
| 2 | System | Checks localStorage | No saved preference on this device |
| 3 | Merchant | Logs in | Dashboard loads in profile language (Mongolian) |
| 4 | Merchant | Changes language to English in settings | Profile updated with new preference |
| 5 | Merchant | Logs out and logs in on another device | Dashboard loads in English automatically |

### Scope

**✅ In Scope:**

**Languages:**

- English (Default)
- Arabic
- Mongolian

**Language Switching:**

- Footer language selector on login page (for not logged-in merchants)
- Settings page language selector (for logged-in merchants)
- Language persistence in localStorage (not logged-in merchants)
- Language persistence in merchant profile (logged-in merchants)

**UI Elements:**

- Language dropdown/selector component
- Language indicator showing current selection
- Flag icons or language names in selector
- RTL (Right-to-Left) support for Arabic in Dashboard

**Content (Dashboard Interface Only):**

- Dashboard navigation menus
- Dashboard buttons and labels
- Settings page elements
- Form labels and placeholders
- Error messages and validation text
- Modal titles and content
- Sidebar menu items
- Empty states and help text

**❌ Out of Scope:**

- Shop/storefront language (customer-facing pages)
- Product content translation (names, descriptions)
- Automatic translation of any content
- Currency conversion
- Locale-specific formatting (dates, numbers) for this phase
- Email template translations
- More than 3 languages (expandable in future)

### Acceptance Criteria

- [ ]  Dashboard supports 3 languages: English (default), Arabic, Mongolian
- [ ]  Language selector visible in footer on login page (for not logged-in users)
- [ ]  Language selector accessible from Settings (for logged-in users)
- [ ]  Clicking language option changes Dashboard interface immediately
- [ ]  Default language is English when no preference exists
- [ ]  Not logged-in merchant preference saved in browser localStorage
- [ ]  Logged-in merchant preference saved to user profile in database
- [ ]  Language persists across sessions for returning merchants
- [ ]  Language syncs across devices for logged-in merchants (via profile)
- [ ]  Arabic language displays Dashboard with RTL layout
- [ ]  All Dashboard UI elements translated in all 3 languages
- [ ]  Language selector shows current active language
- [ ]  Language change triggers interface update (page reload or dynamic)
- [ ]  Login page respects language preference from localStorage
- [ ]  Dashboard respects language preference from profile after login

### Technical Notes

- Language detection order: User profile (if logged in) → Browser localStorage → Default (English)
- Implement i18n framework for translation management
- Store translations in JSON files
- RTL support required for Arabic Dashboard (css direction: rtl)
- Language code format: ISO 639-1 (en, ar, mn)
- Dashboard language is independent from shop/storefront language

### Dependencies

- Merchant profile system (for saving preferences)
- Authentication system
- i18n library (e.g., i18next, react-intl)
- Translation files for all 3 languages
- RTL support implementation

---

## UI Flow (Source of Truth)

**Not Logged-in Merchant Flow:**

```
[Login Page - English]
    ↓
[View Footer]
    ↓
[Click Language Selector]
    ├─ Current: English ▼
    ├─ 🇬🇧 English
    ├─ 🇸🇦 العربية
    └─ 🇲🇳 Монгол
    ↓
[Select Arabic]
    ↓
[Login Page Reloads - Arabic RTL]
    ├─ Interface: Arabic
    └─ localStorage: { dashboard_lang: 'ar' }
    ↓
[Merchant Logs In]
    ↓
[Dashboard Loads - Arabic]
```

**Logged-in Merchant Flow:**

```
[Dashboard - Any Language]
    ↓
[Click Settings]
    ↓
[Settings Page]
    ├─ Account
    ├─ Password
    └─ Language [Current ▼]
        ├─ English
        ├─ العربية
        └─ Монгол
    ↓
[Select Mongolian]
    ↓
[Preference Saved]
    ├─ Database: user.dashboard_lang = 'mn'
    ├─ UI: Mongolian
    └─ All Dashboard elements in Mongolian
```

**Returning Merchant Flow:**

```
[Merchant Opens Dashboard]
    ↓
[On Login Page]
    ↓
[System Checks]
    ├─ localStorage has lang? → Use localStorage (show login page in that language)
    └─ Default → English
    ↓
[Merchant Logs In]
    ↓
[System Checks Profile]
    ├─ Profile has lang? → Use profile language
    └─ Use localStorage language
    ↓
[Dashboard Loads with Detected Language]
```

---

## Attachments

- 📎 **Linked Request:** REQ-DASH-001
- 🎨 **Design:** [Figma - Dashboard Multi-Language]
- 🧪 **Test Cases:** TC-DASH-001 through TC-DASH-030

---

## Change Log

- 2026-02-22 — Behdad — Initial draft created (corrected: Dashboard-only language feature)