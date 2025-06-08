# Feature Name: Subscription
**Version number:** 2.0.0  
**Date created/updated:** 2025/5/6  

## 1. Overview:
**Subscription** is designed to enable monetization of platform features through various subscription plans. This feature primarily serves as a revenue-generation mechanism for 

Starting from this version, whenever a user purchases the Pro or Premium plan, a Stripe subscription is automatically created for auto-renewal.

Shops with legacy plans or admin-activated plans will have their renewals managed via shop credit deduction.



## 2. User Story:

- **As a** merchant,  
- **I want to** subscribe to higher-tier subscription plans,  
- **So that I can** unlock and use additional premium features in my store.



## 3. Pre-requisites and Flow:

- **Pre-requisites**:
  - Merchant must be logged in and have an existing store on Droplinked.
  - A valid payment method (credit/debit card or crypto wallet) must be provided.
  - Only merchants with active stores are eligible to purchase subscription plans.
  

- **Flow**:
  1. Merchant logs into the Droplinked dashboard.
  2. Clicks on the profile icon in the top-right corner of the dashboard.
  3. From the dropdown menu, selects the "Plans" option.
  4. Merchant is redirected to the subscription plans page.
  5. All plans (Starter, Pro, Premium, Enterprise) are visible with options for monthly, yearly, and 3-year billing.
  6. Merchant selects a desired plan and billing cycle.
  7. Chooses a payment method (credit card or crypto wallet).
  8. Completes the payment.
  9. Subscription is automatically activated and corresponding features are unlocked.



## 4. Inputs:

- **User Inputs**:
  - **Billing Cycle Selection**: The merchant selects the preferred billing cycle (monthly, yearly, or 3-year).
  - **Plan Selection**: The merchant selects the desired subscription plan (Pro, Premium, or Enterprise).
  - **Payment Method**: The merchant chooses a payment method (credit/debit card or crypto wallet).


## 5. Logic and Process:
The feature follows the logic outlined below:
1. **Transaction Initialization**: When a subscription is purchased (via Stripe or Web3 wallet), a transaction is recorded in the system and the plan is initialized but not yet active.
2. **Callback Verification**: After successful payment confirmation, a callback is triggered to the backend which marks the subscription as active.
3. **Feature Activation**: Based on the selected plan, corresponding features are automatically enabled. Each plan is linked to a predefined set of features.
4. **Plan Upgrade Handling**: If a merchant upgrades to a higher-tier plan, the remaining duration of the current plan is added to the new plan. For example, if 2 weeks are left on a Pro plan and the user upgrades to Premium, the new Premium plan will be valid for 1 month and 2 weeks.
5. **Subscription Expiration**: Upon expiration, the system checks for available shop credit:
   - If credit is available, the system deducts from the credit and auto-renews the plan.
   - If no credit is available, the subscription is marked as expired and premium features are disabled.



## 6. Outputs:
The feature produces the following outputs:
- **Success Notification**: Upon successful subscription activation, a success message is displayed to the merchant.
- **Plan Status Display**: In the top-right dropdown menu (accessed by clicking the profile icon), the merchant can view:
  - The current active plan
  - The number of days remaining on the subscription


## 7. Error Handling and Troubleshooting:

- **Error 1**: Payment is not completed or callback is not triggered
  - **Solution**: The subscription plan remains inactive. No changes are made. User may retry the payment process.

- **Error 2**: Callback (from Stripe or Web3) is not handled properly
  - **Solution**: Currently, no fallback mechanism exists. The plan is not activated. A future improvement could include retry logic or admin alerts.

- **Error 3**: Client secret is not generated correctly during Stripe/Web3 payment
  - **Solution**: Display a clear error message to the user and prevent payment from proceeding.

- **Error 4**: Subscription renewal fails due to shop credit not being properly applied
  - **Solution**: Currently, no fallback is applied. Ideally, the system should default the merchant back to the Starter plan when credit is insufficient.




## 8. Testing and Validation:

**Test Case 1**: Verify that a merchant can successfully purchase the Pro plan using Stripe.
  - **Expected Result**: The payment is processed successfully, a transaction is recorded, and Pro plan features are unlocked immediately.

**Test Case 2**: Verify that a merchant can successfully purchase the Premium plan using a Web3 wallet.
  - **Expected Result**: The transaction is confirmed via Web3, the callback is triggered, and Premium plan features are activated.

**Test Case 3**: Verify automatic renewal of an expired subscription when shop credit is sufficient.
  - **Expected Result**: The system deducts credit from the merchant’s balance and renews the plan without user action.

**Test Case 4**: Verify behavior when shop credit is insufficient during subscription renewal.
  - **Expected Result**: The plan is not renewed, and the merchant is downgraded to the Starter plan.

**Test Case 5**: Verify correct handling and visibility of legacy plans created before the new system.
  - **Expected Result**: Legacy plans are recognized and displayed properly, and their features remain intact.

**Test Case 6**: Verify system behavior when callback is not triggered or fails during payment.
  - **Expected Result**: The plan remains inactive, and no features are unlocked. Admin or system logs the failed callback.

**Test Case 7**: Verify behavior when client secret fails to generate during Stripe/Web3 payment initialization.
  - **Expected Result**: A clear error message is displayed to the merchant, and no payment is processed.

**Test Case 8**: Verify that a merchant cannot purchase a lower-tier plan while a higher-tier plan is active.
  - **Expected Result**: The system displays an error preventing the downgrade purchase.

**Test Case 9**: Verify that upgrading from one plan to another works and remaining time is added correctly.
  - **Expected Result**: New plan activates, and the total duration includes both remaining old time and new plan time.



## 9. Change Log:

- **2024**: Initial release of the Subscription feature, enabling merchants to purchase tiered plans for unlocking premium features.
- **2025-05-06**: Added automatic plan renewal using shop credit when subscription expires. Previously, expired plans were not auto-renewed.
- **2025-05-06**: Database schema updated to support price and duration fields for Enterprise plans, which were previously undefined.


## 10. Limitations and Notes:

- **Limitations**:
  - Only one subscription plan can be active at a time for each merchant.
  - Currently, there are no options to pause or transfer a subscription plan.

- **Notes**:
  - There is significant technical debt in the current implementation of the subscription system.
  - The database schema requires refactoring for better maintainability and scalability.
  - During the initial release, all existing legacy shops were assigned to the Enterprise plan by default, using a different structure that needs to be reconciled and standardized.
