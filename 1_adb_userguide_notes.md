ADB Guide: Basics to Intermediate Usage
=======================================

 Intro: Android Debug Bridge (ADB)
--------------------------------------
- CLI to communicate with Android devices.
- Works over USB/Wi-Fi
- supports multiple connected devices.

Getting Started
---------------
   .. code-block:: bash

      adb devices                                    # list of devices.
      --- op=>
      List of devices attached
      emulator-5554	device                       // usb device.
      192.168.31.91:5555	device                 // wifi device.

      adb kill-server                                # KIll ADB server if needed
      adb start-server                               # and Restart ADB server if needed

--------------------------------------

Enable ADB Over Wi-Fi
---------------------

1. Connect phone via USB first.
2. Enable TCP mode on port 5555:

   .. code-block:: bash

      |------------------------------------------------------------------------------------|
      | adb devices. => show connected devices.                                            |
      | adb tcpip 5555 => to turn on adb over wifi in your pc                              |
      | asb shell ip route => will pull the phones ip address and set it to use with WIFI. |
      | adb shell ip route =>  to set the pulled ip to connecting over wifi.               |
      | adb connect <ip_address>:5555                                                      | 
      |____________________________________________________________________________________|


---------------------------
X.NMAP  = finding with Nmap.
---------------------------
    1.check subnet:
        .. code-block:: bash

          ip route  => to check your supbent.
          output:
          default via 192.168.1.1 dev wlan0
          192.168.1.0/24 dev wlan0 proto kernel scope link src 192.168.1.23
          
        so : subnet => 192.168.1.0/24

    2.find ip of the required phone.
        .. code-block:: bash

            | nmap -sn <subnet> |
            nmap -sn 192.168.1.0/24                            # scan multyple times for better resualt. (only active devices will show.)
            sudo nmap -Pn -sP 192.168.31.0/24                  # agresive (treate every devise as active)

            Nmap scan report for 192.168.1.101
            Host is up.
            MAC Address: AA:BB:CC:DD:EE:FF (Samsung Electronics)
    
        Here from HOSTNAME and MANIFRACTURE we will find IP.

X.NMAP  END.
---------------------------



Connecting Specific Device.
---------------------------

   .. code-block:: bash
     
      adb connect 192.168.1.101:5555
      adb -s <serial|ip:port> <command>  // alias like "-s" seting.
      adb -s 192.168.1.101:5555 shell  // this will give -s name to the divice

       USE:->
      adb -s emulator-5554 install myapp.apk
      adb -s 192.168.1.101:5555 pull /sdcard/Music/A ~/Music/A

--------------------------------------


File Transfer
-------------

   .. code-block:: bash
      adb pull /sdcard/Download/file.txt ~/Downloads/           # pull 
      adb push ~/myfile.txt /sdcard/Documents/                  # push
      adb pull "/sdcard/Music/A" "$HOME/Phone Music/A"          # Use quotes if path has spaces.

      adb shell getprop                                         # Basic system info:
      adb shell pm list packages                                # Installed packages
      adb shell getprop ro.product.model                        # Check device model and Android version
      adb shell getprop ro.build.version.release                # Check device model and Android version
      adb shell dumpsys battery                                 # Battery status
      adb shell df -h                                           # Storage info

Shell Access
------------
   .. code-block:: bash

      adb shell
      ---------
      all above commands without ADB.
      ---------
      exit

--------------------------------------

Tips
----

- Always check `adb devices` before running commands.
- Use `-s` to avoid accidentally running commands on the wrong device.
- To avoid changing IPs, use DHCP reservation in router (if possible).
- Use scripts for automation and repeated file transfers.

