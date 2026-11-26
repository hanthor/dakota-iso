# Utah gnome-setup medium (experimental)

The raw live medium that boots Utah and installs it by block copy with GNOME
STF's gnome-setup driving systemd-sysinstall. Moved from `projectbluefin/utah`;
the design is its ADR-0001, and the shared design with Dakota is under
discussion in #413.

- Paths below this directory mirror the Utah repository (`iso/raw/`,
  `iso/scripts/`, `scripts/`), and the files are byte-identical to
  `hanthor/utah` `feat/shared-gnome-setup`, so the move can be checked with
  `diff -r`. `compare_paths.py`, which reads Utah's own CI, stayed behind.
- gnome-setup is built from the `bluefin` branch of `hanthor/gnome-setup`, the
  one meant for Dakota too.
- `iso/scripts/luks-unlock.py` duplicates `live/src/luks-unlock.py` with a
  different monitor transport. Merge them after the cutover, when both lanes
  can be tested together.
- `.github/workflows/utah-image-installer.yml` builds it and runs the GUI E2E on
  pull requests that touch it, on dispatch, and when called.
- `.github/workflows/publish-utah-installer.yml` publishes it to
  `https://projectbluefin.dev/utah-installer-latest.raw.zst`, monthly and on
  dispatch: from the signature-verified `utah:stable` digest, only after the
  GUI E2E passed on that digest, with no test credentials, checksum signed
  keylessly. It replaces the fisherman ISO (`build-iso-utah.yml`, now dispatch
  only).

## Cutover

Until all of these hold, `projectbluefin/utah` keeps its copy, its nightly run
and the publishing of `ghcr.io/projectbluefin/utah-installer`:

1. The E2E passes here.
2. The first medium is published here; Utah's README then points to it and
   `utah-live-latest.iso` is retired.
3. Utah drops `iso/raw/`, `image-installer.yml` and `publish-installer.yml` in
   the same change window, so there is one copy again.

`dakota/` beside this directory is left for #394 or whatever #413 decides.
