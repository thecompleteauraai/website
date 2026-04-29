# Aura Intake — n8n Setup Guide
## Complete in ~15 minutes, zero coding required

---

## Step 1 — Sign up for n8n.cloud

1. Go to **https://n8n.io** → click **Get started free**
2. Create your account (no credit card needed for trial)
3. You'll land on your n8n dashboard

---

## Step 2 — Import the workflow

1. In your n8n dashboard, click **"+ New workflow"**
2. Click the **"..."** menu (top right) → **"Import from file"**
3. Upload **`aura_n8n_workflow.json`**
4. The workflow will appear with 6 nodes already connected

---

## Step 3 — Connect Google Sheets

1. Click the **"Google Sheets — Log Lead"** node
2. Click **"Credential"** → **"Create new"** → sign in with your Google account
3. Create a new Google Sheet called **"Aura Lead Dashboard"**
4. Add a sheet tab named **"Leads"** with these column headers in row 1:

   `Timestamp | Name | Email | Industry | Bottleneck | Requirements | Returning | Entry Page | Referrer | Files`

5. Copy the Sheet ID from the URL:
   `https://docs.google.com/spreadsheets/d/`**`THIS_PART`**`/edit`
6. Paste it into the **Document ID** field in the node
7. Click **Save**

---

## Step 4 — Connect Gmail

1. Click the **"Gmail — Send Lead Email"** node
2. Click **"Credential"** → **"Create new"** → sign in with `thecompleteauraai@gmail.com`
3. Grant the requested permissions
4. The **To** field is already set to `thecompleteauraai@gmail.com`
5. Do the same for the **"Gmail — Auto Reply to Lead"** node (same Gmail credential)

---

## Step 5 — Add your Google Sheet link to the email

1. Open the **"Gmail — Send Lead Email"** node
2. Find the text: `YOUR_GOOGLE_SHEET_LINK`
3. Replace it with your actual Google Sheet URL
4. Click **Save**

---

## Step 6 — Get your webhook URL

1. Click the **"Webhook — Aura Intake"** node
2. Click **"Listen for test event"** — this activates the webhook
3. Copy the webhook URL shown — it looks like:
   `https://your-name.app.n8n.cloud/webhook/aura-intake`

---

## Step 7 — Paste the webhook URL into your website

1. Open **`index.html`** in any text editor (Notepad, VS Code, etc.)
2. Press **Ctrl+F** (Find) and search for:
   `https://YOUR_N8N_WEBHOOK_URL/webhook/aura-intake`
3. Replace it with your actual webhook URL from Step 6
4. Save the file

---

## Step 8 — Test it

1. Open `index.html` in your browser
2. Click the ✦ button (bottom right)
3. Complete the full chat flow and upload a test file
4. Check your n8n dashboard — the workflow should trigger
5. Check `thecompleteauraai@gmail.com` for the lead email
6. Check your Google Sheet for the new row

---

## Step 9 — Activate the workflow

Once testing works:
1. Click the **"Activate"** toggle (top right in n8n) to turn it ON
2. Your workflow is now live 24/7

---

## What each node does

| Node | Purpose |
|---|---|
| Webhook — Aura Intake | Receives the form data + files from the chatbot |
| Respond Immediately | Sends instant 200 OK so the chatbot doesn't freeze |
| Set — Format Fields | Cleans and formats all the data for email/sheets |
| Google Sheets — Log Lead | Appends a new row to your spreadsheet |
| Gmail — Send Lead Email | Sends the executive brief to you with WhatsApp + email reply buttons |
| Gmail — Auto Reply to Lead | Sends a confirmation email to the visitor |

---

## File handling note

n8n receives uploaded files as binary attachments on the webhook.
The Gmail node is configured to attach them directly to the email you receive.
No extra storage needed — files arrive as email attachments.

For files larger than 5MB, n8n cloud free tier may time out.
If this is an issue, upgrade to n8n Starter ($20/mo) or self-host.

---

## Troubleshooting

**Webhook not receiving data?**
- Make sure the workflow is **Activated** (toggle is blue)
- Check the webhook URL in `index.html` matches exactly

**Google Sheets error?**
- Re-authenticate the Google credential
- Make sure the Sheet name is exactly **"Leads"** (capital L)

**Gmail not sending?**
- Gmail has a daily send limit of 500 emails on free accounts
- For high volume, switch to the SMTP node with SendGrid (free 100/day)

**CORS error in browser console?**
- This is expected when testing locally from `file://`
- It will work correctly once deployed to your live domain (thecompleteaura.com)
- To test locally: use a local server (`npx serve .` in terminal)
