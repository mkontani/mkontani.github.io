# mkontani.github.io

Public hub for app/extension **privacy policies** and **support** pages, served via GitHub Pages at https://mkontani.github.io/.

## Structure

```
/index.html              landing page listing products
/<product>/privacy.html  privacy policy
/<product>/support.html  support page
/assets/style.css        shared styles
/.nojekyll               serve files as static (no Jekyll)
```

## Add a new product

1. Copy an existing `/<product>/` folder (e.g. `jwt-lens/`) to `/<new-product>/` and edit the text.
2. Add a card linking to it in `index.html`.
3. Commit & push — GitHub Pages redeploys automatically.

## Products

- **JWT Lens** — [privacy](https://mkontani.github.io/jwt-lens/privacy.html) · [support](https://mkontani.github.io/jwt-lens/support.html)
