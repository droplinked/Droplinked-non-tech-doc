# Droplinked Lens (Standalone App)

**Date:** January 31, 2026

**Status:** Draft

**Author:** Product Manager

---

## 1. Project Overview

We are looking to develop **Droplinked Lens**, a standalone mobile application that simplifies the selling process on the Droplinked ecosystem. This app will function separately from the main Droplinked dashboard, acting as a lightweight, "camera-first" tool for rapid inventory creation.

**Your Goal:** Take this brief and convert it into a comprehensive Product Requirements Document (PRD) and functional specifications for our engineering team.

## 2. The Core Concept

The primary value proposition is **"Snap, Price, Sell."**
We want to remove all the barriers of traditional ecommerce setups (shipping zones, tax settings, complex forms) and allow a user to list a physical product just by pointing their camera at it.

## 3. Key Functional Modules

### 3.1. Standalone Onboarding

- **Experience:** The app must be accessible to users who have never used Droplinked before.
- **Requirement:** Users must be able to Sign Up or Log In directly in this app.
- **Account Logic:** If a user is new, the system must automatically provision a default shop/storefront for them in the background, so they can start selling immediately without dashboard configuration.

### 3.2. Product Creation (The "Lens" Feature)

The app must support two specific methods for adding products to the user's inventory.

**Method A: Quick Upload (Manual)**

- User takes a photo of the item.
- User manually types a Title and Price.
- Product is listed instantly.

**Method B: AI-Assisted Listing**

- User takes a photo of the item.
- User provides a simple input (optional text prompt or voice note).
- **The system (via AI) must:**
    - Analyze the image.
    - Generate a descriptive Title.
    - Write a full Product Description.
    - Suggest a Category.
- User reviews the AI-generated content, adds a Price, and publishes.

### 3.3. Inventory Simplification

- Users need a simple view of their active listings.
- No complex analytics or order management—just a "Portfolio" view of what is currently on sale.

### 3.4. Store Sharing

- Since the app is for creating supply, the "Storefront" remains the web-based Droplinked URL.
- **Requirement:** The app must provide easy ways to distribute this URL:
    - **QR Code:** A scannable code for in-person selling (markets, pop-ups).
    - **Social Share:** One-tap sharing of the store link to other apps.

## 4. Target Audience

- **Casual Sellers:** Individuals selling personal items.
- **Influencers:** Creators making "Drops" of curated products.
- **Field Merchants:** Sellers at physical locations (flea markets) who need digital catalogs instantly.

## 5. Success Criteria

- **Speed:** Listing a product should take less than 45 seconds.
- **Simplicity:** Zero training required. A user should understand how to list a product immediately upon opening the app.
- **Autonomy:** The user should never need to visit the desktop dashboard to complete a basic sale cycle.

## 6. Constraints to Note

- The app interacts with our existing ecosystem via API, but acts as a separate client.
- It does not handle fulfillment or shipping label generation (that happens in the main platform/email).