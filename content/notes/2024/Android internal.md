+++
title = 'Android internal Notes'
date  = 2024-01-17T08:19:32.3232+05:30
tags  = ['android']
draft = false
+++


## File system
Android devices typically have a partitioned storage system, where the storage is divided into different partitions, each serving a specific purpose.

1. **/system:** This directory contains the Android operating system and system applications. It is read-only to ensure the integrity of the system. Modifying files in this directory could potentially harm the device.

	1. **/app:** prebundled apps from Google, as well as any vendor or carrier-installed apps
	2. **/bin:** various daemons, as well as shell commands contain` bootanimation,dalvikvm, adb,` etc.
	3. **/etc:** Miscellaneous configuration files
	4. **/framework:** contained in .jar files, with their executable dex files optimized alongside them in .odex .
	5. **/lib:** native ELF shared object (.so) files. This directory serves the same role as /lib in vanilla Linux.
	6. **/media:** Alarm, notification, ringtone and UI-effect audio files in .ogg format,and the system boot animation
	7. **/usr:** Support files, such as unicode mappings (icudt511.dat), key layout files for keyboards and devices, etc.
	8. **/vendor:** Vendor specific files
    
2. **/data:** This directory stores user data, settings, and application data. It includes subdirectories for individual apps, each identified by its package name. User-generated data, app preferences, and other user-specific information are stored here.
	1. **/anr:**  Used by EVNQTUBUF to record stack traces of non-responsive Android Apps. 
	2. **/app:** User-installed applications. Downloaded .apk files can be found here.
	3. **/data:** Data directories for installed applications Each application gets its own subdirectory, in reverse DNS format example `com.android.providers.calendar`
    
3. **/cache:** This directory is used for storing temporary files, such as cached data from applications. Clearing the cache can help free up storage space, but it won't delete essential app data.
    
4. **/mnt or /storage:** These directories are used for mounting external storage devices like SD cards or USB drives. The actual path may vary across devices and Android versions.
    
5. **/sdcard or /storage/emulated/0:** This is the default directory for the primary external storage on the device. It's often used to store user-accessible files such as photos, videos, and downloaded content. Note that on many modern devices, the "sdcard" directory might actually be part of the internal storage and not necessarily refer to an external SD card.
    
6. **/proc:** This virtual directory contains information about system processes. It provides a way to access real-time information about the system and running processes.
    
7. **/dev:** This directory contains device files representing hardware components and peripheral devices. It allows communication between the kernel and these devices.
    
8. **/sbin and /bin:** These directories contain essential system binaries and executable files needed for the device's basic functionality.
    
9. **/etc:** This directory contains configuration files used by the system and various applications.
    
10. **/lib:** This directory contains shared libraries needed by the system and apps. These libraries provide essential functions and services.
    
11. **/vendor:** This directory contains files related to the device's hardware, including proprietary drivers and firmware.

## boot process
1. **Bootloader (Boot ROM):**
    - When you power on the Android device, the Boot ROM (Read-Only Memory) or bootloader is the first piece of code that runs.
    - The bootloader is responsible for initializing hardware components, checking for the connected peripherals, and loading the next stage of the boot process.
2. **Bootloader Stage 2:**
    - The second stage of the bootloader is often responsible for loading the Android kernel into memory.
    - It may also initialize the root file system and prepare for the transition to the Linux kernel.
3. **Linux Kernel Initialization:**
    - Android is built on the Linux kernel. Once the bootloader hands control over, the Linux kernel is loaded into memory.
    - The kernel initializes the device's hardware, such as the CPU, memory, display, input devices, and various peripherals.
4. **Init Process:**
    - The init process is the first user-space process started by the Linux kernel. It has process ID 1 and is responsible for initializing the Android system.
    - The init process reads the `init.rc` file, which contains instructions for initializing various system properties and starting essential system services.
5. **Zygote and System Server:**
    - The Zygote process is a special system process that serves as a template for creating new application processes. It helps to speed up the launch of apps by preloading some common resources.
    - The system server is started by the init process and manages core system services like the package manager, window manager, and telephony services.
6. **Android Runtime (ART or Dalvik):**
    - The Android Runtime (ART) or Dalvik (in older versions) is responsible for running Android applications. ART is the default runtime as of Android 5.0 (Lollipop).
    - The runtime loads and executes the bytecode of Android apps.
7. **System Services and User Interface:**
    - As the system server starts, it initiates various system services that are essential for the functioning of the Android operating system.
    - The user interface components, including the home screen and launcher, are also started during this phase.
8. **App Launch:**
    - Finally, the Android device is ready for use, and the user can interact with the system. Apps can be launched, and the user interface becomes responsive.

## Android Debug bridge

CMD line tool it allows developers to communicate with and control Android devices over a USB connection or a TCP/IP network connection. ADB is a versatile tool that provides a wide range of functionalities for debugging, installing and uninstalling apps, copying files, and more.

**adb** is a client-server tool that includes three main components:

- **a client** that runs on your development machine and sends commands. You can execute it from a command line by running an **adb** command.
- **a daemon** (**adbd**) that runs as a background process on each device and executes commands on a device.
- **a server** that manages communication between the client and the daemon, it runs as a background process on your development machine.

#### How to connect
1. Install **adb** pkg in linux by `sudo apt install adb`



## The FastBoot Protocol






## Tool
- https://www.charlesproxy.com/ -> to proxy the  network request