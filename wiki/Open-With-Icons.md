# Open With and File Association Icons

In Windows Explorer, you can assign a program to open some file extension by default, or add it to a list of programs you can quickly open any file with from the right-click "Open with" menu. The "Contained & Controlled" nature of scoop packages makes that a bit harder, but it's still possible.

You can add new programs to the list for any file type by right-clicking > `Open With` > `Choose another app` > `Browse...`. However, there are a few things to keep in mind.

It is IMPERATIVE that you add the proper program path (example: `$HOME/scoop/apps/mpv/current/mpv.exe`), and not the shim path.
   - If you do not go through the `current` junction shortcut and go through the actual version directory (like `0.32.0`), the association will break whenever you update the program.

If you add the shim path (example: `$HOME/scoop/shims/mpv.exe`) instead, it will spell disaster.
 - The shim will show up as a nameless blob with an ugly "unknown" icon in the "Open With" menu.
 - They will be impossible to tell between when you have multiple shims registered.
 - All the associated files too will forever be haunted by that icon.
 - Sometimes the shim won't even close itself after launching the program.

# Fixing Mistakes

Once you add the shim for a program, Windows will not even let you let you add the proper path for it as an option anymore, presumably because it thinks that a program with the same basename (the shim) is already registered and so there's no need to add the new path.

If you have accidentally added a shim, you have to uninstall the package providing that shim and then try to open a file with that program, to trigger Windows into deleting the outdated entry. And then you can install the package again and add the proper path. Or you can do some manual registry hacking.

# Issues

This method does not work across updates with programs that have unstable executable pathing, like GIMP (`bin/gimp-2.10.exe`)
