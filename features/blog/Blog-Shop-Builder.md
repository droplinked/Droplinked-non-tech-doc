# Feature Name: Blog Shop Builder
**Version number:** 2.0.0  
**Date created/updated:** 2025/5/6  

## 1. Overview:

**Blog Shop Builder** is designed to enable merchants to create blogs directly from the Shop Builder panel. This feature addresses the need for merchants to have an integrated blog creation and content management system within the shop. It allows merchants to not only create blogs but also add content and have it automatically displayed in the blog list on the Shop Front. The goal is to provide merchants with additional options to enhance their storefronts and engage customers with dynamic content.

## 2. User Story:

- **As a** merchant,  
- **I want to** enter the Shop Builder, go to the "Blogs" section via the sidebar, click on the "Create" button, and provide a title, content, main image, author name, and tags,  
- **So that I can** publish my blog and have it displayed on the Shop Front for customers to read.

## 3. Pre-requisites and Flow:

- **Pre-requisites**:
  - The merchant must be registered and logged in to the Shop Builder platform.

- **Flow**:
  1. The merchant logs in to the Shop Builder.
  2. From the sidebar, the merchant navigates to the **Style Center** and clicks on **Blog Editor**.
  3. The merchant sees a list of existing blogs.
  4. The merchant clicks on **New Post** to create a new blog.
  5. In the **Create Post** page, the merchant:
     - Enters the **Title** of the blog.
     - Adds content in the **Body** section.
     - Writes a **Search Engine Summary** (currently saved for future use).
     - Selects or types a **Category**.
     - Adds **Keywords** (one or more).
     - Uploads a **Featured Picture**.
     - Chooses the **Visibility Status** (Draft or Visible).
     - Toggles the **Feature Option** (active or inactive).
  6. Finally, the merchant clicks **Publish** to create the blog.

## 4. Inputs:

- **User Inputs**:
  - **Title**: The title of the blog post.
  - **Body Content**: The main content of the blog.
  - **Search Engine Summary**: Text for the SEO summary (currently saved for future use).
  - **Category**: The category of the blog (merchant can either select or type it).
  - **Keywords**: One or more keywords related to the blog.
  - **Featured Picture**: The image uploaded as the main blog image.
  - **Visibility Status**: Whether the blog is in draft or visible state.
  - **Feature Option**: Whether the blog is marked as featured or not.

## 5. Logic and Process:

The feature follows the logic outlined below:

1. When the merchant enters the blog creation page:
   - The merchant is presented with the blog creation inputs.
   - If the merchant is editing an existing blog, the system pre-fills the fields with the previous blog data.

2. Data validation:
   - **Search Engine Summary** is optional and not mandatory.
   - **Category** and **Keywords** fields are required.

3. Once the merchant hits **Publish**:
   - The blog is created and saved.
   - The blog becomes visible on the Shop Front for customers to read.
   - The blog appears in the list of blogs in the Shop Builder, allowing the merchant to edit or delete the blog later.


## 6. Outputs:

The feature produces the following outputs:

- **Created Blog**: The blog is created and displayed on the Shop Front for customers to read.
- **Updated Blog List**: The blog appears in the merchant’s list of blogs in the Shop Builder, allowing for future edits or deletion.
- **Visibility Update**: 
  - If the blog's visibility status is set to **Draft**, it will only be visible in the Shop Builder and not on the Shop Front.
  - If the blog's visibility status is set to **Visible**, it will be displayed on the Shop Front for customers to read.
- **Featured Status**: If the blog is marked as featured, it will appear in a separate **Featured** section on the Shop Front for customers to view.


## 7. Error Handling and Troubleshooting:

- **Error 1**: Missing required fields (Category, Keywords)
  - **Solution**: The merchant will receive an error message if they attempt to save or publish the blog with any required fields (Category, Keywords) left empty. Ensure all mandatory fields are filled before proceeding.

- **Error 2**: Invalid image upload for the Featured Picture
  - **Solution**: If the merchant tries to upload an invalid image (wrong format or size), they will receive an error. Ensure the image meets the platform’s requirements for format and size.

- **Error 3**: Unable to save or publish the blog
  - **Solution**: If the merchant is unable to save or publish the blog, they should contact support for assistance.



## 8. Testing and Validation:

- **Test Case 1**: Verify that a merchant can successfully create a blog with all required fields filled.
  - **Expected Result**: The blog is created successfully and visible on the Shop Front. All the details, including title, body content, category, keywords, and featured picture, are displayed correctly. The blog appears in the merchant's blog list in the Shop Builder.

- **Test Case 2**: Verify that a merchant receives an error when trying to save a blog with missing required fields.
  - **Expected Result**: The system displays an error message indicating that the **Category** and **Keywords** fields are required before the blog can be saved or published.

- **Test Case 3**: Verify that the merchant can upload a valid image for the Featured Picture.
  - **Expected Result**: The image is uploaded successfully without errors, and it is displayed correctly as the featured picture in the blog.

- **Test Case 4**: Verify that the merchant receives an error when uploading an invalid image (wrong format or size).
  - **Expected Result**: The system displays an error message informing the merchant that the uploaded image is invalid due to format or size restrictions.

- **Test Case 5**: Verify that the blog is displayed on the Shop Front when its visibility status is set to **Visible**.
  - **Expected Result**: The blog appears on the Shop Front for customers to read.

- **Test Case 6**: Verify that the blog is not displayed on the Shop Front when its visibility status is set to **Draft**.
  - **Expected Result**: The blog is visible in the merchant's blog list in the Shop Builder but does not appear on the Shop Front.

- **Test Case 7**: Verify that the Featured Blog is shown in the **Featured** section on the Shop Front when marked as featured.
  - **Expected Result**: The blog is displayed in a separate **Featured** section on the Shop Front for customers to see.

- **Test Case 8**: Verify that the merchant can edit or delete a blog from the Shop Builder.
  - **Expected Result**: The merchant can edit the content or delete the blog from the blog list in the Shop Builder, and the changes are reflected accordingly.

- **Test Case 9**: Verify that the merchant is able to save a draft blog without publishing it.
  - **Expected Result**: The blog is saved as a draft, and the merchant can continue editing it later. The draft blog does not appear on the Shop Front.


## 9. Change Log:

- **[6 May 2025]**: Version 2.0.0 - Redesigned the Blog Shop Builder feature. Added the **Featured** section to display featured blogs separately on the Shop Front and incorporated the **Search Engine Summary** field for future use.






