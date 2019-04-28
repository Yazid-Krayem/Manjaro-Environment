# Android Studio Installation guide

 resize `tmp` folder  using this code 
`sudo mount -o remount,size=10G,noatime /tmp`
`df -h` to check the size 

it's temporary file system **SO** Don't reboot until you finish the download 


1. INSTALL REACT-NATIVE GLOBALLY 
`npm install -g react-native-cli`

2. INSTALL `ANDROID-STUDIO` FROM `AUR`

###Steps in `ANDROID-STUDIO`:

a. choose`Custom` not `Standard` after that add `android virtual device`

---

3. INSTALL `watchman` FROM `AUR`

4. use this command 
```
export ANDROID_HOME=~/Android/Sdk/
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/platform-tools

// for check 

 ### echo $ANDROID_HOME

/////

export "ANDROID_HOME=~/Android/Sdk/" >> ~/.bashrc

```


5. Android-Studio >> Configure >>  SDK manager >> SDK tools and install `Android Emulator` 


 ## [README](README.md)
 
