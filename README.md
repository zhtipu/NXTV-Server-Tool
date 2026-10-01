# NX TV Server Tool

A free Windows program that reads a TV server for you and turns it into a file you can copy to your Switch. No typing long addresses on the Switch keyboard, no editing settings files by hand.

It is the companion to [NX TV](https://github.com/zhtipu/nx-tv), a live TV app for the Nintendo Switch. This repository is only the tool.

## What it does

1. You give it a website or an address (a playlist link, an API, an Xtream Codes login).
2. It finds the channels, shows them in a list, and works out the settings by itself where it can.
3. You press one button and it writes a small `.nxtv` file for that server.
4. You copy the file to your SD card. NX TV picks it up and the server appears in the app.

Each server is its own file, so you can add as many as you have.

## Get it

Download `NXTV-Server-Tool.exe` from the [latest release](https://github.com/zhtipu/NXTV-Server-Tool/releases/latest). There is nothing to install. Double-click it to run. It works on Windows 10 and 11.

If Windows shows a "protected your PC" warning, choose **More info → Run anyway**. The tool is not code-signed.

## Quick start

1. **Paste the address.** Put the server's website (or the address of its playlist or API) into **Website or API address** at the top left and press **Analyze**.
2. **Pick a source.** The **Analysis** tab lists what it found, for example "JSON list, 121 channels". Press **Use this** on the one that looks right. The form on the left fills itself in and the channels appear in the **Channels** tab.
3. **Check it.** Look through the channel list. Type in the filter box to find a channel. Double-click a row to copy that channel's stream address.
4. **Name it.** Change **Server name** to something you will recognise, such as "Sports pack". This is the name shown in NX TV.
5. **Send it to the Switch.** Take the SD card out of the Switch and put it in your PC. Press **Write to SD card** and pick the root of the card (for example `E:\`). The tool creates `switch/NXTV/servers/Sports_pack.nxtv` on the card.
6. **Open NX TV.** Put the card back and start the app. The server is under **Settings → Your servers**. Press **Reload channels** if its channels are not in the list yet.

No SD card slot on your PC? Press **Save server file** instead, save the file anywhere, and copy it to `/switch/NXTV/servers/` by whatever means you like (a card reader, FTP, a USB transfer).

## Adding more servers

Repeat the steps. Every server gets its own file, and NX TV loads all of them. You never have to edit or merge anything.

If a file with the same name is already in the folder, the tool asks:

- **Replace** swaps in the new version. Use this to refresh a server whose streams expired.
- **Keep both** saves the new one as `Name (2).nxtv`, so both appear as separate servers in the app.
- **Cancel** does nothing.

## When Analyze cannot find the server

Analyze reads the page and its scripts and tests every address it finds. It works best when you paste the address of the website or of the playlist or API itself. If it says nothing usable was found, fill in the form by hand instead.

Pick the **Server type** on the left:

| Type | Use it for | What you fill in |
|---|---|---|
| **JSON API** | A web address that returns a list of channels as JSON | The API address, the path to the channel list, and which field holds each channel's name, stream address, category and logo. Optional login and extra headers such as an API key. |
| **M3U** | A playlist link (`.m3u` or `.m3u8`) | The playlist address. Optional login. |
| **Xtream** | A provider that gave you a server address, username and password | The server address (like `http://host:port`), username and password. |
| **Web + token** | A web page that lists channels and hands out a temporary stream address for each one | The page address and the patterns that find the channels and the stream. For advanced users. Analyze can usually work these out for you. |

Then press **Fetch channels** to read the server and see the list. Extra headers are separated with `|`, for example `API-KEY: abc | X-Token: def`.

If a server needs a login or key, fill in the login or headers fields before pressing **Analyze** or **Fetch channels**. The tool uses what is in the form.

## Updates and About

The **About** tab shows the version, what the tool does and who made it. It also checks for updates:

- The tool looks for a newer version in the background when it starts. If there is one, a message appears and the **About** tab gets a dot.
- Open **About** and press **Check for updates** at any time. It tells you if you are already on the latest version.
- Press **Download and install** to update. A progress bar shows the download, then the tool closes, swaps in the new version and starts again by itself. **Cancel** stops the download and leaves your current version untouched.

Updates come from this project's [releases page](https://github.com/zhtipu/NXTV-Server-Tool/releases).

## Things worth knowing

- **Some streams expire.** A few servers give out streams that stop working after an hour or a few hours. If a server's channels stop playing in the app, run the tool again and use **Replace** on the same file. Servers with permanent stream addresses never need this.
- **Live vs. channel-list files.** If NX TV can read the server directly, the file only holds the server's settings and the app always gets the latest channels. If the server needs the extra steps the app cannot do (for example per-channel tokens or filters), the file carries the channel list that you just fetched.
- **Categories.** The app sorts channels into Sports, News, Bangla, Hindi and Entertainment by keyword. Anything else goes under Entertainment.
- **Your passwords stay in the file.** A server that needs a login stores it inside its `.nxtv` file in plain text. Do not share those files.
- **Privacy.** The tool talks only to the addresses you give it. It has no accounts, no analytics and sends nothing anywhere else.
- **Only use servers you are allowed to use.** The tool reads what a server already offers you. It does not get around logins or protection.

## What it cannot find

- Sites that only build their channel list with JavaScript after the page loads. The tool sees an empty page.
- Details hidden inside a phone app.
- Anything behind a login you did not give it, or sites that block automated requests.

In these cases, use the address of the playlist or API directly if you have it, or fill in the form by hand.

## The Advanced tab

For people who prefer to edit NX TV's settings file themselves. It shows the server as a snippet of NX TV's `iptv.json` file and has three buttons:

- **Save .m3u** saves the channels as a normal playlist that works in any IPTV player.
- **Copy entry** copies the snippet so you can paste it into the `customServers` list of `iptv.json`.
- **Merge into iptv.json** adds the server to an `iptv.json` file you pick (a backup is saved as `.bak` first).

You do not need any of this for normal use.

## Troubleshooting

**Analyze says "Could not read the website".**
Check the address and your internet connection. If the server is only reachable from a certain network (for example your internet provider's), run the tool on that network.

**Fetch channels shows an error.**
Read the message. The most common causes are a wrong address, a missing login or API key, or a path or field name that does not match the server's response.

**The server shows in NX TV but has no channels.**
Press **Reload channels** in the app's Settings. Open the file's server in the app and use **Test server** to see the error. If the streams expire, run the tool again and replace the file.

**The app does not show my file.**
Make sure the file is directly inside `/switch/NXTV/servers/` on the card and ends in `.nxtv`. You can also use **Settings → Import a server file** in NX TV to pick it by hand.

## Disclaimer

This tool reads what servers already provide to you. It does not host or store any video content. You are responsible for making sure you have the right to watch what you stream. Not affiliated with Nintendo or any channel or server operator.
