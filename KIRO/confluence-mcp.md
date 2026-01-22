# Confluence MCP Usage Rules

These rules govern all interactions with Confluence through the MCP tools.

## Error Handling - CRITICAL

### Rate Limit Errors

When encountering `Rate limit exceeded` or similar throttling errors:

1. **STOP immediately** - Do not continue with assumptions or cached data
2. **Inform the user** clearly about the rate limit
3. **Offer alternatives:**
   - Wait a few minutes and retry
   - Ask user to copy/paste the page content directly
   - Ask user to download and share the page as a file

**Never** proceed to answer questions about Confluence content if the API call failed.

### Access/Permission Errors

When encountering `403 Forbidden`, `401 Unauthorized`, or "page not found" errors:

1. **STOP immediately**
2. **Inform the user** that access was denied
3. **Request alternative:** Ask user to either:
   - Provide the page content directly (copy/paste)
   - Download the page and share as attachment
   - Verify they have access and the page ID/URL is correct

### Any Other API Errors

For any Confluence API failure:
1. **Do not fabricate or assume content**
2. **Report the exact error** to the user
3. **Ask for guidance** on how to proceed

## General Usage

- Confluence MCP tools are read-only, so no confirmation needed before reading
- Always verify successful response before using content in answers
- If content retrieval fails, the user's question cannot be answered from that source

## Response Pattern for Errors

```
I couldn't retrieve the Confluence page due to: [exact error]

Options:
1. Wait a few minutes and I'll retry
2. Copy/paste the relevant content here
3. Download the page and attach it

How would you like to proceed?
```
