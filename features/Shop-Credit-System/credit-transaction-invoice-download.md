# Feature Name: Credit Transaction Invoice Download
**Version number:** 2.0.0  
**Date created/updated:** 2025/5/6  


## 🔍 Description
This feature adds the ability for merchants to download PDF invoices for each credit transaction within their shop. A new "Invoice" icon will be displayed next to each credit transaction in the shop's credit activity section. When clicked, a PDF invoice for that transaction will be shown in a new tab, with an option to download it as a PDF file. This feature helps merchants to keep track of their credit-related transactions and have an official record for each one.

## 👤 User Stories
- As a **merchant**, I want to **download invoices for my credit transactions**, so that **I can keep official records of all credit-related activities in my shop.**
- As a **merchant**, I want to **click on an invoice icon next to each credit transaction** to **quickly access the corresponding invoice** and download it as a PDF.

## 🧠 Business Logic

### Step 1: **Displaying the Invoice Icon**
- **Logic**: Each credit transaction in the merchant's credit activity section will have an "Invoice" icon next to it.
- **Rules**: 
  - The icon is displayed only for transactions that have an associated invoice available.
  - The icon will be clickable, triggering the invoice preview.
- **Validation**: The icon must be correctly linked to the invoice for each transaction.
- **Outcome**: The merchant sees the invoice icon next to each credit transaction.

### Step 2: **Previewing the Invoice**
- **Logic**: When the merchant clicks on the "Invoice" icon, a new tab opens displaying the PDF invoice for that specific credit transaction.
- **Rules**: The PDF invoice will display the necessary details of the transaction (e.g., transaction ID, date, amount, credit balance affected).
- **Validation**: The system must load the correct invoice based on the selected transaction.
- **Outcome**: The merchant sees a preview of the invoice in a new tab.

### Step 3: **Downloading the Invoice**
- **Logic**: Inside the invoice preview tab, a "Download" button will be available.
- **Rules**: 
  - Clicking the "Download" button will trigger the download of the invoice as a PDF file.
  - The downloaded file will be saved to the merchant's device in PDF format.
- **Validation**: The download link must trigger the correct PDF file download.
- **Outcome**: The merchant successfully downloads the invoice as a PDF.

## 📌 Change Log

| Version | Date       | Change Summary         | Author     |
|---------|------------|-------------------------|------------|
| 2.1.0   | 2025-06-08 | Initial version for Credit Invoice Download | Behdad  |

## 🔗 Dependencies
- **Shop Credit System**: This feature is dependent on the credit transaction records in the **Shop Credit System**. Invoices are generated based on the credit activities logged there.

