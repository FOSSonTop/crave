---
order: 8
---

# More Info

## Bonus: Building without Devspace CLI

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


## Setting up the Project - Alternative Method

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

## Workspace Persistence

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
[here](./building-crave-run.md#how-to-build-using-crave-run-command)

## Crave.yaml

This is a file to pass specific instructions to crave. Because repo init
put files in .repo/manifests, that's where our crave.yaml should go.
Move it to [Workspace Dir]/.repo/manifests/crave.yaml.

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

## Optional: Setting up build environment

Use this command to enter into a machine with your source code mounted:

```
crave ssh
```

This is good for changing files for the purpose of testing but this will
be wiped if you do a clean build.

**Do not run repo sync or build the ROM here, wait in the crave run
queue for a more powerful machine.**

You could get banned for syncing or building in this environment!

## Paid Queue
You can use crave tokens to skip the queue and get the build to start faster! To use tokens from your wallet, use the flag `--platform aosp-silver`

Like this:
```
crave run --no-patch --platform aosp-silver -- "build commands here"
```

If there are no tokens left in your wallet, the paid build will fail(but you can still use the free queue). A running build is able to go into Negative credits but no new paid builds can be requested while having less than 0 credits.

[Crave doc for Wallet](https://foss.crave.io/docs/wallets)
