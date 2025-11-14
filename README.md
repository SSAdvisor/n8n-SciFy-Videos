# n8n SciFy Videos Workflow Import Instructions

## Overview

This repository contains a complete n8n workflow system for automated SciFy video creation. 
The system includes **3 main workflows** and **29 sub-workflows (modules)** that work together 
to generate, process, schedule, and publish science fiction video content to YouTube in both 
short-form and long-form formats.

**Note:** Some of the names of the files below are related to my environment and may not
be beneficial to you therefore you may not want to import them. Be sure to follow the import 
instructions as the order of import is important. Review each module (especially the foundation
sub-workflows) and modify them if needed to fit your needs.

## Repository Structure

```
├── workflows/          # Main workflows (3 files)
│   ├── WF002 - SciFy Shorts.json
│   ├── WF020 - YouTube Scheduler.json
│   └── WF021 - SciFy Shorts Expanded 2 Long Form.json
│
├── modules/           # Sub-workflows/dependencies (35 files)
│   ├── SWF003 - Generate Video Metadata.json
│   ├── SWF007 - Upload to YT.json
│   ├── SWF007.1 - YT CHAN 1 Upload to YT.json
│   ├── SWF007.2 - YT CHAN 2 Upload to YT.json
│   ├── SWF008 - Download File for given URL.json
│   ├── SWF009 - Upload to GGL.json
│   ├── SWF010 - Generate Avatar.json
│   ├── SWF011 - Generate Images.json
│   ├── SWF012 - Generate Slideshow.json
│   ├── SWF013 - Add Slideshow Captions.json
│   ├── SWF014 - Generate Script.json
│   ├── SWF015 - Generate Thumbnail.json
│   ├── SWF016 - Find GGL Drive Path.json
│   ├── SWF016.1 - Find GGL ACCT 1 GGL Drive Path.json
│   ├── SWF016.2 - Find GGL ACCT 2 GGL Drive Path.json
│   ├── SWF017 - Update YT Video.json
│   ├── SWF019 - Find GGL Document File.json
│   ├── SWF020 - Temporary Binary File Storage.json
│   ├── SWF021 - Upload Video.json
│   ├── SWF022 - Update Video Ledger Spreadsheet.json
│   ├── SWF023 - Update YT Playlist.json
│   ├── SWF023.1 - Update YT Playlist YT CHAN 1.json
│   ├── SWF023.2 - Update YT Playlist YT CHAN 2.json
│   ├── SWF024 - Haves and Have Nots.json
│   ├── SWF026 - Create GGL Sheets File.json
│   ├── SWF026.1 - Create GGL Sheets File GGL ACCT 1.json
│   ├── SWF026.2 - Create GGL Sheets File GGL ACCT 2.json
│   ├── SWF030 - Download And Save.json
│   ├── SWF051 - Generate Long Story.json
│   ├── SWF052 - Download And Save Temporarily.json
│   ├── SWF053 - Long Story Video Generation.json
│   ├── SWF054 - LLM Completeness Check.json
│   ├── SWF055 - Does File Exist.json
│   ├── SWF056 - Create File.json
│   └── SWF057 - Get File Contents.json
│
├── webhooks/          # Webhook handlers (1 file)
│   └── WF003 - Webhook File Server.json
│
└── errorManagement/   # Error handling workflows (2 files)
    ├── SWF018 - HTTP Request Error Management.json
    └── WF010 - Oops Something is Wrong.json
```

**Total: 41 workflow files**

## Prerequisites

Before importing these workflows, ensure you have:

1. **Self-hosted latest version n8n instance** (version 1.0+)
2. **Required credentials configured** (see Credentials Setup section)
3. **Admin access** to your n8n instance
4. **Understanding of workflow dependencies** and execution order
5. **Sufficient storage** [SSDNodes](https://www.ssdnodes.com/manage/aff.php?aff=2117&register=true) for temporary files and video processing
6. **Zero2Lauch** [API key](https://www.skool.com/z2l-premium-builders-circle-7291/about?ref=a9f6fac834ae4192ad86455a3cee0439)

## Import Order (Critical!)

⚠️ **IMPORTANT**: You must import workflows in the correct order to avoid dependency errors. Sub-workflows must exist before main workflows that reference them.

### Phase 1: Error Management Workflows (Import First)
These provide error handling for all other workflows.

```
1. SWF018 - HTTP Request Error Management.json
2. WF010 - Oops Something is Wrong.json
```

### Phase 2: Level 1 Sub-workflows (Import Second)
These have no dependencies on other sub-workflows.

```
3. SWF003 - Generate Video Metadata.json
4. SWF007.1 - YT CHAN 1 Upload to YT.json
5. SWF007.2 - YT CHAN 2 Upload to YT.json
6. SWF008 - Download File for given URL.json
7. SWF013 - Add Slideshow Captions.json
8. SWF016.1 - Find GGL ACCT 1 GGL Drive Path.json
9. SWF016.2 - Find GGL ACCT 2 GGL Drive Path.json
10. SWF020 - Temporary Binary File Storage.json
11. SWF024 - Haves and Have Nots.json
12. SWF026.1 - Create GGL Sheets File GGL ACCT 1.json
13. SWF026.2 - Create GGL Sheets File GGL ACCT 2.json
14. SWF051 - Generate Long Story.json
15. SWF054 - LLM Completeness Check.json
16. SWF055 - Does File Exist.json
17. SWF056 - Create File.json
18. SWF057 - Get File Contents.json
```

### Phase 3: Level 2 Sub-workflows (Import Third)
These may depend on Phase 2 modules.

```
19. SWF007 - Upload to YT.json
21 .SWF010 - Generate Avatar.json
22 .SWF014 - Generate Script.json
23 .SWF016 - Find GGL Drive Path.json
24 .SWF019 - Find GGL Document File.json
25 .SWF023.1 - Update YT Playlist YT CHAN 1.json
26 .SWF023.2 - Update YT Playlist YT CHAN 2.json
27 .SWF026 - Create GGL Sheets File.json
28 .SWF030 - Download And Save.json
29 .SWF052 - Download And Save Temporarily.json
```

### Phase 4: Level 3 Sub-workflows (Import Fourth)
These depend on content generation modules.

```
30. SWF009 - Upload to GGL.json
31. SWF011 - Generate Images.json
32. SWF012 - Generate Slideshow.json
33. SWF015 - Generate Thumbnail.json
34. SWF022 - Update Video Ledger Spreadsheet.json
35. SWF023 - Update YT Playlist.json
```

### Phase 5: Level 4 Sub-workflows (Import Fifth)
These handle final video processing and distribution.

```
36. SWF017 - Update YT Video.json
37. SWF053 - Long Story Video Generation.json
```

### Phase 6: Level 5 (Import Sixth)
```
SWF021 - Upload Video.json
```

### Phase 7: Webhook Handlers (Import Sixth)
```
33. WF003 - Webhook File Server.json
```

### Phase 8: Main Workflows (Import Last)
These orchestrate all the sub-workflows.

```
34. WF002 - SciFy Shorts.json
35. WF020 - YouTube Scheduler.json
36. WF021 - SciFy Shorts Expanded 2 Long Form.json
```

## How to Import Each Workflow

### Method 1: Via n8n UI (Recommended)
1. Open your n8n instance in your web browser
2. Go to **Workflows** section
3. Click **"Add workflow"** or the **+** button
4. Click **"Import from file"** or **"Import from URL"**
5. Select the JSON file from the repository (or paste the GitHub raw URL)
6. Click **"Import"**
7. **The workflow will appear with the default name "My Workflow"**
8. **Click on the workflow name at the top and rename it** to match the filename (e.g., "WF002 - SciFy Shorts")
9. Save the workflow

**Tips:**
- This is the most reliable method for all deployment types (Docker, npm, etc.)
- Works the same regardless of how n8n is installed
- The workflow name is NOT stored in the JSON file - you must rename it manually
- Use the filename as a guide for what to name the workflow

### Method 2: Copy-Paste JSON
1. Open the JSON file in a text editor or on GitHub
2. Copy the entire JSON content (use "Raw" view on GitHub)
3. In n8n, create a new workflow
4. Click anywhere on the canvas
5. Press **Ctrl+V** (or **Cmd+V** on Mac) to paste
6. The workflow nodes will appear on the canvas
7. **Click on "My Workflow" at the top and rename it** appropriately (use the filename as reference)
8. Save the workflow

**Tips:**
- Useful for quick imports of single workflows
- Good for testing or modifying workflows before saving
- Always rename the workflow to match the intended name
- May require manual credential configuration

**Important Note**: The workflow name is stored in n8n's database, not in the exported JSON file. Both import methods will result in a default name like "My Workflow". You must manually rename each workflow after import to maintain proper organization.

## Credentials Setup

After importing, you'll need to configure credentials for various services.

### Required Credentials

1. **YouTube API (Multiple Accounts)**
   - Type: `Google OAuth2 API`
   - Accounts needed:
     - SciFy-Shorts Account
     - Other credentials per channels
   - Used for: Video uploads, updates, and management
   - Setup: Go to Google Cloud Console, enable YouTube Data API v3
   - Documentation: https://docs.n8n.io/integrations/builtin/credentials/google/

2. **Google Drive/Sheets**
   - Type: `Google Service Account` or `Google OAuth2 API`
   - Used for: File storage, spreadsheet operations, and video ledgers
   - Setup: Create service account with Drive and Sheets API access
   - Documentation: https://docs.n8n.io/integrations/builtin/credentials/google/

3. **AI/LLM Services (Z2L API)**
   - **API Endpoint**: https://api.zero2launch.com
   - **Type**: HTTP Request with API Key authentication
   - **Models Available**: 
     - `openai-large` (for metadata generation)
     - `roblox-rp` (for script generation)
     - Plus: GPT-4, GPT-5, DeepSeek, Gemini, and more
   - **Used for**: Script generation, story creation, metadata optimization
   - **How to Get API Key**:
     1. Join the Z2L Premium: Builder's Circle community
     2. Visit: https://www.skool.com/z2l-premium-builders-circle-7291/about?ref=a9f6fac834ae4192ad86455a3cee0439
     3. Membership: $49/month (Founding Member pricing - increases to $79/mo)
     4. Benefits: Unlimited use of all AI models (GPT-4, GPT-5, DeepSeek, Gemini, Flux, etc.)
     5. Once joined, obtain your API key from the community resources
   - **Setup in n8n**:
     - Create HTTP Request credential
     - Set Base URL: `https://api.zero2launch.com`
     - Add Header: `Authorization: Bearer YOUR_API_KEY`
     - Or follow specific credential setup provided in the community

4. **Image Generation Service (Flux)**
   - **API Endpoint**: Available through Z2L API (https://api.zero2launch.com)
   - **Models**: Flux, Flux Kontext
   - **Used for**: AI image generation, avatars, thumbnails
   - **Setup**: Use same Z2L API credentials as LLM services
   - Note: Flux image generation is included in Z2L Premium membership

5. **Additional Services Available via Z2L API**
   - **Video Generation**: Wan Image→Video
   - **Lipsync API**: For avatar animation
   - **Text-to-Speech (TTS)**: Voice generation
   - **Whisper**: Speech-to-text transcription
   - **Vision**: Image analysis
   - All included with Z2L Premium membership

### Credential Configuration Steps

1. **After importing workflows**, go to **Credentials** section in n8n
2. **Create new credentials** for each required service
3. **Name them descriptively** for your own reference (e.g., "YouTube", "Z2L API", "Google Drive", "Google Sheets")
4. **After import, manually update each workflow**:
   - Open each imported workflow
   - Look for nodes with red warning indicators (credential errors)
   - Click on each node with an error
   - Select the appropriate credential from the dropdown
   - Save the workflow
5. **Test each credential** to ensure proper connection

**Important**: Credential IDs are instance-specific and don't transfer between n8n installations. You will need to manually map credentials in imported workflows regardless of naming conventions. Having matching or similar names just makes it easier to identify which credential to select from the dropdown.

### Z2L API Configuration Example

For workflows using the Z2L API, you'll typically configure the HTTP Request node like this:

```
Authentication: Generic Credential Type
Credential for HTTP Request:
  - Name: Z2L API
  - Authentication: Header Auth
  - Header Name: Authorization
  - Header Value: Bearer YOUR_API_KEY_HERE
```

Or if using a predefined credential:
```
Base URL: https://api.zero2launch.com
API Key: YOUR_API_KEY_HERE
```

Refer to the Z2L Premium community documentation for the most up-to-date API configuration instructions.

## Post-Import Configuration

### 1. Update Workflow Controls

Each main workflow has a "Workflow Controls" node. Update these settings:

**WF002 - SciFy Shorts:**
```json
{
  "yt_upload": true,           // Set to false for testing
  "yt_credentials": "YOUR_CREDENTIAL_NAME",
  "yt_privacy": "public",      // or "private" for testing
  "ggl_upload": false,         // Set to true if using Google Drive
  "ggl_credentials": "YOUR_CREDENTIAL_NAME"
}
```

**WF020 - YouTube Scheduler:**
```json
{
  "list": [
    {
      "chan": "HOA_NIGHTMARES",
      "path": "HOA_Nightmares",
      "schd": {"morn": "6:00", "evng": "18:00"}
    },
    // Add other channels as needed
  ]
}
```

**WF021 - SciFy Shorts Expanded 2 Long Form:**
```json
{
  "yt_upload": true,
  "yt_credentials": "YOUTUBE CREDENTIAL IDENTIFIER",
  "yt_privacy": "private",
  "ggl_spreadsheet_path": "/ConnectCircuits/SciFy",
  "ggl_spreadsheet_name": "video_ledger",
  "llm_model": "openai-large",
  "story_word_count": 5000,
  "story_genre": "science fiction"
}
```

### 2. Configure Schedules

**WF002 - SciFy Shorts:**
- Default: Every 6 hours
- Adjust: Modify Schedule Trigger node as needed

**WF020 - YouTube Scheduler:**
- Default: Daily at 8:00 PM
- Adjust: Modify Schedule Trigger node for your preferred time

**WF021 - SciFy Shorts Expanded 2 Long Form:**
- Default: Every 12 hours
- Adjust: Modify Schedule Trigger node as needed

### 3. Update Sub-workflow References

All "Execute Workflow" nodes reference workflows by ID. When importing to a new instance:

1. Open each main workflow
2. Check each "Execute Workflow" node
3. Verify the workflow ID matches your imported sub-workflows
4. Update if necessary (IDs change when importing to different instances)

**Pro Tip:** Use n8n's workflow ID from the URL bar when viewing each workflow

### 4. Test Each Sub-workflow Independently

Before running main workflows:
1. Test each sub-workflow individually with sample data
2. Verify all credentials work properly
3. Check file paths and storage locations
4. Ensure API rate limits are understood

## Workflow Dependencies Map

### WF002 - SciFy Shorts (Short-Form Video Generation)
```
WF002 (Main) depends on:
├── SWF014 (Generate Script)
├── SWF011 (Generate Images)
├── SWF012 (Generate Slideshow)
├── SWF013 (Add Slideshow Captions)
├── SWF007 (Upload to YT)
├── SWF009 (Upload to GGL)
└── SWF022 (Update Video Ledger)
```

### WF020 - YouTube Scheduler (Video Scheduling)
```
WF020 (Main) - Standalone
└── No sub-workflow dependencies
    Uses: Direct YouTube API and Google Sheets integration
```

### WF021 - SciFy Shorts Expanded 2 Long Form (Long-Form Video Generation)
```
WF021 (Main) depends on:
├── SWF016 (Find GGL Drive Path)
├── SWF053 (Long Story Video Generation)
│   └── May depend on: SWF051, SWF052, SWF010
├── SWF003 (Generate Video Metadata)
├── SWF015 (Generate Thumbnail)
│   └── May depend on: SWF010, SWF020
├── SWF021 (Upload Video)
│   └── Depends on: SWF007, SWF009, SWF023 variants
└── SWF022 (Update Video Ledger Spreadsheet)
    └── Depends on: SWF016, SWF019, SWF026 variants
```

### Supporting Modules (Used by multiple workflows)
```
File Operations:
├── SWF008 (Download File)
├── SWF030 (Download And Save)
└── SWF052 (Download And Save Temporarily)

Google Drive/Sheets:
├── SWF016 (Find GGL Drive Path)
├── SWF019 (Find GGL Document)
└── SWF026 variants (Create GGL Sheets)

Video Processing:
├── SWF010 (Generate Avatar)
├── SWF011 (Generate Images)
├── SWF012 (Generate Slideshow)
├── SWF013 (Add Slideshow Captions)
└── SWF015 (Generate Thumbnail)

Content Generation:
├── SWF003 (Generate Video Metadata)
├── SWF014 (Generate Script)
├── SWF051 (Generate Long Story)
└── SWF053 (Long Story Video Generation)

Upload & Management:
├── SWF007 (Upload to YT)
├── SWF009 (Upload to GGL)
├── SWF017 (Update YT Video)
├── SWF021 (Upload Video)
├── SWF022 (Update Video Ledger)
└── SWF023 variants (Update YT Playlist)

Error Handling:
├── SWF018 (HTTP Request Error Management)
└── WF010 (General Error Handler)
```

## Common Issues and Solutions

### Issue 1: "Workflow not found" errors
**Cause**: Sub-workflow IDs don't match after import  
**Solution**: 
1. Find the correct workflow ID from the URL when viewing the sub-workflow
2. Update the Execute Workflow node with the correct ID
3. Save and test the workflow

### Issue 2: Credential errors
**Cause**: Missing or incorrectly configured credentials  
**Solution**:
1. Create credentials for each required service
2. Open workflows with red error indicators
3. Click each node showing errors
4. Select the appropriate credential from the dropdown
5. Test each credential connection before running workflows

### Issue 3: Import fails or hangs
**Cause**: Large JSON files or complex dependencies  
**Solution**:
1. Import one workflow at a time
2. Refresh browser between imports
3. Check n8n logs for specific errors
4. Ensure sufficient server resources (RAM, disk space)

### Issue 4: Module dependencies not resolved
**Cause**: Sub-workflows imported out of order  
**Solution**:
1. Follow the import order exactly as documented
2. Re-import sub-workflows if needed
3. Verify each phase completes before moving to next

### Issue 5: API rate limits exceeded
**Cause**: Too many API calls in short period  
**Solution**:
1. Add wait/delay nodes between API calls
2. Implement batch processing with appropriate delays
3. Monitor API usage and adjust workflow execution frequency

### Issue 6: File storage issues
**Cause**: Incorrect paths or insufficient permissions  
**Solution**:
1. Verify Google Drive paths exist
2. Check service account permissions
3. Ensure adequate storage space
4. Test file operations with small files first

### Issue 7: Workflows appear with wrong names
**Cause**: Workflow names are not stored in JSON files  
**Solution**:
1. After importing, manually rename each workflow
2. Use the JSON filename as a guide (e.g., "WF002 - SciFy Shorts.json" → "WF002 - SciFy Shorts")
3. Keep consistent naming for easier management

## Testing Your Installation

### Phase 1: Test Foundation Modules
```bash
# Test file operations
Run SWF008 with a test URL
Run SWF030 with sample file

# Test Google Drive integration  
Run SWF016 with your drive path
Run SWF019 with test document name
```

### Phase 2: Test Content Generation
```bash
# Test script generation
Run SWF014 manually with sample prompts

# Test image generation  
Run SWF011 with test image prompts

# Test metadata generation
Run SWF003 with sample video script
```

### Phase 3: Test Video Production
```bash
# Test slideshow creation
Run SWF012 with test images and text

# Test thumbnail generation
Run SWF015 with sample text and avatar

# Test caption overlay
Run SWF013 with test video and captions
```

### Phase 4: Test Upload Systems
```bash
# Test YouTube upload (use private status)
Run SWF007 with test video

# Test Google Drive upload
Run SWF009 with test file

# Test spreadsheet updates
Run SWF022 with test data
```

### Phase 5: Integration Testing
```bash
# Test short-form workflow (with testing settings)
Set yt_upload: false in Workflow Controls
Set yt_privacy: "private" for safety
Execute WF002 manually

# Test long-form workflow
Execute WF021 manually with one test item

# Test scheduler (disable auto-run first)
Execute WF020 manually
```

## Production Deployment

### 1. Security Configuration
- Review all credentials for production security
- Set appropriate privacy settings (start with "private")
- Configure proper access controls on Google Drive
- Backup your encryption key
- Enable two-factor authentication on all service accounts
- Rotate API keys regularly

### 2. Performance Optimization
- Monitor execution times for each workflow
- Adjust concurrent execution limits
- Configure appropriate timeouts for long-running operations
- Set up monitoring and alerting for failed executions
- Implement rate limiting for API calls
- Optimize video file sizes for faster uploads

### 3. Backup Strategy
- Export workflows regularly (weekly minimum)
- Backup credential configurations (encrypted)
- Document any custom modifications
- Test restore procedures monthly
- Maintain version history of workflow changes
- Store backups in multiple locations

### 4. Monitoring Setup
- Set up email/SMS alerts for workflow failures
- Monitor API quota usage
- Track video upload success rates
- Monitor storage usage on Google Drive
- Set up uptime monitoring for n8n instance
- Create dashboards for key metrics

## Production Checklist

- [ ] All workflows imported successfully
- [ ] All workflows renamed appropriately
- [ ] All credentials configured and tested
- [ ] Workflow IDs updated and verified
- [ ] Test runs completed for each workflow
- [ ] Production settings configured
- [ ] Schedules set appropriately
- [ ] Monitoring and alerting configured
- [ ] Backup strategy implemented
- [ ] Documentation updated with customizations
- [ ] Team trained on workflow operations
- [ ] Rollback plan established
- [ ] Support contacts documented

## Support and Troubleshooting

### Debug Mode
Enable debug logging in n8n:
```bash
export N8N_LOG_LEVEL=debug
```

### Common Log Locations
- **Docker**: `docker logs n8n-container`
- **Direct install**: Check n8n startup logs
- **Workflow execution**: View in n8n UI execution panel
- **System logs**: `/var/log/n8n/` (if configured)

### Performance Monitoring
```bash
# Check n8n resource usage (Docker)
docker stats n8n-container

# Monitor workflow execution times
# Use n8n UI > Executions tab

# Check database size
# Regularly clean old execution data
```

### Getting Help
1. **n8n Documentation**: https://docs.n8n.io/
2. **Community Forum**: https://community.n8n.io/
3. **GitHub Issues**: https://github.com/n8n-io/n8n/issues
4. **Discord Community**: n8n Discord server
5. **Z2L Premium Community**: https://www.skool.com/z2l-premium-builders-circle-7291/about?ref=a9f6fac834ae4192ad86455a3cee0439

## Customization Guidelines

### Modifying Workflows
1. **Always backup** before making changes
2. **Test in development** environment first
3. **Document changes** for future reference
4. **Update this documentation** if workflows change significantly
5. **Version control** your modifications

### Adding New Features
1. Create new sub-workflows for reusability
2. Follow existing naming conventions (SWF###)
3. Add proper error handling (use SWF018)
4. Update dependency documentation
5. Test thoroughly before production deployment
6. Consider performance impact

### Best Practices
- Keep workflows modular and focused
- Use descriptive names for nodes
- Add sticky notes for documentation
- Implement proper error handling
- Test with sample data first
- Monitor execution performance
- Maintain clear variable naming
- Document complex logic

---

**Last Updated**: October 2025  
**Version**: 2.1 (Corrected)  
**Tested with**: n8n v1.0+ (self-hosted)  
**Total Workflows**: 35 (3 main + 29 modules + 2 error + 1 webhook)

For questions or issues specific to this workflow collection, please refer to the repository issues or contact the maintainer.
