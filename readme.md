# Android Network Tool v1.1 ![image](https://github.com/stealthcopter/AndroidNetworkTools/blob/master/app/src/main/res/mipmap-xhdpi/ic_launcher.png)

[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-AndroidNetworkTools-green.svg?style=true)](https://github.com/amir067)
[![CircleCI](https://circleci.com/gh/stealthcopter/AndroidNetworkTools.svg?style=svg)](https://github.com/amir067)

Disappointed by the lack of good network apis in android / java I developed a collection of handy networking tools for everyday android development.

* Port Scanning
* Subnet Device Finder (discovers devices on local network)
* Ping
* Wake-On-Lan
* & More :)

## <a href="https://github.com/amir067/CCN-Project/raw/main/CCN-App-27dec-final.apk"><img src="https://github.com/amir067/CCN-Project/blob/main/Preview/DBTN.png" height="80" width="300" ></a>


## Preview
![image](https://github.com/amir067/CCN-Project/blob/main/Preview/Preview%20(2).jpeg)

## General info

The javadoc should provide all information needed to understand the methods, but if not feel free to add a issue in github and I'll address any questions! :)


## Usage

### Add as dependency
This library is not yet released in Maven Central, until then you can add as a library module or use JitPack.io

add remote maven url

```groovy

    repositories {
        maven {
            url "https://jitpack.io"
        }
    }
```
    
then add a library dependency. 

```groovy
    dependencies {
        compile 'com.github.stealthcopter:AndroidNetworkTools:0.4.5.3'
    }
```

### Add permission
Requires internet permission (obviously...)
```xml
  <uses-permission android:name="android.permission.INTERNET" />
```

### Port Scanning

A simple java based TCP / UDP port scanner, fast and easy to use. By default it will try and guess the best timeout and threads to use while scanning depending on if the address looks like localhost, local network or remote. You can override these yourself by calling setNoThreads() and setTimeoutMillis()

```java
    // Synchronously
    ArrayList<Integer> openPorts = PortScan.onAddress("192.168.0.1").setMethodUDP().setPort(21).doScan();

    // Asynchronously
    PortScan.onAddress("192.168.0.1").setTimeOutMillis(1000).setPortsAll().setMethodTCP().doScan(new PortScan.PortListener() {
      @Override
      public void onResult(int portNo, boolean open) {
        if (open) // Stub: found open port
      }

      @Override
      public void onFinished(ArrayList<Integer> openPorts) {
        // Stub: Finished scanning
      }
    });

```

### Subnet Devices

Finds devices that respond to ping that are on the same subnet as the current device. You can set the timeout for the ping with setTimeOutMillis() \[default 2500\] and the number of threads with setNoThreads() \[default 255\]

```
    // Asynchronously
    SubnetDevices.fromLocalAddress().findDevices(new SubnetDevices.OnSubnetDeviceFound() {
        @Override
        public void onDeviceFound(Device device) {
            // Stub: Found subnet device
        }

        @Override
        public void onFinished(ArrayList<Device> devicesFound) {
            // Stub: Finished scanning
        }
    });

```

### Ping

Uses the native ping binary if available on the device (some devices come without it) and falls back to a TCP request on port 7 (echo request) if not.

```java
     // Synchronously 
     PingResult pingResult = Ping.onAddress("192.168.0.1").setTimeOutMillis(1000).doPing();
     
     // Asynchronously
     Ping.onAddress("192.168.0.1").setTimeOutMillis(1000).setTimes(5).doPing(new Ping.PingListener() {
      @Override
      public void onResult(PingResult pingResult) {
        ...
      }
    });
```

Note: If we do have to fall back to using TCP port 7 (the java way) to detect devices we will find significantly less than with the native ping binary. If this is an issue you could consider adding a ping binary to your application or device so that it is always available.


Note: If you want a more advanced portscanner you should consider compiling nmap into your project and using that instead.

### Wake-On-Lan

Sends a Wake-on-Lan packet to the IP / MAC address

```java
      String ipAddress = "192.168.0.1";
      String macAddress = "01:23:45:67:89:ab";
      WakeOnLan.sendWakeOnLan(ipAddress, macAddress);
```

### Misc

Other useful methods:

```java
      // Get a MAC Address from an IP address in the ARP Cache
      String ipAddress = "192.168.0.1";
      String macAddress = ARPInfo.getMacFromArpCache(ipAddress);
```

## Building

It's a standard gradle project.

# Contributing

I welcome pull requests, issues and feedback.

- Fork it
- Create your feature branch (git checkout -b my-new-feature)
- Commit your changes (git commit -am 'Added some feature')
- Push to the branch (git push origin my-new-feature)
- Create new Pull Request
