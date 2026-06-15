# Career Coach — Free & Private

Three honest job-search coaches that run **100% in your browser**. No servers, no accounts,
no AI fees, and your resume/LinkedIn data never leaves your device.

| Tab | What it does |
|-----|--------------|
| 📄 **Resume Optimizer** | Paste a job description + your resume → match score, missing keywords, and a **tailored resume** (your real content, reordered and aligned to the job). |
| 🎤 **Interview Coach** | Paste the JD → likely questions, a STAR answer builder, and smart questions to ask them. |
| 🤝 **Referral Coach** | Paste your LinkedIn text + target role → personalized, honest outreach drafts (connection request, referral ask, follow-up). |

> **Honest by design:** nothing here fabricates experience or hides text. It helps you present
> *what's true* in the strongest, most job-specific way. (Hidden "white text" keyword tricks get
> resumes auto-rejected and are treated as fraud — this tool intentionally doesn't do that.)

---

## Run it locally

Just open `index.html` in any modern browser. That's it — no build step, no install.

---

## Optional: on-device AI rephrasing (free & private)

On the Resume Optimizer tab there's a checkbox: **"🧠 Enable on-device AI rephrasing"**.

- It downloads a **small AI model that runs entirely in your browser** using WebGPU
  ([WebLLM](https://github.com/mlc-ai/web-llm)). Nothing is sent to any server — still 100% private and free.
- **First use** downloads the model (~1–2 GB), then it's cached for next time.
- **Requirements:** Chrome or Edge 113+ on a device with a reasonably modern GPU (WebGPU enabled).
- If your browser/device doesn't support it, the rest of the app still works fully — the AI part just stays off.

**How to use it:**
1. Fill in your resume + job description, click **Analyze match**.
2. Tick **Enable on-device AI rephrasing** and wait for "✅ On-device AI ready".
3. Click **✨ AI-enhance bullets** in the Tailored resume box.
4. **Review every change** — keep only edits that stay 100% true to your real experience.

---

## Deploy free on Azure Static Web Apps (auto-deploys on every commit)

Azure **Static Web Apps** has a **Free tier** and deploys automatically every time you push to
GitHub. Here's the full path from these files to a live site.

### Step 1 — Put the project on GitHub
1. Create a new repo on GitHub (e.g. `career-coach`). Keep it **public** or private — both work.
2. In this folder, run:
   ```powershell
   git init
   git add .
   git commit -m "Initial commit: Career Coach"
   git branch -M main
   git remote add origin https://github.com/<your-username>/career-coach.git
   git push -u origin main
   ```

### Step 2 — Create the Azure Static Web App
1. Go to the [Azure Portal](https://portal.azure.com) → search **Static Web Apps** → **Create**.
   (You need a free Azure account — a credit card may be required for signup, but the Free SWA tier costs $0.)
2. Fill in:
   - **Subscription / Resource Group:** create or pick one.
   - **Name:** `career-coach`.
   - **Plan type:** **Free**.
   - **Region:** pick the closest.
   - **Deployment source:** **GitHub** → sign in and authorize Azure.
   - **Organization / Repository / Branch:** select your repo and `main`.
3. **Build Details:**
   - **Build Presets:** **Custom**.
   - **App location:** `/`
   - **Api location:** *(leave empty)*
   - **Output location:** *(leave empty)*
4. Click **Review + create** → **Create**.

### Step 3 — Let Azure wire up GitHub Actions
- When you connect the repo, Azure **automatically commits a workflow file** to
  `.github/workflows/` **and** adds the secret `AZURE_STATIC_WEB_APPS_API_TOKEN` to your repo.
- That kicks off the first deployment immediately. Watch it under your repo's **Actions** tab.
- After it finishes (1–2 min), your site is live at a URL like
  `https://<random-name>.azurestaticapps.net`.

> **About the included workflow file:** this repo already ships
> `.github/workflows/azure-static-web-apps.yml`. If Azure also generates its own workflow, you'll
> have two — just **delete one** (keep Azure's auto-generated one, since it's guaranteed to match
> the secret name Azure created). The included file uses the standard secret name
> `AZURE_STATIC_WEB_APPS_API_TOKEN`, so if you prefer it, delete Azure's generated file instead.

### Step 4 — Continuous deployment (the part you asked for)
From now on, **every `git push` to `main` automatically rebuilds and redeploys the site**:
```powershell
# make any edit, then:
git add .
git commit -m "Update coach"
git push
```
Within ~1–2 minutes the live site reflects your change. Pull requests also get a free
**preview URL** automatically, which is closed when the PR is merged/closed.

---

## Project structure

```
career-coach/
├─ index.html                       # the entire app (3 tabs, all logic)
├─ staticwebapp.config.json         # Azure SWA routing/headers
├─ .gitignore
└─ .github/workflows/
   └─ azure-static-web-apps.yml     # CI/CD: deploy on every push to main
```

## Privacy

All processing happens in your browser. The only optional network calls are:
- the **PDF reader** (loaded from a CDN only if you upload a PDF), and
- the **on-device AI model** download (only if you enable the AI toggle).

Neither sends your resume anywhere — they run locally after loading.
