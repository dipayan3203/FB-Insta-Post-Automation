# Setup Guide: FB & Insta Post Automation

This comprehensive guide will walk you through setting up the Facebook & Instagram Post Automation workflow from scratch.

## 📋 Table of Contents

1. [Prerequisites](#prerequisites)
2. [Initial Setup](#initial-setup)
3. [API Credentials Setup](#api-credentials-setup)
4. [Google Sheets Configuration](#google-sheets-configuration)
5. [n8n Workflow Import](#n8n-workflow-import)
6. [Testing & Validation](#testing--validation)
7. [Production Deployment](#production-deployment)
8. [Troubleshooting](#troubleshooting)

---

## Prerequisites

### Required Accounts

- [ ] n8n instance (Cloud or Self-hosted)
- [ ] Google Account (for Sheets)
- [ ] Facebook Business Account
- [ ] Instagram Business Account
- [ ] Twitter/X Developer Account
- [ ] Telegram Bot Account
- [ ] Google Cloud Account (for Gemini API)
- [ ] Blotoato Account (or alternative Instagram API service)

### Technical Requirements

- **n8n Version**: 1.0.0 or higher
- **Node.js**: 18.x or higher (for self-hosted)
- **Internet Connection**: Stable connection required
- **Storage**: ~500MB for workflow execution logs

### Estimated Setup Time

- **Quick Setup**: 30-45 minutes (basic configuration)
- **Full Setup**: 2-3 hours (including testing and optimization)

---

## Initial Setup

### Step 1: Verify n8n Installation

#### For n8n Cloud Users:
```bash
# Simply log in to your n8n cloud dashboard
https://app.n8n.cloud/
```

#### For Self-Hosted Users:
```bash
# Check n8n is running
docker ps | grep n8n

# Or if installed via npm
n8n --version

# Start n8n if not running
docker-compose up -d
# OR
n8n start
```

### Step 2: Access n8n Interface

1. Open your browser
2. Navigate to your n8n instance:
   - Cloud: `https://app.n8n.cloud/`
   - Self-hosted: `http://localhost:5678/` (default)
3. Log in with your credentials

---

## API Credentials Setup

### 1. Google Sheets API

#### Step-by-Step Setup:

1. **Go to Google Cloud Console**
   - Visit: https://console.cloud.google.com/

2. **Create a New Project**
   ```
   - Click "Select a project" → "New Project"
   - Name: "n8n Social Media Automation"
   - Click "Create"
   ```

3. **Enable Google Sheets API**
   ```
   - Navigate to "APIs & Services" → "Library"
   - Search for "Google Sheets API"
   - Click "Enable"
   ```

4. **Create OAuth 2.0 Credentials**
   ```
   - Go to "APIs & Services" → "Credentials"
   - Click "Create Credentials" → "OAuth client ID"
   - Application type: "Web application"
   - Name: "n8n Google Sheets"
   - Authorized redirect URIs: Add your n8n OAuth callback URL
     - Cloud: https://app.n8n.cloud/rest/oauth2-credential/callback
     - Self-hosted: http://YOUR_DOMAIN:5678/rest/oauth2-credential/callback
   - Click "Create"
   - Download the JSON file (save it securely)
   ```

5. **Add Credentials in n8n**
   ```
   - In n8n: Go to "Credentials" → "Add Credential"
   - Select "Google Sheets OAuth2 API"
   - Enter Client ID and Client Secret from downloaded JSON
   - Click "Connect my account"
   - Authorize the app
   - Test the connection
   ```

### 2. Facebook Graph API

#### Prerequisites:
- Facebook Business Account
- Facebook Page (must be admin)

#### Setup Steps:

1. **Create Facebook App**
   - Visit: https://developers.facebook.com/
   - Click "My Apps" → "Create App"
   - Use case: "Business"
   - App name: "n8n Social Automation"
   - Click "Create App"

2. **Add Facebook Login**
   ```
   - In App Dashboard → "Add Product"
   - Select "Facebook Login" → "Set Up"
   - Choose "Web" platform
   - Skip quickstart
   ```

3. **Get Access Token**
   ```
   - Go to Tools → "Graph API Explorer"
   - Select your app
   - Click "Generate Access Token"
   - Required permissions:
     ✓ pages_manage_posts
     ✓ pages_read_engagement
     ✓ pages_show_list
   - Copy the Access Token
   ```

4. **Get Page ID**
   ```
   - Visit your Facebook Page
   - Click "About"
   - Scroll down to find "Page ID"
   - Copy the numeric ID
   ```

5. **Add to n8n**
   ```
   - Credentials → "Add Credential"
   - Select "Facebook Graph API"
   - Paste Access Token
   - Save
   ```

### 3. Instagram via Blotoato

#### Alternative Services:
- Blotoato (recommended)
- Instagram Graph API (official, requires Facebook Business)
- Combin API
- Hootsuite API

#### Blotoato Setup:

1. **Sign Up**
   - Visit: https://blotoato.com/ (or your chosen service)
   - Create an account
   - Choose a plan (free trial available)

2. **Connect Instagram**
   ```
   - Dashboard → "Connect Account"
   - Follow Instagram authorization
   - Copy API Key
   - Copy API URL endpoint
   ```

3. **Add to Workflow**
   ```
   - In workflow "Workflow Configuration" node
   - Replace placeholder values:
     - blotoatoApiUrl: https://api.blotoato.com/v1/post
     - blotoatoApiKey: your_api_key_here
   ```

### 4. Twitter/X API

#### New Twitter API Setup (2025):

1. **Apply for Developer Account**
   - Visit: https://developer.twitter.com/
   - Click "Apply for access"
   - Complete the application form
   - Wait for approval (usually 24-48 hours)

2. **Create App**
   ```
   - Developer Portal → "Projects & Apps"
   - Create new project
   - Create new app
   - Name: "n8n Social Automation"
   ```

3. **Get API Keys**
   ```
   - App Settings → "Keys and tokens"
   - Generate:
     ✓ API Key
     ✓ API Key Secret
     ✓ Bearer Token
     ✓ Access Token
     ✓ Access Token Secret
   - Save all keys securely
   ```

4. **Configure App Permissions**
   ```
   - Settings → "User authentication settings"
   - Enable OAuth 2.0
   - Permissions: Read and Write
   - Callback URL: Your n8n OAuth callback
   - Save
   ```

5. **Add to n8n**
   ```
   - Credentials → "Twitter OAuth2 API"
   - Enter API Key and Secret
   - Complete OAuth flow
   ```

### 5. Telegram Bot

#### Quick Setup:

1. **Create Bot with BotFather**
   ```
   - Open Telegram
   - Search for @BotFather
   - Send: /newbot
   - Follow prompts:
     - Bot name: "My Social Media Bot"
     - Username: "mysocialmedia_bot"
   - Copy the API Token
   ```

2. **Get Chat ID**
   ```
   - Start a chat with your bot
   - Send any message
   - Visit: https://api.telegram.org/bot<YOUR_TOKEN>/getUpdates
   - Copy the "chat_id" from response
   ```

3. **Add to n8n**
   ```
   - Credentials → "Telegram API"
   - Paste Access Token
   - In workflow node, add Chat ID
   ```

### 6. Google Gemini API

#### Setup Steps:

1. **Get API Key**
   - Visit: https://makersuite.google.com/app/apikey
   - Sign in with Google Account
   - Click "Create API Key"
   - Select your project (or create new)
   - Copy API Key

2. **Add to n8n**
   ```
   - Credentials → "Google PaLM API"
   - Paste API Key
   - Test connection
   ```

---

## Google Sheets Configuration

### Step 1: Create the Spreadsheet

1. **Create New Sheet**
   ```
   - Go to: https://sheets.google.com/
   - Click "Blank" to create new spreadsheet
   - Name it: "Social Media Content Queue"
   ```

2. **Set Up Columns**
   
   Create these exact column headers in Row 1:

   | A | B | C |
   |---|---|---|
   | Image Url | Posted | Ad Generated |

   **Column Details:**
   - **Column A (Image Url)**: Direct links to images
   - **Column B (Posted)**: Leave empty (workflow fills "done")
   - **Column C (Ad Generated)**: Leave empty (workflow fills "complete")

### Step 2: Add Sample Data

Add test data in Row 2:
```
Image Url: https://example.com/test-image.jpg
Posted: (leave empty)
Ad Generated: (leave empty)
```

### Step 3: Get Sheet URL

1. Copy the full URL from browser
2. Format: `https://docs.google.com/spreadsheets/d/SHEET_ID/edit?gid=0#gid=0`
3. You'll need this for workflow configuration

### Step 4: Set Permissions

```
- Click "Share" button
- Change to "Anyone with the link can view"
- OR add your n8n service account email
```

---

## n8n Workflow Import

### Step 1: Download Workflow File

```bash
# If from GitHub
git clone https://github.com/yourusername/repo-name.git
cd repo-name
```

### Step 2: Import into n8n

1. **Via UI**
   ```
   - In n8n → "Workflows"
   - Click "Import from File"
   - Select "FB & Insta Post Automation.json"
   - Click "Import"
   ```

2. **Via URL (if hosted)**
   ```
   - Click "Import from URL"
   - Paste raw GitHub URL
   - Import
   ```

### Step 3: Update Placeholders

#### In "Workflow Configuration" Node:
```javascript
// Replace these values:
blotoatoApiUrl: "https://api.blotoato.com/v1/post"
blotoatoApiKey: "your_actual_api_key"
```

#### In "Post to Facebook" Node:
```javascript
// Update node parameter:
node: "YOUR_FACEBOOK_PAGE_ID"
```

#### In All Google Sheets Nodes:
```javascript
// Update documentId URL:
"https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit?gid=0#gid=0"
```

### Step 4: Connect Credentials

For each node requiring credentials:

1. **Click on the node**
2. **Under "Credentials"**:
   - If red warning: Click "Select Credential"
   - Choose your configured credential
   - Or click "Create New Credential"
3. **Test the connection**
4. **Save**

Required credential connections:
- [ ] Google Sheets OAuth2 (3 nodes)
- [ ] Facebook Graph API (1 node)
- [ ] Twitter OAuth2 API (1 node)
- [ ] Telegram API (1 node)
- [ ] Google Gemini API (1 node)
- [ ] HTTP Header Auth for Blotoato (1 node)

---

## Testing & Validation

### Pre-Flight Checklist

- [ ] All credentials are connected
- [ ] Google Sheet has test data
- [ ] Placeholder values replaced
- [ ] All nodes show green check marks
- [ ] No red error indicators

### Step 1: Test Individual Nodes

#### Test Google Sheets Connection:
```
1. Click "Get Picture Links from Sheet" node
2. Click "Execute Node" (play button)
3. Verify: Should return your sheet data
4. Check output tab for row data
```

#### Test Image Download:
```
1. Use a valid image URL in your sheet
2. Execute "Download Picture" node
3. Check binary data is received
```

#### Test AI Generation:
```
1. Execute "Generate Post with Gemini" node
2. Review generated ad copy
3. Verify format matches expectations
```

### Step 2: Test Social Media Posting (CAREFUL!)

⚠️ **Warning**: These will post to live accounts

#### Safe Testing Approach:
```
1. Create test posts first:
   - Use test Facebook page
   - Use private Twitter account
   - Test Telegram group

2. Or temporarily disable posting nodes:
   - Right-click node → "Disable"
   - Test rest of workflow
   - Re-enable when ready
```

### Step 3: Full Workflow Test

1. **Manual Execution**
   ```
   - Click "Execute Workflow" button
   - Monitor each node execution
   - Check for errors in output panels
   - Verify Google Sheet is updated
   ```

2. **Verify Results**
   ```
   ✓ Sheet "Ad Generated" column = "complete"
   ✓ Sheet "Posted" column = "done"
   ✓ Post appears on Facebook
   ✓ Post appears on Instagram
   ✓ Tweet posted
   ✓ Telegram message sent
   ```

### Step 4: Schedule Test

```
1. Set Schedule Trigger to near future
   - Example: 5 minutes from now
2. Activate workflow
3. Wait for scheduled execution
4. Check execution log
5. Verify all posts created
6. Reset to desired schedule
```

---

## Production Deployment

### Step 1: Optimize Schedule

```javascript
// In Schedule Trigger node
"interval": [
  {
    "triggerAtHour": 11,  // 11 AM
    "triggerAtMinute": 0
  }
]

// Recommended schedules:
// - B2C: 11 AM, 3 PM, 7 PM
// - B2B: 8 AM, 12 PM
// - E-commerce: 10 AM, 6 PM, 9 PM
```

### Step 2: Enable Workflow

```
1. Toggle "Active" switch in workflow
2. Confirm activation
3. Note: Will run on next scheduled time
```

### Step 3: Set Up Monitoring

#### Add Error Notifications:

1. **Via Email**
   ```
   - Add "Send Email" node after error outputs
   - Configure SMTP credentials
   - Send to your admin email
   ```

2. **Via Slack**
   ```
   - Add "Slack" node
   - Connect to workspace
   - Post to #alerts channel
   ```

### Step 4: Configure Execution Data

```
- Settings → Execution
- Save Execution Data: "Save"
- Execution History: Keep last 100
- Error workflow: (optional) Create error handler
```

### Step 5: Backup Configuration

```bash
# Export workflow
1. Workflow → Settings → Export
2. Save JSON file
3. Commit to Git repository

# Recommended: Daily automated backups
4. Set up backup script
5. Store credentials separately (not in workflow)
```

---

## Troubleshooting

### Common Issues & Solutions

#### Issue 1: "Credentials not found"
```
Solution:
1. Re-create credentials in n8n
2. Update credential ID in workflow
3. Test connection
```

#### Issue 2: "Google Sheets: Permission denied"
```
Solution:
1. Check OAuth scopes
2. Re-authorize Google connection
3. Verify sheet sharing settings
```

#### Issue 3: "Facebook Graph API error"
```
Solution:
1. Verify Page ID is correct
2. Check access token hasn't expired
3. Confirm page admin permissions
4. Review API rate limits
```

#### Issue 4: "Image download fails"
```
Solution:
1. Verify image URL is publicly accessible
2. Check URL format (must include https://)
3. Test URL in browser
4. Ensure no authentication required
```

#### Issue 5: "AI generation returns empty"
```
Solution:
1. Check Gemini API quota
2. Verify API key is active
3. Review prompt in system message
4. Test with simpler prompt
```

#### Issue 6: "Instagram post fails via Blotoato"
```
Solution:
1. Verify API key is valid
2. Check Instagram account connection
3. Review image format (JPEG/PNG)
4. Confirm image size limits (max 8MB)
5. Check API rate limits
```

### Getting Help

1. **Check Execution Logs**
   ```
   - Executions tab → Failed
   - Click execution → View details
   - Check error messages
   ```

2. **n8n Community**
   - Forum: https://community.n8n.io/
   - Discord: https://discord.gg/n8n

3. **API-Specific Support**
   - Facebook: https://developers.facebook.com/support/
   - Twitter: https://developer.twitter.com/en/support
   - Google: https://support.google.com/

---

## Next Steps

After successful setup:

1. [ ] Add more images to Google Sheet
2. [ ] Customize AI prompt for your brand
3. [ ] Adjust posting schedule
4. [ ] Monitor performance metrics
5. [ ] Optimize based on engagement
6. [ ] Scale to multiple products

## Additional Resources

- [n8n Documentation](https://docs.n8n.io/)
- [Workflow Best Practices](https://docs.n8n.io/workflows/best-practices/)
- [API Rate Limits Guide](https://docs.n8n.io/integrations/builtin/app-nodes/)

---

**Setup completed?** Check out the main [README.md](README.md) for usage instructions!

**Need help?** Open an issue on GitHub or contact support.
