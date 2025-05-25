# How to make a proper report of problems (or bugs)

## I. Before reporting any problem,

1. Please first read the guides and FAQs above.
    - Why? Because _it is unlikely that you are the first person in the world that has experienced this problem_, so it is likely that there will already be an answer in the guides and FAQ. This then frees us to focus on problems that have never been reported before.

2. Please make sure that you are running the latest release of OrangeFox.
    - Why? Because _the problem that you are reporting may already have been fixed_.

3. Make sure to first search the groups and threads for that specific problem.
    - Why? Because _it may already have been reported_ and there may already be several answers for the problem. 

## II. What you MUST provide

1. The name, and code name, and hardware specifications of __your device__
    - (eg, "Poco F3, alioth, 6GB RAM")
2. The precise version number of __OrangeFox release that you are running__
    - (eg, "OrangeFox R11.1_4_1 Stable")
3. The precise name and version number of __the ROM that you are running__
    - (eg, “crDroidAndroid-13.0-20231213-alioth-v9.12”)
4. The precise name and version number of __the firmware that you are running__ on your device
    - (eg, “V14.0.8.0.TKHMIXM”)
5. Full explanations, and full details, of __the problem that you have experienced__
6. A list of precisely __all the steps that you took__, which led to this situation
    - In other words, how can we reproduce the problem?
7. Copies of the __OrangeFox recovery logs__ which show the problem
    - See (III.) below for how to do this.
    - It is best to take the logs immediately after you experience the problem.
    - Do NOT post a copy of "lastrecoverylog.log" or any other log, if it does not show the problem.
    - **NOTE**: if you do not supply logs, it will be impossible to help you.
8. If the problem arises after flashing a ROM, then you must:
    - Specify the precise name and version number of the ROM
    - Provide a link for the download of that ROM

Please understand that we are not sitting next to you, observing what is going on. We will only know what you tell us.

### What is NOT HELPFUL

1. Posting a screenshot
    - it will simply be deleted; instead, post the logs
2. Posting a video
    - it will simply be deleted; instead, post the logs 
3. Being non-specific
    - instead, provide very full and very specific details
4. Saying things like "it doesn't work", or “I got errors”, etc
    - we have no idea what any of this means
5. Saying things like "I am using the latest OrangeFox"
    - "latest" can mean anything
6. Asking questions like "why is my OrangeFox rebooting to recovery?"
    - or any other “_**why**_” question
    - instead, post very full and very specific details
7. Saying things like "I flashed Blah-Blah ROM" (whatever the ROM) without providing
    - the ROM's full specifications,
    - release version number,
    - whether it is official or unofficial

## III. How do I take the OrangeFox logs for posting?

There are a variety of ways:

- Tap on "_Menu -> More -> Copy Log to SD_", and swipe.
    - This will copy the logs to your internal storage `/sdcard/Fox/`
    - Look for recovery.log there
- Connect a PC to your device via adb, and run the command:
    - `adb pull /tmp/recovery.log`
    - The recovery.log file will be downloaded to the location from which you ran this command
    - And you can then attach the file to your post
- If the boot process gets stuck on the OrangeFox logo, then you should
    - Try the `adb pull /tmp/recovery.log` command referred to above
    - Try to get a logcat, with `adb shell logcat` or `adb logcat`
- For logcats, you can run something like:
    - `adb shell logcat -d >logcat.log`
    - to save the logcat to a file ("logcat.log") on your PC.
- Send the contents of the device's `/sdcard/Fox/logs/` folder
    - Put them all in a single zip archive - as long as at least one of them contains a log showing the problem that you have encountered.

## IV. Last steps

- Open an issue at https://github.com/cachiusa/orangefox_device_xiaomi_veux/issues/new (you will need a GitHub account)
