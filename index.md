---
# https://vitepress.dev/reference/default-theme-home-page
layout: home

hero:
  name: "FOSSonTop"
  text: "Crave.io AOSP Docs"
  actions:
    - theme: brand
      text: Add Content to Docs
      link: https://github.com/FOSSonTop/crave
    - theme: alt
      text: Go Back
      link: ../index

#features:
#  - icon: We'll fill these later as we're doing the migration
#    title: 
#    link: mainly crave guide, crave rules, crave debugging tips, crave tricks
---

# Context

Earlier, Cirrus CI gave the ROM Builders community servers for OSS developement of Android builds, as these builds need a lot of resources that most people don't have lying around, like double digit RAM requirements (GB), triple digit storage requirements (GB), a capable processor, a specific Linux environment, etc.

Cirrus CI has discontinued ROM Builders around Nov 2023. Now, [crave.io](https://crave.io) is providing Build Servers with a pretty similar Queue System. It is also a lot more flexible as it allows custom commands, has Docker image support, allows entering into build storage without starting a build, etc.

This guide attempts to help new and old users with Android development and using crave.io through Devspace CLI and alternatively, Github Actions.

## Crave Resources

Here are some useful guides, links and write-ups for using Crave.io resources for building android ROMs:

- [Crave Devspaces CLI](./wiki/Crave_Devspace)
- [Crave Devspaces CLI - Rules](./wiki/Crave_Rules)
- [Crave Devspaces CLI - Additional Tips and Tricks](./wiki/Crave_Tricks)
- [Crave Devspaces CLI - Signing Builds(Advanced/WIP)](./wiki/Crave_Signing)
- [Crave AOSP Builder (Github Actions)](https://github.com/sounddrill31/crave_aosp_builder)

### General Workflow

To use crave, you'll likely be doing this:
- Enter Devspace CLI ([Guide](./Crave_Devspace#downloading))
- Crave Cloning a new project and entering that folder([Guide](./Crave_Devspace#setting-up-the-project))
- Set up workspace Persistance ([Guide](./Crave_Devspace#workspace-persistence))
- Using Crave Run to start build ([Guide](./Crave_Devspace#building-using-crave-run-command))
- Pulling your Output ([Guide](./Crave_Devspace#pulling-output))
- Destroying old crave clone(optional) ([Guide](./Crave_Devspace#setting-up-the-project))
