# Feature Conflicts & Edge Cases Report

**Date:** 2026-02-11  
**Features Analyzed:**
- Shop Currency Selection (SHOP-CUR-001)
- Affiliate Network (IAA-AFF-500 series)
- Onchain Inventory (IAA-WEB3-001)
- On-Chain Crypto Payment Settlement (IAA-CHK-501)
- Checkout & Order Processing (CHK-*** series)

---

## Shop Currency Scenarios

1. **Credit Balance After Currency Change**
   If a shop changes its base currency from USD to EUR, how should their credit balance be calculated? Does the $500 credit become €450, or do we track multiple currency balances?

2. **Printful Integration with Non-USD Currency**
   Printful pricing is always in USD. If a shop changes currency to EUR, how do we handle POD product costs? Who bears the exchange rate risk - merchant or platform?

3. **Payment Provider Currency Support**
   Some payment providers (e.g., PayPal) don't support all currencies. How do we notify the user when they change their shop currency that their selected payment provider doesn't support it?

4. **Existing Products After Currency Change**
   If a user creates products priced at $100 USD and later changes their shop currency to EUR, what actions should be taken? Do old products stay in USD or get converted?

5. **Pending Orders During Currency Change**
   A merchant changes currency from USD to GBP. But they have 5 pending orders from the old currency. How are payouts calculated for these orders?

6. **Negative Balance in Different Currency**
   A merchant has $500 USD credit. Their shop is now in EUR and they have a negative share of €50 (owe platform money). Can the USD credit cover the EUR debt? What exchange rate applies?

7. **Multi-Currency Credit Display**
   A merchant's shop was USD for 6 months (earned $500), then changed to EUR for 3 months (earned €300). How is their total balance displayed and calculated?

---

## Affiliate Network + Currency Scenarios

8. **Affiliate Requires USD But Shop Changes Currency**
   Affiliate Network requires both shops to be USD. If an Affiliator shop changes currency to EUR after Co-sellers have already imported their products, what happens? Do products become unavailable? Stay with old pricing?

9. **Mixed Currency Affiliate Order**
   Affiliator shop = USD, Co-seller shop = EUR (somehow validation was bypassed). Customer pays in EUR on Co-seller shop. Product price is $100 USD. How much commission does Co-seller receive? In which currency?

10. **Co-seller Currency Change After Import**
    Co-seller imports products when shop was USD. Later changes shop currency to GBP. Affiliate products are supposed to sync price from Affiliator. How is commission calculated now? Which currency is used for the split?

11. **Non-USD Shop Tries to Activate Affiliate**
    A shop with EUR currency tries to activate Affiliate Network. Current behavior blocks with warning. Should we allow currency conversion prompt? Or force them to change to USD first?

12. **Currency Change With Active Co-sellers**
    An Affiliator has 10 Co-sellers importing their products. The Affiliator changes currency from USD to JPY. What happens to existing imports? Do they break? Stay in USD?

13. **Commission Calculation Timing**
    Co-seller shop = EUR, Affiliator shop = USD. Order happens in EUR. Commission should be in USD for Affiliator. When is conversion applied? At order time or payout time? What if exchange rate changes between those times?

---

## On-Chain Crypto Payment + Affiliate Scenarios

14. **Crypto Payment for Affiliate Product**
    Customer pays $100 USDC on Polygon for an affiliate product. Commission is 15% ($15 to Co-seller, $85 to Affiliator). Current crypto flow sends ALL to one merchant's connected wallet. Which merchant receives it? How is the split handled on-chain?

15. **Mixed Cart with Crypto Payment**
    Order contains both regular products AND affiliate products. Merchant has connected wallet for crypto payments. How does the smart contract handle split payments? Does Co-seller need a connected wallet too?

16. **Partial Payment Failure**
    Payment should split between Affiliator and Co-seller. One wallet receives funds, the other fails. Order is partially complete. Who is responsible for fulfillment? What happens to the customer?

17. **Crypto Payment Network Mismatch**
    Merchant connects wallet on Polygon. Customer tries to pay on Ethereum. Merchant doesn't have Ethereum wallet connected. Do we block payment? Allow but merchant receives credit instead? Auto-convert between networks?

---

## Order Cancellation + Crypto Scenarios

18. **Cancelled Order with Crypto Payment**
    Customer pays with crypto (confirmed on-chain). Order is cancelled. Funds already transferred to merchant wallet. How do we recover money for refund? What if merchant already withdrew it?

19. **Cancelled Affiliate Order**
    Affiliate order is cancelled. Co-seller already received $15 commission credit. Affiliator already received $85 credit. How do we reverse these credits? What if they already spent the credits?

20. **Partial Refund for Crypto Payment**
    Customer paid with crypto (confirmed on-chain). Customer only wants to return 1 of 3 items. How is partial refund handled when original payment was crypto and irreversible?

21. **Dispute with On-Chain Settlement**
    Customer claims product never arrived. Payment was crypto and already settled to merchant's wallet. Platform cannot reverse crypto transaction. Who bears the loss? Platform or merchant?

22. **Unshipped Product with On-Chain Settlement**
    Order is paid and funds are split on-chain immediately, but the product is never shipped. What happens? Merchant has the money but won't ship. Customer has no product. How do we protect the customer?

---

## Onchain Inventory + Affiliate Scenarios

23. **Recorded Product Added to Affiliate**
    Product is recorded on blockchain (becomes read-only), then added to Affiliate. Recorded products cannot be edited. But Affiliator may want to change commission rate later. Can commission be changed independently?

24. **Affiliated Product Then Recorded**
    Product is affiliated with 5 Co-sellers selling it. Then Affiliator records it on blockchain. Recording makes product read-only. Can Affiliator still change commission for Co-sellers? Or is everything locked?

25. **Wallet Lock Conflicts**
    Onchain Inventory requires wallet lock per network after first recording. But merchant wants to use different wallets for different features. How do we manage wallet permissions across features?

26. **NFT Verification on Co-seller Shop**
    Recorded products show NFT verification link on storefront. But affiliate products display in Co-seller's shop. Should customers see the NFT verification link on the Co-seller's shop page?

27. **Recorded Product Exported to Wallet**
    If someone records a product, who owns it? What if they affiliate it? If a recorded product is exported to a wallet, should it still be available for sale or affiliation? What happens to Co-sellers already selling this product?

---

## Onchain Inventory Scenarios

28. **Bulk Recording Fee Ownership**
    If someone records products in bulk (100 products at once), who owns the fee? Is there a bulk discount mechanism? If recording fails halfway, who pays for the failed transactions?

29. **Wallet Lock on Multiple Networks**
    Merchant records product on Polygon with Wallet A. Wallet is now locked for Polygon. Merchant tries to record on Ethereum with Wallet B. Is this allowed? Or are wallets locked across all networks?

30. **Product Edit After Recording**
    Product is recorded and becomes read-only. Merchant discovers a typo in product description. Product cannot be edited. Should we allow "re-recording" with corrections? What happens to the original NFT?

31. **Network Congestion During Recording**
    Merchant records product on Ethereum. Gas price spikes during transaction. Transaction is stuck pending for hours. Merchant tries to record another product. Wallet lock requires same wallet. But previous transaction hasn't completed. Can merchant start new recording?

32. **Insufficient Gas Balance**
    Merchant tries to record product. Gas fee is 0.05 ETH. Merchant wallet only has 0.01 ETH. Do we check this before starting? What happens if they approve but don't have enough gas?

---

## Payment Provider + Currency Scenarios

33. **Currency Change Disables Payment Method**
    Merchant has PayPal and Stripe connected. Changes currency to TRY (Turkish Lira). PayPal doesn't support TRY. Should we: a) Block currency change? b) Auto-disable PayPal with warning? c) Allow but show error at checkout?

34. **Checkout with Disabled Payment Method**
    Merchant changed currency, which disabled PayPal. But PayPal is still showing as connected in settings. Customer tries to checkout with PayPal. What error message do they see? Is it clear why PayPal is unavailable?

35. **Stripe Currency Limitations**
    Merchant in India connects Stripe. Shop currency is INR. But some features require USD. Can Stripe handle both? Or do we need separate payment flows for different features?

---

## Affiliate Network Scenarios

36. **Commission Rate Change Timing**
    Affiliator changes commission from 15% to 5%. Co-seller has product in their cart. Customer checks out 1 minute after change. Which commission applies? 15% or 5%? What about items already in customer's cart from yesterday?

37. **Affiliator Disables Affiliate**
    Affiliator has 50 Co-sellers selling their products. Affiliator disables affiliate network. Products become "Out of Stock" for Co-sellers. What happens to pending orders? What about orders in progress?

38. **Affiliator Deletes Product**
    Affiliator deletes a product that 10 Co-sellers are selling. Product is removed from all Co-seller shops. What happens to customers who have this product in their cart? Do we remove it automatically? Show error at checkout?

39. **Affiliator Changes Price**
    Affiliator changes product price from $100 to $120. Price syncs to Co-seller shops. Customer adds to cart at new price. Affiliator changes price again to $130 before customer checks out. Customer checks out - what price applies? What commission applies?

40. **Co-seller Removes Import**
    Co-seller removes imported product from their shop. Product disappears from their storefront. But a customer has the product in their cart. What happens when customer tries to checkout?

41. **Role Across Multiple Shops**
    Merchant is Affiliator in Shop A. Merchant creates Shop B. Can they be Co-seller in Shop B? Or is role tied to user account (permanent across all shops)?

42. **Affiliator Tries to Import**
    Affiliator sees a good product on marketplace and wants to import it. Affiliators cannot import (permanent role). But what if they really want that product? Is there any workaround? Should we allow exceptions?

---

## Mixed Cart Scenarios

43. **Coupon with Mixed Cart**
    Cart contains regular product (coupon allowed) + affiliate product (coupon blocked) + recorded product. Customer tries to apply coupon. Current behavior blocks entire coupon. Is this clear to customer why coupon doesn't work?

44. **Mixed Cart Checkout Flow**
    Cart has regular product + affiliate product + POD product. Order needs to split between: regular (merchant keeps all), affiliate (commission split), POD (Printful cost). How does checkout handle 3 different payout flows in one order?

45. **Shipping Calculation Mixed Cart**
    Cart has physical product (shipping from merchant) + POD product (shipping from Printful) + affiliate product (shipping from Affiliator). How is shipping calculated and displayed? Who pays for shipping in each case?

---

## Order Processing Scenarios

46. **Merchant Credit Insufficient**
    Order creates negative merchant share (merchant owes platform $10). Merchant only has $5 credit. Order creation is blocked. Customer sees error at checkout. How do we explain this to customer without exposing internal credit system?

47. **Order with Zero Total**
    Cart total is $0 after discounts. Payment step is skipped. Order is confirmed immediately. But affiliate commission is $0 too. Co-seller gets nothing. Is this fair? Should we allow $0 orders for affiliate products?

48. **Payment Method Change During Checkout**
    Customer starts checkout with Stripe. Changes mind and switches to PayPal. Commission calculations might differ (different fees). Do we recalculate? What if merchant would get negative share with PayPal fees?

49. **Affiliate Order Customer Info**
    Affiliate order shows customer shipping info to Affiliator (who ships). But Co-seller sees order without customer details. What if Co-seller needs to contact customer about something? Should they see customer info too?

50. **Fulfillment Status Sync**
    Affiliator marks order as "shipped". Co-seller sees the same order in their dashboard. Does status sync? What if Affiliator never updates status? Co-seller can't do anything about it.

---

## Commission Calculation Scenarios

51. **Commission on Discounted Price**
    Product price is $100. Commission is 15%. Customer applies 20% coupon. Final price is $80. Is commission calculated on $100 (original) or $80 (discounted)? Document says original price, but is this fair to Co-seller?

52. **Referral Share + Affiliate**
    Shop has referral program (3% of profit) AND affiliate product (15% commission). Order has both. Customer was referred by someone. Who gets what? Is referral calculated on full amount or after affiliate commission?

53. **Droplinked Commission Stacking**
    Droplinked takes 1% commission on all orders. Plus payment fees (3% Stripe). Plus affiliate commission (15%). Plus referral (3%). That's 22% total. Merchant might end up with very little. Do we show merchant this breakdown before they activate affiliate?

54. **Negative Commission Edge Case**
    Product costs $10 to make (POD). Sells for $12. Commission is 15% = $1.80. But product only made $2 profit. Co-seller gets $1.80, Affiliator gets $10.20. Affiliator loses money on sale. Should we block such low-margin products from affiliate?

---

## Technical Edge Cases

55. **Browser Close During Recording**
    User starts recording product on blockchain. Progress shows "Step 3 of 5". User closes browser. Transaction is pending on chain. User comes back later. How do we recover the state? Do we check blockchain for pending tx?

56. **Transaction Failure After Broadcast**
    Recording transaction is broadcast to network. Then fails on-chain (out of gas, contract error). User already paid gas fee. Transaction failed. Product is not recorded. User wants to retry. Do they pay gas again?

57. **Double Recording Attempt**
    User accidentally clicks "Record" twice quickly. Two transactions are submitted. Both could succeed (depending on timing). Product would be recorded twice on blockchain. How do we prevent this? Check before broadcasting?

58. **Product Already Recorded on Same Network**
    Merchant tries to record a product that's already recorded on Polygon. Current spec says "based on business rule". Should we allow re-recording (update)? Or block (prevent duplicates)?

59. **Wallet Disconnected During Recording**
    User is recording on external wallet network. Wallet gets disconnected during step 2. Transaction can't be signed. What happens? Do we pause? Timeout? Allow retry with reconnected wallet?

60. **Network Unavailable**
    User selects Ethereum network. But Ethereum nodes are down/unreachable. Should we show network status indicator? Disable that option? Or show error when they try to record?

---

## Credit & Payout Scenarios

61. **Credit vs Payout Decision**
    Merchant has Stripe connected. Order comes in. Should merchant share go directly to Stripe (payout) or to credit first? What if merchant prefers credit to accumulate? Do we give them a choice?

62. **Coinbase Commerce Default**
    Coinbase Commerce payments always go to credit (never direct to merchant). But merchant connected crypto wallet expecting immediate settlement. They're confused why crypto payment went to credit. How do we explain this?

63. **Wallet Payment Wrong Chain**
    Customer pays with wallet on Polygon. Merchant has wallet connected but only on Ethereum. Payment arrives in platform wallet. Should we: Convert to credit? Hold until merchant connects Polygon wallet? Auto-convert between chains?

64. **Affiliate Commission in Credit**
    Co-seller gets $15 commission credit. They want to withdraw to bank. Minimum withdrawal is $50. They only have $15. They need to wait for more sales. Is this clear to them when they sign up as Co-seller?

---

## Notification & Communication Scenarios

65. **Currency Change Notification**
    Merchant changes currency. This affects: pricing display, credit balance, payment methods, affiliate eligibility. Do we send one summary email? Or multiple notifications for each impact?

66. **Commission Change Notification**
    Affiliator changes commission from 15% to 10%. 20 Co-sellers are affected. Do we notify all Co-sellers? What if they don't check email? Should we show warning in their dashboard?

67. **Product Removal Notification**
    Affiliator deletes product. Co-sellers lose it from their shop. Do we notify affected Co-sellers? What about customers who had it in cart - do we email them?

68. **Failed Recording Notification**
    Product recording fails on blockchain. Gas was spent but no NFT created. How do we notify merchant? Do we explain technical details? Or just say "failed, try again"?

---

**Total Scenarios: 68**

**Next Step:** Review with team and prioritize which scenarios need immediate answers before development continues.
