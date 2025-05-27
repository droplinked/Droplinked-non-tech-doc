# Feature Name: Signup Onboarding

**Version number:** 1.0.0  
**Date created/updated:** 2025/05/27


## 1. Overview:
Sign Up and Onboarding is designed to allow new users to register as merchants on the platform through a guided multi-step process. This feature addresses the mandatory registration process while also providing an onboarding flow that helps users complete essential setup fields and better understand the system before fully registering. It is intended to improve merchants’ understanding of the dashboard and available features, making it easier for them to configure their settings upfront rather than searching through settings after registration. The goal is to provide a smooth and guided merchant onboarding experience for new users.


## 2. User Story:
- **As a** new merchant,  
- **I want to** register, confirm my email, and complete the required initial settings through onboarding,  
- **So that I can** easily understand and complete mandatory steps upfront, making registration smoother and avoiding confusion or having to search for these settings later inside the dashboard.


## 3. Pre-requisites and Flow:

- **Pre-requisites**:
  - The user must successfully complete the signup process and have their registration confirmed.

- **Flow**:
  1. **Step 1**: The user clicks the signup button.
  2. **Step 2**: The user fills in the signup form and submits it.
  3. **Step 3**: The user is redirected to the email confirmation page where they enter a 6-digit code sent to their email.
  4. **Step 4**: Upon successful email confirmation, the user is taken to the merchant onboarding process.
  5. **Step 5**: The user sets up shop details including shop name, logo, and URL, with optional assistance from AI design tools.
  6. **Step 6**: The user enters basic payment details.
  7. **Step 7**: The user selects desired plans from available options.
  8. **Step 8**: The user configures social media integrations.
  9. **Step 9**: The user watches a 1-minute onboarding video.
  10. **Step 10**: The onboarding process completes and the user is redirected to the dashboard.



## 4. Inputs:
- **User Inputs**:
  - During registration: email, password, and referral code.
  - During email confirmation: a 6-digit verification code.
  - During onboarding: mandatory shop information including shop name and URL.


## 5. Logic and Process:
The feature follows the logic outlined below:
1. **Validate Registration Inputs**: The system checks the email format, password strength, and validity of the referral code.
2. **Send Verification Code**: Upon successful registration submission, a 6-digit code is sent to the user’s email.
3. **Verify Email Confirmation**: The user must enter the 6-digit code received via email. The system verifies the code matches the sent code.
4. **Prevent Progress Without Verification**: The user cannot proceed to onboarding without successfully confirming their email.
5. **Onboarding Mandatory Fields**: During onboarding, the user is required to enter mandatory shop details such as shop name and URL before proceeding.
6. **Conditional Handling**:
   - If the verification code is incorrect or expired, the user is prompted to re-enter or request a new code.
   - If mandatory onboarding fields are missing, the system prevents the user from moving forward and displays appropriate error messages.
7. **Completion**: Upon successful completion of all onboarding steps, the user is redirected to the dashboard.


## 6. Outputs:
- Creation of user account and associated merchant profile in the system database.

## 7. Change Log:
- **2025-05-27**: Initial creation of the Sign Up and Onboarding feature documentation.


## 8. Limitations and Notes:
- **Limitations**:  
  - If the user logs out during the onboarding process before entering the mandatory shop name and URL, the onboarding flow breaks and the user may not be able to resume smoothly.
- **Notes**:  
  - Developers should consider implementing session persistence or checkpoints to prevent flow interruption due to unexpected logout during onboarding.
