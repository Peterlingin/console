# Peer Channel

A minimal, single-file text chat that links two browsers directly. Messages and files travel peer to peer over an encrypted WebRTC connection. There is no backend, no account, and no server that relays or stores anything. Closing the tab ends the session.

Built for short-lived exchanges of technical information between two people, such as findings, commands, snippets and small files.

## Features

- Direct browser-to-browser connection (WebRTC data channel), encrypted in transit
- Serverless handshake: exchange two short codes by copy/paste or QR
- Verification code shown on both sides to detect interception
- Code blocks with a copy button
- File transfer with progress
- Timestamped transcript export
- One self-contained HTML file, with no external dependencies or build step

## Quick start (GitHub)

1. Put `index.html` in a repository.
2. Open **Settings → Pages**, set **Source** to *Deploy from a branch*, choose `main` and `/ (root)`, and save.
3. Open `https://<username>.github.io/<repository>/` once it's live (usually within a minute) and share the URL with your peer.

Any static host with HTTPS works. `http://localhost` is fine for testing. Opening the file from disk (`file://`) is not recommended, because the browser may block the hashing API.

## Usage

One person is the **initiator** (A) and the other the **joiner** (B).

**A: initiator**

1. Click **Start session**.
2. Wait for the code to appear (a few seconds), click **Copy**, and send it to B.

**B: joiner**

3. Click **Join session**, paste A's code, and click **Generate reply**.
4. Copy the reply code and send it back to A.

**A: initiator**

5. Paste B's reply code and click **Connect**.

The chat opens on both sides and the status turns green. **Show QR** displays a code as a QR image to ease moving it to a phone. It does not scan, so read it with a phone camera app and copy the text.

### Verify the connection

Both sides see a **Verification code** at the top of the chat, for example `4F2A-91C7-03BE`. Compare it over a **different channel** than the one used for the handshake (a phone call or in person). If the codes differ, end the session and start over. The code is derived from both sides' encryption fingerprints, so an intercepted or altered handshake produces a mismatch.

### In the chat

| Action | How |
|---|---|
| Send a message | Type and press **Enter** (**Shift+Enter** for a new line) |
| Share code or output | **Code block** button, or wrap text in triple backticks |
| Send a file | **Send file**; the receiver gets a download link |
| Keep a record | **Export transcript** saves a timestamped `.txt` |
| Close | **End session** reloads the page and wipes memory |

## Privacy and security

**What it protects.** Message and file content is encrypted between the two browsers and is never sent to or stored on a server. There are no accounts, no analytics and no stored history.

**What it does not protect.** Read this before relying on it.

- **Peers see each other's IP addresses.** WebRTC requires exchanging network addresses. Behind a VPN the real public or local IP may still leak, depending on the browser and settings.
- **Handshake codes contain network details.** Whatever channel carries the codes (messenger, email) can see who connected and when, though not the chat itself.
- **A public STUN server is used.** By default the page contacts `stun.l.google.com`, which sees your public IP and the connection time, never content. Edit the `STUN` constant in the source to use your own server, or remove it for LAN-only use.
- **You must trust the page you load.** The code is fetched from the host each time. A compromised repository or hosting account could serve modified JavaScript that captures what you type, and encryption does not help against that. Protect the account with 2FA, review changes, and prefer hosting you control. For sensitive use, distribute the file directly and open it locally, or compare a file hash before use.
- **Your device is part of the trust boundary.** Browser extensions can read page content, clipboard managers may store copied codes and code blocks, and screenshots or screen sharing capture the chat.
- **Exports and downloads are plain files.** Transcripts and received files are stored unencrypted locally and may end up in backups or cloud-synced folders.
- **Identity is not authenticated.** The tool proves you are connected to the same endpoint that produced the handshake, not who the person is. Skipping the verification code removes even that guarantee.
- **Received files are untrusted.** Nothing is scanned. Confirm the sender before opening anything.

### Recommendations

- Always compare the verification code before sharing anything sensitive.
- Use a clean browser profile without extensions.
- Share the minimum needed, and avoid credentials.
- End the session when done, and delete exports and downloads you no longer need.
- Follow the rules of your engagement or organization on what may be shared.

## Limitations

- Exactly two participants per session.
- Some corporate or carrier networks block direct peer connections. A TURN relay would be needed, which requires a server and is not included.
- No message history. If either side closes the tab or loses connectivity, a new handshake is required.
- Large files are held in memory while transferring.

## Troubleshooting

| Problem | What to try |
|---|---|
| Code takes long to appear | The page waits up to 5 seconds for network candidates. Reload and retry. |
| "Not a valid code" | The code was truncated or altered. Copy it again in full. |
| "Connection failed" | A firewall or carrier network is blocking direct links. Try another network or a hotspot. |
| Chat never opens after Connect | Use the reply code from the same session. Each Start session creates a new pair. |
| Page shows a browser error | Use a current Chrome, Edge, Firefox or Safari. |

## Browser support

Requires WebRTC and the Compression Streams API: current versions of Chrome, Edge, Firefox and Safari.

## Disclaimer

Provided as is, without warranty. Use it only for lawful and authorized purposes, and in line with the rules of your engagement and organization.

## License

MIT
