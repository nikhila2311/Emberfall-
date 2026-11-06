# Multiplayer (co-op)

## Model
- Up to **5 players** per room. A sixth player is refused with "That room is full".
- The **host's browser runs the shared world**: monsters, companions, NPCs, quests, AI conversations, events and the economy.
- Joiners send their own position and actions to the host and draw what the host sends back (about 10 updates per second). Each player keeps their own level, gold and gear, saved on their own computer.
- In co-op the NPCs treat the party as one group. Study-mode logging pauses during co-op.

## Networks
| Network | Role | Account needed |
|---|---|---|
| Supabase Realtime (Broadcast + Presence) | Game messages and room membership | A Supabase project (a default one is built in) |
| PeerJS + WebRTC data channels | Backup network; the host is the hub and relays messages | No |
| WebRTC audio | Voice chat, browser to browser, encrypted | No (optional TURN relay) |

The host connects to both game networks at once. A joiner tries PeerJS first and falls back to Supabase.

URL switches for testing: `?net=local` (two tabs of one browser, no internet), `?net=peer` (PeerJS only), `?net=supabase` (Supabase only).

## Troubleshooting
| Message | Meaning | Fix |
|---|---|---|
| No game found with code … | Wrong code, or the host is not online on the same version | Check the code; everyone uses the same `index.html` |
| Found the room, but this network blocks direct connections | Peer-to-peer links are blocked (common on campus Wi-Fi and mobile data) | Add a TURN relay URL under Co-op → Advanced on **every** computer, or use a mobile hotspot |
| Couldn't reach either co-op network | The network blocks live connections, or the computer is offline | Try a mobile hotspot |
| channel error: transport failure (Supabase) | Supabase project paused or blocked | Restore the project in the Supabase dashboard, or rely on the PeerJS route |

## Using your own Supabase project
1. Create a free project at supabase.com.
2. Copy the **Project URL** and the **publishable (anon) key** from Project Settings → API.
3. Paste both under Co-op → Advanced and click Save. Everyone in the room must use the same project.

Never paste a *secret* or *service_role* key into the game.

## TURN relay
Many home, campus and mobile networks block direct browser-to-browser links. A TURN relay forwards the traffic instead. A free option is Metered "Open Relay": create an app, copy its credentials URL (`https://<app>.metered.live/api/v1/turn/credentials?apiKey=<key>`) and paste it under Co-op → Advanced. Keep this URL private; it is stored only in your browser.
