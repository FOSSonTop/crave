---
order: 3
---

# Getting Started
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

> [!NOTE]
> It is highly recommended that you take a second and set up crave.yaml and come back here. You can find the link to the guide here: [[link]](./more-info.md#craveyaml)


> [!WARNING]
> Do not use repo sync or repo init here or try to build in here. You are still in the `Devspace CLI` environment where these actions are prohibited and will likely get you banned. Go to the next section to follow how to do it the right way.