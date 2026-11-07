# EZtrash website

The EZtrash rules website, set up to run online for free on **Render**, with the rules kept in this
**GitHub repo**. No credit card, no plan to pick, and nothing to renew. The link works whether or not the
can's Raspberry Pi is on.

```
phone / laptop -> https://eztrash.onrender.com -> saves every rule change to this GitHub repo
                                                          |
                                     the can (Raspberry Pi) reads the rules from here
                                     every 90 seconds while the sorter runs
```

The website saves rules to GitHub because Render's free websites lose their files whenever they restart
or sleep. GitHub keeps them. The can reads the rules straight from GitHub, so it keeps working even while
the website is asleep.

## 1. Put this folder on GitHub (once)

1. On github.com, click **+** (top right), then **New repository**.
2. Name it `eztrash-website` and make it **Public** (the can reads the rules from it without a password).
   Don't add a README. Click **Create repository**.
3. Click **uploading an existing file**, drag in everything inside this folder (including the `presets`
   and `rulebooks` folders), and click **Commit changes**.

## 2. Make a GitHub token (once)

The token lets the website save rules into the repo.

1. On GitHub, click your picture, then **Settings**, then **Developer settings** (at the bottom),
   then **Personal access tokens**, then **Fine-grained tokens**, then **Generate new token**.
2. Name it `eztrash`. Under **Repository access**, choose **Only select repositories** and pick
   `eztrash-website`.
3. Under **Permissions**, find **Contents** and set it to **Read and write**.
4. Pick the longest expiration it offers. Click **Generate token** and copy it. GitHub only shows it once.

## 3. Start the website on Render (once)

1. Go to **render.com** and sign up with your GitHub account. It's free and doesn't ask for a card.
2. Click **New**, then **Blueprint**, and pick the `eztrash-website` repo. Render reads `render.yaml`.
3. It asks for three things:
   - `EZTRASH_ACCESS_CODE`: the 6 capital letters people type to sign in, like `KXQMRB`
   - `EZTRASH_GITHUB_REPO`: your repo, like `yourname/eztrash-website`
   - `EZTRASH_GITHUB_TOKEN`: the token from step 2
4. Click **Apply** (or **Deploy Blueprint**) and wait a few minutes. Your link is shown at the top of the
   service's page, like `https://eztrash.onrender.com` (Render adds letters if that name is taken).

## 4. Connect the can (once)

1. Copy the new `cloud_sync.py`, `run_sorter.sh` and `start.sh` onto the Pi
   (`start.sh` goes on the Desktop, the others go in `~/Downloads`).
2. On the website, click the **QR button** at the top. The **Connect the can** box shows something like
   `github:yourname/eztrash-website`.
3. On the Pi, open the EZtrash launcher, press **8**, paste it, and press **Enter** when it asks for a
   sync key.

From then on, starting the sorter (launcher **3**) also keeps the rules in step with the website.
The launcher's **online** line shows what the can is connected to.

## Good to know

- **The website sleeps after 15 minutes with no visitors.** The next person to open it waits about a
  minute while it wakes up. The can isn't affected, because it reads from GitHub.
- **Every rule change shows up in the repo** as a commit ("EZtrash website: update rulebook.json"), so you
  can see what changed and when.
- **Change rules on the website, not on the Pi**, once the can is connected. While the sorter runs it
  replaces the Pi's rules with the website's. To go back to Pi-only, press **8**, then **d**.
- **The repo is public**: anyone can see the code and the rules (they're the same as the city's public
  rules). The access code and the token are kept in Render's settings, not in the repo.
- **If saving stops working**, the home page shows a red note. Usually the token has expired: make a new
  one (step 2) and paste it into Render under **Environment**, then **EZTRASH_GITHUB_TOKEN**.
- **To change the sign-in code**, edit `EZTRASH_ACCESS_CODE` under **Environment** on Render.
- **To update the website's code**, upload the new files to the repo, then on Render click
  **Manual Deploy**, then **Deploy latest commit**. It doesn't redeploy by itself, because the website
  commits a rule change to the repo every time someone saves.
