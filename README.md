# BitwigRemotes

A community collection of **device remote control panels** (a.k.a. "device panels") for
[Bitwig Studio](https://www.bitwig.com/) - ready-made knob mappings for hundreds of
third-party VST2, VST3 and CLAP plugins.

Drop one into your Bitwig library and, from then on, every time you load that plugin
Bitwig automatically shows a set of pre-labelled remote controls. If you have a
supported hardware controller set up, those controls map to your knobs and faders
automatically too - no manual mapping, no per-project setup.

> These are **device-level** remote controls, not preset-level ones. They load with the
> *plugin*, regardless of which preset or project you open. Bitwig's own native devices
> already ship with panels like this; most third-party plugins do not - which is what
> this repo fills in.

This collection was started by **aMUSEd / kymeia** and grown by contributors in the KVR
[Device panel sharing thread](https://www.kvraudio.com/forum/viewtopic.php?t=480998).
Thanks to everyone who has shared their mappings.

---

## What's in here

The repo is organised into folders by manufacturer or category
(`FabFilter/`, `uhe/`, `Valhalla VST3/`, `Misc Effects VST3/`, …). Each plugin's
mapping is a single `.zip`, named so you can read it at a glance:

```
FabFilter/FabFilter Saturn vst-46536174.zip
uhe/DIVA CLAP.zip
Eventide/Eventide Blackhole VST3.zip
```

Inside every zip is one folder containing one file:

```
vst-46536174/
└── Default.bwremotecontrols
```

That folder name - `vst-46536174` - is **Bitwig's internal device ID** for the plugin.
It is not decoration: Bitwig uses it to match the mapping to the right plugin. The three
ID styles you'll see correspond to the plugin format:

| Prefix       | Format | Example                                   | What the ID is                                  |
|--------------|--------|-------------------------------------------|-------------------------------------------------|
| `vst-`       | VST2   | `vst-46536174`                            | The plugin's 4-byte VST unique ID, in hex (`46536174` = `FSat`) |
| `vst3-`      | VST3   | `vst3-565354...`                          | Derived from the VST3 class UID                 |
| `clap-`      | CLAP   | `clap-com.u-he.Diva`                      | The CLAP plugin's reverse-DNS ID                |

The `.bwremotecontrols` file itself is Bitwig's own binary format - you don't edit it by
hand; you create and tweak it inside Bitwig (see [Contributing](#contributing-add-your-own)).

---

## Installing a device panel

1. **Find your Bitwig `devices` folder.** It lives inside your Bitwig library, in a
   hidden `.settings` folder:

   - **macOS:** `~/Documents/Bitwig Studio/Library/.settings/devices`
   - **Windows:** `...\Bitwig Studio\Library\.settings\devices`
   - **Linux:** `~/Bitwig Studio/Library/.settings/devices`

   > **The folder is hidden.** On macOS the `.settings` folder is invisible in Finder -
   > press <kbd>⌘</kbd>+<kbd>Shift</kbd>+<kbd>.</kbd> to reveal hidden files, or use
   > *Go → Go to Folder…* and paste the path above. On Windows/Linux, enable
   > "show hidden files".

   > **No `devices` folder yet?** It is created automatically the first time you save a
   > remote control in Bitwig. If it isn't there, you can either save one panel first (see
   > below) or just create the `devices` folder yourself at the path above.

2. **Back up first.** If you already have panels in there, copy the `devices` folder
   somewhere safe before adding anything.

3. **Unzip into `devices`.** Each zip extracts to a `vst-…` / `vst3-…` / `clap-…` folder
   containing `Default.bwremotecontrols`. Copy **both the folder and the file** into
   `devices` - the folder name is what tells Bitwig which plugin the mapping belongs to.
   The zips are structured so this happens correctly on extraction.

   ```
   devices/
   ├── vst-46536174/
   │   └── Default.bwremotecontrols
   └── clap-com.u-he.Diva/
       └── Default.bwremotecontrols
   ```

4. **If asked to overwrite,** don't - until you've checked which version you want to keep.
   An existing folder means you already have a panel for that plugin.

5. **Restart Bitwig** (or reopen the plugin). Load the plugin and its remote controls
   panel appears automatically.


---

## Contributing: add your own

Made a good panel for a plugin that isn't here yet? Please share it. Two ways: open a Pull
Request on this repo, or post it in the
[KVR thread](https://www.kvraudio.com/forum/viewtopic.php?t=480998).

### 1. Create the remote controls in Bitwig

1. Add the plugin to a track in Bitwig.
2. Open the device panel and click the **remote controls** area (the knob page). Add a page
   and map knobs to the plugin parameters you care about - right-click a knob → *Map to
   controller/page*, or use the mapping browser to pick parameters. Give each knob a clear
   label.
3. Open the panel's context menu (the **≡ / drop-down** on the remote controls header) and
   choose **Save as default for this device** (wording varies slightly by Bitwig version -
   look for the "default"/"save" option). This writes the mapping to
   `.settings/devices/<device-id>/Default.bwremotecontrols`.

### 2. Locate the saved files

Go to your `devices` folder (paths above, remember it's hidden). The newest `vst-…`,
`vst3-…` or `clap-…` folder is the one Bitwig just created. Confirm it contains
`Default.bwremotecontrols`.

> **Which folder is which plugin?** If it's ambiguous, sort by modified date - the one you
> just saved is newest. For VST2 you can also decode the hex: `vst-46536174` →
> `46536174` is hex for the ASCII `FSat` (FabFilter Saturn).

### 3. Zip it, keeping the folder

Zip the `vst-…` folder itself (so the archive contains
`vst-…/Default.bwremotecontrols`), **not** just the loose file. On macOS/Linux:

```bash
cd ~/Documents/Bitwig\ Studio/Library/.settings/devices
zip -r "FabFilter Saturn vst-46536174.zip" vst-46536174
```

On Windows: right-click the `vst-…` folder → *Send to → Compressed (zipped) folder*.

### 4. Name and place it in the repo

- **File name:** `<Readable Plugin Name> <device-id>.zip`
  e.g. `FabFilter Saturn vst-46536174.zip`. Including the plugin **format** helps
  (`... VST3.zip`, `... CLAP.zip`) since the same plugin has different IDs per format.
- **Folder:** drop it in the matching manufacturer folder (`FabFilter/`, `uhe/`, …). If
  there's no obvious home, use the appropriate `Misc …` folder by type
  (`Misc Effects VST3`, `Misc Synth VST3`, `Misc Effects CLAP`, etc.).

### 5. Open a Pull Request

```bash
git checkout -b add-<plugin-name>
git add "FabFilter/FabFilter Saturn vst-46536174.zip"
git commit -m "Add FabFilter Saturn (VST2) device panel"
git push -u origin add-<plugin-name>
```

Then open a PR describing which plugin(s) and format(s) you added. A short note on what the
knobs are mapped to (e.g. "8 knobs: drive, tone, mix, feedback…") is welcome but not
required.

### Contribution guidelines

- **One plugin per zip**, folder preserved inside the archive.
- **Don't overwrite** an existing panel unless yours is clearly better - if in doubt, note
  the difference in your PR and let the maintainer decide.
- **Only share the mapping.** `.bwremotecontrols` files contain remote-control layouts, not
  the plugin itself or any licensed presets - no copyrighted plugin content, please.
- Keep names consistent with the existing files so the collection stays browsable.

---

## Credits & links

- **Original collection & thread:** aMUSEd / kymeia -
  [Device panel sharing thread @ KVR](https://www.kvraudio.com/forum/viewtopic.php?t=480998)
- **Other community collections** referenced in the thread:
  - [lxegs/bitwig](https://github.com/lxegs/bitwig)
  - GoatGirl, strovoknights and DrSyncenstein (links in the KVR thread)

Contributions welcome - the whole point of this collection is that, together, the community
can eventually cover every plugin worth mapping.
