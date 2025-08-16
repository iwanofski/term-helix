# Creating a macOS Helix Application Using Alacritty

![Demonstrate TermHelix App](output.gif)

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
property termAppPath : "/Applications/TermHelix.app"
property hxPath : "/opt/homebrew/bin/hx"
property windowTitle : "Helix"
property winColumns : 220 -- width in characters
property winLines : 30    -- height in characters

on run
	-- No files: just launch Helix
	my launchHelix({})
end run

on open theItems
	-- Files dropped: open them all
	my launchHelix(theItems)
end open

on launchHelix(theItems)
	set argString to ""
	repeat with anItem in theItems
		set fp to POSIX path of anItem
		set argString to argString & " " & quoted form of fp
	end repeat
	
	-- Build command with window size options
	set cmd to "open -a " & quoted form of termAppPath & ¬
		" --args --title " & quoted form of windowTitle & ¬
		" --option window.dimensions.columns=" & winColumns & ¬
		" --option window.dimensions.lines=" & winLines & ¬
		" -e " & quoted form of hxPath & argString
	
	do shell script cmd
end launchHelix
```

7. **Export the AppleScript**:

   * In Script Editor, choose **File → Export…**.
   * Select **Application** as the file format.
   * Enable **Run-only** mode.
   * Save it to `/Applications`, e.g. `/Applications/Helix.app`.

8. **Update the launcher’s icon**:

   * Right-click the new `Helix.app` and select **Get Info**.
   * Drag the provided Helix logo PNG into the icon preview in the top-left corner of the Info window.
