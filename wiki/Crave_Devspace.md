

## Setting Up The Project

1. First use list to check the available projects:

```
crave clone list  # note down the project id for the next step
```

2. Then, use crave clone create for the project that you'd like to
build! This will automatically snapclone the latest source code for the
project that you are trying to build in a matter of seconds

```
crave clone create --projectID 72 /crave-devspaces/Lineage21
```

3. Remember to enter that folder you just created
```
cd /crave-devspaces/Lineage21
```

After you're done building the ROM at the end of this guide, use crave
clone destroy to delete the folder. Do not use rm -rf!

```
crave clone destroy /crave-devspaces/Lineage21
```

## Building Using Crave Run command

### How to build using Crave Run Command

Simply use crave run! On a normal server, we'd be running commands one
by one. But since crave uses a queue system, we run all the commands in
one go, and wait for our turn in queue. Then, the build node will
execute our commands, one by one.

- Syntax:

```
crave run --no-patch -- "your commands"
```

- If you'd like a clean build, add --clean flag

Note: using clean build will reset the image to default. This means it removes any of your progress e.g. synced dt/or the out folder from previous build and ensures we're back to the default source code of the base project. 

Please avoid doing this needlessly as resyncing and building from takes a lot of time. 

Syntax:

```
crave run --clean --no-patch -- "your commands"
```

When you run a build using crave run, it adds you to the build queue,
where a build node comes, picks it up and compiles your build for you!

### How to clone device sources inside Crave Run Command

Options: officially supported devices, local manifests, sync scripts,
and manual git clone.

- Officially Supported devices: They will automatically clone source
  code through breakfast or brunch commands, just refer to the official
  instructions for your device. You will need proprietary blobs, which
  can usually be found on TheMuppets repositories.

``` 
crave run  --no-patch -- "rm -rf .repo/local_manifests; \
git clone https://github.com/TheMuppets/manifests --depth 1 -b lineage-21.0 .repo/local_manifests; \
/opt/crave/resync.sh; \
source build/envsetup.sh; \
brunch Mi439"   
```

- Local manifests example:

```
crave run  --no-patch -- "rm -rf .repo/local_manifests; \
git clone https://github.com/sounddrill31/reponame --depth 1 -b branchname .repo/local_manifests; \
/opt/crave/resync.sh; \
source build/envsetup.sh; \
lunch lineage_oxygen-eng; \
m bacon"   
```

[~marado](https://tilde.pt/~marado/) made an easy to follow guide on
making your own local manifests over at
[tilde.pt](https://tilde.pt/~marado/blog/repo-using-a-local-manifest.html).

You can also use sounddrill's [Generator script](https://github.com/sounddrill31/actions_generate_local_manifests) which works most of the time 

Local manifests rely on repo sync. We have made a simple script to repo
sync while avoiding majority of conflicts which arise due to uncommitted
changes, or when building a different ROM. You can find the source code
to resync.sh
[here](https://github.com/accupara/docker-images/blob/master/aosp/common/resync.sh)

- Git clone example:

```
crave run  --no-patch -- "rm -rf device/oem/codename kernel/oem/codename vendor/oem/codename; \
git clone https://github.com/sounddrill31/android_device_oem_codename --depth 1 -b branchname device/oem/codename; \
git clone https://github.com/sounddrill31/android_kernel_oem_codename --depth 1 -b branchname kernel/oem/codename; \
git clone https://github.com/sounddrill31/android_vendor_oem_codename --depth 1 -b branchname vendor/oem/codename; \
source build/envsetup.sh; \
lunch lineage_oxygen-eng; \
m bacon"
```

## Bonus: Building Unsupported ROM
This is discouraged as it causes many sync conflicts and is a bit difficult to debug, however, it is not against the rules.
Rules for doing this:

- Do not use rm -rf * or cd into another folder in crave run before
  syncing, no matter who tells you to
- Fix sync conflicts instead of running the above commands
- (General Rule) Do not queue multiple builds at once
- Sync android 14 ROMs on Android 14 base project only

Example: Building crDroid 14

1. Set up closest cousin project that crave supports. Since it's
crDroid a14, you could use CipherOS or LineageOS for this (like
[this](./Crave_Devspace#setting-up-the-project))
using crave clone create.

2. Run crave run command, but reinit your preferred rom inside

```
crave run --no-patch -- "rm -rf .repo/local_manifests; \
repo init -u https://github.com/crdroidandroid/android.git -b 14.0 --git-lfs; \
git clone https://github.com/youraccount/local_manifests --depth 1 -b rising-14 .repo/local_manifests; \ 
/opt/crave/resync.sh; \
source build/envsetup.sh; \
breakfast device_codename userdebug; \ 
mka bacon"
```

## Pulling Output

### Automatic Method: Github Releases upload.sh script

- Enter the directory where you used crave run
- Create token.txt with your github PAT in there(ensure it has necessary
  permissions)
- Use the upload script

```
bash /opt/crave/github-actions/upload.sh 'tag' 'device' 'repository' 'release title' 'extra files'
```

(Replace tag with the tag you want your release to use, device with the
device's codename, repository with the link to the github repo and
release title with your preferred title. Extra files can be left blank)

You can find the source code for the script [here](https://github.com/accupara/docker-images/blob/master/aosp/common/upload.sh)

Tip: By default, size limit is set to 2147483648. To change it, run this
before the above command

```
export GH_UPLOAD_LIMIT="yourvalue"
```

### Automatic Method: Telegram upload.sh script

- Enter the directory where you used crave run
- [Set up
  telegram-upload](https://github.com/Nekmo/telegram-upload#-usage)
- Use the upload script

```
/opt/crave/telegram/upload.sh 'device' 'extra files'
```

Uploads will land in your saved messages. You can find the source code
for the script [here](https://github.com/accupara/docker-images/blob/master/aosp/common/tgup.sh)

Tip: By default, size limit is set to 2147483648. To change it, run this
before the above command

```
export TG_UPLOAD_LIMIT="yourvalue"
```

### Manual Method: How to pull output to Devspace CLI

- Simply use the crave pull command inside the same directory as before:

```
crave pull out/target/product/*/*.zip
```

```
crave pull out/target/product/*/*.img
```

- This will pull the built zip file to your devspace CLI from the build
  node.

<!-- -->

- From here, you can upload using github releases, [SourceForge](https://github.com/MaheshTechnicals/upload_to_sourceforge), telegram-upload, etc

# Advanced

### Bonus: Building without Devspace CLI

- Follow all the steps except do not enter into Devspace CLI(crave
  devspace command).
- Crave run, pull, etc should work on local machine as well(as long as
  crave is installed/set up along with a valid crave.conf)
- You might also want to install google's repo tool:

```
mkdir -p ~/.bin;
```
```
PATH="${HOME}/.bin:${PATH}";
```
```
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo;
```
```
chmod a+rx ~/.bin/repo
```
**Do Not Do This Inside Crave Devspace CLI!**


### Setting up the Project - Alternative Method

Instead of setting up through crave clone create, you could directly repo init a supported ROM too! Crave will see git project is set up and assign accordingly. Note that this method is not recommended if you're using Devspace CLI or RAS(web client). This makes sense when you're trying to trigger without devspace

```
mkdir /crave-devspaces/Lineage21; cd /crave-devspaces/Lineage21
```

```
repo init -u https://github.com/LineageOS/android.git -b lineage-21.0 --git-lfs --depth=1
```

- At the time of writing, there was a known issue with LineageOS
  projects (Error: Found multiple matching projects, please specify
  project id on command line).
- If you're initializing lineageOS, use the following repos instead of
  LineageOS/android

For lineageOS 21, run

```
repo init -u https://github.com/LineageOS/android.git -b lineage-21.0 --git-lfs --depth=1
```

For lineageOS 20, run

```
repo init -u https://github.com/accupara/los20.git -b lineage-20.0 --git-lfs --depth=1
```

**Do not run repo sync or build the ROM here as it slows down devspace
CLI and causes trouble for other users. Devspace CLI also does not have
the resources to build a ROM or a similar huge project directly.**

### Workspace Persistence

Workspace persistence here refers to the preservation of what you have
synced and compiled in the past. This is not needed if you're building from Crave RAS(the web shell)

This build storage is reset when one of these 4 factors change:

```
1. Project UUID
2. User ID  
3. Workspace Dir 
4. Workspace Branch Sig 
```

- **Project UUID** is a unique ID to differentiate one project from
  another (this is automatically handled by crave and cannot be changed
  by users)
- **User ID** is a unique ID assigned to users (this is automatically
  handled by crave and cannot be changed by users)
- **Workspace Dir** is the name of the folder you have set up just
  now(the name/path matters, not the contents). Here are the recommended
  directory names if you frequently use my [github actions
  repo](https://github.com/sounddrill31/crave_aosp_builder):

```
/crave-devspaces/Arrow13
/crave-devspaces/Lineage20
/crave-devspaces/Lineage21 
/crave-devspaces/DerpFest13 
/crave-devspaces/Cipher14 
/crave-devspaces/Pixel14
/crave-devspaces/Lineage16
/crave-devspaces/Cyanogen14
```

Tip: If you find this part confusing, just ensure your workspace folder
is named as one of the folders listed above or the one you previously
built with

- **Workspace Branch Sig** is the Signature generated from the machine
  name. This changes if you trigger builds using different machines,
  often with different hostnames. To ignore this, you can use
  ignoreClientHostname inside crave.yaml as shown in the next section.

Tip: You use --clean flag in crave run to reset the build storage as
shown
[here](./Crave_Devspace#how-to-build-using-crave-run-command)

### Crave.yaml

This is a file to pass specific instructions to crave. Because repo init
put files in .repo/manifests, that's where our crave.yaml should go.
Move it to <Workspace Dir>/.repo/manifests/crave.yaml.

crave.yaml can be used to pass environment variables, push certain
files, use a certain docker image, etc.

The name has to match with the project name on dashboard(eg "Arrow OS"
"LOS 20"). You can also find this through crave clone list.

Examples:

```
CipherOS:
  ignoreClientHostname: true 
Arrow OS:
  ignoreClientHostname: true 
DerpFest-aosp:
  ignoreClientHostname: true 
LOS 20:
  ignoreClientHostname: true 
LOS 21:
  ignoreClientHostname: true
PixelOS:
  ignoreClientHostname: true
Rising OS:
  ignoreClientHostname: true
LOS 16:
  ignoreClientHostname: true
LOS CM 14.1:
  ignoreClientHostname: true  
```

```
TWRP:
  ignoreClientHostname: true
  image: "sounddrill31/crave-archlinux-twrp@sha256:605892162a907ea3760813643c9aa1bdb63a7ae0dce6ca159b0f6e20a7c0815b"
```

Do not change the image unless you know what you're doing and have a very good reason. 

For more info, read the [official
documentation](https://foss.crave.io/docs/crave-usage/#location-of-the-craveyaml-file)

Tip: If you find this part confusing, just run this command inside the
folder we made before

```
rm .repo/manifests/crave.yaml* || true; # Removes existing crave.yamls

curl -o .repo/manifests/crave.yaml https://raw.githubusercontent.com/sounddrill31/crave_aosp_builder/main/configs/crave/crave.yaml.aosp # Downloads crave.yaml
```

### Optional: Setting up build environment

Use this command to enter into a machine with your source code mounted:

```
crave ssh
```

This is good for changing files for the purpose of testing but this will
be wiped if you do a clean build.

**Do not run repo sync or build the ROM here, wait in the crave run
queue for a more powerful machine.**

You could get banned for syncing or building in this environment!

### Paid Queue
You can use crave tokens to skip the queue and get the build to start faster! To use tokens from your wallet, use the flag `--platform aosp-silver`

Like this:
```
crave run --no-patch --platform aosp-silver -- "build commands here"
```

If there are no tokens left in your wallet, the paid build will fail(but you can still use the free queue). A running build is able to go into Negative credits but no new paid builds can be requested while having less than 0 credits.

[Crave doc for Wallet](https://foss.crave.io/docs/wallets)
