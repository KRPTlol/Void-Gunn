# Void Runner — Online Multiplayer FPS

Real-time PvP deathmatch. Node.js + Socket.io server, Three.js client.
Movement is client-reported (smooth, low-latency); hits are validated
server-side against fixed damage/fire-rate/range rules so a client can't
just claim arbitrary damage.

## How it works
- `server/server.js` — authoritative match state: spawns, health, kills,
  deaths, respawns. Relays player positions and shot tracers between
  clients at ~15 times/second.
- `public/index.html` — the game itself. Connects via Socket.io, renders
  other players as glowing capsule avatars with name tags, sends your
  position, and reports hits it detects locally (server double-checks
  range and fire-rate before applying damage).

## Run it locally first (recommended)
You'll need [Node.js](https://nodejs.org) 18+ on a computer.

```bash
cd server
npm install
npm start
```

Then open `http://localhost:3000` in a browser — or on your phone, if
it's on the same Wi-Fi, open `http://<your-computer's-LAN-IP>:3000`.
Open it in two tabs/devices to test multiplayer with yourself.

## Deploy it live (so anyone can join from anywhere)

**Render.com (free tier, easiest)**
1. Push this `void-runner-mp` folder to a GitHub repo.
2. On [render.com](https://render.com) → New → Web Service → connect the repo.
3. Root directory: `server`
4. Build command: `npm install`
5. Start command: `npm start`
6. Deploy. Render gives you a URL like `https://your-app.onrender.com`.
7. Share that URL — anyone who opens it on their phone can join the match.

Note: Render's free tier spins down after inactivity, so the first join
after a while takes ~30–60s to wake up. Fine for casual play with friends.

**Alternatives:** Railway.app and Glitch.com work the same way and also
have free tiers — same three settings (root: `server`, build: `npm install`,
start: `npm start`).

## Known limitations (worth knowing)
- Movement is trusted from the client (no server-side movement physics),
  so a modified client could in theory teleport — fine for casual play
  with friends, not tournament-grade anti-cheat.
- No matchmaking/lobbies — everyone who opens the URL joins the same arena.
  For separate matches, you'd deploy separate instances or add room codes.
- No persistence — kills/deaths reset if the server restarts.

## Extending it
- **Rooms/lobbies**: use Socket.io namespaces or rooms keyed by a match code.
- **Voice/text chat**: add a simple Socket.io `chat` event + a text overlay.
- **Weapons**: extend `weapon` in the client and add a `weaponType` field
  server-side so damage/fire-rate differ per gun.
- **Teams**: tag each player with a team on join, color avatars accordingly,
  and block friendly-fire in the server's `hit` handler.
