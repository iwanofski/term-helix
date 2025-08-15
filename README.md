# Creating a macOS Helix Application Using Alacritty

## Why?

I prefer to run my editor as a separate, dedicated macOS application rather than from an existing terminal session.

## How?

It's just Alacritty started with hx command on startup using an AppleScript.
 
## Steps

1. **Copy the existing Alacritty application bundle** (or the provided bundle, if available):

   ```sh
   cp -R /Applications/Alacritty.app /Applications/TermHelix.app
   ```

2. **Copy the HelixEditor application** to your `/Applications` directory.

3. **Edit the application metadata** in `/Applications/TermHelix.app/Contents/Info.plist`:

   * Change `CFBundleName` to `TermHelix`
   * Change `CFBundleIdentifier` to something unique, e.g. `se.iwanofski.termhelix`
   * Change `CFBundleIconFile` to `helix.icns`

4. **Copy helix.icns file**:

   * Copy helix.icns into `/Applications/TermHelix.app/Contents/Resources/`

5. **Refresh the Launch Services cache**:

   ```sh
   touch /Applications/TermHelix.app
   ```

6. **Create an AppleScript launcher** (using the Script Editor or modifying the existing one in repo):

   ```applescript
   -- Adjust the path if Helix is installed somewhere else.
   -- This example assumes Helix was installed via Homebrew.
   do shell script "open -a /Applications/TermHelix.app --args --title 'Helix' -e /opt/homebrew/bin/hx"
   ```

7. **Export the AppleScript**:

   * In Script Editor, choose **File → Export…**.
   * Select **Application** as the file format.
   * Enable **Run-only** mode.
   * Save it to `/Applications`, e.g. `/Applications/Helix.app`.

8. **Update the launcher’s icon**:

   * Right-click the new `Helix.app` and select **Get Info**.
   * Drag the provided Helix logo PNG into the icon preview in the top-left corner of the Info window.
