*Do you have a question that's not answered here? Please create an issue.*

### How do I update my apps?

First, update Scoop to get the latest manifests:

```
scoop update
```

Then update the app, e.g. Git:

```
scoop update git
```

If you want to update all your apps at once, you can use the wildcard '*':

```
scoop update *
```

### How do I install a specific version of an app?

If you need to install a specific version of an app, use `scoop install [app]@[version]`. E.g. for Git:

```
scoop install git@2.23.0.windows.1
```

### Can I install multiple versions of a specific app with scoop?

You can install multiple versions of an app by using the `@[version]` syntax with the install command:

```
scoop install git@2.19.0.windows.1

scoop install git@2.23.0.windows.1
```

You also install new versions of an app by running `scoop update` when a new version is available: the old version will be persisted until you remove them with the `scoop cleanup` command.

**Please note:** running `scoop list` or `scoop info` will show the LATEST version installed, not all of them.

### How do I switch between different versions of an app?

Use `scoop reset [app]@[version]`. E.g. for terraform:

```
scoop reset terraform@0.11.14
```
will set the active installation of terraform to version 0.11.14.
```
scoop reset terraform@0.12.11
```
will reset the active installation of terraform to version 0.12.11.
```
scoop reset terraform
```
will reset the active installation of terraform to the *latest installed version*.

**Please note:** `scoop reset` allows you to switch only between versions that are already installed.

### How do I uninstall an app?

Use `scoop uninstall [app]`. E.g. for Git:

```
scoop uninstall git
```

### Scoop is very slow when installing, locks up the CPU, or shows access denied errors

It's likely that your antivirus or anti-malware program is doing a realtime scan as files are being extracted. Please see [Antivirus and Anti-Malware Problems](https://github.com/lukesampson/scoop/wiki/Antivirus-and-Anti-Malware-Problems) for more information and possible workarounds.

*[todo: insert more FAQS here]*