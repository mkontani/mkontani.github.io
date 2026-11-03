# mkontani.github.io

Personal site of mkontani (software engineer) and public hub for app/extension **landing pages**, **privacy policies** and **support** pages, served via GitHub Pages at https://mkontani.github.io/.

## Structure

```
/index.html              profile page (intro + apps section)
/favicon.svg             site favicon
/<product>/index.html    product landing page
/<product>/privacy.html  privacy policy
/<product>/support.html  support page
/assets/style.css        shared styles for privacy/support pages
/assets/apps/*.png       app icons shown on the profile page
/assets/og.png           social preview image for the profile page
/.nojekyll               serve files as static (no Jekyll)
```

## Add a new product

1. Copy an existing `/<product>/` folder (e.g. `jwt-lens/`) to `/<new-product>/` and edit the text.
2. Add an app tile in the `#apps` section of `index.html`, put its icon in `/assets/apps/<new-product>.png`, and update the `N products` count.
3. Commit & push — GitHub Pages redeploys automatically.

## Products

- **SSH Keytool** — [website](https://mkontani.github.io/ssh-keytool/) · [Chrome Web Store](https://chromewebstore.google.com/detail/ssh-keytool/ppomhlbhgiaefnionnblaiaolpkjagkc) · [privacy](https://mkontani.github.io/ssh-keytool/privacy.html) · [support](https://mkontani.github.io/ssh-keytool/support.html)
- **Cert Tool** — [website](https://mkontani.github.io/cert-tool/) · [Chrome Web Store](https://chromewebstore.google.com/detail/cert-tool/ojgmjpfknnlfcfjcljkdcfpbpgciefkk) · [privacy](https://mkontani.github.io/cert-tool/privacy.html) · [support](https://mkontani.github.io/cert-tool/support.html)
- **てくてくねこ** — [website](https://mkontani.github.io/tekuteku-neko/) · [App Store](https://apps.apple.com/jp/app/id6807918602) · [privacy](https://mkontani.github.io/tekuteku-neko/privacy.html) · [support](https://mkontani.github.io/tekuteku-neko/support.html)
- **まじょっこたいぴんぐ** — [website](https://mkontani.github.io/majo-type/) · [App Store](https://apps.apple.com/jp/app/id6802051469) · [privacy](https://mkontani.github.io/majo-type/privacy.html) · [support](https://mkontani.github.io/majo-type/support.html)
- **nokori（のこり）** — [website](https://mkontani.github.io/nokori/) · [App Store](https://apps.apple.com/jp/app/id6772641995) · [privacy](https://mkontani.github.io/nokori/privacy.html) · [support](https://mkontani.github.io/nokori/support.html)
- **サクラ診断** — [website](https://mkontani.github.io/sakura-grade/) · [Chrome Web Store](https://chromewebstore.google.com/detail/kjdodbkbkkncabkdgjapbioggmgnielo) · [privacy](https://mkontani.github.io/sakura-grade/privacy.html) · [support](https://mkontani.github.io/sakura-grade/support.html)
- **JWT Lens** — [website](https://mkontani.github.io/jwt-lens/) · [App Store](https://apps.apple.com/jp/app/jwt-lens/id6774901745) · [privacy](https://mkontani.github.io/jwt-lens/privacy.html) · [support](https://mkontani.github.io/jwt-lens/support.html)
