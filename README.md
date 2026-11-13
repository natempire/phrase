# Phrase by Phrase

A French-to-English learning app. Each learner gets a personal link, their progress syncs to a Google Sheet, and you can follow it there.

- `index.html`: the app (one file, hosted on GitHub Pages)
- `Code.gs`: the sync script (runs in your Google Sheet)

Without the Sheet, the app still works: progress just stays on each phone.

## 1. Put the app on GitHub Pages

1. Create a **public** repository, for example `phrase-by-phrase`. Free GitHub Pages needs a public repo. Nothing private goes in it: learner codes live only in your Sheet.
2. Upload `index.html` and this README.
3. Go to **Settings > Pages**. Under Source, choose **Deploy from a branch**, branch `main`, folder `/ (root)`, then click Save.
4. After about a minute the app is live at `https://YOUR-USERNAME.github.io/phrase-by-phrase/`.

## 2. Create the Sheet and the script

1. Create a new Google Sheet, for example "Phrase by Phrase - suivi".
2. Open **Extensions > Apps Script**. Replace the content of `Code.gs` with the provided `Code.gs`.
3. At the top, set `APP_URL` to your Pages address. Keep the trailing slash.
4. Save. In the toolbar, select the `setup` function and click **Run**.
   Google will ask for authorisation and warn that the app is unverified. That's normal for your own script: click **Advanced > Go to project (unsafe)**, then **Allow**.
   This creates three tabs: Apprenants, Progression, Données.
5. Click **Deploy > New deployment**. Choose the type **Web app**, set *Execute as* to **Me** and *Who has access* to **Anyone**, then click Deploy.
6. Copy the **Web app URL**. It ends in `/exec`.

## 3. Connect the app

In `index.html`, find this line:

```js
const SYNC_URL = '';
```

Paste your `/exec` URL between the quotes, then commit. The live app updates within a minute.

## 4. Add a learner

1. Reload the Sheet. A **Phrase by Phrase** menu appears.
2. Click **Phrase by Phrase > Ajouter un apprenant** and enter a first name. A dialog shows their personal link, like `.../?u=koffi-x7k2mq`.
3. Send them the link. They should open it in **Chrome**, not in WhatsApp's built-in browser: in WhatsApp, tap the link, then **⋮ > Open in Chrome**. After the first visit, the app remembers them.
4. On Android: **⋮ > Add to Home screen**. On iPhone: **Share > Add to Home Screen**. On iPhone this matters: Safari erases a site's data after about 7 days without a visit, unless the site is on the Home Screen.

The app footer shows the learner's name and the last sync time.

## What you see in the Sheet

**Progression** has one row per learner who shares their progress:

| Column | Meaning |
|---|---|
| Dernière synchro | Last time the app sent data |
| Dernier jour de pratique | Last day they practised |
| Jours pratiqués / Série | Total practice days / current streak |
| Phrases vues, Maîtrisées, En cours, Total | Phrases met so far, mastered (still remembered after 2+ weeks), still learning, total in the app |
| À revoir | Phrases due for review today |
| Grammaire | Best exercise score per lesson (L1 = lesson 1) |
| Phrases difficiles | The phrases they fail most, with the number of failures |

**Données** stores the raw progress used for syncing. Don't edit it.

## Privacy

- Untick **Progression visible** in the Apprenants tab and that learner's Progression row is removed. Their raw data still sits in the Données tab, though, because syncing needs it. As the Sheet owner, you could technically read it. Tell learners this honestly.
- Anyone who has a learner's link can open that learner's progress. Treat the link like a low-stakes password.

## When you change the script

Saving `Code.gs` is not enough. Go to **Deploy > Manage deployments**, click the pencil, set **Version: New version**, then click Deploy. Otherwise the URL keeps running the old code. The URL itself doesn't change.

The `mergeState` function exists in both `index.html` and `Code.gs`. If you change it, change both copies.

## How syncing behaves

- Progress is saved on the phone first, so the app works offline.
- Changes are sent about 8 seconds after an answer, and when the app is closed. If they're offline, the changes go out the next time they're online.
- With two devices, the most recent answer for each phrase wins, and practice days are combined.
- "Remettre à zéro" erases progress on every device of that learner.
- Free Google quotas are far more than a few learners need.
