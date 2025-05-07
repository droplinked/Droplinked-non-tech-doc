# Feature Name: Shop Credit System*
**Version number:** 2.0.0  
**Date created/updated:** 2025/5/6  


## 1. Overview:

**Shop Credit System** is a foundational feature within Droplinked designed to manage and utilize merchant earnings internally on the platform. This feature addresses the challenge of handling merchant transactions by allowing profits (e.g., from sales) to be accumulated as platform credits, which are tied to the specific shop.

These credits—equivalent to U.S. dollars—can be used across the platform for various actions, such as upgrading subscription plans or covering platform fees, without requiring immediate external bank transactions. The goal is to provide greater flexibility and efficiency for merchants by enabling automated deductions and reducing reliance on external banking systems for every transaction.


## 2. User Story:

- **As a** merchant,  
- **I want to** use credits within my shop to cover transactions and fees,  
- **So that I can** reduce reliance on banking systems and automate withdrawals and payments.


## 3. Pre-requisites and Flow:

- **Pre-requisites**:
  - A shop must be created on the Droplinked platform for the credit system to be activated. Credits are initialized at 0 upon shop creation and cannot be added until the shop is created.

- **Flow**:
  1. The merchant creates a shop on the platform, which triggers the initialization of the credit system with a balance of 0.
  2. The merchant logs into their dashboard.
  3. From the sidebar, the merchant navigates to the "Account Settings" section and selects the "Credits and Account Activity" tab.
  4. In this tab, the merchant can view their current credit balance, along with the history of credit changes.
  5. The merchant can apply a date filter to see the credit increases and decreases over time, along with a detailed table showing the list of events that have impacted the credit balance.


## 4. Inputs:
- **User Inputs**:
  - None. The user only needs to navigate to the "Credits and Account Activity" section to view their current credit balance and transaction history.

- **System Inputs**:
  - The system automatically updates the credit balance based on merchant activities (e.g., sales, payments, etc.).



## 5. Logic and Process:

The credit system follows a set of rules and processes that determine how credits are added or deducted based on various activities:

1. **Order**:
   - If the merchant's Stripe account or wallet is not connected, the profit from the order is added to the merchant's credit balance.

2. **Order with Discount**:
   - If the merchant provides a discount code to the customer and this results in a lower profit for Droplinked, the difference is deducted from the merchant's credit balance.

3. **Credit Recharge**:
   - The merchant can manually recharge their credit balance via the "Add Credit" button on the credit page. The merchant enters the amount, and the payment is processed through Stripe.

4. **Affiliate Share**:
   - If the merchant participates in an affiliate program and co-sells a product, the profit generated from the sale is added to the merchant's credit balance.

5. **Subscription Renewal**:
   - When a subscription plan is renewed automatically, the subscription amount is deducted from the merchant's credit balance.

6. **Gamification Rewards**:
   - Merchants can earn points through the platform's gamification system. As they complete stages, rewards in the form of credits are added to their balance.

7. **Withdrawal**:
   - The merchant can withdraw their credit balance, and the corresponding amount will be deducted from the shop's credit balance and transferred to the merchant.

8. **Referral**:
   - The merchant can refer others to the platform, and when those referred users make purchases, a portion of the profits from those sales is credited to the merchant's account.

9. **Admin Credit Addition**:
   - The merchant's credit can be manually added by the super admin. In this case, the Droplinked admin adds credits directly to the merchant's account from the admin panel.




## 6. Outputs:

The feature produces the following outputs:

- The merchant can view their credit balance for a specific period based on the applied date filter. This includes:
  - **Credit Inflows**: The total amount added to the credit during the selected period.
  - **Credit Outflows**: The total amount deducted from the credit during the selected period.
  - **Current Credit Balance**: The current balance of credits available in the merchant's account.

- A detailed table is provided, listing each transaction that impacted the credit balance. This table includes:
  - **Transaction Cause**: The reason for the credit change (e.g., order, discount, withdrawal, etc.)
  - **Amount**: The value added or deducted from the credit balance.
  - **Date**: The date of the transaction.
  - **Additional Details**: Any additional information or context related to the transaction.



## 7. Error Handling and Troubleshooting:

- **Error 1**: **Failure to Charge Credit via Stripe**
  - **Description**: If the merchant is unable to charge their credit via Stripe, no proper error message is currently displayed.
  - **Solution**: The merchant will need to retry the payment process. A proper error message will be implemented in the future to inform the merchant of the failure reason.

- **Error 2**: **Withdrawal Failure**
  - **Description**: If the merchant tries to withdraw their credit and the transaction fails, the system will display an error suggesting that the merchant should contact support.
  - **Solution**: The merchant should reach out to the support team for assistance in resolving the issue.

- **Error 3**: **Profit Not Added to Credit**
  - **Description**: There are various cases in which the system may fail to add profit from sales to the merchant's credit balance, such as issues with Stripe or internal errors.
  - **Solution**: The merchant should contact support for troubleshooting, and the issue will be investigated.

- **Error 4**: **Unsuccessful Stripe Account Connection**
  - **Description**: If the merchant's Stripe account is not properly connected, credit will not be added correctly. This issue has not yet been handled by the system.
  - **Solution**: The merchant needs to verify and reconnect their Stripe account. An appropriate error message will be added in the future.

- **Error 5**: **Incomplete or Incorrect Transaction History**
  - **Description**: If the merchant's transaction history or credit-related information is not displaying correctly, this issue has not yet been handled.
  - **Solution**: The merchant should contact support for further assistance.



## 8. Testing and Validation:

### **Test Case 1: Charge Credit via Stripe**
- **Scenario**: The merchant attempts to add credit to their account via Stripe.
- **Expected Outcome**: 
  - If the payment is successful, the credit balance should be updated accordingly.
  - If the payment fails, an appropriate error message should be displayed, and the merchant should be prompted to retry the payment process.
  
### **Test Case 2: Withdrawal of Credit**
- **Scenario**: The merchant attempts to withdraw their credit balance.
- **Expected Outcome**:
  - If the withdrawal is successful, the credit balance should be deducted accordingly, and the merchant should receive the withdrawn amount.
  - If the withdrawal fails, the system should display an error message indicating the issue and suggest contacting support.

### **Test Case 3: Credit Added from Sales (Order) Without Stripe Connected**
- **Scenario**: The merchant does not have their Stripe account connected, and they make a sale.
- **Expected Outcome**:
  - The profit from the sale should be automatically added to the merchant's credit balance.
  - The system should correctly add the amount to the credit balance without needing Stripe connection.

### **Test Case 4: Credit Deduction Due to Discount**
- **Scenario**: The merchant applies a discount code, reducing the profit margin from a sale.
- **Expected Outcome**:
  - The difference in the profit (due to the discount) should be deducted from the merchant's credit balance.
  - The transaction should be reflected correctly in the credit history.

### **Test Case 5: Automatic Subscription Renewal**
- **Scenario**: The merchant's subscription is automatically renewed, and the subscription cost is deducted from the credit balance.
- **Expected Outcome**:
  - The subscription renewal fee should be deducted from the merchant's credit balance automatically.
  - The transaction should be recorded in the transaction history.

### **Test Case 6: Credit Recharge via "Add Credit" Button**
- **Scenario**: The merchant adds funds manually via the "Add Credit" button and makes a payment through Stripe.
- **Expected Outcome**:
  - The credit balance should be updated accordingly after the payment is processed.
  - The merchant should receive a confirmation of the successful credit recharge.

### **Test Case 7: Transaction History and Credit Tracking**
- **Scenario**: The merchant applies a date filter to view credit inflows and outflows.
- **Expected Outcome**:
  - The system should correctly display the credit inflows and outflows within the selected period.
  - A detailed table should show all transactions that affected the credit balance, including the reason (e.g., sale, discount, withdrawal), the amount, and the date.

### **Test Case 8: Affiliate Share Adding Credit**
- **Scenario**: The merchant participates in an affiliate program and earns commission from a sale as a co-seller.
- **Expected Outcome**:
  - The earned affiliate commission should be added to the merchant's credit balance.
  - The transaction should be reflected in the credit history.

### **Test Case 9: Credit Addition by Admin (Super Admin)**
- **Scenario**: A super admin manually adds credit to the merchant's account.
- **Expected Outcome**:
  - The merchant's credit balance should be updated with the manually added amount.
  - The transaction should be recorded in the credit history with details about the source (super admin).

### **Test Case 10: Stripe Account Connection Error**
- **Scenario**: The merchant attempts to connect their Stripe account but encounters an error.
- **Expected Outcome**:
  - The system should display a clear error message explaining the connection issue and guide the merchant on how to resolve it (e.g., reattempt connection or contact support).
  
### **Test Case 11: Insufficient Credit for Withdrawal**
- **Scenario**: The merchant attempts to withdraw an amount greater than their available credit balance.
- **Expected Outcome**:
  - The system should display an error message stating that the withdrawal exceeds the available balance, and the withdrawal request should not be processed.

### **Test Case 12: Handling Multiple Discounts**
- **Scenario**: The merchant applies multiple discount codes to a single order.
- **Expected Outcome**:
  - The system should properly calculate the total discount and deduct the correct amount from the merchant's credit balance based on the final profit.

### **Test Case 13: Gamification Rewards Impact on Credit**
- **Scenario**: The merchant completes a stage in the gamification system and earns a reward in the form of credit.
- **Expected Outcome**:
  - The reward should be added to the merchant's credit balance.
  - The transaction should be reflected in the credit history with details of the gamification reward.

### **Test Case 14: Handling of Credit in the Event of a System Failure**
- **Scenario**: There is a temporary server issue or internal failure while processing a credit transaction (e.g., adding credit via Stripe or processing a sale).
- **Expected Outcome**:
  - The system should either retry the transaction or alert the merchant with an error message explaining the failure.
  - A support contact should be provided if the issue persists.



## 9. Change Log:

- **2024**: Initial addition of the Shop Credit System feature to the platform.

- **May 25, 2025**: Recent updates to the Shop Credit System:
  - Added monthly subscription renewal via credit balance.
  - Documentation created for the Shop Credit System feature.
  - Introduced credit recharge service directly from the merchant panel.

## 10. Limitations and Notes:

- **Limitations**:
  - There are currently no specific limits on the credit balance, but the merchant cannot use the credit in all sections of the platform at this time.

- **Notes**:
  - The database may need to be updated and optimized, as there is a large amount of data stored in the related table.

