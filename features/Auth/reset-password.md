
# Feature Name: Reset Password

**Version number:** 2.0.0  
**Date created/updated:** 2025/05/27

## 🔍 Description
The **Reset Password** feature allows users to reset their password through a simple process initiated from the login page. When a user forgets their password, they can request a reset by entering their email address. A 6-digit code is sent to their email, which the user then enters to verify their identity. After successfully entering the code, the user can set a new password, which will be validated based on certain conditions. Once the password is updated, the user is notified of the success and can proceed to log in with their new password.

## 👤 User Stories
- As a **user**, I want to **reset my password** if I forget it, so that **I can regain access to my account**.
- As a **user**, I want to **receive a confirmation after updating my password**, so that **I know my password has been successfully changed**.

## 🧠 Business Logic

### Step 1: **Accessing the Reset Password Page**
- **Logic**: On the login page, the user clicks on the "Reset Password" button to start the reset process.
- **Rules**: 
  - The "Reset Password" button is visible on the login page.
  - Clicking the button navigates the user to the Reset Password page.
- **Validation**: The button should redirect to the Reset Password page.
- **Outcome**: The user is taken to the Reset Password page.

### Step 2: **Entering the Email Address**
- **Logic**: On the Reset Password page, the user is prompted to enter their email address.
- **Rules**: 
  - The input field must only accept a valid email format.
  - The user must click "Continue" after entering their email address.
- **Validation**: The email format is validated before allowing the user to proceed.
- **Outcome**: The user successfully enters their email address and clicks "Continue".

### Step 3: **Sending the 6-Digit Code**
- **Logic**: Once the email is entered and validated, a 6-digit code is sent to the user’s email address.
- **Rules**: 
  - The code is unique to the user’s email and can be used only once.
  - The code expires after a set period (e.g., 10 minutes).
- **Validation**: The email address must exist in the system for the code to be sent.
- **Outcome**: The user receives the 6-digit code via email.

### Step 4: **Entering the 6-Digit Code**
- **Logic**: The user enters the 6-digit code received in their email on the same Reset Password page.
- **Rules**: 
  - The code must match the one sent to the user’s email.
  - The system checks if the code is valid and within the expiration time.
- **Validation**: The code is verified before allowing the user to proceed.
- **Outcome**: If the code is correct, the user is redirected to the next step to enter the new password.

### Step 5: **Setting the New Password**
- **Logic**: After the code is successfully verified, the user is prompted to set a new password and confirm it by entering it again.
- **Rules**: 
  - The new password must meet the system’s security criteria (e.g., minimum length, contains a special character, etc.).
  - The password confirmation field must match the new password field.
- **Validation**: 
  - The password must meet all security criteria.
  - The confirmation password must match the original new password.
- **Outcome**: The user successfully sets a new password.

### Step 6: **Confirming Password Change**
- **Logic**: Once the new password is successfully set, a confirmation page appears, informing the user that their password has been successfully changed.
- **Rules**: 
  - A "Login" button is displayed on this page to allow the user to log in with their new password.
- **Validation**: No further validation required at this point.
- **Outcome**: The user sees a confirmation message and can click "Login" to go to the login page.

### Step 7: **Logging in with New Password**
- **Logic**: After clicking the "Login" button, the user is redirected to the login page.
- **Rules**: 
  - The user can now log in using the newly updated password.
- **Validation**: The user’s credentials are verified during the login process.
- **Outcome**: The user successfully logs in with their new password.

## 📌 Change Log

| Version | Date       | Change Summary         | Author     |
|---------|------------|-------------------------|------------|
| 2.0.0     | 2025-06-08 | Initial version created | Behdad  |

## 🔗 Dependencies
- **Login Page**: This feature is dependent on the login page for initiating the password reset process.

## 🧪 Test Cases
- **Test Case ID**: Reset Password
