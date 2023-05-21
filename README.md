# JustTrustMe

This guide explains how to add your own certificate as a system certificate using the JustTrustMe method. Please note
that this method requires root access to the device. Proceed with caution, as modifying system files can have unintended
consequences.

## Prerequisites

- Root access to the Android device.
- Basic knowledge of using command line tools.

## Steps

1. Obtain the Certificate
    - Convert your certificate to the `.pem` format if it is not already in that format.
2. Determine the Hash of the Certificate
    - Open a terminal or command prompt on your computer.
    - Execute the following command to get the hash of the certificate:
      ````shell
      openssl x509 -inform PEM -subject_hash_old -in CERTIFICATE.pem | head -1
      ````
    - Make a note of the hash value, as it will be used in later steps.
3. Clean Up the Certificate File
    - If the certificate file has Windows line endings (\r\n), remove them using the following command:
      ````shell
      sed -i 's/\r//g' CERTIFICATE.0
      ````
4. Transfer the Certificate to the Device
    - Connect your Android device to the computer via USB.
    - Ensure USB debugging is enabled on your device.
    - Execute the following command to push the certificate to the device:
      ````shell
      adb push CERTIFICATE.0 /storage/emulated/0/
      ````
5. Copy the Certificate as Root
    - Execute the following commands to copy the certificate to the system directory:
      ````shell
      adb shell
      su
      mount -o rw,remount /
      cp /storage/emulated/0/CERTIFICATE.0 /system/etc/security/cacerts/
      chmod 644 /system/etc/security/cacerts/CERTIFICATE.0
      reboot
      ````
6. Install the Certificate
    - After the device reboots, install the certificate normally using the device's security settings.
    - The certificate will now be added as a system certificate rather than a user certificate.

**Note:** Modifying system files can have unintended consequences and may lead to system instability or security
vulnerabilities. Proceed with caution and take appropriate backups before making any changes.