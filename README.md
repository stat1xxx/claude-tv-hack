# Claude TV Hack: a clean home screen for any Samsung TV, in one prompt

This is the prompt from the video. You paste one GitHub link into Claude Code, and Claude does the rest: it reads the repo, installs Samsung's developer tools, adapts the app to your TV model, connects to the TV over Wi-Fi, signs the app and installs it.

Your job: a few clicks on the remote and one unplug.

## What you need

- A Samsung smart TV running Tizen (most Samsung TVs from 2016 onward)
- A Mac, Windows or Linux computer on the **same Wi-Fi** as the TV
- [Claude Code](https://claude.com/claude-code)
- About 2 GB of free disk space for Samsung's tools
- A free Samsung account (needed to create the signing certificate)
- A home-screen app repo for Tizen. The video used [CleanFrame](https://github.com/nthnclrk/CleanFrame), built for Samsung The Frame. All credit for the app goes to its author.

## How to run it

```bash
mkdir samsung-tv && cd samsung-tv
claude
```

Then paste the prompt below. Put your repo link on the first line.

---

## The prompt

```
Help me install this app on my Samsung TV: <PASTE GITHUB LINK HERE>

<goal>
Install the app from the repo above on my Samsung TV and make it launch.
Done means: the app is installed on the TV, it opens without errors, and I have a short report of everything you changed.
</goal>

<how_to_work>
1. Read the whole repo first: the README, any setup or install guide, the install scripts and the app's config.xml. Tell me in a few lines what the app does, which TV models and Tizen versions it was built for, and how the author installs it.

2. Install Samsung's Tizen tools (Tizen Studio CLI and the TV extensions) for my operating system. Use the official Samsung / Tizen download source only.
   - Install into a folder path with NO spaces (for example ~/tizen-studio). Samsung's installer breaks on spaces.
   - If a step fails, read the error, fix the cause and retry. Don't stop on ordinary errors.

3. Ask me for my TV's model code before you touch anything else. Tell me where to find it: Settings > Support > About This TV.
   - Work out the TV's model year and Tizen version from the code.
   - Compare it with what the app was built for. If my TV is older or different, tell me plainly what could break, and ask me if I want to continue.

4. If I say continue, adapt the app to my TV:
   - Change only what is needed: the required Tizen version, the profile and the privileges in config.xml, and any API call my Tizen version doesn't have.
   - Change one thing at a time and explain each change in one line.
   - Keep a backup copy of every original file in ./backup/ before you edit it.

5. Walk me through Developer Mode on the TV, one step at a time, and wait for me after each step:
   - Open Apps on the TV.
   - Press 1, 2, 3, 4, 5 on the remote. A hidden Developer Mode menu appears.
   - Switch Developer Mode ON and enter this computer's local IP address (find it for me and print it).
   - Restart the TV completely: unplug it for 10 seconds, then plug it back in.
   - Ask me for the TV's IP address (Settings > General > Network > Network Status > IP Settings), or find it on the local network.

6. Connect to the TV with sdb over Wi-Fi and confirm the connection. Tell me the Tizen version the TV reports.

7. Create the signing certificate.
   - Newer Samsung TVs need a Samsung certificate that includes the TV's DUID. Read the DUID from the TV for me.
   - If a step needs me to log in to my Samsung account in a browser, give me the exact steps and wait. Never ask me to paste my password into the chat.

8. Build the app package (.wgt), sign it with that certificate, install it on the TV and launch it.
   - If the install or launch fails, read the error, fix the cause and try again.

9. When the app runs, give me a short report: what you changed and why, where the backups are, how to update the app later, and how to remove it from the TV.
</how_to_work>

<rules>
- Only install the app. Never touch the TV firmware, system apps, or factory settings.
- Before any step on the TV, tell me what you are about to do.
- Steps that need my yes first: anything that installs software outside this folder and the Tizen tools folder, and continuing when my TV model is not officially supported.
- Steps that need no approval: reading files, downloading the Tizen tools from the official source, editing files inside this folder, building the package, and retrying a failed command.
- If something is truly blocked on me (a remote button, a login, a model code), ask one clear question and wait.
</rules>

Start with step 1.
```

---

## Undo it

- Remove the app: on the TV, open Apps, find the app, choose **Delete**. Or ask Claude: `uninstall the app from my TV`.
- Turn off Developer Mode: Apps > 1-2-3-4-5 > switch **Off** > restart the TV.

## Good to know

- Developer Mode is an official Samsung feature for testing apps. It doesn't change the TV's firmware.
- Apps installed in Developer Mode can be removed by a firmware update. If that happens, run the same prompt again in the same folder. Claude will reuse the tools and the certificate.
- Older TVs (like the 2020 Q60 in the video) may run an older Tizen version than the app expects. That is what step 3 and step 4 are for.
- Want something different on the home screen? After it works, just tell Claude what to change, for example: `switch the layout to a dock and make the background frosted glass`.
