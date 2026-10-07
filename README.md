# Rebel Ink — Web Service deploy (no Blueprint required)

This version is meant to be deployed as a normal **Render Web Service**.  
You create a PostgreSQL database yourself and paste three environment variables.

## Deploy steps (do these in order)

### 1. Push the code
- Put the contents of this folder at the **root** of a GitHub repository.
- You should see `server.js`, `package.json`, and `public/` at the root.

### 2. Create a PostgreSQL database
1. Render Dashboard → **New → PostgreSQL**
2. Name it anything (e.g. `rebel-ink-db`)
3. Plan: **Free**
4. Create the database and wait until it is **Available**

### 3. Create the Web Service
1. Render Dashboard → **New → Web Service**
2. Connect the same GitHub repository
3. Settings:
   - **Runtime**: Node
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Instance type**: Free
4. **Before** clicking Deploy, open the **Environment** section and add:

| Key | Value |
|-----|--------|
| `DATABASE_URL` | Paste the **Internal Database URL** from the Postgres page (starts with `postgresql://`) |
| `JWT_SECRET` | Any long random string (at least 32 characters). Example: open a terminal and run `openssl rand -hex 32` |
| `ADMIN_SETUP_TOKEN` | Another long random string (at least 24 characters). Example: `openssl rand -hex 24` |

5. Click **Create Web Service** / Deploy.

### 4. First admin setup
1. Open the Web Service **Logs**
2. Look for:
   ```
   FIRST ADMIN SETUP REQUIRED. Open /admin-setup?token=...
   ```
3. Copy the full URL (it includes your `ADMIN_SETUP_TOKEN`) and open it in the browser
4. Choose your admin email and password
5. After that, `/admin-setup` permanently closes

## If you already have a Web Service
1. Create the Postgres database (step 2 above) if you don’t have one
2. Go to the Web Service → **Environment**
3. Add the three variables listed above
4. Save → **Manual Deploy → Deploy latest commit**

## Notes
- Free Postgres expires after 30 days. Upgrade the database plan before then if you need the data longer.
- Do **not** use the External Database URL unless you know you need it; prefer the **Internal** one.
- Never commit real secrets to GitHub.
