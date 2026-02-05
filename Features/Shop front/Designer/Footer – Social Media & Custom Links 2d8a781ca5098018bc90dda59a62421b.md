# Footer – Social Media & Custom Links

**Feature ID:** IAA-FOOTER-102

**Category:** Shop Builder | **Actors:** Merchant, Customer

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Merchants need to promote their social media presence and add important external links to their storefront footer. This increases customer engagement, drives traffic to social channels, and provides essential information links. Customers need easy access to merchant's social media profiles and important pages directly from the storefront.

### **Desired Outcome**

- Allow merchants to add social media links to the footer through Designer.
- Support major social media platforms with automatic icon recognition.
- Enable merchants to create custom footer sections with external links.
- Ensure all links open in new tabs for better user experience.
- Handle various link formats (with or without domain prefix) automatically.
- Display footer links and social media icons correctly on storefront.

---

# **2) Scope – In / Out**

### **In Scope**

- Editing footer via Designer Canvas (click on footer component).
- Adding social media links for supported platforms:
    - Discord
    - Facebook
    - Instagram
    - Telegram
    - LinkedIn
    - Messenger
    - X (Twitter)
    - TikTok
- Automatic handling of link formats (with or without `https://` or domain).
- Creating up to **2 custom footer sections**.
- Each custom section includes:
    - Section title
    - Multiple custom links with custom names
- All links open in new tab when clicked by customers.
- Social media icons display correctly on storefront footer.
- Custom links display with their custom names in footer sections.

### **Out of Scope**

- More than 2 custom footer sections.
- Newsletter signup in footer.
- Footer copyright text editing.
- Footer layout customization (column arrangement).
- Dynamic footer content based on customer location.
- Footer analytics tracking.
- Other social media platforms not listed above.

---

# **3) Key User Journeys**

### **Journey 1 – Merchant Adds Social Media Links**

As a **Merchant**, I can add my social media links to the footer.

**Step 1:** Merchant opens Designer Canvas.

**Step 2:** Clicks on the **Footer** component.

**Step 3:** Sees a **Social Media** section with options for:

- Discord
- Facebook
- Instagram
- Telegram
- LinkedIn
- Messenger
- X
- TikTok

**Step 4:** Selects a social media platform (e.g., Instagram).

**Step 5:** Enters the link in any of these formats:

- `https://instagram.com/myshop`
- `instagram.com/myshop`
- `myshop` (username only)

**Step 6:** System automatically handles and formats the link correctly.

**Step 7:** Merchant can add multiple social media links.

**Step 8:** Clicks **Publish** to save changes.

**Step 9:** Social media icons appear in storefront footer.

---

### **Journey 2 – Merchant Creates Custom Footer Section**

As a **Merchant**, I can create custom footer sections with links.

**Step 1:** Merchant is in Footer edit mode in Designer.

**Step 2:** Clicks **Add Custom Section** (maximum 2 sections allowed).

**Step 3:** Enters a section title (e.g., "About Us", "Help Center", "Legal").

**Step 4:** Adds custom links to the section:

- Link name: "Privacy Policy"
- Link URL: `https://example.com/privacy`

**Step 5:** Adds more links to the same section:

- Link name: "Terms of Service"
- Link URL: `https://example.com/terms`

**Step 6:** Repeats for a second custom section if needed.

**Step 7:** Clicks **Publish** to save changes.

**Step 8:** Custom sections appear in storefront footer with all links.

---

### **Journey 3 – Customer Clicks Social Media Icon**

As a **Customer**, I can access the merchant's social media from the footer.

**Step 1:** Customer is browsing the storefront.

**Step 2:** Scrolls to the footer.

**Step 3:** Sees social media icons (e.g., Instagram, Facebook, TikTok).

**Step 4:** Clicks on an icon (e.g., Instagram).

**Step 5:** Link opens in a **new tab**.

**Step 6:** Customer is redirected to the merchant's Instagram profile.

**Step 7:** Can return to storefront easily (original tab remains open).

---

### **Journey 4 – Customer Clicks Custom Footer Link**

As a **Customer**, I can access important pages via footer links.

**Step 1:** Customer scrolls to storefront footer.

**Step 2:** Sees custom footer sections (e.g., "Legal", "Help").

**Step 3:** Each section has a title and list of links.

**Step 4:** Clicks on a link (e.g., "Privacy Policy").

**Step 5:** Link opens in a **new tab**.

**Step 6:** Customer is redirected to the external page.

**Step 7:** Can return to storefront (original tab remains open).

---

### **Journey 5 – Merchant Edits Social Media Links**

As a **Merchant**, I can update or remove social media links.

**Step 1:** Merchant opens Designer and clicks on Footer.

**Step 2:** Sees current social media links.

**Step 3:** Updates a link (e.g., changes Instagram username).

**Step 4:** Or removes a social media link by clearing the field.

**Step 5:** Clicks **Publish**.

**Step 6:** Changes reflect immediately on storefront footer.

---

### **Journey 6 – Merchant Removes Custom Footer Section**

As a **Merchant**, I can delete a custom footer section.

**Step 1:** Merchant is editing footer in Designer.

**Step 2:** Clicks **Delete** on a custom section.

**Step 3:** Section is removed immediately.

**Step 4:** Clicks **Publish**.

**Step 5:** Section disappears from storefront footer.

---

### **Journey 7 – Link Format Handling**

As a **Merchant**, I can enter social media links in various formats.

**Step 1:** Merchant enters Instagram link as: `myshopname`

**Step 2:** System automatically converts to: `https://instagram.com/myshopname`

**Step 3:** Merchant enters Facebook link as: `facebook.com/mypage`

**Step 4:** System automatically converts to: `https://facebook.com/mypage`

**Step 5:** Merchant enters TikTok link as: `https://tiktok.com/@myshop`

**Step 6:** System uses the link as-is.

**Step 7:** All links work correctly when published.

---

# **4) Business Acceptance Criteria (BAC)**

**BAC 1:** Merchant must be able to access footer editing by clicking on the footer component in Designer Canvas.

**BAC 2:** Footer editor must support adding links for all 8 social media platforms: Discord, Facebook, Instagram, Telegram, LinkedIn, Messenger, X, TikTok.

**BAC 3:** System must automatically handle social media links entered in these formats:

- Full URL with `https://`
- Domain with path (e.g., `instagram.com/username`)
- Username only (e.g., `username`)

**BAC 4:** Merchant can create up to **2 custom footer sections**.

**BAC 5:** Each custom section must have:

- A customizable title
- Ability to add multiple links with custom names

**BAC 6:** All social media icons must display correctly in the storefront footer.

**BAC 7:** All custom links must display with their custom names in the storefront footer.

**BAC 8:** Clicking any social media icon or custom link must open in a **new tab** (`target="_blank"`).

**BAC 9:** All links must redirect correctly to the specified URL without errors.

**BAC 10:** Merchant cannot create more than 2 custom footer sections.

**BAC 11:** Removing a social media link or custom section must update the storefront footer immediately after publishing.

**BAC 12:** Footer must display responsively on mobile, tablet, and desktop devices.

**BAC 13:** If a social media link is not provided, its icon must not appear in the footer.

**BAC 14:** Custom footer sections with no links must not appear in the storefront footer.

**BAC 15:** All external links must include security attributes (`rel="noopener noreferrer"`) when opened in new tab.