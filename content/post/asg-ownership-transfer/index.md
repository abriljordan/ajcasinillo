---
title: Transfer Google Docs/Files Ownership
date: 2025-01-07
lastmod: 2025-01-08
description: Transfer Google Docs/Files Ownership
tags:
    - Google OAuth 2.0
    - OAuth Playground
categories:
    - Google OAuth 2.0
    - OAuth Playground
toc: true
---

## Tutorial: Setting Up Google OAuth 2.0 and Using the OAuth Playground with Node.js

This tutorial guides you through creating a Google Cloud project, setting up OAuth 2.0 credentials, and using the OAuth Playground to obtain a refresh token for Node.js integration.

### Step 1: Create a Google Cloud Project

1. Go to the Google Cloud Console.

2. Click Select a Project > New Project.

3. Enter a Project Name and choose your Organization (if applicable).

4. Click Create and wait for the project to be created.

### Step 2: Set Up OAuth 2.0 Credentials

1. Navigate to the APIs & Services section on the left sidebar and select Credentials.

2. Click Create Credentials > OAuth Client ID.

3. If prompted, configure the OAuth consent screen:
   - Set the User Type to External.
   - Add a suitable App Name.
   - Set the Publishing Status to Testing.
   - Add Test Users by entering their email addresses.
   - Leave Scopes blank for now.
   - Save the configuration.

4. Return to Credentials and continue creating an OAuth client:
   - Select Web Application as the application type.
   - Enter a suitable name.
   - In Authorized Redirect URIs, add:
      ```plaintext
      https://developers.google.com/oauthplayground
      ```
   - Click Create.

5. Copy the Client ID and Client Secret from the generated credentials.

### Step 3: Authorize APIs in the OAuth Playground

1. Visit the OAuth Playground.

2. In the top-right corner, click the Settings icon.
   - Provide the Client ID and Client Secret from the previous step.
   - Check the box for “Use your own OAuth credentials.”
   - Save the settings.

3. On the left side, under Select & authorize APIs, expand the Google Drive API category.
   - Select the scope `https://www.googleapis.com/auth/drive`.
   - Click Authorize APIs.

4. Complete the authorization by signing in with a test user account you added earlier.

5. Click Exchange authorization code for tokens to generate an access token and refresh token.

6. Copy the Refresh Token displayed.

## Tutorial: How to Get the File ID from Google Drive

The File ID is a unique identifier for each file stored in Google Drive. It is required when using Google Drive APIs to manage or manipulate files programmatically.

Using the Google Drive Web Interface

1. Open Google Drive
   Visit Google Drive and log in with your Google account.

2. Locate Your File
   Navigate to the file for which you want to retrieve the File ID.

3. Right-Click and Select “Get Link”
   - Right-click on the file and choose “Get Link”.
   - Ensure the link-sharing settings are configured appropriately (e.g., anyone with the link can view).

4. Copy the File ID
   - The shared link will look something like this:
     ```bash
     https://drive.google.com/file/d/FILE_ID/view?usp=sharing
     ```
   - The portion between /d/ and /view is the File ID.
   For example, if the link is:
   ```bash
   https://drive.google.com/file/d/1MMPSPbuXezEOucdYvbgIsZtiYcdYa7qT4eBdv_gWm3M/view?usp=sharing
   ```
   The File ID is:
   ```bash
   1MMPSPbuXezEOucdYvbgIsZtiYcdYa7qT4eBdv_gWm3M
   ```

## Tutorial: Use Node.js in transferring ownership

#### Install Dependencies

```bash
npm init -y
npm install googleapis
```

#### Node.js Code Example

```javascript
const { google } = require('googleapis');

// OAuth2 Client Setup
const CLIENT_ID = 'your-client-id';
const CLIENT_SECRET = 'your-client-secret';
const REDIRECT_URI = 'https://developers.google.com/oauthplayground';
const REFRESH_TOKEN = 'your-refresh-token';

// File and Owner Details
const FILE_ID = 'your-file-id'; // Replace with your file ID
const NEW_OWNER_EMAIL = 'new-owner-email@example.com'; // Replace with the new owner's email

const oauth2Client = new google.auth.OAuth2(
  CLIENT_ID,
  CLIENT_SECRET,
  REDIRECT_URI
);

// Set credentials with the refresh token
oauth2Client.setCredentials({ refresh_token: REFRESH_TOKEN });

// Google Drive API client setup
const drive = google.drive({ version: 'v3', auth: oauth2Client });

// Function to transfer ownership
async function transferOwnership(fileId, newOwnerEmail) {
  try {
    // Step 1: Check the current permissions of the file
    const res1 = await drive.permissions.list({
      fileId,
      supportsAllDrives: true,
      pageSize: 100,
      fields: "*",
    });

    // Find if the new owner is already in the permissions list
    let permissionId = "";
    const permission = res1.data.permissions.find(
      ({ emailAddress }) => emailAddress === newOwnerEmail
    );

    // If no permission found, create new permission for writer role
    if (permission) {
      permissionId = permission.id;
    } else {
      const { data: { id } } = await drive.permissions.create({
        fileId: fileId,
        sendNotificationEmail: true,
        supportsAllDrives: true,
        requestBody: {
          role: "writer",
          type: "user",
          emailAddress: newOwnerEmail,
        },
      });
      permissionId = id;
    }

    // Step 2: Update the permission to set the new owner as "pendingOwner"
    const res2 = await drive.permissions.update({
      fileId,
      permissionId,
      supportsAllDrives: true,
      requestBody: {
        role: "writer",
        pendingOwner: true,
      },
    });

    console.log('Ownership transfer is pending:', res2.data);
  } catch (error) {
    console.error('Error transferring ownership:', error.message);
  }
}

// Main function to run the process
async function main() {
  await transferOwnership(FILE_ID, NEW_OWNER_EMAIL);
}

// Run the main function
main();
```

### Instructions for Use

1. Replace the placeholders (your-client-id, your-client-secret, your-refresh-token, your-file-id, new-owner-email@example.com) with your actual data.

2. Follow the tutorial to set up your Google OAuth 2.0 credentials and obtain the refresh token.

3. Use this code in your Node.js project to transfer file ownership securely.