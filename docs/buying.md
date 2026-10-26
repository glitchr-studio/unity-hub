# Buying

A paid package of a hub that sells shows **Buy** and its lowest price. The
editor has no session on the hub's site, so the purchase follows OAuth's
device flow (RFC 8628):

1. The window asks the hub for a code (`checkout.device`). The hub answers a
   device code, kept by the window, a user code such as `ABCD-EFGH`, and a
   page.
2. The page opens in the browser. Signed in, the user sees the code, buys the
   package's licence through the site's shop, then presses "Link this
   editor".
3. Meanwhile the window asks the hub (`checkout.token`) every few seconds.
   Once linked, it receives the user's licence key - once - and the registry
   it opens.
4. The key is written as that registry's token in the user's
   `~/.upmconfig.toml` (`%USERPROFILE%\.upmconfig.toml` on Windows, or the
   file `UPM_USER_CONFIG_FILE` names):

   ```toml
   [npmAuth."https://glitchr.io/upm"]
   token = "glk_..."
   alwaysAuth = true
   ```

5. Unity's Package Manager reads that file when Unity starts: the window
   offers to restart the project; then **Install**.

The key opens every licence its seat's e-mail holds on that hub, as it does
for Composer. Nothing of it is written in the project.
