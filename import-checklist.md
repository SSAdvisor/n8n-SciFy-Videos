# n8n SciFy Videos - Quick Import Checklist

## Pre-Import Setup ✅

- [ ] n8n instance running (version 1.0+)
- [ ] Admin access confirmed
- [ ] Repository cloned/downloaded from GitHub
- [ ] Backup existing workflows (if any)

## Import Order (Follow Exactly!)

### Phase 1: Error Management
- [ ] Import `SWF018 - HTTP Request Error Management.json`
- [ ] Import `WF010 - Oops Something is Wrong.json`
- [ ] Test: Both workflows import without errors

### Phase 2: Sub-workflows (All from /modules folder)
- [ ] Import `SWF007 - Upload to YT.json`
- [ ] Import `SWF008 - Download File for given URL.json`
- [ ] Import `SWF009 - Upload to GGL.json`
- [ ] Import `SWF011 - Generate Images.json`
- [ ] Import `SWF012 - Generate Slideshow.json`
- [ ] Import `SWF013 - Add Slideshow Captions.json`
- [ ] Import `SWF014 - Generate Script.json`
- [ ] Import `SWF016 - Find GGL Drive Path.json`
- [ ] Import `SWF019 - Find GGL Document File.json`
- [ ] Import `SWF020 - Temporary Binary File Storage.json`
- [ ] Import `SWF022 - Update Video Ledger Spreadsheet.json`
- [ ] Import `SWF026 - Create GGL Sheets File EBOYD53.json`
- [ ] Import `SWF026 - Create GGL Sheets File.json`
- [ ] Import `SWF026-- Create GGL Sheets File SSADVISOR.json`
- [ ] Import `SWF030 - Download And Save.json`
- [ ] Test: All sub-workflows visible in workflow list

### Phase 3: Webhooks
- [ ] Import `WF003 - Webhook File Server.json`
- [ ] Test: Webhook workflow imports successfully

### Phase 4: Main Workflow
- [ ] Import `WF002 - SciFy Shorts.json`
- [ ] Test: Main workflow imports without dependency errors

## Post-Import Configuration

### Credentials Setup
- [ ] Create YouTube API credentials (name: EBOYD53 or update workflow)
- [ ] Create Google Drive/Sheets credentials (name: EBOYD53 or update workflow)
- [ ] Create AI/LLM API credentials (OpenAI or equivalent)
- [ ] Create Image Generation service credentials
- [ ] Test all credentials connectivity

### Workflow Configuration
- [ ] Open `WF002 - SciFy Shorts` main workflow
- [ ] Locate "Workflow Controls" set node
- [ ] Update credential names to match your setup
- [ ] Set `yt_upload: false` for initial testing
- [ ] Set `yt_privacy: "private"` for testing
- [ ] Save workflow changes

### Dependency Verification
- [ ] Check all "Execute Workflow" nodes have correct IDs
- [ ] Verify no "red" error nodes in main workflow
- [ ] Confirm all sub-workflows are properly linked
- [ ] Update any mismatched workflow IDs

## Testing Phase

### Individual Component Tests
- [ ] Test SWF014 (Generate Script) with sample data
- [ ] Test SWF011 (Generate Images) with test prompts
- [ ] Test SWF012 (Generate Slideshow) functionality
- [ ] Test credential-dependent workflows
- [ ] Verify error handling workflows respond correctly

### Integration Test
- [ ] Run main workflow manually (with testing settings)
- [ ] Monitor execution logs for errors
- [ ] Verify file generation works
- [ ] Check API calls succeed
- [ ] Confirm no credential errors

### Production Readiness
- [ ] Set `yt_upload: true` (when ready for live uploads)
- [ ] Set `yt_privacy: "public"` (when ready for public videos)
- [ ] Configure schedule trigger as needed
- [ ] Set up monitoring/alerting
- [ ] Document any customizations made

## Troubleshooting Quick Fixes

### Common Issues:
- **"Workflow not found"**: Update Execute Workflow node IDs
- **Credential errors**: Check credential names match exactly
- **Import hangs**: Try importing one file at a time
- **Nodes show errors**: Verify all required fields populated

### Emergency Commands:
```bash
# View n8n logs
docker logs n8n-container

# Restart n8n if needed
docker restart n8n-container

# Check workflow IDs via API
curl -X GET http://localhost:5678/api/v1/workflows
```

## Success Indicators ✅

- [ ] All workflows imported without errors
- [ ] No red error indicators in workflow nodes
- [ ] All credentials test successfully
- [ ] Main workflow executes end-to-end without failures
- [ ] Generated content appears in expected locations
- [ ] System ready for production use

## Notes Section

**Your Credential Names:**
- YouTube: ________________
- Google Drive: ________________
- AI/LLM: ________________
- Image Gen: ________________

**Workflow IDs (if different):**
- SWF014: ________________
- SWF011: ________________
- SWF012: ________________
- SWF013: ________________
- SWF007: ________________
- SWF009: ________________
- SWF022: ________________

**Custom Settings:**
- Schedule: ________________
- Privacy: ________________
- Upload destinations: ________________

---
**Import Date:** _______________
**n8n Version:** _______________
**Status:** _______________