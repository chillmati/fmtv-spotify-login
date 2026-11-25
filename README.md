# fmtv-spotify-login

The phone-side sign-in page for Free My TV's Spotify login, served at https://chillmati.github.io/fmtv-spotify-login/ (also the redirect URI in the Spotify app's settings).

The TV shows a QR code that opens this page with its Spotify client ID, a one-time state and a PKCE code challenge. The page waits for the person to tap *Continue to Spotify* (it never goes there by itself), and Spotify always shows its approval screen (`show_dialog=true`): whoever made a link holds its verifier, so a sign-in must never finish without the person's say. When Spotify sends the phone back, the page checks the state and posts the sign-in code to the ntfy.sh topic `fmtv-<state>`, where the TV picks it up (or, after 15 s with no answer from ntfy, offers *Send again*). A sign-in that ends without a code (cancelled, or a Spotify error) posts `error <reason>` to the same topic instead, which the TV shows as cancelled; the page posts nothing for a state this tab didn't start.

It holds no secrets: no client secret (PKCE needs none), no keys, no tracking, no outside files. The code it passes on is single-use, short-lived and useless without the code verifier, which never leaves the TV.

To delete it: delete this repository (Settings > Danger Zone > Delete this repository), or turn off Pages (Settings > Pages > Source: None). Then remove the redirect URI from the Spotify app at developer.spotify.com/dashboard. Spotify sign-in on the TV stops working until another page takes its place.
