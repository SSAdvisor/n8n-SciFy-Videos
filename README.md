# n8n SciFy Videos Workflow Import Instructions

## Overview

This repository contains a complete n8n workflow system for automated SciFy video creation. The system includes a main workflow and multiple sub-workflows (modules) that work together to generate, process, and publish science fiction video content to YouTube.

## Repository Structure

```
├── workflows/          # Main workflows
│   └── WF002 - SciFy Shorts.json
├── modules/           # Sub-workflows (dependencies)
│   ├── SWF007 - Upload to YT.json
│   ├── SWF008 - Download File for given URL.json
│   ├── SWF009 - Upload to GGL.json
│   ├── SWF011 - Generate Images.json
│   ├── SWF012 - Generate Slideshow.json
│   ├── SWF013 - Add Slideshow Captions.json
│   ├── SWF014 - Generate Script.json
│   ├── SWF016 - Find GGL Drive Path.json
│   ├── SWF019 - Find GGL Document File.json
│   ├── SWF020 - Temporary Binary File Storage.json
│   ├── SWF022 - Update Video Ledger Spreadsheet.json
│   ├── SWF026 - Create GGL Sheets File EBOYD53.json
│   ├── SWF026 - Create GGL Sheets File.json
│   ├── SWF026-- Create GGL Sheets File SSADVISOR.json
│   └── SWF030 - Download And Save.json
├── webhooks/          # Webhook handlers
│   └── WF003 - Webhook File Server.json
└── errorManagement/   # Error handling workflows
    ├── SWF018 - HTTP Request Error Management.json
    └── WF010 - Oops Something is Wrong.json
```

## Prerequisites

Before importing these workflows, ensure you have:

1. **Self-hosted n8n instance** (version 1.0+)
2. **Required credentials configured** (see Credentials Setup section)
3. **Admin access** to your n8n instance
4. **Understanding of workflow dependencies** and execution order

## Import Order (Critical!)

⚠️ **IMPORTANT**: You must import workflows in the correct order to avoid dependency errors.

### Step 1: Import Error Management Workflows First
1. `SWF018 - HTTP Request Error Management.json`
2. `WF010 - Oops Something is Wrong.json`

### Step 2: Import Sub-workflows (Modules)
Import all files from the `modules/` folder in any order:
- `SWF007 - Upload to YT.json`
- `SWF008 - Download File for given URL.json`
- `SWF009 - Upload to GGL.json`
- `SWF011 - Generate Images.json`
- `SWF012 - Generate Slideshow.json`
- `SWF013 - Add Slideshow Captions.json`
- `SWF014 - Generate Script.json`
- `SWF016 - Find GGL Drive Path.json`
- `SWF019 - Find GGL Document File.json`
- `SWF020 - Temporary Binary File Storage.json`
- `SWF022 - Update Video Ledger Spreadsheet.json`
- `SWF026 - Create GGL Sheets File EBOYD53.json`
- `SWF026 - Create GGL Sheets File.json`
- `SWF026-- Create GGL Sheets File SSADVISOR.json`
- `SWF030 - Download And Save.json`

### Step 3: Import Webhook Handlers
1. `WF003 - Webhook File Server.json`

### Step 4: Import Main Workflow
1. `WF002 - SciFy Shorts.json`

## How to Import Each Workflow

### Method 1: Via n8n UI (Recommended)
1. Open your n8n instance
2. Go to **Workflows** section
3. Click **"Add workflow"**
4. Click **"Import from file"**
5. Select the JSON file from the repository
6. Click **"Import"**
7. The workflow will appear in your workflows list

### Method 2: Via Command Line
```bash
# Navigate to your n8n installation directory
cd /path/to/n8n

# Import a single workflow
n8n import:workflow --input=/path/to/workflow.json

# Import multiple workflows
n8n import:workflow --input=/path/to/workflows/
```

### Method 3: Copy-Paste JSON
1. Open the JSON file in a text editor
2. Copy the entire JSON content
3. In n8n, create a new workflow
4. Paste the JSON content directly into the canvas (Ctrl+V)

## Credentials Setup

After importing, you'll need to configure credentials for various services. The workflows require:

### Required Credentials

1. **YouTube API (EBOYD53)**
   - Type: `Google OAuth2 API`
   - Used for: Video uploads to YouTube
   - Setup: Go to Google Cloud Console, enable YouTube Data API v3

2. **Google Drive/Sheets (EBOYD53)**
   - Type: `Google Service Account` or `Google OAuth2 API`
   - Used for: File storage and spreadsheet operations
   - Setup: Create service account with Drive and Sheets API access

3. **AI/LLM Services**
   - Type: `OpenAI API` or equivalent
   - Used for: Script generation
   - Setup: Obtain API key from your preferred LLM provider

4. **Image Generation Service**
   - Type: Custom API or service integration
   - Used for: AI image generation (likely Flux or similar)
   - Setup: Configure according to your image generation service

### Credential Configuration Steps

1. **After importing workflows**, go to **Credentials** section in n8n
2. **Create new credentials** for each required service
3. **Name them exactly** as referenced in the workflows:
   - `EBOYD53` for YouTube/Google services
   - Match other credential names as they appear in the workflow nodes

4. **Test each credential** to ensure proper connection
5. **Update workflow nodes** if credential names don't match

## Post-Import Configuration

### 1. Update Workflow Controls
In the main workflow (`WF002 - SciFy Shorts`), locate the "Workflow Controls" node and update:

```json
{
  "yt_upload": true,           // Set to false for testing
  "yt_credentials": "YOUR_CREDENTIAL_NAME",
  "yt_privacy": "public",      // or "private" for testing
  "ggl_upload": false,         // Set to true if using Google Drive
  "ggl_credentials": "YOUR_CREDENTIAL_NAME"
}
```

### 2. Configure Schedule
The main workflow uses a Schedule Trigger (every 6 hours). Adjust as needed:
- For testing: Disable the schedule trigger
- For production: Keep or modify the interval

### 3. Update Sub-workflow References
Check that all "Execute Workflow" nodes reference the correct workflow IDs:
1. Open each Execute Workflow node
2. Verify the workflow ID matches your imported sub-workflows
3. Update if necessary (IDs change when importing to different instances)

### 4. Test Each Sub-workflow Independently
Before running the main workflow:
1. Test each sub-workflow individually
2. Provide sample data to verify functionality
3. Check that all credentials work properly

## Common Issues and Solutions

### Issue 1: "Workflow not found" errors
**Cause**: Sub-workflow IDs don't match
**Solution**: 
1. Find the correct workflow ID from the URL
2. Update the Execute Workflow node with the correct ID

### Issue 2: Credential errors
**Cause**: Missing or incorrectly named credentials
**Solution**:
1. Create credentials with exact names used in workflows
2. Or update workflow nodes to use your credential names

### Issue 3: Import fails or hangs
**Cause**: Large JSON files or complex dependencies
**Solution**:
1. Import one workflow at a time
2. Refresh browser between imports
3. Check n8n logs for specific errors

### Issue 4: Nodes show errors after import
**Cause**: Missing dependencies or credentials
**Solution**:
1. Check each node configuration
2. Ensure all required fields are populated
3. Test credentials connectivity

## Workflow Dependencies Map

```
WF002 (Main) depends on:
├── SWF014 (Generate Script)
├── SWF011 (Generate Images)
├── SWF012 (Generate Slideshow)
├── SWF013 (Add Slideshow Captions)
├── SWF007 (Upload to YT)
├── SWF009 (Upload to GGL)
└── SWF022 (Update Video Ledger)

Sub-workflows may depend on:
├── SWF008 (Download File)
├── SWF016 (Find GGL Drive Path)
├── SWF019 (Find GGL Document)
├── SWF020 (Temporary Binary Storage)
├── SWF026 variants (Create GGL Sheets)
└── SWF030 (Download And Save)

Error handling:
├── SWF018 (HTTP Request Error Management)
└── WF010 (General Error Handler)
```

## Testing Your Installation

### 1. Test Individual Components
```bash
# Test script generation
Run SWF014 manually with sample prompts

# Test image generation  
Run SWF011 with test image prompts

# Test file operations
Run SWF008 with a test URL
```

### 2. Test Integration
```bash
# Run main workflow with testing settings
Set yt_upload: false in Workflow Controls
Set yt_privacy: "private" for safety
Execute WF002 manually
```

### 3. Monitor Execution
1. Check execution logs for each step
2. Verify file generation and storage
3. Confirm API calls are successful
4. Test error handling paths

## Production Deployment

### 1. Security Configuration
- Review all credentials for production security
- Set appropriate privacy settings
- Configure proper access controls
- Backup your encryption key

### 2. Performance Optimization
- Monitor execution times
- Adjust concurrent execution limits
- Configure appropriate timeouts
- Set up monitoring and alerting

### 3. Backup Strategy
- Export workflows regularly
- Backup credential configurations (encrypted)
- Document any custom modifications
- Test restore procedures

## Support and Troubleshooting

### Debug Mode
Enable debug logging in n8n:
```bash
export N8N_LOG_LEVEL=debug
```

### Common Log Locations
- Docker: `docker logs n8n-container`
- Direct install: Check n8n startup logs
- Workflow execution: View in n8n UI execution panel

### Getting Help
1. Check n8n documentation: https://docs.n8n.io/
2. Community forum: https://community.n8n.io/
3. GitHub issues: https://github.com/n8n-io/n8n/issues

## Customization Guidelines

### Modifying Workflows
1. **Always backup** before making changes
2. **Test in development** environment first
3. **Document changes** for future reference
4. **Update this documentation** if workflows change significantly

### Adding New Features
1. Create new sub-workflows for reusability
2. Follow existing naming conventions
3. Add proper error handling
4. Update dependency documentation

---

**Last Updated**: October 2025
**Version**: 1.0
**Tested with**: n8n v1.0+ (self-hosted)

For questions or issues specific to this workflow collection, please refer to the repository issues or contact the maintainer.