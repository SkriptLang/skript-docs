![Skript Docs](.github/assets/cover.jpg)

---

# Skript Documentation Site
Skript's features are documented directly in it's Java code. But we still need

1. Clear tutorials, not just "you can check the syntax pattern"
2. Examples explained, if needed
3. Addon tutorials
4. Syntax patterns explained

## Folders Structure
- `docs/templates/` is used by Skript Docs generation process to produce the docs pages (`docs/`).
- `docs/` is the production files where Github pages displays the content of.
- `docs/nightly/master/` is Skript's nightly builds documentation pages (this gets updated with every commit to master/dev branches in [Skript's repo](https://github.com/SkriptLang/skript)).

You can find more about the docs templates in [docs/templates](docs/templates/README.md).

## Generating the documentation
Currently, the docs files is generated using Skript plugin.

1. You will need to create a directory named `docs/templates` in `plugins/Skript/`, and copy everything from [docs/templates folder](docs/templates) into that directory.
2. Execute the command `/sk gen-docs`.
3. The `docs/` directory will be created _(if not created already)_ in `plugins/Skript` containing the website's files.
4. Open `index.html` and browse the documentation.
5. _(Optionally)_ Add this system property `-Dskript.forceregisterhooks` in your server startup script (before the -jar property) to force generating hooks docs.
