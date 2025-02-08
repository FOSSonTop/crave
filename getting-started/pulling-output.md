---
order: 6
---

# Getting Started 
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
