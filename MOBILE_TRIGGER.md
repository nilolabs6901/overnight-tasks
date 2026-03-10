# How to Trigger Your Overnight Tasks from Your Phone

You have three ways to kick off the task runner whenever you want — no need to wait for midnight.

---

## Option 1: GitHub Mobile App (Easiest)

1. Download the **GitHub** app from the App Store (free).
2. Sign in with your `nilolabs6901` account.
3. Open the **overnight-tasks** repository.
4. Tap the **Actions** tab at the bottom.
5. Tap **Overnight Tasks** in the workflow list.
6. Tap **Run workflow** in the top right.
7. Tap the green **Run workflow** button.

That's it — Claude will start working through your issues immediately.

---

## Option 2: One-Tap iPhone Shortcut (Best for Quick Access)

This creates a button on your home screen that triggers the workflow in one tap.

### Step 1: Create a Personal Access Token

1. Go to https://github.com/settings/tokens?type=beta on your phone or computer.
2. Tap **Generate new token**.
3. Name it: `overnight-tasks-trigger`
4. Set expiration to **90 days** (or "No expiration" if you prefer, but less secure).
5. Under **Repository access**, select **Only select repositories** and pick `overnight-tasks`.
6. Under **Permissions**, expand **Repository permissions** and set:
   - **Contents**: Read-only
   - **Metadata**: Read-only (this is automatic)
7. Tap **Generate token**.
8. **Copy the token immediately** — you won't be able to see it again. It starts with `github_pat_`.

### Step 2: Create the iPhone Shortcut

1. Open the **Shortcuts** app on your iPhone.
2. Tap the **+** in the top right to create a new shortcut.
3. Tap **Add Action**.
4. Search for **Get Contents of URL** and tap it.
5. Configure it like this:
   - **URL**: `https://api.github.com/repos/nilolabs6901/overnight-tasks/dispatches`
   - **Method**: POST
   - **Headers** (tap "Add new header" for each):
     - `Accept` → `application/vnd.github+json`
     - `Authorization` → `Bearer YOUR_TOKEN_HERE` (replace YOUR_TOKEN_HERE with the token you copied)
     - `X-GitHub-Api-Version` → `2022-11-28`
   - **Request Body**: JSON
     - Add a key: `event_type` with value: `run-tasks`
6. Tap the name at the top and rename it to **Run Tasks**.
7. Tap the icon to pick an emoji (try the robot 🤖 or moon 🌙).
8. Tap **Done**.
9. To add it to your home screen: Long-press the shortcut > **Details** > **Add to Home Screen**.

Now you have a one-tap button on your home screen that fires off Claude.

---

## Option 3: Curl Command (For Automations or Zapier)

If you want to trigger the workflow from any tool that can make HTTP requests:

```bash
curl -X POST \
  https://api.github.com/repos/nilolabs6901/overnight-tasks/dispatches \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  -d '{"event_type": "run-tasks"}'
```

Replace `YOUR_TOKEN_HERE` with your Personal Access Token.

A successful trigger returns an empty response with a `204 No Content` status — that means it worked.

---

## Quick Reference

| What | Value |
|------|-------|
| **Repository** | `nilolabs6901/overnight-tasks` |
| **Webhook URL** | `https://api.github.com/repos/nilolabs6901/overnight-tasks/dispatches` |
| **Event type** | `run-tasks` |
| **Issues tab** | https://github.com/nilolabs6901/overnight-tasks/issues |
| **Actions tab** | https://github.com/nilolabs6901/overnight-tasks/actions |

---

## Renewing Your Token

Your Personal Access Token will expire based on the expiration you chose. When it expires:
1. Go to https://github.com/settings/tokens?type=beta
2. Click on `overnight-tasks-trigger`
3. Click **Regenerate token**
4. Copy the new token and update it in your iPhone Shortcut
