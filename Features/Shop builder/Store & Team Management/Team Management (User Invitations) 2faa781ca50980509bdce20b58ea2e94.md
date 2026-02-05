# Team Management (User Invitations)

# Team Management (User Invitations)

### Feature ID:

**[IAA-STM-401]**

### Title:

**Team Management – Invite Members to Store**

### Category:

**Shop Settings** | **Actors**: Merchant (Admin), Merchant (Member) | **Channel**: Web

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants need to collaborate with team members, partners, or staff to manage their store. Currently, only the store creator can access the dashboard. This feature enables merchants to invite additional users to their store, allowing multiple people to manage products, orders, and settings collaboratively.

**Desired Outcome:**

- Admin (store owner) can invite new members to the store via email.
- Invited users receive an email invitation with a link to join.
- Existing Droplinked users can accept and immediately access the store.
- New users must register first, then gain access to the store.
- Admin can view all team members and pending invitations.
- Admin can revoke invitations or remove members (except themselves).
- All members have equal access permissions to the store dashboard.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Team List View**
    - Display table of all members and pending invitations
    - Columns: Email, Date Added, Status, Action
    - Status types: Admin, Member, Pending, Expired
    - Action: Remove/Cancel button (not available for Admin)
- **Invite New Member**
    - Modal with email input
    - Email validation and duplicate check
    - Static warning message about full access
    - Send invitation email
- **Invitation Flow**
    - Email contains unique invitation link
    - Link expires after 24 hours
    - Existing users → Login page → Accept → Access granted
    - New users → Signup page → Register → Access granted
- **Remove/Cancel Actions**
    - Cancel pending invitation
    - Remove active member
    - Admin (owner) cannot be removed

**Out of Scope:**

- Role-based permissions (all members have equal access)
- Resend invitation functionality
- Notification to admin when invitation is accepted
- Transfer of store ownership
- Email notifications on member removal

---

### 3) **Key User Journeys**

---

**Journey 1: Admin Views Team List**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Admin | Navigates to Settings → Team Management | Team list page loads |
| 2 | Admin | Views the team table | Shows all members and invitations |
| 3 | System | Displays Admin's own email | Shown with "Admin" status tag, no action button |
| 4 | System | Displays members | Shown with "Member" status, Remove button available |
| 5 | System | Displays pending invitations | Shown with "Pending" status, Cancel button available |
| 6 | System | Displays expired invitations | Shown with "Expired" status, can be removed or re-invited |

---

**Journey 2: Admin Invites New Member**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Admin | Clicks "New User" button | Invite modal opens |
| 2 | Admin | Views modal | Email input + warning message displayed |
| 3 | Admin | Enters email address | Real-time validation |
| 4 | Admin | Clicks "Send Invitation" | System validates email |
| 5 | System | Email not in team list | Invitation created, email sent |
| 6 | System | Adds to table | New row with "Pending" status appears |
| 7 | Invited User | Receives email | Email contains invitation link |

---

**Journey 3: Existing User Accepts Invitation**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Invited User | Clicks invitation link in email | Redirected to login page |
| 2 | System | Pre-fills email field | Email is read-only |
| 3 | Invited User | Enters password | Validates credentials |
| 4 | System | Credentials valid | Logs in user |
| 5 | System | Processes invitation | Adds user as Member to the store |
| 6 | System | Updates invitation status | Status changes from Pending to Member |
| 7 | Invited User | Sees store in their store list | Can switch to this store anytime |

---

**Journey 4: New User Accepts Invitation**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Invited User | Clicks invitation link in email | Redirected to signup page |
| 2 | System | Pre-fills email field | Email is read-only |
| 3 | Invited User | Enters password + confirms | Creates account |
| 4 | System | Creates new merchant account | Account created |
| 5 | System | Processes invitation | Adds user as Member to the store |
| 6 | System | Redirects to onboarding | User completes their own store setup |
| 7 | Invited User | After onboarding | Has access to both their store AND invited store |

---

**Journey 5: Admin Cancels Pending Invitation**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Admin | Finds pending invitation in table | Sees "Cancel" action button |
| 2 | Admin | Clicks "Cancel" | Confirmation dialog appears |
| 3 | Admin | Confirms cancellation | Invitation invalidated |
| 4 | System | Updates table | Row removed from list |
| 5 | System | Invitation link becomes invalid | If user clicks link, shows error |

---

**Journey 6: Admin Removes Active Member**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Admin | Finds member in table | Sees "Remove" action button |
| 2 | Admin | Clicks "Remove" | Confirmation dialog appears |
| 3 | Admin | Confirms removal | Member access revoked |
| 4 | System | Updates table | Row removed from list |
| 5 | System | Revokes store access | Store removed from member's store list |
| 6 | Removed User | Next login | Cannot see or access this store anymore |

---

**Journey 7: Invitation Expires**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | System | 24 hours pass since invitation sent | Invitation expires |
| 2 | System | Updates status | Status changes to "Expired" |
| 3 | Invited User | Clicks expired link | Error page: "Invitation has expired" |
| 4 | Admin | Views team list | Sees expired invitation |
| 5 | Admin | Can remove expired entry | Or invite the same email again |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Team List** |  |
| BAC-1 | Team list displays columns: Email, Date Added, Status, Action |
| BAC-2 | Admin (store owner) is always shown with "Admin" status |
| BAC-3 | Admin row does NOT have a Remove/Cancel action button |
| BAC-4 | Active members show "Member" status with "Remove" action |
| BAC-5 | Pending invitations show "Pending" status with "Cancel" action |
| BAC-6 | Expired invitations show "Expired" status |
| **Invitation** |  |
| BAC-7 | "New User" button opens invitation modal |
| BAC-8 | Modal displays email input field |
| BAC-9 | Modal displays static warning: "This person will have full access to your store" |
| BAC-10 | Email must be valid format |
| BAC-11 | Email cannot already be in team list (member or pending) |
| BAC-12 | On valid email, invitation is created and email is sent |
| BAC-13 | Invitation link expires after 24 hours |
| **Acceptance Flow** |  |
| BAC-14 | Existing users clicking invitation link go to login page with pre-filled email |
| BAC-15 | New users clicking invitation link go to signup page with pre-filled email |
| BAC-16 | After successful login/signup, user is added as Member to the store |
| BAC-17 | Expired invitation link shows error message |
| **Removal** |  |
| BAC-18 | Canceling invitation invalidates the link immediately |
| BAC-19 | Removing member revokes store access immediately |
| BAC-20 | Removed member can no longer see the store in their store list |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Initial document creation | New Feature |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Admin** | The original creator/owner of the store. Has full access and cannot be removed. |
| **Member** | A user who has been invited and accepted. Has full access (same as Admin). |
| **Pending** | An invitation that has been sent but not yet accepted. |
| **Expired** | An invitation that was not accepted within 24 hours. |
| **Invitation Link** | Unique URL sent via email that allows the recipient to join the store. |
| **Team** | Collective term for Admin + all Members of a store. |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Team List Loading**

```
ON PageLoad(/settings/team-management):
    members = API.getStoreTeam(currentStoreId)

    FOR EACH member IN members:
        IF member.role == 'admin':
            member.status = 'Admin'
            member.canRemove = false
        ELSE IF member.role == 'member':
            member.status = 'Member'
            member.canRemove = true
        ELSE IF member.role == 'pending':
            IF member.invitedAt + 24hours < NOW:
                member.status = 'Expired'
            ELSE:
                member.status = 'Pending'
            member.canRemove = true

    RENDER table(members)

```

---

### **FL-2: Invite Modal Logic**

```
ON ClickNewUser:
    OPEN InviteModal

InviteModal:
    - emailInput (empty)
    - warningText = "This person will have full access to your store"
    - sendButton (disabled initially)

```

---

### **FL-3: Email Validation**

```
ON EmailInputChange(value):
    IF !isValidEmailFormat(value):
        SET error = "Please enter a valid email address"
        SET sendButton.disabled = true
        RETURN

    IF value == currentUser.email:
        SET error = "You cannot invite yourself"
        SET sendButton.disabled = true
        RETURN

    // Check if already in team
    existingMember = API.checkTeamMember(storeId, value)

    IF existingMember.exists AND existingMember.status IN ['admin', 'member']:
        SET error = "This email is already a team member"
        SET sendButton.disabled = true
        RETURN

    IF existingMember.exists AND existingMember.status == 'pending':
        SET error = "An invitation is already pending for this email"
        SET sendButton.disabled = true
        RETURN

    // Valid
    SET error = null
    SET sendButton.disabled = false

```

---

### **FL-4: Send Invitation**

```
ON ClickSendInvitation:
    SET loading = true

    // Check if email is registered user
    userExists = API.checkUserExists(email)

    invitation = {
        store_id: currentStoreId,
        email: email,
        invited_by: currentUser.id,
        invited_at: NOW,
        expires_at: NOW + 24hours,
        status: 'pending',
        token: generateUniqueToken()
    }

    response = API.createInvitation(invitation)

    IF response.success:
        IF userExists:
            SEND email with loginInvitationTemplate(invitation.token)
        ELSE:
            SEND email with signupInvitationTemplate(invitation.token)

        CLOSE modal
        REFRESH teamList
        SHOW success "Invitation sent successfully"
    ELSE:
        SHOW error response.message

    SET loading = false

```

---

### **FL-5: Invitation Link Processing**

```
ON NavigateTo(/invite/{token}):
    invitation = API.getInvitation(token)

    IF invitation == null:
        SHOW error "Invalid invitation link"
        REDIRECT to /login
        RETURN

    IF invitation.status != 'pending':
        SHOW error "This invitation is no longer valid"
        REDIRECT to /login
        RETURN

    IF invitation.expires_at < NOW:
        API.updateInvitation(token, { status: 'expired' })
        SHOW error "This invitation has expired"
        REDIRECT to /login
        RETURN

    // Store token in session for post-auth processing
    SESSION.invitationToken = token

    // Check if user exists
    userExists = API.checkUserExists(invitation.email)

    IF userExists:
        REDIRECT to /login?email={invitation.email}&readonly=true
    ELSE:
        REDIRECT to /signup?email={invitation.email}&readonly=true

```

---

### **FL-6: Post-Authentication Invitation Acceptance**

```
ON SuccessfulLogin OR SuccessfulSignup:
    IF SESSION.invitationToken EXISTS:
        token = SESSION.invitationToken
        invitation = API.getInvitation(token)

        IF invitation AND invitation.status == 'pending':
            // Add user to store
            API.addStoreTeamMember({
                store_id: invitation.store_id,
                user_id: currentUser.id,
                role: 'member',
                added_at: NOW
            })

            // Mark invitation as accepted
            API.updateInvitation(token, {
                status: 'accepted',
                accepted_at: NOW
            })

            CLEAR SESSION.invitationToken

            // Redirect to the invited store
            REDIRECT to /dashboard?store={invitation.store_id}

```

---

### **FL-7: Cancel Invitation**

```
ON ClickCancel(invitationId):
    SHOW confirmDialog("Are you sure you want to cancel this invitation?")

    IF confirmed:
        response = API.cancelInvitation(invitationId)

        IF response.success:
            REFRESH teamList
            SHOW success "Invitation cancelled"
        ELSE:
            SHOW error response.message

```

---

### **FL-8: Remove Member**

```
ON ClickRemove(memberId):
    member = getMemberById(memberId)

    IF member.role == 'admin':
        SHOW error "Cannot remove store owner"
        RETURN

    SHOW confirmDialog("Are you sure you want to remove this member? They will lose access to this store.")

    IF confirmed:
        response = API.removeStoreTeamMember(storeId, memberId)

        IF response.success:
            REFRESH teamList
            SHOW success "Member removed"
        ELSE:
            SHOW error response.message

```

---

### **FL-9: Expired Invitation Handling (Background Job)**

```
CRON JOB (runs every hour):
    expiredInvitations = DB.query(
        "SELECT * FROM invitations
         WHERE status = 'pending'
         AND expires_at < NOW"
    )

    FOR EACH invitation IN expiredInvitations:
        UPDATE invitation SET status = 'expired'

```

---

### B3) Behavioral Flow

```
[Admin Opens Team Management]
    ↓
[Load Team List]
    ↓
[Display Table: Email | Date Added | Status | Action]
    │
    ├─ [Admin Row] → No Action Button
    ├─ [Member Row] → Remove Button
    ├─ [Pending Row] → Cancel Button
    └─ [Expired Row] → Remove Button

[Admin Clicks "New User"]
    ↓
[Modal Opens: Email Input + Warning]
    ↓
[Enter Email] → [Validate]
    │
    ├─ Invalid Format → Error
    ├─ Already Member → Error
    ├─ Pending Exists → Error
    └─ Valid → Enable Send Button

[Click Send]
    ↓
[Create Invitation + Send Email]
    ↓
[Update Table with Pending Status]

---

[Invited User Clicks Link]
    ↓
[Validate Token]
    │
    ├─ Invalid → Error Page
    ├─ Expired → Error Page
    └─ Valid → Continue
        │
        ├─ User Exists → Login Page (email pre-filled)
        └─ New User → Signup Page (email pre-filled)

[After Auth]
    ↓
[Add as Member]
    ↓
[Redirect to Invited Store]

```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| (none) | Invitation created | Pending | Email sent to user |
| Pending | User accepts (login) | Member | Store access granted |
| Pending | User accepts (signup) | Member | Account created, store access granted |
| Pending | 24 hours pass | Expired | Link becomes invalid |
| Pending | Admin cancels | Cancelled | Row removed, link invalid |
| Member | Admin removes | Removed | Store access revoked |
| Expired | Admin removes | Removed | Row removed |
| Expired | Admin invites same email | Pending | New invitation created |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Admin tries to remove themselves | Action button not shown; if API called directly, return error |
| **EC-2:** User clicks expired link | Show "Invitation expired" error, redirect to login |
| **EC-3:** User clicks already-accepted link | Show "Invitation already used" error |
| **EC-4:** User clicks cancelled link | Show "Invalid invitation" error |
| **EC-5:** Admin invites same email that's expired | Allow new invitation (old one stays as expired history) |
| **EC-6:** Invited user already has access to store | Show error "Already a team member" |
| **EC-7:** Network error during invitation send | Show error, allow retry |
| **EC-8:** User signs up with different email than invited | Should not be possible (email field is readonly) |
| **EC-9:** Member removed while logged in | On next API call, session invalidated for that store |
| **EC-10:** Admin account deleted | Store ownership transfer required (out of scope - not supported) |

---

### B6) Data Requirements

**Invitation Table:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Unique invitation ID |
| store_id | UUID | Store being invited to |
| email | String | Invited email address |
| invited_by | UUID | User ID of admin who invited |
| invited_at | DateTime | When invitation was created |
| expires_at | DateTime | When invitation expires (invited_at + 24h) |
| status | Enum | pending, accepted, expired, cancelled |
| token | String | Unique token for invitation link |
| accepted_at | DateTime | When invitation was accepted (nullable) |

**Store Team Member Table:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Unique record ID |
| store_id | UUID | Store ID |
| user_id | UUID | User ID |
| role | Enum | admin, member |
| added_at | DateTime | When user was added to store |

**API Endpoints:**

| Endpoint | Method | Purpose |
| --- | --- | --- |
| `/api/stores/{id}/team` | GET | Get all team members and invitations |
| `/api/stores/{id}/team/invite` | POST | Create new invitation |
| `/api/stores/{id}/team/{memberId}` | DELETE | Remove member |
| `/api/invitations/{token}` | GET | Get invitation details |
| `/api/invitations/{id}/cancel` | POST | Cancel invitation |
| `/api/users/check-email` | POST | Check if email is registered |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Load team list | Shows admin with "Admin" status, no remove button |
| TC-2 | Invite valid new email | Invitation created, email sent, shows as Pending |
| TC-3 | Invite already member email | Error: "Already a team member" |
| TC-4 | Invite already pending email | Error: "Invitation already pending" |
| TC-5 | Invite own email | Error: "Cannot invite yourself" |
| TC-6 | Existing user clicks valid link | Goes to login with pre-filled email |
| TC-7 | New user clicks valid link | Goes to signup with pre-filled email |
| TC-8 | User clicks expired link | Error: "Invitation expired" |
| TC-9 | User clicks cancelled link | Error: "Invalid invitation" |
| TC-10 | Cancel pending invitation | Status changes, link becomes invalid |
| TC-11 | Remove active member | Member loses store access |
| TC-12 | Try to remove admin | Not possible (no button / API returns error) |
| TC-13 | Invitation expires after 24h | Status changes to Expired |
| TC-14 | Invite same email after expiry | New invitation created successfully |
| TC-15 | Removed member tries to access store | Access denied |