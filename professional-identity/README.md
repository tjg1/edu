# Professional Identity Studio

A 90 minute professional identity activity for up to 12 UK Year 2 medical students.

## Current status

Implemented and uploaded to the edu repository. Automated application tests pass. Real teaching network and full group testing remain required before classroom use. Artwork is never committed to this repository.

## Host on GitHub Pages

1. Create a repository for the website code. Student submissions never belong in the repository.
2. Upload the contents of `dist/` to the repository root, including `vendor/`. Keep this folder structure. No build step or npm installation is required.
3. In the repository’s Settings → Pages, select deployment from the main branch and the root folder. Use HTTPS.
4. Open the resulting website address. Select **Set up a tutor session**.

The GitHub-ready ZIP places these website files at its root, ready to upload. Its `README.md` and `session.test.cjs` are optional supporting files. For the test included in that ZIP, run `node session.test.cjs` from the extracted folder.

## Run the session

1. Before teaching, test with the tutor device and at least one student device on the actual teaching network. Ideally test the full group size. Verify drawing, submission, enlargement, updates and deletion. Test expiry separately if needed by temporarily shortening the duration in a development copy.
2. The tutor opens the site and selects **Set up a tutor session**, then **Start 90 minute session**. The clock starts at that point.
3. Display the student QR code or copy and share the generated student link in the room. It is a separate session-specific link with no sign-in. Do not share the tutor screen while setting up other sensitive material.
4. Students draw freehand inside the person, using any brush size, colours and textures. Add palette labels and an optional explanation. The explanation can also be used instead of drawing. Students can undo, redo and erase.
5. Each student receives a random animal name. They select **Submit to tutor**, and wait for the receipt. Edits remain local until **Update submission** is selected.
6. The tutor can hide the joining instructions, pause submissions, and select any gallery card to enlarge the person and read its labels. Previous and next buttons support discussion.
7. Select **End and delete** before leaving. The app also clears active pages after 90 minutes.

**Keep the tutor tab open and the device awake.** A refresh or tab closure loses the gallery. There is intentionally no recovery or saved tutor link. The tutor endpoint exists only in that running tab. The website itself can start a fresh session later, but old student links expire and cannot recover earlier work.

## Privacy and its practical limits

- No names, email addresses, student numbers, accounts, analytics or app cookies are requested. The app uses no localStorage, sessionStorage, IndexedDB, service worker, database, file upload or persistent artwork store.
- Drawings and explanations exist in browser memory. Submissions travel directly over encrypted WebRTC data channels to the tutor. Other students are never sent the gallery. The website host does not receive artwork.
- PeerJS Cloud (`0.peerjs.com`) provides signalling. Google STUN (`stun.l.google.com:19302`) helps browsers find a direct path. Those services process connection information, including IP addresses and signalling metadata. Browser peers can learn each other’s network addresses. The website host processes routine connection information too. Provider log retention and location have not been verified. Do not describe this as “no personal data is processed” or as institutionally approved hosting.
- Only a STUN server is configured. No TURN relay or third party artwork storage is configured. Some networks prevent a direct connection. There is no fallback through an artwork server. Try another permitted network if the teaching network blocks WebRTC; obtain local IT advice if necessary.
- The random session identifier and joining token are in the link’s fragment. They are not sent to the website host as part of the page request. A session link is still a bearer invitation: anyone given it during the session can join, up to the 12 person limit. There is no student identity verification. Keep it within the room.
- This is **application-level clearing of volatile state**, not a forensic erasure guarantee. There is no artwork database backup to delete. The app cannot erase screenshots, browser or operating system snapshots, a modified client, or copies someone deliberately makes.
- At 90 minutes, running tabs clear the drawing, explanation and gallery. A suspended tab clears when its script next runs. “End and delete” instructs connected students to clear immediately. A disconnected student cannot receive that instruction; their expiry still applies when the page next runs. This is why students should close their tabs at the end as well.
- Students see: “Do not include names, patient details or information you would not want discussed with the group.” Free text can still identify someone if they ignore this instruction. Participation and explanations should remain voluntary.

## Educational use

Prompt: **What is your professional identity? What qualities, values, beliefs and skills do you associate with being a medical student?**

Each student decides what a colour or texture means, how much space it occupies and where it belongs. No predefined meanings or anatomical interpretations are supplied. Ask students why they chose a colour and position rather than interpreting the picture for them. This is a reflection activity, not an assessment or psychological measure.

## Technical notes

- No build dependencies. Files under `dist/` are the website.
- Vendored PeerJS 1.5.5 and qrcode-generator 1.4.4. Their licence notices are included with the vendored source (PeerJS licence also provided separately).
- Incoming explanations and labels are rendered as text, not HTML. Artwork must be a PNG data URL within the payload limit. Maximum 20 palette labels and 1,500 characters of explanation. Random peer IDs and session tokens use browser cryptography.
- No camera or microphone access is requested.
- An editable or forged client can evade its local expiry or retain its own work. Standard browser software cannot prevent a participant keeping a copy of what is displayed on their device.
- Tests simulate the application protocol and DOM; they do not validate PeerJS Cloud availability, real WebRTC delivery, university firewall compatibility, accessibility with assistive technology, or visual rendering.

For the original development layout, run `node session.test.cjs` from this directory. A local development server is provided in `preview.mjs`; run `node preview.mjs`, then open `http://127.0.0.1:8765`. The development server is not needed by GitHub Pages.
