# ai.storyteller.pdfcraft

PdfCraft, ArtCraft's PDF reader and editor with OCR, and its command line
(`flatpak run --command=pdfcraft-cli ai.storyteller.pdfcraft`): the
`linux-x86_64` tarball of storytold/pdfcraft's GitHub release, as upstream's
Flatpak ships it, on `io.github.lucarickli.Platform`. Checked against the
release's `SHA256SUMS.txt` only: upstream signs nothing.

Upstream's permissions, less what serves its update check (the network,
host commands and the ArtCraft Launcher's versions) and the accessibility
bus (screen readers): Wayland (X11 where there is none), the GPU, printing
through the host's CUPS and Avahi, `~/.cups` read-only, Documents and
Downloads; other files through the file chooser and drag and drop. The
update check is off, as the Flatpak brings the updates:
`CRAFT_VERSIONS_FILE` keeps PdfCraft on its version, as the launcher would,
so Check for updates… says it "follows the version chosen in the launcher"
(its "Check for updates when PdfCraft starts" then does nothing). Without
the network there is no `--control` from the host (`pdfcraft-cli ui
--control` from another `flatpak run`) and no print server from
`~/.cups/client.conf`.

```sh
# once: a token with read:packages in ~/.config/flatpak/oci-auth.json (or /etc/flatpak/), as {"auths":{"ghcr.io":{"auth":"<base64 of USER:TOKEN>"}}}
flatpak remote-add --user --if-not-exists packages oci+https://lucarickli.github.io/packages
flatpak install --user packages ai.storyteller.pdfcraft
# NVIDIA, unless a Flathub app brought its GL driver along:
flatpak install flathub org.freedesktop.Platform.GL.nvidia-$(tr . - </sys/module/nvidia/version)//1.4
# the network back, for those (the update check stays off):
flatpak override --user --share=network ai.storyteller.pdfcraft
```
