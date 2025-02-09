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

features:
  - title: Crave Devspace CLI
    details: A guide to using Crave Devspace CLI to build android
    link: ./getting-started/reading-rules
  - title: Crave Rules
    details: Rules for Using Crave.io's FOSS Services
    link: ./rules
  - title: Crave Tricks
    details: Some tips and tricks we discovered when we first discovered crave
    link: ./tricks
  - title: Crave AOSP Builder(Github Actions)
    details: sounddrill's workflow to automate builds using Crave.io
    link: https://github.com/sounddrill31/crave_aosp_builder
  #- title: Add More
  #  details: Suggestions for More Entries
---
<!--mainly crave guide, crave rules, crave debugging tips, crave tricks-->
# Context

Earlier, Cirrus CI gave the ROM Builders community servers for OSS developement of Android builds, as these builds need a lot of resources that most people don't have lying around, like double digit RAM requirements (GB), triple digit storage requirements (GB), a capable processor, a specific Linux environment, etc.

Cirrus CI has discontinued ROM Builders around Nov 2023. Now, [crave.io](https://crave.io) is providing Build Servers with a pretty similar Queue System. It is also a lot more flexible as it allows custom commands, has Docker image support, allows entering into build storage without starting a build, etc.

This guide attempts to help new and old users with Android development and using crave.io through Devspace CLI and alternatively, Github Actions.

### General Workflow

To use crave, you'll likely be doing this:
- Enter Devspace CLI ([Guide](./getting-started/installing-crave.md))
- Crave Cloning a new project and entering that folder([Guide](./getting-started/setting-project.md))
- Set up workspace Persistance ([Guide](./getting-started/more-info.md#craveyaml))
- Using Crave Run to start build ([Guide](./getting-started/building-crave-run.md))
- Pulling your Output ([Guide](./getting-started/pulling-output.md))
- Destroying old crave clone(optional) ([Guide](./getting-started/setting-project.md))
