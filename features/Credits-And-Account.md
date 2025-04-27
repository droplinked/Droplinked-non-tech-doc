## Credits and Account Activity

### Overview
The Credits system represents the merchant’s available balance within the Droplinked platform, expressed in USD. It tracks all financial activities related to earnings, deductions, and manual adjustments.

### Details
Credits serve as the internal financial record for each merchant. They reflect the net earnings, expenses, and adjustments within the Droplinked ecosystem. Here's how it works:

- **Account Balance:** Shows the merchant’s current total Credits available.
- **Date Filter:** Allows merchants to view their credit activity within a selected timeframe.
- **Inbound / Outbound Summary:** Displays total inflows (Credits earned or added) and outflows (Credits deducted) separately.
- **Credit Log Table:** A detailed list of all credit activities, each entry showing:
  - **Type** (source or reason for credit change)
  - **Amount** 
  - **Date**
  - **Status** (`completed` or `failed`)
  - **Transaction ID**

**Types of Credit Activities:**

| Type | Description |
| --- | --- |
| ORDER | Earnings from customer orders. The merchant's share is added to Credits after deducting Droplinked's commission. |
| DISCOUNT_CODE | When a merchant offers a discount that reduces Droplinked’s commission, the difference is deducted from their Credits. |
| CREDIT_BALANCE | Merchant manually adds funds via dashboard payment gateway. |
| AFFILIATE_SHARE | Earnings from co-selling another merchant’s product, credited to the merchant’s balance. |
| SUBSCRIPTION_UPDATE | Subscription plan fees are automatically deducted from the Credits monthly. |
| GAMIFICATION_REWARD | Rewards earned through completing dashboard quests are credited. |
| WITHDRAW | Merchant requests a withdrawal from Credits to their wallet or bank account. |
| REFERRAL | Earnings from referring other merchants who make successful sales. |
| CUSTOMER_SUPPORT_FEE | Monthly management fees deducted for shops with dedicated support/management service. |
| BULK_ORDER | (Needs review: currently overlaps with Sample Order operations.) |
| MANUAL_CREDIT_ADJUSTMENT | Manual credit adjustments performed by the Droplinked team, typically when payments are made outside the dashboard. |

### Why It Matters
The Credits and Account Activity dashboard gives merchants complete visibility and control over their earnings and expenses on Droplinked. By providing real-time tracking, detailed logs, and simple filtering, merchants can easily manage their finances, optimize their operations, and maintain transparency in their business activities. This feature ensures trust, efficiency, and seamless financial management within the platform.

![Account Balance Overview](https://upload-file-droplinked.s3.amazonaws.com/7e9f52bf5ba34e1a66a46ef5a474e072717e7616234d03324169c6774419372f.png)


![Account Balance Overview](https://upload-file-droplinked.s3.amazonaws.com/0d64ecf355b22dfe796eeea9610c735822fc6c51ad5f2d45927e6ad13695ede9.png)