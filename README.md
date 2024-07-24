# Congratulations & welcome to the Evolution X team!

#### This README.md will walk you though the new device maintainer onboarding proccess!

**Note:** References to "us" or "we" mean [Joey](https://github.com/joeyhuab) or [Anierin](https://github.com/AnierinBliss) in the #maintainers channel of the [Evolution X Discord server](https://discord.gg/Evolution-X). The usage of `codename` should be replaced with your device's codename throughout this proccess.
***

## Device repositories & initial installation images
Please provide us with your [GitHub](https://github.com) and [SourceForge](https://sourceforge.net/) usernames. Specify the repositories you need created or access to under the [Evolution X Devices GitHub organization](https://github.com/Evolution-XYZ-Devices). Once these repositories are created, and you have been granted access and pushed, please set up `evolution.dependencies` in each applicable repository for device source syncing via [roomservice](https://github.com/Evolution-XYZ/vendor_evolution/blob/udc/build/tools/roomservice.py). Additionally, provide a list of image names required to be flashed in order to boot Evolution X recovery (initial installation images) coming from the stock ROM of your device.

***You MAY NOT substitute Evolution X recovery with TWRP or any of its derivatives (OFOX/PBRP):***

**Example message using panther:**

```
# Device:
Device name: Google Pixel 7
Device codename: panther

# Usernames:
GitHub Username: AnierinBliss
SourceForge Username: anierin

# Device Repositories:
1. Repository Name: device_google_panther
   Repository Description: Roomservice & scripts support

2. Repository Name: device_google_pantah
   Repository Description: Device tree for the Google Pixel 7 & 7 Pro

3. Repository Name: device_google_gs201
   Repository Description: Board tree for Google Tensor G2 devices

4. Repository Name: device_google_gs101
   Repository Description: Board tree for Google Tensor G1 devices

5. Repository Name: device_google_gs-common
   Repository Description: Common interfaces & sepolicies for tensor-based Google Pixel devices

6. Repository Name: packages_apps_PixelParts
   Repository Description: Parts application for tensor-based Google Pixel devices

# Initial installation images (required to boot Evolution X recovery when coming from the stock ROM):
1. boot
2. dtbo
3. vendor_kernel_boot
4. vendor_boot
```

**Example evolution.dependencies roomservice chain using panther:**

`device_google_panther`
```
[
  {
    "repository": "device_google_pantah",
    "target_path": "device/google/pantah"
  }
]
```
`device_google_pantah`
```
[
  {
    "repository": "device_google_gs201",
    "target_path": "device/google/gs201"
  },
  {
    "repository": "device/google/pantah-kernel",
    "target_path": "device/google/pantah-kernel",
    "remote": "aosp-pantah"
  }
]
```
`device_google_gs201`
```
[
  {
    "repository": "device_google_gs101",
    "target_path": "device/google/gs101"
  }
]
```
`device_google_gs101`
```
[
  {
    "repository": "device_google_gs-common",
    "target_path": "device/google/gs-common"
  },
  {
    "repository": "packages_apps_PixelParts",
    "target_path": "packages/apps/PixelParts"
  }
]
```

## Signed releases
Maintainers are required to sign all releases with [our private keys](https://github.com/Evolution-XYZ/vendor_evolution-priv_keys). There are no manual signing steps required; just clone the repository to `$ANDROID_BUILD_TOP/vendor/evolution-priv/keys/` and compile. Releases not signed by these keys will be removed from sourceforge without warning.

***Please keep these keys confidential. Any intentional or accidental leak will result in immediate removal from the project, and you won't be allowed back. Be cautious, especially if you're using a shared server!***

## Upload to SourceForge
Maintainers are responsible for uploading releases and initial installation images to SourceForge.

**Example upload using panther:**
```
scp EvolutionX-14.0-20240724-panther-v9.2-Official.zip anierin@frs.sourceforge.net:/home/frs/p/evolution-x/panther/14/
```
```
scp boot.img anierin@frs.sourceforge.net:/home/frs/p/evolution-x/panther/14/boot/
```
```
scp dtbo.img anierin@frs.sourceforge.net:/home/frs/p/evolution-x/panther/14/dtbo/
```
```
scp vendor_kernel_boot.img anierin@frs.sourceforge.net:/home/frs/p/evolution-x/panther/14/vendor_kernel_boot/
```
```
scp vendor_boot.img anierin@frs.sourceforge.net:/home/frs/p/evolution-x/panther/14/vendor_boot/
```

## Create an XDA thread
Create an [XDA forums thread](https://xdaforums.com/all-forums-by-manufacturer) using the `$ANDROID_BUILD_TOP/evolution/XDA/generate_xda_thread.sh` script found in the [XDA repository](https://github.com/Evolution-XYZ/XDA).

***Please be aware that you will need to manually update the installation instructions to reflect the procedure specific to your device. The ability to automatically update these instructions during the generation script will be added in the future.***

## Add your device to OTA
Fork the [OTA](https://github.com/Evolution-XYZ/OTA) repository and add your device using the json located at `$ANDROID_BUILD_TOP/out/target/product/codename/codename.json`. The contents must match that of the uploaded build! If the device is not currently in the OTA repository, your codename.json will look similar to the example below. it is up to you to fill out the unpopulated fields and create a codename.txt changelog. Subsequent jsons in `$ANDROID_BUILD_TOP/out/target/product/codename/codename.json` will be fully populated using `$ANDROID_BUILD_TOP/evolution/OTA/codename.json` after your initial commit to OTA has been merged, allowing you to directly copy/paste.

**Example codename.json name using panther:**
```json
{
  "response": [
    {
      "maintainer": "",
      "oem": "",
      "device": "",
      "filename": "EvolutionX-14.0-20240724-panther-v9.2-Official.zip",
      "download": "https://sourceforge.net/projects/evolution-x/files/panther/14/EvolutionX-14.0-20240724-panther-v9.2-Official.zip/download",
      "timestamp": 1721265041,
      "md5": "07140ee6012c7915c62aede743b3351c",
      "sha256": "2c3fb0151a2f3541c6384524ae63cc182a517b9138d4bac68b81031545da5223",
      "size": 2699564180,
      "version": "9.2",
      "buildtype": "",
      "forum": "",
      "firmware": "",
      "paypal": "",
      "telegram": ""
    }
  ]
}

```

When done, commit, push, and submit a pull request via [comparing forks](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/viewing-and-comparing-commits/comparing-commits#comparing-across-forks) with the main OTA repository; we will review and merge it at our earliest convenience.

**Example initial commit message using panther:**
```
OTA: Add Google Pixel 7 (panther)
```

**Example subsequent commit message using panther:**
```
Panther: 07/24/2024 Update
```
---
# KeepEvolving!
