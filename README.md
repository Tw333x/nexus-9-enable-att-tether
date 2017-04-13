# nexus-9-enable-att-tether
Hack for a Nexus 9 LTE (volantisg/flounderlte) to support tethering with AT&amp;T 

For whatever reason, Google has disabled tethering options when using at AT&T SIM in the Nexus 9 tablet:
https://android.googlesource.com/device/htc/flounder/+/master/overlay/frameworks/base/core/res/res/values-mcc310-mnc410/config.xml

This annoys me, as my plan supports tethering. I know this can be fixed by modifying framework-res.apk, but I'm not really interested in having to modify it with each OS update. I figured out that you can overlay framework-res.apk, and built a simple apk that brings back these options.

You'll have to disable verity validation to be able to successfully modify the /vendor (or /system) partitions; I used this:
https://build.nethunter.com/android-tools/no-verity-opt-encrypt/

# Installing my pre-built APK

If you just want to install this, and trust my APK:
* Make sure you have verity disabled, or none of this will work.
* Download the APK and the most recent TWRP for your device
* Unlock the bootloader if not unlocked already (this will wipe all user data of course)
* Boot into TWRP (adb reboot bootloader ; sleep 5 ; fastboot boot twrp.img)
* Mount /vendor
* 'adb shell mkdir /vendor/overlay'
* 'adb push framework-res-overlay.apk /vendor/overlay'
* Unmount /vendor, and reboot

..you should have the options!

# Building your own APK

Note - I'm not an Android developer; this may be the wrong way to do this... but it works.  :)  Pull requests appreciated!

* Install the Android SDK
* Grab a copy of /system/framework/framework-res.apk from your tablet
* Clone this repo
* Go into the source directory from this repo, and run:
`<</path/to/sdk>>/build-tools/<<version>>/aapt package -S res/ -I <<path/to/framework-res.apk>> -M AndroidManifest.xml -f -v -F ../framework-res-overlay.apk`

Then, follow the directions above.
