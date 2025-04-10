---
order: 5
---

# Building Using Crave Run command

> [!NOTE]
> This section shows you how commands work. Skip to The next half for [how to also clone device sources in there](#Clone-Device-Sources-During-Build)

```mermaid
graph LR
    A[Devspace User 1] --> M
    B[Devspace User 2] --> M
    C[Devspace User 3] --> M
    D[Devspace User 4] --> M

    M[Queue:: Build 1,2,3,4,.... n]

    M --> E[Build server 1]
    M --> F[Build server 2]
    M --> G[Build server 3]

    style A stroke:#0047b3
    style B stroke:#0047b3
    style C stroke:#0047b3
    style D stroke:#0047b3
    
    style M stroke:#cc7a00
    
    style E stroke:#006600
    style F stroke:#006600
    style G stroke:#006600

    P("Devspace is a small development platform for individual Crave users")
    Q("This is the queue which can be accessed in foss.crave.io<br>")
    R("Build nodes are supreme build machines that receive builds from the queue.")

    S("**PROJECTS ARE NEITHER SYNCED HERE NOR BUILT HERE! ie.repo sync and build commands must not be used here**")
    T("(You will wait here in the queue till build node is allocated)")
    U("The actual build happens here, ie. all the scripts in the build request will be executed.<br>**(REPO SYNC and BUILD COMMANDS are allowed)**")
    P ==> Q ==> R
    S === T === U
```
<!-- (Diagram idea by @subhahbus, converted by [Yuvraaj](https://github.com/Uvatbc))-->
<sub>TODO: Give credit</sub>

Simply use crave run! On a normal server, we'd be running commands one
by one. But since crave uses a queue system, we run all the commands in
one go, and wait for our turn in queue. Then, the build node will
execute our commands, one by one.

- Syntax:

```bash
crave run --no-patch -- "your commands"
```

- If you'd like a clean build, add --clean flag

Note: using clean build will reset the image to default. This means it removes any of your progress e.g. synced dt/or the out folder from previous build and ensures we're back to the default source code of the base project. 

Please avoid doing this needlessly as resyncing and building from takes a lot of time. 

Syntax:

```bash
crave run --clean --no-patch -- "your commands"
```

When you run a build using crave run, it adds you to the build queue,
where a build node comes, picks it up and compiles your build for you!

# Clone Device Sources During Build

Options: officially supported devices, local manifests, sync scripts,
and manual git clone.

- Officially Supported devices: They will automatically clone source
  code through breakfast or brunch commands, just refer to the official
  instructions for your device. You will need proprietary blobs, which
  can usually be found on TheMuppets repositories.

```bash
crave run  --no-patch -- "rm -rf .repo/local_manifests; \
git clone https://github.com/TheMuppets/manifests --depth 1 -b lineage-21.0 .repo/local_manifests; \
/opt/crave/resync.sh; \
source build/envsetup.sh; \
brunch Mi439"   
```

- Local manifests example:

```bash
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

```bash
crave run  --no-patch -- "rm -rf device/oem/codename kernel/oem/codename vendor/oem/codename; \
git clone https://github.com/sounddrill31/android_device_oem_codename --depth 1 -b branchname device/oem/codename; \
git clone https://github.com/sounddrill31/android_kernel_oem_codename --depth 1 -b branchname kernel/oem/codename; \
git clone https://github.com/sounddrill31/android_vendor_oem_codename --depth 1 -b branchname vendor/oem/codename; \
source build/envsetup.sh; \
lunch lineage_oxygen-eng; \
m bacon"
```


> [!NOTE]
> This section assumes you're trying to build only Crave-supported ROMs. Skip to The next section if you want to build unsupported ROMs.