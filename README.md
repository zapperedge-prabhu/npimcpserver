# NPI Lookup Tool — User Manual for Health Care Professionals

A free, AI-powered NPI registry lookup tool that lets you search **9.3 million+ US healthcare providers** directly inside Claude AI. No login, no subscription, no forms to fill out.

**Server:** `https://npimcpserver.zapperedge.com/sse`
**Data Source:** CMS NPPES (updated weekly)
**Last Verified:** March 2026

---

## Table of Contents

- [What You Can Do](#what-you-can-do)
- [Step 1 — Set Up Claude (5 minutes)](#step-1--set-up-claude-5-minutes)
- [Step 2 — Set Up Other MCP Clients](#step-2--set-up-other-mcp-clients)
- [Step 3 — Verify It's Working](#step-3--verify-its-working)
- [How to Search — With Examples](#how-to-search--with-examples)
- [Test Cases for Medical Coders](#test-cases-for-medical-coders)
- [Tips & Tricks](#tips--tricks)
- [FAQ](#faq)

---

## What You Can Do

| Task | Example |
|------|---------|
| Look up a provider by NPI | *"Look up NPI 1083738934"* |
| Verify an NPI is valid | *"Is NPI 1234567890 valid?"* |
| Find doctors by name & state | *"Find Dr. Smith in California"* |
| Find specialists by taxonomy code | *"Find providers with taxonomy 207RC0000X in Houston TX"* |
| Find hospitals & clinics | *"Find organizations with Hospital in the name in Brooklyn NY"* |
| Search by ZIP code | *"Find providers in ZIP 90210"* |
| Verify a provider's specialty | *"What specialty is NPI 1538283494?"* |

> **Important — Specialty Searches:** The NPPES dataset does not include human-readable specialty descriptions — only taxonomy codes (e.g. `207RC0000X`). Searching a word like "cardiology" will not return results. Always use the taxonomy code. See the [Common Taxonomy Codes](#common-taxonomy-codes-for-medical-coders) table below.

---

## Step 1 — Set Up Claude (5 minutes)

Claude is the easiest MCP client to get started with. You just need a free Claude account.

### 1.1 — Create a Claude Account

Go to **[claude.ai](https://claude.ai)** and sign up for a free account if you don't have one.

> **Note:** MCP connectors work on all Claude plans including the free tier.

---

### 1.2 — Add the NPI MCP Server

1. Log in to **[claude.ai](https://claude.ai)**
2. Click your **profile icon** (top right corner)
3. Click **Settings**
4. In the left sidebar, click **Connectors**
5. Click the **"+ Add connector"** or **"Add MCP Server"** button
6. Fill in the form:

   | Field | Value |
   |-------|-------|
   | **Name** | `NPI Registry` *(or any name you like)* |
   | **SSE URL** | `https://npimcpserver.zapperedge.com/sse` |

7. Click **Save** or **Connect**

---

### 1.3 — Start a New Conversation

> **Important:** You must start a **new conversation** after adding the connector. It won't appear in existing chats.

1. Click **New Chat** (or the pencil icon)
2. Type:
   ```
   Check the NPI database status
   ```
3. Claude should respond with something like:
   > *"The NPI database is ready with 9,368,082 records, last updated March 8, 2026..."*

**You're connected!** You can now search the full NPI registry by just talking to Claude.

---

## Step 2 — Set Up Other MCP Clients

### Option B — Cursor (AI Code Editor)

If you use **[Cursor](https://cursor.sh)**:

1. Open Cursor
2. Press `Cmd+Shift+P` (Mac) or `Ctrl+Shift+P` (Windows)
3. Search for **"Open MCP Settings"** or go to **Settings → MCP**
4. Add the following to your `mcp.json` config file:

```json
{
  "mcpServers": {
    "npi-registry": {
      "url": "https://npimcpserver.zapperedge.com/sse",
      "transport": "sse"
    }
  }
}
```

5. Save the file and restart Cursor
6. Open the AI chat panel and type: `Check the NPI database status`

---

### Option C — Windsurf (Codeium)

If you use **[Windsurf](https://codeium.com/windsurf)**:

1. Open Windsurf
2. Go to **Settings → AI → MCP Servers**
3. Click **Add Server**
4. Enter:

```json
{
  "name": "NPI Registry",
  "url": "https://npimcpserver.zapperedge.com/sse",
  "transport": "sse"
}
```

5. Save and restart
6. Test by typing: `Check the NPI database status`

---

### Option D — Continue.dev (VS Code Extension)

If you use **[Continue](https://continue.dev)** in VS Code:

1. Open VS Code
2. Go to the Continue extension settings
3. Edit your `~/.continue/config.json` and add:

```json
{
  "experimental": {
    "modelContextProtocolServers": [
      {
        "transport": {
          "type": "sse",
          "url": "https://npimcpserver.zapperedge.com/sse"
        }
      }
    ]
  }
}
```

4. Reload VS Code
5. Test in the Continue chat: `Check the NPI database status`

---

### Option E — Any MCP-Compatible Client

For any other MCP client that supports SSE transport, use:

- **SSE Endpoint:** `https://npimcpserver.zapperedge.com/sse`
- **Messages Endpoint:** `https://npimcpserver.zapperedge.com/messages`
- **Transport:** `SSE`
- **Auth:** None required

---

## Step 3 — Verify It's Working

Once connected in any client, run this quick 3-step check:

**Check 1 — Database is live:**
```
Check the NPI database status
```
Expected: Record count ~9.3 million, status "ready"

**Check 2 — Lookup works:**
```
Look up NPI 1083738934
```
Expected: Returns "Lois M Lumpkin, Registered Nurse, Martinez CA"

**Check 3 — Search works:**
```
Find providers with taxonomy 207RC0000X in Modesto CA
```
Expected: Returns a list of cardiovascular disease (cardiology) providers with addresses and phone numbers

> Note: Use the taxonomy code `207RC0000X`, not the word "cardiology". See [Why Taxonomy Codes Are Required](#faq) in the FAQ.

If all 3 pass — you're good to go!

---

## How to Search — With Examples

Just talk to Claude naturally. Here are the most useful patterns for medical coders:

### Look Up a Specific NPI
```
Look up NPI 1538283494
```
```
What are the details for NPI 1234567890?
```
```
Who is NPI 1962485938?
```

---

### Validate an NPI Number
```
Is NPI 1538283494 a valid NPI?
```
```
Validate NPI 9999999999
```
> Useful for catching typos before submitting claims.

---

### Find a Provider by Name
```
Find Dr. Johnson in New York
```
```
Search for providers with last name PATEL in Texas
```
```
Look for a nurse named Catherine in California
```

---

### Find by Name + Specialty
```
Find providers with last name Smith and taxonomy 207RC0000X in Florida
```
```
Search for Dr. Williams with taxonomy 207X00000X in Chicago IL
```

---

### Find Specialists by Taxonomy Code

> **Specialty searches require taxonomy codes.** The NPPES database stores taxonomy codes only — descriptions like "cardiology" or "surgeon" are not stored and cannot be searched. Use the code table below.

```
Find providers with taxonomy 207RC0000X in Houston TX
```
```
Search for taxonomy 207Q00000X in ZIP code 900*
```

#### Common Taxonomy Codes for Medical Coders

| Code | Specialty |
|------|-----------|
| `207RC0000X` | Cardiovascular Disease (Cardiology) |
| `207Q00000X` | Family Medicine |
| `207R00000X` | Internal Medicine |
| `207X00000X` | Orthopedic Surgery |
| `207N00000X` | Dermatology |
| `207V00000X` | Obstetrics & Gynecology |
| `207T00000X` | Neurological Surgery |
| `2084P0800X` | Psychiatry |
| `208600000X` | Surgery (General) |
| `163W00000X` | Registered Nurse |
| `363L00000X` | Nurse Practitioner |
| `363A00000X` | Physician Assistant |

> Full taxonomy code list: [nucc.org](https://www.nucc.org/index.php/code-sets-mainmenu-41/provider-taxonomy-mainmenu-40)

---

### Find Hospitals & Organizations
```
Find organizations with Hospital in the name in Brooklyn NY
```
```
Find NPI-2 organizations with MERCY in the name in state OH
```
```
Find organizations with Medical Center in ZIP code 606*
```

---

### Search by ZIP Code
```
Find providers in ZIP code 77001
```
```
Find providers in ZIP 900* in California
```
> The `*` wildcard lets you search an entire ZIP prefix (e.g. all 77xxx codes in Houston).

---

### Pagination — Get More Results
```
Find physicians in Miami FL, show me the first 10
```
Then:
```
Show me the next 10
```

---

## Test Cases for Medical Coders

Use these to verify the tool works correctly for billing and coding scenarios.

---

### Test 1 — Validate a Known Good NPI
**Prompt:**
```
Validate NPI 1538283494
```
**Expected result:** Valid — format correct, Luhn check digit passes

---

### Test 2 — Validate a Bad NPI
**Prompt:**
```
Validate NPI 1234567890
```
**Expected result:** Invalid — fails Luhn check digit (useful for catching claim errors)

---

### Test 3 — Look Up an Individual Provider
**Prompt:**
```
Look up NPI 1538283494
```
**Expected result:**
- Name: Catherine Liane Winter, O.D.
- Type: Individual
- Specialty: Optometry (taxonomy `152W00000X`)
- Location: 1630 N Main St, Salinas, CA 93906
- Phone: (831) 443-4422

---

### Test 4 — Look Up an Organization (NPI-2)
**Prompt:**
```
Look up NPI 1689614562
```
**Expected result:**
- Organization: Valley Heart Associates Medical Group Inc
- Type: Organization
- Location: 1540 Florida Ave, Modesto CA

---

### Test 5 — Find a Provider by Name and State
**Prompt:**
```
Find providers with last name JOHNSON in New York
```
**Expected result:** List of providers named Johnson in NY with NPI numbers, specialties, and addresses

---

### Test 6 — Find Specialists in a City
**Prompt:**
```
Find providers with taxonomy 207RC0000X in Modesto California
```
**Expected result:** List of cardiovascular disease (cardiology) providers in Modesto CA

> Use the taxonomy code directly. Searching "cardiologists" as a word will not return results.

---

### Test 7 — Find Hospitals in a State
**Prompt:**
```
Find NPI-2 organizations with HOSPITAL in the name in New York state
```
**Expected result:** List of hospital organizations in NY with addresses

---

### Test 8 — Search by ZIP Code
**Prompt:**
```
Find providers in ZIP code 10001
```
**Expected result:** Providers in the 10001 ZIP code (Manhattan, NY)

---

### Test 9 — Wildcard Name Search
**Prompt:**
```
Find providers with last name starting with JOHN in California, limit 5
```
**Expected result:** Mix of JOHN, JOHNSON, JOHNS, JOHNSTONE etc. in CA

---

### Test 10 — Verify Provider Specialty
**Prompt:**
```
What specialty does NPI 1962485938 practice?
```
**Expected result:** Mohamed Hassan, MD — Cardiovascular Disease (taxonomy `207RC0000X`), Modesto CA

---

### Test 11 — Confirm Provider is Active in a State
**Prompt:**
```
Is there a provider named WINTER practicing in California?
```
**Expected result:** List of providers named Winter in CA with their specialties

---

### Test 12 — Pagination Test
**Prompt:**
```
Find providers with last name PATEL in California, show 5 results
```
Then:
```
Show the next 5
```
**Expected result:** Two distinct pages of results with no duplicate NPIs

---

### Quick Pass Checklist

| # | Test | Pass if... |
|---|------|------------|
| 1 | Validate good NPI | Returns "valid" |
| 2 | Validate bad NPI | Returns "invalid" |
| 3 | Lookup individual | Returns full provider details |
| 4 | Lookup organization | Returns org name and address |
| 5 | Name + state search | Returns provider list |
| 6 | City + taxonomy code | Returns specialists in that city |
| 7 | Hospital org search | Returns hospital organizations |
| 8 | ZIP code search | Returns providers in that ZIP |
| 9 | Wildcard search | Returns multiple name variants |
| 10 | Specialty verification | Returns correct taxonomy |
| 11 | State provider check | Returns providers in state |
| 12 | Pagination | Page 2 has different results than page 1 |

All 12 passing = Tool is fully working for medical coding use.

---

## Tips & Tricks

**Be conversational** — You don't need to use exact commands. Just ask naturally:
> *"I need to verify the NPI for a cardiologist in Houston"*
> *"Can you find all the orthopedic surgeons in ZIP 60601?"*

**Always use taxonomy codes for specialty searches** — The database stores codes, not descriptions. Searching "cardiology", "surgeon", or "dermatologist" will return no results. Use the exact code:
> Instead of: *"Find cardiologists in Houston"*
> Use: *"Find providers with taxonomy 207RC0000X in Houston TX"*

**Narrow down large results** — If you get too many results, add more filters:
> *"Find Smith in CA"* → too many results
> *"Find Dr. Smith with taxonomy 208600000X in Los Angeles CA"* → more specific

**Wildcard for partial matches** — Add `*` to catch name variations:
> `JOHN*` matches JOHN, JOHNSON, JOHNSTONE, JOHNS

**ZIP prefix search** — Use `900*` to search an entire area code prefix:
> `900*` = all ZIP codes from 90000–90099 (West LA area)

**Verify before submitting claims** — Always confirm:
1. NPI is valid (passes Luhn check)
2. Provider specialty matches what was billed
3. Practice address matches claim address

---

## FAQ

**Q: Is this data official and up to date?**
A: Yes. Data comes directly from CMS NPPES and is refreshed every 7 days. It's the same source as the official NPI Registry at nppes.cms.hhs.gov.

**Q: Is it free to use?**
A: Yes, completely free. The MCP server is hosted and open for public testing. Claude has a free tier at claude.ai.

**Q: How current is the data?**
A: Updated weekly from CMS. You can ask *"Check the NPI database status"* to see the exact last update date.

**Q: Why can't I search by specialty name like "cardiologist" or "surgeon"?**
A: The NPPES dataset does not include human-readable specialty descriptions — only taxonomy codes like `207RC0000X`. The raw data published by CMS omits the description column entirely. To search by specialty, use the taxonomy code. See the [Common Taxonomy Codes](#common-taxonomy-codes-for-medical-coders) table above, or look up any code at [nucc.org](https://www.nucc.org/index.php/code-sets-mainmenu-41/provider-taxonomy-mainmenu-40).

**Q: Can I look up deactivated NPIs?**
A: The database includes all records from NPPES. Provider status (active/inactive) is included in the results.

**Q: What if I get no results?**
A: Try broadening your search — remove one filter at a time. For specialty searches, make sure you are using a taxonomy code, not a description word. Also note the search matches the *beginning* of names (e.g. `SMITH` also returns `SMITHA`).

**Q: Can I use this for automated/bulk lookups?**
A: For bulk automation, contact the server maintainer about API access. Claude is best suited for individual lookups and small batch queries.

**Q: My MCP client isn't listed. Will it work?**
A: Any MCP client that supports SSE transport should work. Use the SSE URL: `https://npimcpserver.zapperedge.com/sse`

---

## Support

If the server is down or returning errors:
- Check server status: `https://npimcpserver.zapperedge.com/health`
- Ask Claude: *"Check the NPI database status"*

For issues or feature requests, drop a note at feature-request@zapperedge.com.

---

*Data provided by CMS NPPES — US Department of Health & Human Services. Updated weekly.*
