### What are buckets?
In Scoop, buckets are collections of apps. Or, to be more specific, a bucket is a Git repository containing JSON [app manifests](App-Manifests) which describe how to install an app.

Scoop has a [main bucket](https://github.com/ScoopInstaller/Main/tree/master/bucket) which is bundled with Scoop and this is always available as the primary source for installing apps.

By default, when you run `scoop install <app>`, it looks in the main bucket, but it's possible to install from other buckets too.

There's an optional [extras bucket](https://github.com/lukesampson/scoop-extras) containing apps that don't quite fit the [criteria of the main bucket](https://github.com/lukesampson/scoop/wiki/Criteria-for-including-apps-in-the-main-bucket), but are still good to have. There is also an optional [versions](https://github.com/ScoopInstaller/Versions) bucket containing older versions of some well-known packages.

And Scoop supports adding other buckets. Anyone can set up their own bucket with their own set of apps, and other people can add and install from this bucket—they just need to know the location of the bucket's Git repository.

### Known buckets

There is a list of known buckets by the community, those can be seen in [`buckets.json`](https://github.com/lukesampson/scoop/blob/master/buckets.json), to see the list of known buckets execute:

```
scoop bucket known
```

### Installing from other buckets
If you want to install from a bucket besides the main one, you need to configure Scoop to know about the bucket. For example, to add the optional 'extras' bucket, run:

    scoop bucket add extras

The 'extras' bucket is a [special bucket](https://github.com/lukesampson/scoop/blob/master/buckets.json), in that it's "well known", i.e. Scoop already knows where this bucket is so you don't have to specify its location.

Just say the extras bucket wasn't well known, the way you'd add it would be:

    scoop bucket add extras https://github.com/lukesampson/scoop-extras.git

That is,

    scoop bucket add <name-of-bucket> <location-of-git-repo>

You can run `scoop help bucket` for more information on buckets.

### Creating your own bucket

#### Using template

You can use the [BucketTemplate](https://github.com/ScoopInstaller/BucketTemplate) repository to create the boilerplate for a bucket. Just click on `Use this template` button. Choose a name for your bucket, follow the instructions at the bottom of the Readme, and you're good to go. The template has a bunch of GitHub Actions and AppVeyor scripts, which gives several advantages:
- **Continuous integration**: Each new commit will have Scoop's tests run against it, to keep files well-formatted and readable.
- **Continuous delivery**: An automated workflow runs every 4 hours, which updates all the manifests in the bucket to their latest versions.
- **Issue handlers**: Common issues like 404 errors and hash-check failures are handled and corrected automatically.
- **Pull Request handlers**: Pull requests are validated by running checks for required properties, hash verification, CI tests etc.

#### Manually

Here's how you might go about manually creating a new bucket, using GitHub to host it. You don't have to use GitHub though—you can use whatever source control repo you like, or even just a Git repo on your local or network drive. Create a new GitHub repo called e.g. `my-bucket`. Add an app to your bucket, like this:

```powershell
git clone https://github.com/<your-username>/my-bucket
cd my-bucket
'{ version: "1.0", url: "https://gist.github.com/lukesampson/6446238/raw/hello.ps1", bin: "hello.ps1" }' > hello.json
git add .
git commit -m "add hello app"
git push
```

#### After the bucket is created

1. Configure Scoop to use your new bucket:

```powershell
scoop bucket add my-bucket https://github.com/<your-username>/my-bucket
```
2. Check that it works:

```powershell
scoop bucket list # -> you should see 'my-bucket'
scoop search hello # -> you should see hello listed under, 'my-bucket bucket:'
scoop install hello
hello # -> you should see 'Hello, <windows-username>!'
```
3. To share your bucket, all you need to do is tell people how to add your bucket, i.e. by running the command in step 3. If you want your bucket listed in the [Scoop Directory](https://github.com/rasa/scoop-directory) , add a topic of `scoop-bucket` to its github page.
