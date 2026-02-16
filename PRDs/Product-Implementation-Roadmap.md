# Droplinked Product Implementation Roadmap (Updated)

---

**Version:** 1.1  
**Date:** 2026-02-16  
**Status:** Draft - Updated with User Feedback  

---

## Executive Summary

This roadmap outlines the implementation of 11 major features across 4 phases over 16 weeks. This version includes updates based on user feedback:

1. **PRD-001** renamed to "Store Setup Wizard (Dashboard)" - always visible until completion
2. **PRD-005** simplified - merchant toggle in settings, automatic handling
3. **PRD-006** enhanced - multiple wallets per network with percentage splits
4. **PRD-007** clarified - currency set in settings (not onboarding), subscriptions in USD

---

## Updated Feature List

| ID | Feature | Priority | Phase | Est. Effort | Key Changes |
|----|---------|----------|-------|-------------|-------------|
| PRD-007 | Shop Currency Configuration | P0 | Phase 1 | 2 weeks | Set in settings, NOT onboarding |
| PRD-011 | Noncustodial Wallet Management | P0 | Phase 1 | 2 weeks | - |
| PRD-009 | Bulk Product Import | P1 | Phase 2 | 2 weeks | - |
| PRD-004 | Full Store Page Customization | P1 | Phase 2 | 2 weeks | - |
| PRD-001 | Store Setup Wizard (Dashboard) | P1 | Phase 2 | 1 week | Renamed, always visible |
| PRD-008 | Affiliate Network | P1 | Phase 3 | 2 weeks | - |
| PRD-006 | Direct Crypto Settlement | P1 | Phase 3 | 2 weeks | Multi-wallet % splits |
| PRD-010 | Onchain Inventory | P1 | Phase 3 | 2 weeks | - |
| PRD-005 | Local Currency Display | P2 | Phase 4 | 1 week | Toggle in settings only |
| PRD-003 | AEO and SEO Discovery | P2 | Phase 4 | 2 weeks | - |
| PRD-002 | Remove Trial Plan | P2 | Phase 4 | 1 week | - |

---

## Key Changes Summary

### 1. Store Setup Wizard (PRD-001) - RENAMED
**Old:** Merchant Dashboard Onboarding  
**New:** Store Setup Wizard (Dashboard)

**Changes:**
- Always visible on dashboard until 100% complete
- Cannot be dismissed before completion
- Shows as persistent widget on dashboard
- After completion, shows as minimized badge
- Accessible via Settings → Store Setup

### 2. Local Currency Display (PRD-005) - SIMPLIFIED
**Changes:**
- Merchant only enables/disables in settings (no individual currency selection)
- System automatically handles based on customer IP/region
- Shows dual display: Shop Currency + Customer Local Currency
- No manual currency selector dropdown

### 3. Direct Crypto Settlement (PRD-006) - ENHANCED
**Changes:**
- Support multiple wallets per network (up to 5)
- Percentage-based splits between wallets (e.g., 70%/30%)
- Configure by token (USDC, USDT) or by bucket (all tokens)
- Smart contract handles automatic splits

### 4. Shop Currency Configuration (PRD-007) - CLARIFIED
**Changes:**
- Currency is set in Shop Settings (NOT during onboarding/registration)
- Platform subscription plans remain in USD regardless of shop currency
- Clear separation between shop currency and billing currency

---

## Implementation Phases (Updated)

### Phase 1: Foundation (Weeks 1-4)
**Goal:** Establish core infrastructure

#### Sprint 1 (Weeks 1-2): Currency System
**Feature:** PRD-007 - Shop Currency Configuration

**Key Deliverables:**
- Currency selection in Shop Settings (NOT onboarding)
- 50+ supported currencies
- Payment provider compatibility checking
- Currency lock after first product
- **Note:** Subscription billing remains in USD

#### Sprint 2 (Weeks 3-4): Wallet Infrastructure
**Feature:** PRD-011 - Noncustodial Wallet Management

**Key Deliverables:**
- Circle and Xion wallet creation
- Multi-network support
- Wallet dashboard with balances
- Deposit/withdrawal functionality

---

### Phase 2: Core Commerce (Weeks 5-8)
**Goal:** Improve merchant productivity

#### Sprint 3 (Weeks 5-6): Productivity Tools
**Features:** 
- PRD-009 - Bulk Product Import
- PRD-001 - Store Setup Wizard (Dashboard) *[RENAMED]*

**PRD-001 Key Changes:**
- Persistent dashboard widget
- Always visible until completion
- Cannot dismiss before 100%
- Minimizes to badge after completion

#### Sprint 4 (Weeks 7-8): Store Customization
**Feature:** PRD-004 - Full Store Page Customization

---

### Phase 3: Advanced Features (Weeks 9-14)
**Goal:** Web3 commerce and affiliate network

#### Sprint 5 (Weeks 9-10): Affiliate Network
**Feature:** PRD-008 - Affiliate Feature

#### Sprint 6-7 (Weeks 11-14): Web3 Commerce
**Features:**
- PRD-006 - Direct Crypto Settlement *[ENHANCED]*
- PRD-010 - Onchain Inventory

**PRD-006 Key Changes:**
- Multiple wallets per network
- Percentage-based splits
- Token-specific or bucket configuration
- Up to 5 wallets per network

---

### Phase 4: Enhancement (Weeks 15-18)
**Goal:** Customer experience improvements

#### Sprint 8 (Weeks 15-16): Customer Experience
**Features:**
- PRD-005 - Local Currency Display *[SIMPLIFIED]*
- PRD-003 - AEO and SEO Discovery
- PRD-002 - Remove Trial Plan

**PRD-005 Key Changes:**
- Simple toggle in settings
- Automatic handling by system
- Dual currency display
- No manual customer selector

---

## Critical Dependencies Map

```
Phase 1: Foundation
├─ PRD-007 (Currency in Settings) ────────────┐
│                                              │
└─ PRD-011 (Wallets) ─────────────────────────┤
                                              │
Phase 2: Core Commerce                        │
├─ PRD-009 (Bulk Import)                      │
├─ PRD-004 (Page Builder)                     │
└─ PRD-001 (Setup Wizard - persistent)        │
                                              │
Phase 3: Advanced Features                    │
├─ PRD-008 (Affiliate) ───── requires PRD-007│
├─ PRD-006 (Crypto - multi-wallet)            │
│            requires PRD-011                 │
└─ PRD-010 (Onchain) ─────── requires PRD-011│
                                              │
Phase 4: Enhancement                          │
├─ PRD-005 (Local Currency - toggle)          │
├─ PRD-003 (AEO/SEO)                          │
└─ PRD-002 (Remove Trial)                     │
```

---

## Technical Notes for Changes

### Store Setup Wizard Implementation
```javascript
// Persistent dashboard widget
if (!setupComplete) {
  showWizardWidget(expanded: true, dismissable: false);
} else {
  showWizardWidget(expanded: false, minimized: true);
}
```

### Multi-Wallet Settlement Logic
```solidity
// Smart contract for percentage splits
function distributePayment(
  address[] memory wallets,
  uint256[] memory percentages, // Must sum to 100
  uint256 totalAmount
) internal {
  for (uint i = 0; i < wallets.length; i++) {
    uint256 share = (totalAmount * percentages[i]) / 100;
    payable(wallets[i]).transfer(share);
  }
}
```

### Local Currency Auto-Handling
```javascript
// Merchant settings
localCurrencyEnabled: boolean; // Simple toggle

// Shopfront logic
if (localCurrencyEnabled) {
  const customerRegion = detectIPLocation();
  const localCurrency = getCurrencyForRegion(customerRegion);
  displayDualPrice(baseCurrency, localCurrency);
}
```

---

## Success Metrics

### PRD-001 (Store Setup Wizard)
- Dashboard visibility: 100% until completion
- Completion rate: > 80%
- Average completion time: < 10 minutes

### PRD-005 (Local Currency)
- Merchant adoption: > 30% enable the feature
- Customer engagement: +15% international conversions
- Support tickets: < 5% related to currency confusion

### PRD-006 (Multi-Wallet Settlement)
- Multi-wallet adoption: > 20% of crypto merchants
- Settlement success rate: > 98%
- Average wallets per merchant: 2.5

### PRD-007 (Currency in Settings)
- Currency set before first product: 100%
- Post-lock change attempts: < 1% (validation working)
- Support tickets: Minimal confusion about USD subscriptions

---

## Risk Mitigation for Changes

### Store Setup Wizard Persistence
**Risk:** Merchants find persistent widget annoying
**Mitigation:** 
- Keep it compact/minimizable
- Celebrate completion
- Option to hide after 30 days if incomplete

### Multi-Wallet Complexity
**Risk:** Configuration errors in percentage splits
**Mitigation:**
- Validate total = 100% before saving
- Visual percentage slider
- Test transaction option (small amount)
- Clear error messages

### Local Currency Auto-Detection
**Risk:** VPN users see wrong currency
**Mitigation:**
- Show both currencies (shop + detected)
- "Approximate" disclaimer
- Override possible (if we add selector later)

---

## Next Steps

1. ✅ Update all PRD documents with changes
2. ⏳ Review changes with engineering team
3. ⏳ Update technical architecture diagrams
4. ⏳ Begin Sprint 1 development
5. ⏳ Set up feature flags for gradual rollout

---

**Document History:**
- v1.0 (2026-02-16): Initial roadmap
- v1.1 (2026-02-16): Updated with user feedback
  - Renamed PRD-001
  - Simplified PRD-005
  - Enhanced PRD-006
  - Clarified PRD-007
