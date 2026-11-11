# PaySauce login prototype

Five layout options for the Phoenix login page, desktop and phone, as one responsive page.

- `index.html`: the five options side by side.
- `login.html?option=A` (A to E): one option, full size. Left and right arrow keys step through them. A small label bottom right names the option on screen.
- `Login.vue`: the same template as a Vue 3 component on PrimeVue 4 (InputText, Button, Select). Pass `option="A"` to `"E"`.
- `login.css`: the one stylesheet both use. Desktop values first, phone overrides under 640px at the bottom.
- `assets/`: the logo files. `paysauce-script-red.svg` is the login app's own logo; `au-mark.svg` and `au-wordmark.svg` are the website header lockup.

To view locally, serve the folder (for example `python3 -m http.server 8000`) and open `http://localhost:8000/`. The HTML loads Vue and PrimeVue from a CDN, so it needs a network connection.
