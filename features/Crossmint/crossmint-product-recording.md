# Feature Name: Crossmint Product Recording

**Version number:** 2.0.0  
**Date created/updated:** 2025/06/08


## 🔍 Description
This feature allows merchants to record their imported products as NFTs using the Crossmint wallet. After importing products using the product input feature, merchants can activate a toggle to record their imported products into the Crossmint wallet. This feature is currently in beta, allowing a maximum of 50 products to be recorded in one batch. Once the request is made, the recording process takes some time (approximately 20 minutes). After the process completes, merchants can export the NFTs to their MetaMask wallet.

## 👤 User Stories
- As a **merchant**, I want to **record imported products as NFTs using Crossmint**, so that **I can manage my digital assets and integrate with blockchain systems.**
- As a **merchant**, I want to **export the recorded NFTs to my MetaMask wallet**, so that **I can store and trade my NFTs easily.**

## 🧠 Business Logic

### Step 1: **Activating the Crossmint Toggle**
- **Logic**: After importing products through the product input feature, the merchant can activate the "Record Imported Products" toggle. 
- **Rules**: 
  - The toggle is disabled by default and must be manually activated by the merchant.
  - The merchant can only record up to 50 products at a time for the beta version.
- **Validation**: The system ensures that no more than 50 products are selected for recording at once.
- **Outcome**: The toggle is successfully activated, and the product recording process begins.

### Step 2: **Product Recording Process**
- **Logic**: Once the toggle is activated, the selected products are recorded to the merchant's Crossmint wallet.
- **Rules**: The process takes around 20 minutes for the products to be recorded.
- **Validation**: The system ensures that the maximum of 50 products is recorded.
- **Outcome**: The products are successfully recorded in the merchant's Crossmint wallet.

### Step 3: **Viewing Recorded NFTs**
- **Logic**: After the products are recorded, the merchant can navigate to the `/analytics/crossmint` route to view a list of the recorded NFTs.
- **Rules**: The list will show the NFTs that have been recorded successfully.
- **Validation**: The merchant can see all the recorded NFTs in the analytics page after the process completes.
- **Outcome**: The merchant successfully views the list of NFTs that have been recorded.

### Step 4: **Exporting NFTs to MetaMask**
- **Logic**: After viewing the recorded NFTs, the merchant can click the "Export NFT" button to transfer the NFTs to their MetaMask wallet.
- **Rules**: The export process moves the NFTs to the MetaMask wallet once the merchant clicks the button.
- **Validation**: The system ensures that the export happens successfully to the merchant's MetaMask wallet.
- **Outcome**: The NFTs are successfully exported to the merchant's MetaMask wallet.

## 📌 Change Log

| Version | Date       | Change Summary         | Author     |
|---------|------------|-------------------------|------------|
| 2.0.0   | 2025-06-08 | Beta version released for Crossmint integration | Matin  |

## 🔗 Dependencies
- **Product Import**: This feature depends on the product import functionality, which allows products to be added to the system before recording as NFTs.

