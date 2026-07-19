# Wren — how to put this on the internet

You have three files. Keep this exact structure:

```
wren/
├── index.html      ← the webpage (safe for anyone to read)
├── api/
│   └── chat.js     ← the server (holds your secret key)
└── README.md       ← this file
```

The `api` folder name matters. Vercel looks for a folder called `api` and
automatically turns every file inside it into a tiny server. Rename it and
nothing works.

---

## Step 1 — Get an Anthropic API key (10 minutes, ~$5)

1. Go to **console.anthropic.com** and sign up. This is a *separate account*
   from the Claude chat app you're using now — different product, different bill.
2. Go to **Billing** and add credit. The minimum is $5. That is genuinely
   enough for hundreds of demo conversations.
3. Go to **API Keys** → **Create Key**. Copy it. It starts with `sk-ant-`.
4. Paste it somewhere safe for the next ten minutes. You cannot view it again
   after you close that window — you'd have to make a new one.

**Do not** put this key in `index.html`. Do not put it in a GitHub repo. Do not
paste it in Slack. It is a credit card number.

---

## Step 2 — Put the code on GitHub (15 minutes)

Vercel deploys *from* GitHub, so the code needs to live there first.

1. Make a free account at **github.com**.
2. Click **New repository**. Name it `wren`. Choose **Public**. Click Create.
3. On the empty repo page, click **uploading an existing file**.
4. Drag in `index.html` and `README.md`.
5. For the server file: click **Add file → Create new file**. In the name box,
   type `api/chat.js` — typing the slash creates the folder automatically.
   Paste the contents of `chat.js` in, and click **Commit changes**.

You now have your project on GitHub. Notice the key is nowhere in it. Good.

---

## Step 3 — Deploy on Vercel (5 minutes)

1. Go to **vercel.com** → **Sign up** → **Continue with GitHub**.
2. Click **Add New → Project**. Your `wren` repo will be listed. Click **Import**.
3. **Before you click Deploy**, expand **Environment Variables**. This is the
   important part. Add:

   | Name | Value |
   |------|-------|
   | `ANTHROPIC_API_KEY` | `sk-ant-...` (paste your key) |

   This is how the key reaches your server without ever being in your code.
   `process.env.ANTHROPIC_API_KEY` in `chat.js` is reading exactly this.

4. Click **Deploy**. Wait about a minute.

You'll get a URL like `wren-abc123.vercel.app`. That's your live demo. Send it
to anyone.

---

## If something breaks

**The chat says "That didn't send."** → Something failed on the server. In
Vercel, go to your project → **Logs**. The error will be printed there in plain
English. The usual culprit: the environment variable is misspelled. It must be
exactly `ANTHROPIC_API_KEY`.

**You changed the key and it still fails** → Vercel only picks up new
environment variables on a fresh deploy. Go to **Deployments** → the "..." menu
on the newest one → **Redeploy**.

**Nothing loads at all** → Make sure your file is named `index.html`, lowercase.

---

## What this costs

Vercel hosting: **$0**. The free tier is generous and a demo won't dent it.

Claude API: roughly **a fraction of a cent per conversation**. Your $5 will
outlast your entire cohort. Set a spend limit in the Anthropic console anyway —
Billing → Limits — so a bug or a bored stranger can't run up a bill.

---

## What to change to make it *yours*

Everything that defines the business is in **one place**: the `buildSystem()`
function in `api/chat.js`. Swap the company, the questions it asks, and the
booking slots, and you have a receptionist for a dentist, a law firm, a salon.

Same code. Different business. That's the actual product you're building.
