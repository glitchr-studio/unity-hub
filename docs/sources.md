# Sources

The window reads every source the user added (Sources tab), in order, and
lists what each offers.

## A hub manifest

A JSON document that names a hub and its sources - what a forge serves at
`/upm/-/hub.json`, and what a static hub publishes as `hub.json`:

```json
{
  "version": 1,
  "name": "Glitch Art",
  "logo": "https://glitchr.io/assets/images/brand/monkey.png",
  "color": "#fc6875",
  "sources": [
    {"name": "Glitch Art", "type": "upm", "url": "https://glitchr.io/upm", "scopes": ["com.glitchr"], "catalog": "https://glitchr.io/upm/-/hub/catalog.json"},
    {"name": "A tool", "type": "git", "url": "https://github.com/someone/some-tool.git"}
  ],
  "games": "https://glitchr.io/upm/-/hub/catalog.json",
  "checkout": {"device": "https://glitchr.io/upm/-/hub/device", "token": "https://glitchr.io/upm/-/hub/token"}
}
```

- A `upm` source with a `catalog` is read from it: its packages with their
  versions and changelogs, pictures, prices, and whether the user's key
  covers them.
- A `upm` source without one is read as any registry: its search
  (`/-/v1/search?text=<scope>`), then each package's document.
- A `git` source is one package, installed by its URL.
- `checkout`, when present, lets the window offer to buy (see
  [buying.md](buying.md)).

## A UPM registry

Its URL and its scopes (`com.example`): any npm registry Unity's Package
Manager can read - OpenUPM, Verdaccio, a forge's `/upm`. Installing a
package writes the registry, with these scopes, into the project's
`Packages/manifest.json`.

## A Git package

Its URL, as Unity's Package Manager takes it: `https://...git`, with
`#<tag>` or `?path=<folder>` when needed.

## A development site

A site with a self-signed certificate is refused by the editor's requests.
For development only, give the source its certificate's SHA-256 fingerprint
(`openssl x509 -in cert.pem -noout -fingerprint -sha256`): that certificate
is then accepted for that source. Unity's Package Manager needs it too: a
`caFile` in its global configuration (Unity manual, "Configure your network
for Package Manager").
