# AI Assistance Chatbot

### Feature ID:

**CHATBOT-AI-001**

### Title:

**AI Assistance Chatbot – User Support & Escalation**

### Category:

**Support / AI Assistance** | **Actors**: Visitor (non-logged-in), Logged-in User, AI Chatbot, Support Team | **Channel**: Storefront / Merchant Dashboard

### Status:

**Defined**

### Owner:

Behdad

---

### 1) **Summary**

**Problem/Value:**

Users visiting Droplinked stores or dashboards often have questions that can be answered instantly by AI. Without a built-in chatbot, they either leave or overwhelm support staff. At the same time, AI cannot answer every question; when it fails, there must be a smooth hand-off to human support.

**Desired Outcome:**

- A self-service **AI Assistance Chatbot** opens when the user clicks “AI Assistance.”
- Logged-in users are automatically identified by their email; non-logged-in visitors must enter and verify their email first.
- Questions are sent to an AI engine, which returns answers in real time.
- If the AI cannot confidently answer, it asks the user to confirm escalation and then sends the question to support by email, linking the user’s email for follow-up.

---

### 2) **Scope – In / Out**

**In:**

- “AI Assistance” button visible in the interface (storefront and dashboard).
- Chat window opens in-page; supports real-time Q&A.
- **User identification**:
    - Logged-in → use stored verified email automatically.
    - Not logged-in → prompt for email and verify before chat starts.
- **AI response engine** integrated with Droplinked backend.
- **Escalation flow**:
    - AI flags unanswerable/low-confidence questions.
    - Chatbot asks user “Do you want to send this to support?”
    - On confirm, system emails the question + user email to support address.
- Logging:
    - Save conversation transcripts with status (answered by AI vs escalated).
    - Link escalated cases to support ticket ID/email.

**Out:**

- AI training/knowledge-base management (handled separately).
- Real-time human chat (only email escalation in this feature).
- Multilingual support (future phase).

---

### 3) **Key User Journeys**

- **Journey 1 – Logged-In User Opens Chat**
    - **Trigger:** User clicks “AI Assistance” button.
    - **Action:** Chat window opens with greeting; user’s verified email is auto-attached.
    - **Outcome:** User can immediately start chatting with AI.
- **Journey 2 – Non-Logged-In Visitor**
    - **Trigger:** Visitor clicks “AI Assistance.”
    - **Action:** Chatbot prompts for email; user enters and confirms.
    - **Outcome:** After verification, chat starts and email is attached to session.
- **Journey 3 – AI Answers Question**
    - **Trigger:** User sends a message.
    - **Action:** System sends question to AI, receives answer, displays it in chat window.
    - **Outcome:** User receives immediate automated help.
- **Journey 4 – AI Cannot Answer (Escalation)**
    - **Trigger:** AI flags low-confidence or unsupported question.
    - **Action:** Chatbot shows “We may need a human to help — send this to support?”
        
        User clicks “Yes.”
        
        System emails the question + user email to support address (support@…).
        
    - **Outcome:** Support team receives the question; user sees confirmation “Your message has been sent to support.”

---

### 4) **Business Acceptance Criteria (BAC)**

- **BAC 1:** Clicking “AI Assistance” opens the chat window → Pass/Fail.
- **BAC 2:** Logged-in users automatically skip email prompt; non-logged-in users must enter a valid email to continue → Pass/Fail.
- **BAC 3:** Messages typed by the user are relayed to the AI engine and responses are displayed in real time → Pass/Fail.
- **BAC 4:** When AI cannot confidently answer, the chatbot offers escalation; on user confirmation, the question + email are sent to support via email → Pass/Fail.
- **BAC 5:** Support email contains user’s email, original question, and chat context → Pass/Fail.
- **BAC 6:** All conversations and escalations are logged with timestamps and status for audit → Pass/Fail.