---
order: 6
---

# Bonus: Building Unsupported ROM

This is discouraged as it causes many sync conflicts and is a bit difficult to debug, however, it is not against the rules.
Rules for doing this:

- Do not use rm -rf * or cd into another folder in crave run before
  syncing, no matter who tells you to
- Fix sync conflicts instead of running the above commands
- (General Rule) Do not queue multiple builds at once
- Sync android 14 ROMs on Android 14 base project only

```mermaid
%%{init: {'themeVariables': { 'primaryColor': '#ffcccc', 'edgeLabelBackground':'#fff', 'tertiaryColor':'#ccffcc'}}}%%
graph LR
    %% Diagram 1: Create crave clone -> start build
    A[Devspace] --> M
    M[Queue] --> N[Build Node]
    
    style A fill:#000000,stroke:#0047b3
    style M fill:#000000,stroke:#cc7a00
    style N fill:#000000,stroke:#006600

    P("Devspace: Create crave clone(using a similar ROM as base) and trigger a build") 
    Q("Queue: Wait for the build to start") 
    R("Build Server: Uses crave run commands to build and sync the ROM you want")
    
    P ==> | eg. LineageOS 22|Q ==> R --> |eg. crDroid 14 | S(Build Done)
  ```

Example: Building crDroid 14

1. Set up closest cousin project that crave supports. Since it's
crDroid a14, you could use CipherOS or LineageOS for this (like
[this](./setting-up-the-project.md))
using crave clone create.

2. Run crave run command, but reinit your preferred rom inside

```bash
crave run --no-patch -- "rm -rf .repo/local_manifests; \
repo init -u https://github.com/crdroidandroid/android.git -b 14.0 --git-lfs; \
git clone https://github.com/youraccount/local_manifests --depth 1 -b rising-14 .repo/local_manifests; \ 
/opt/crave/resync.sh; \
source build/envsetup.sh; \
breakfast device_codename userdebug; \ 
mka bacon"
```