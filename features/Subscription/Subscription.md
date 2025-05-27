# Feature Name: Subscription
**Version number:** 2.0.0  
**Date created/updated:** 2025/5/6  

## 1. Overview:
The Subscription system in Droplinked is designed to monetize platform features through various subscription plans. Starting from this version, when a user purchases the Pro or Premium plan, a Stripe subscription is automatically created for auto-renewal. Legacy and Enterprise plans manage renewals via shop credit deduction, ensuring continuous service without manual intervention.

## 2. User Story:
- **As a** merchant,  
- **I want to** subscribe to higher-tier subscription plans,  
- **So that I can** unlock and use additional premium features in my store.



## 3. Pre-requisites and Flow:
 
- **Pre-requisites**:
  - Merchant must be logged in and have an existing store on Droplinked. 
  - Only merchants with active stores are eligible to purchase subscription plans.

- **Flow**:
  1. Merchant logs into the Droplinked dashboard.
  2. Clicks on the profile icon in the top-right corner of the dashboard.
  3. From the dropdown menu, selects the "Plans" option.
  4. Merchant is redirected to the subscription plans page.
  5. All plans (Starter, Pro, Premium, Enterprise) are visible with options for monthly, yearly, and 5-year billing.
  6. Merchant selects a desired plan and billing cycle.
  7. Chooses a payment method (credit card or crypto wallet).
  8. Completes the payment.
  9. Subscription is automatically activated and corresponding features are unlocked.
  10. After purchasing the Pro or Premium plan, the system automatically creates a Stripe subscription for the user.
  11. The user can deactivate their plan from the dashboard, which also deactivates the Stripe subscription. The current plan remains valid until the end of its term, and auto-renewal is cancelled.
  12. Shops with legacy or admin-activated plans have renewals handled via shop credit deduction.



## 4. Inputs:

- **User Inputs**:
  - **Billing Cycle Selection**: The merchant selects the preferred billing cycle (monthly, yearly, or 5-year).
  - **Plan Selection**: The merchant selects the desired subscription plan (Pro, Premium, or Enterprise).
  - **Payment Method**: The merchant chooses a payment method (credit/debit card or crypto wallet).


## 5. Logic and Process:
The feature follows the logic outlined below:
1. **Transaction Initialization**: When a subscription is purchased (via Stripe or Web3 wallet), a transaction is recorded in the system and the plan is initialized but not yet active.
2. **Callback Verification**: After successful payment confirmation, a callback is triggered to the backend which marks the subscription as active.
3. **Feature Activation**: Based on the selected plan, corresponding features are automatically enabled. Each plan is linked to a predefined set of features.
4. **Plan Upgrade Handling**: If a merchant upgrades to a higher-tier plan, the remaining duration of the current plan is added to the new plan. For example, if 2 weeks are left on a Pro plan and the user upgrades to Premium, the new Premium plan will be valid for 1 month and 2 weeks.
5. **Subscription Expiration**: Upon expiration, the system handles renewals based on the subscription type:
   - **Stripe Subscription**: The system checks if the subscription is not cancelled. If active, the plan is auto-renewed.
   - **Shop Credit**: If credit is available, the system deducts from the credit and auto-renews the plan. If no credit is available, the subscription is marked as expired and premium features are disabled.
6. **Stripe Subscription Management**: Upon purchase of the Pro or Premium plan, a Stripe subscription is automatically created and activated.
7. **Plan Cancellation**: If the plan is cancelled from the dashboard, the Stripe subscription is deactivated, but the plan remains valid until the current term ends and auto-renewal stops.
8. **Legacy Plan Management**: Enterprise shops and shops with legacy plans have their renewals managed through shop credit deduction.



## 6. Outputs:
The feature produces the following outputs:
- **Success Notification**: Upon successful subscription activation, a success message is displayed to the merchant.
- **Plan Status Display**: In the top-right dropdown menu (accessed by clicking the profile icon), the merchant can view:
  - The current active plan
  - The number of days remaining on the subscription
- **Stripe Subscription Notifications**:
  - Upon automatic creation of the Stripe subscription, a success message is shown to the user.
  - Upon plan cancellation from the dashboard, the user is notified that the Stripe subscription has been deactivated and the current plan remains valid until expiration.


## 7. Error Handling and Troubleshooting:

- **Error 1**: Payment is not completed or callback is not triggered
  - **Solution**: The subscription plan remains inactive. No changes are made. User may retry the payment process.

- **Error 2**: Callback (from Stripe or Web3) is not handled properly
  - **Solution**: Currently, no fallback mechanism exists. The plan is not activated. A future improvement could include retry logic or admin alerts.

- **Error 3**: Client secret is not generated correctly during Stripe/Web3 payment
  - **Solution**: Display a clear error message to the user and prevent payment from proceeding.

- **Error 4**: Subscription renewal fails due to shop credit not being properly applied
  - **Solution**: Currently, no fallback is applied. Ideally, the system should default the merchant back to the Starter plan when credit is insufficient.

- **Error 5**: Failure to create Stripe subscription after plan purchase
  - **Solution**: User should retry or contact support. The system must log the error for investigation.

- **Error 6**: Failure to deactivate Stripe subscription from the dashboard
  - **Solution**: Show an error message to the user and alert the support team.




## 8. Testing and Validation:

**Test Case 1**: Verify that a merchant can successfully purchase the Pro plan using Stripe.
  - **Expected Result**: The payment is processed successfully, a transaction is recorded, and Pro plan features are unlocked immediately.

**Test Case 2**: Verify that a merchant can successfully purchase the Premium plan using a Web3 wallet.
  - **Expected Result**: The transaction is confirmed via Web3, the callback is triggered, and Premium plan features are activated.

**Test Case 3**: Verify automatic renewal of an expired subscription when shop credit is sufficient.
  - **Expected Result**: The system deducts credit from the merchant's balance and renews the plan without user action.

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

**Test Case 10**: Verify purchasing Pro or Premium plan and automatic creation of Stripe subscription.
  - **Expected Result**: Stripe subscription is created and activated successfully.

**Test Case 11**: Verify deactivating the plan from the dashboard.
  - **Expected Result**: Stripe subscription is cancelled while the plan remains valid until expiration.

**Test Case 12**: Verify renewal process for legacy shops.
  - **Expected Result**: Shop credit deduction works correctly for renewal.



## 9. Change Log:

- **2024**: Initial release of the Subscription feature, enabling merchants to purchase tiered plans for unlocking premium features.
- **2025-05-06**: Added automatic plan renewal using shop credit when subscription expires. Previously, expired plans were not auto-renewed.
- **2025-05-06**: Database schema updated to support price and duration fields for Enterprise plans, which were previously undefined.
- **2025-05-27**: 
  - Added automatic creation of Stripe subscriptions for Pro and Premium plans
  - Enabled plan cancellation and Stripe subscription deactivation from the dashboard
  - Supported two renewal methods (Stripe auto-renew and shop credit deduction) depending on shop and plan type
  - Refactored database schema for improved maintainability and scalability
  - Resolved significant technical debt in the subscription system implementation


## 10. Limitations and Notes:

- **Limitations**:
  - Only one subscription plan can be active at a time for each merchant.
  - Currently, there are no options to pause or transfer a subscription plan.

- **Notes**:
  - During the initial release, all existing legacy shops were assigned to the Enterprise plan by default, using a different structure that needs to be reconciled and standardized.
