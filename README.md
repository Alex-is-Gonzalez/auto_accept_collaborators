# Auto Accept Collaborator Requests

This project automates the process of accepting collaborator requests using GitHub Actions. This is especially useful if you receive frequent collaboration invitations and want to streamline the onboarding process.

---

##  **Features**

* Automatically accepts collaborator invitations every 5 minutes.
* Uses GitHub Actions to poll for pending invitations.
* Securely leverages GitHub Personal Access Tokens (PAT).

---
## **Step: 0 : Clone repo
 -If this repo is cloned, you will be skipping step 3 as the workflow will already be inside the clone

##  **Step 1: Generate a GitHub Personal Access Token (PAT)**

1. Navigate to **GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)**.
2. Click **Generate new token** → **Generate new token (classic)**.
3. Add a **Note** (e.g., `Auto Collab Accept`).
4. Set the **Expiration** (recommended: 90 days or 180 days).
5. Under **Select Scopes**, enable the following:

   * `repo` → Full control of private repositories.
   * `admin:repo_hook` → Read and write repository hooks.
   * `write:repo_hook` → To accept invites.
6. Click **Generate Token**.
7. **Copy the token immediately**—you won't be able to see it again.

---

## **Step 2: Add the Token to Your Repository**

1. Go to your repository on GitHub.
2. Navigate to **Settings → Secrets and variables → Actions → New repository secret**.
3. Name the secret **PERSONAL\_ACCESS\_TOKEN**.
4. Paste the token you copied in Step 1.
5. Click **Add secret**.

---

##  **Step 3: Create the GitHub Action Workflow**

1. In the root of your repository, create the following folder structure:

   ```plaintext
   .github/
   └── workflows/
       └── auto-accept.yml
   ```

2. Inside `auto-accept.yml`, add the following content:

   ```yaml
   name: Auto Accept Collabs

   on:
     schedule:
       - cron: '*/5 * * * *' # Runs every 5 minutes

   jobs:
     file_sync:
       runs-on: ubuntu-latest
       steps:
         - name: Fetching Local Repository
           uses: actions/checkout@v3

         - name: Auto Accept Collabs
           uses: kbrashears5/github-action-auto-accept-collabs@v2.0.0
           with:
             TOKEN: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
   ```

---

##  **Step 4: Test the Workflow**

1. Send a collaborator invite to your GitHub account from another repository.
2. Wait 5 minutes for the cron job to execute.
3. Check if you have been automatically added to the repository.

---

##  **Troubleshooting**

* If you see an error like `Missing download info`, verify the version (`main`) of the GitHub Action. Currently this action is running V 2.0.0
* Ensure your `PERSONAL_ACCESS_TOKEN` is correctly named and has the right permissions.
* Check the Action logs for more details if you do not see the auto-accept happening.

---


