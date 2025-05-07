# Feature Name: Change Log*
**Version number:** 2.0.0  
**Date created/updated:** 2025/5/6  



## 1. Overview:
**Change Log** is designed to allow admins to create and manage version-specific logs that detail updates and changes made in each release of the platform. This feature addresses the need for clear communication of updates, ensuring that users are aware of new features, bug fixes, and improvements. The goal is to provide transparency and keep users informed, helping them better understand how the platform evolves over time.



## 2. User Story:
- **As a** support team member or developer,  
- **I want to** create a new version log with update descriptions,  
- **So that I can** ensure users can view the latest platform changes in the Change Log section of the shop builder.


## 3. Pre-requisites and Flow:

- **Pre-requisites**:
  - The admin must be logged in as a **Super Admin** and have admin access to create and manage logs.
  - Users viewing the Change Log in the shop builder do not require any special permissions.

- **Flow**:
  1. The Super Admin navigates to the **Change Log** section.
  2. The Admin clicks on the **Add Log** button.
  3. The Admin enters the following mandatory details:
     - **Title**
     - **Description**
     - **Summary**
     - **Tags**
     - **Date**
     - **Version Number**
  4. The Admin clicks **Create** to save the log.
  5. The newly created log appears in the list of version logs within the **Shop Builder** under the **Change Log** section.


## 4. Inputs:
- **User Inputs**:
  - **Title**: The title of the update or version log. (Required)
  - **Description**: A detailed description of the changes or updates in the new version. (Required)
  - **Summary**: A brief summary of the changes in the version. (Required)
  - **Tags**: Tags or keywords that help categorize the version log. (Required)
  - **Date**: The date of the update or version release. (Required, date format: YYYY-MM-DD)
  - **Version Number**: The version number of the update (e.g., 1.0.0). (Required)


## 5. Logic and Process:
The feature follows the logic outlined below:
1. **Log Creation**: The admin enters the required information (Title, Description, Summary, Tags, Date, and Version Number) and clicks **Create**.
2. **Log Storage**: The new log is saved in the system, associated with the specific version and date provided.
3. **Display in Shop Builder**: The logs are displayed in the **Change Log** section of the Shop Builder, sorted by date.
4. **Log Access**: Users can click on any log to view its detailed page, which displays the full description, version number, and other details of the selected log.


## 6. Outputs:
The feature produces the following outputs:
- **Version Log List**: The newly created log is added to the list of version logs in the **Change Log** section of the Shop Builder, sorted by date.
- **Log Details Page**: When a user clicks on a version log, the detailed page for that log is displayed, showing the full description, version number, tags, and other details.
- **Confirmation**: A confirmation message or indication that the log has been successfully created and saved.


## 8. Testing and Validation:
- **Test Case 1**: Admin logs in as Super Admin, navigates to the Change Log section, and successfully creates a new log with all required fields.
- **Test Case 2**: Verify that the new log appears in the list of version logs in the Shop Builder, sorted by the correct date.
- **Test Case 3**: Verify that when clicking on any version log, the detailed page for that log is displayed with all the entered details (Title, Description, Summary, Tags, Date, Version Number).
- **Test Case 4**: Admin attempts to create a log without entering all required fields and receives an error message prompting them to fill out all required fields.
- **Test Case 5**: Verify that the logs are properly displayed in the **Change Log** section of the Shop Builder and are accessible to users without admin access.


## 9. Change Log:
- **2025-05-06**: Initial version of the **Change Log** feature created, allowing admins to create version logs that are displayed in the Shop Builder's Change Log section.
