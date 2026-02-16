# PRD-001: Store Setup Wizard (Dashboard)

---

## Feature Specification


### Problem Statement

New merchants need a guided setup wizard to complete essential store configuration tasks. The wizard should be prominently displayed on the dashboard and remain visible until all steps are completed. Unlike the registration process, this wizard focuses on store setup tasks that prepare the merchant for selling.

Merchant Logs In → Dashboard Loads
    ↓
Setup Wizard Visible (Persistent Widget/Panel)
    ↓
Merchant Completes Steps Sequentially or Selectively
    ↓
Progress Updates in Real-Time
    ↓
All Steps Complete → Wizard Shows "Setup Complete" with Celebration
    ↓
Wizard Collapses to Minimized State (still accessible)
```

**Journey 2: Persistent Dashboard Access**
```
Merchant Logs In (Any Time)
    ↓
Dashboard Shows Setup Wizard Widget (if incomplete)
    ↓
Or Shows "Setup Complete" Badge (if done)
    ↓
Can Expand/Collapse Wizard Anytime
    ↓
Can Access Full Wizard from Settings → Store Setup
```

### Scope

#### ✅ In Scope:
- Persistent dashboard widget showing setup progress
- Progressive setup checklist (5-7 steps)
- Progress tracking and visualization
- Contextual tooltips and guidance
- Always visible until all steps completed
- Ability to collapse/expand wizard
- Step-by-step guidance for essential setup tasks
- Integration with existing dashboard features
- Mobile-responsive wizard experience
- Settings access for wizard management

#### ❌ Out of Scope:
- Interactive product tours (like walkthrough tooltips)
- Video tutorials or multimedia content
- Gamification elements (badges, points)
- Personalized onboarding based on merchant category
- AI-driven adaptive onboarding paths
- In-app messaging or chat during onboarding
- Email drip campaigns for onboarding

### Acceptance Criteria

- ☑ Setup wizard widget displays on dashboard on every login until 100% complete
- ☑ Checklist contains 5-7 essential setup steps (e.g., complete profile, add first product, connect payment, customize storefront)
- ☑ Each step links directly to the relevant dashboard section
- ☑ Progress bar shows completion percentage (e.g., "3 of 7 completed")
- ☑ Completed steps are visually marked (checkmark or strikethrough)
- ☑ Wizard can be collapsed but remains accessible
- ☑ Once 100% complete, wizard minimizes to compact "Setup Complete" badge
- ☑ Merchant can access full wizard anytime via Settings → Store Setup
- ☑ Wizard state persists across sessions
- ☑ Progress saves automatically as steps are completed
- ☑ Mobile-responsive design for wizard components
- ☑ Wizard completes when all steps are done (no manual dismiss before completion)

### Technical Notes

- Store onboarding progress in merchant profile/settings database
- Use localStorage as fallback for temporary persistence
- Trigger completion detection by monitoring relevant data changes
- Consider using existing UI components (modals, checklists, progress bars)
- Ensure onboarding doesn't block critical dashboard functionality
- Analytics tracking for onboarding funnel (step completion rates, drop-off points)

### Dependencies

- Merchant profile system
- Product management system
- Payment method configuration
- Storefront customization features
- Settings/preferences storage
- Analytics tracking infrastructure

---
