_Article stub. Please fill out/correct as needed. Maybe this is already or better documented elsewhere, in that case remove/redirect._

For general understanding and troubleshooting it can be helpful to know what folders Scoop uses and where they are. These are examples, configuration options could change where they are on your machine.

`%USERPROFILE%\scoop` - per user root location (default)  
`%SCOOP_GLOBAL%` - root location of apps installed for all users, `%SYSTEMDRIVE%\ProgramData\scoop`  
  
`...\scoop\buckets` - manifests of installable apps (also a git clone of the bucket source repositories)  
`...\scoop\cache` - the downloaded installers  
`...\scoop\persist` - ...?  
`...\scoop\shims` - added to PATH, wrappers that point to the installed applications  

`%USERPROFILE%\.config\scoop` - _(?) seems to be where network path to primary Scoop repository is defined_




