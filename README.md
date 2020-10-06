*Please be aware that this application / sample is provided as-is for demonstration purposes without any guarantee of support*
=========================================================

# EMDK-DeviceIdentifiers-Sample

How to access device identifiers such as serial number and IMEI on Zebra devices running Android 10

Android 10 limited access to device identifiers for all apps running on the platform regardless of their target API level.  As explained in the docs for [Android 10 privacy changes](https://developer.android.com/about/versions/10/privacy/changes) this includes the serial number, IMEI and some other identifiable information.

**Zebra mobile computers running Android 10 are able to access both the serial number and IMEI** however applications need to be **explicitly granted the ability** to do so and use a proprietary API.

To access the serial number and IMEI file on Zebra Android devices running Android 10 or higher, first declare a new permission in your AndroidManifest.xml

```xml
<uses-permission android:name="com.zebra.provider.READ"/>
```

Then use the [MX access manager](https://techdocs.zebra.com/mx/accessmgr/) to allow your application to call the service identifiers associated with the serial number and IMEI

The MX access manager settings to enable this are as follows:
- Service Access Action: "AllowCaller" (or 'Allow Caller to Call Service')
- Service Identifier: For the serial number use content://oem_info/oem.zebra.secure/build_serial.  For the IMEI use content://oem_info/wan/imei.  If you want to allow your app access to both, you will need to declare two different instances of the AccessManager.
- Caller Package Name: Your package name, in the case of this sample it is com.zebra.emdk_deviceidentifiers_sample.
- Caller Signature: The signing certificate of your application.  For more information on generating this see https://github.com/darryncampbell/MX-SignatureAuthentication-Demo.

You can apply the MX access manager settings in one of three ways:
1. Via StageNow
2. Via your EMM
3. Via your application, using the EMDK Profile Manager.

For example, StageNow will look as follows to enable access to the serial number:

![StageNow](https://github.com/darryncampbell/EMDK-DeviceIdentifiers-Sample/raw/master/screenshots/stagenow.png)

You can then run this sample app and should see something like the below:

![Working](https://github.com/darryncampbell/EMDK-DeviceIdentifiers-Sample/raw/master/screenshots/working.jpg)

Or, on a device that does not have WAN capabilities, i.e. no SIM card or data connection: 

![Non-WAN](https://github.com/darryncampbell/EMDK-DeviceIdentifiers-Sample/raw/master/screenshots/non_wan.jpg)

## Handling errors:

If you failed to correctly allow your application access to oem_info service, you will see an error stating so against each property you did not assign access to, as shown below:

![no_service_access](https://github.com/darryncampbell/EMDK-DeviceIdentifiers-Sample/raw/master/screenshots/no_service_access.jpg)

Assign access to your device and re-run the application.


