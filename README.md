# Automated House Search

Local n8n automation for collecting house listing alerts, extracting listing details, and writing them to Google Sheets.

## Requirements

- Docker Desktop
- Docker Compose
- A Google account with access to Gmail and Google Sheets
- A Google Cloud OAuth client for n8n Gmail/Sheets credentials

## Run Locally

Start n8n:

```powershell
docker compose up -d
```

Open n8n:

```text
http://localhost:5678
```

Check that n8n is running:

```powershell
docker compose ps
```

Health check:

```powershell
Invoke-WebRequest -UseBasicParsing http://localhost:5678/healthz
```

Stop n8n:

```powershell
docker compose down
```

## Google OAuth Setup

In Google Cloud Console, create an OAuth client with type `Web application`.

Authorized JavaScript origin:

```text
http://localhost:5678
```

Authorized redirect URI:

```text
http://localhost:5678/rest/oauth2-credential/callback
```

If the OAuth app is in testing mode, add your Gmail address under:

```text
Google Auth Platform -> Audience -> Test users
```

Enable the APIs you use:

- Gmail API
- Google Sheets API

Do not commit downloaded OAuth client secret JSON files. Keep them local and paste the Client ID/Client Secret into n8n credentials.

## Workflow Outline

The intended n8n workflow is:

```text
Gmail Trigger
-> Extract listing fields
-> Google Sheets lookup by listing URL
-> IF not duplicate
-> Append row to Google Sheets
```

Use the listing URL as the dedupe key. Titles and prices can change, but the URL should identify the listing.

## Duplicate Prevention

Before appending to Google Sheets, search the sheet for the extracted `listing_url`.

In the Google Sheets lookup node:

- Lookup column: `listing_url`
- Lookup value: `{{ $('Extract listing').item.json.listing_url }}`
- Enable `Always Output Data` in node settings if the IF node does not run when no row is found

In the IF node:

- Check whether the lookup result is empty
- `true` means no duplicate was found, append the row
- `false` means the listing already exists, skip it

## Troubleshooting

If Docker Hub image pulls fail with a CloudFront `EOF`, this project uses the GitHub Container Registry image instead:

```yaml
image: ghcr.io/n8n-io/n8n:latest
```

If Google login fails with `redirect_uri_mismatch`, verify the redirect URI is exactly:

```text
http://localhost:5678/rest/oauth2-credential/callback
```

If Google login fails with `access_denied` while the app is in testing mode, add your Gmail account as a test user.

If the Gmail trigger only returns one email, that is normal in manual testing. Activate the workflow and test with a new incoming email, or use a Gmail search/get-many node to process older emails.
