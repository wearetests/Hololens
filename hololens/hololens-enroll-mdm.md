---
title: Enroll HoloLens in MDM
description: Learn how to enroll HoloLens in mobile device management (MDM) for easier management of multiple devices.
ms.prod: hololens
ms.sitesec: library
ms.assetid: 2a9b3fca-8370-44ec-8b57-fb98b8d317b0
ms.topic: article
ms.localizationpriority:
ms.date: 5/25/2021
ms.reviewer: 
appliesto:
- HoloLens (1st gen)
- HoloLens 2
---

# Enroll HoloLens in MDM

You can manage multiple Microsoft HoloLens devices simultaneously using solutions like [Microsoft Intune](/intune/windows-holographic-for-business). You'll be able to manage settings, select apps to install and set security configurations tailored to your organization's need. See [Manage devices running Windows Holographic with Microsoft Intune](/intune/windows-holographic-for-business), the [configuration service providers (CSPs) that are supported in Windows Holographic](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference#hololens), and the [policies supported by Windows Holographic for Business](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#hololenspolicies).

> [!NOTE]
> Mobile device management (MDM), including the VPN, Bitlocker, and kiosk mode features, is only available when you [upgrade to Windows Holographic for Business](hololens1-upgrade-enterprise.md).

## Requirements

 Your organization will need to have Mobile Device Management (MDM) set up in order to manage HoloLens devices. Your MDM provider can be Microsoft Intune or a 3rd party provider that uses Microsoft MDM APIs.

## Enrollment per scenario

Depending on what stage you are in your deployment we have the following recommendations:

- For multi-user shared devices being deployed in production it is suggested you use [Autopilot](hololens2-autopilot.md).
- For multi-user shared devices that are being initially part of a pilot program, [Azure AD Join during OOBE](hololens-enroll-mdm.md#auto-enrollment-in-mdm) should be sufficient.
- For a proof of concept joining a device via the Settings menu may suit your needs if you don't need multiple users per device.

## Different ways to enroll

Depending on the type of [identity](hololens-identity.md) chosen either during OOBE or post sign-in, there are different methods of enrollment.

> [!NOTE]
> If your tenant is in a GCC High enviorment you will be unable to select "sign in from another device". You'll need to manually enter your user credentials.

### For Multi-User Shared Devices

- If Identity is Azure AD and device has been pre-registered with Intune MDM server with specific configuration profile assigned to it, then Azure AD-Join and [automatic MDM enrollment](hololens-enroll-mdm.md#auto-enrollment-in-mdm) will occur during OOBE.
  - Also called [Autopilot flow](hololens2-autopilot.md) Available in [19041.1103+ builds](hololens-release-notes-2004.md#windows-holographic-version-2004).
- If Identity is Azure AD, the during OOBE device can enroll.
  - For Azure AD, [automatic MDM enrollment](hololens-enroll-mdm.md#auto-enrollment-in-mdm) only occurs if Azure AD has been configured with enrollment URLs.

### For Single User Devices

- If Identity is Azure AD, then either during OOBE or **Settings App** -> **Access Work or School** -> **Connect** button.
  - For Azure AD, [automatic MDM enrollment](hololens-enroll-mdm.md#auto-enrollment-in-mdm) only occurs if Azure AD has been configured with enrollment URLs.
- If Identity is MSA, then using **Settings App** -> **Access Work or School** -> **Connect** button.
  - Also called Add Work Account (AWA) flow.
- If Identity is Local User, then using **Settings App** -> **Access Work or School** -> **Enroll only in device management** link.
  - Also called pure MDM enrollment flow.

Once the device is enrolled with your MDM server, the Settings app will now reflect that the device is enrolled in device management.

## Auto-enrollment in MDM

If your organization has an [Azure Premium subscription](https://azure.microsoft.com/overview/), is using Azure Active Directory (Azure AD) and an MDM solution that accepts an Azure AD token for authentication (currently, only supported in Microsoft Intune and AirWatch), your IT admin can configure Azure AD to automatically allow MDM enrollment after the user signs in with their Azure AD account. [Learn how to configure Azure AD enrollment](/mem/intune/enrollment/windows-enroll#enable-windows-10-automatic-enrollment) and [Azure active directory integration with MDM](/windows/client-management/mdm/azure-active-directory-integration-with-mdm) for detailed background information.

When auto-enrollment is enabled, no extra manual enrollment is needed. When the user signs in with an Azure AD account, the device is enrolled in MDM after completing the first-run experience.

When a device is Azure AD Joined it may affect who considered the [device owner](security-adminless-os.md#device-owner).

## Unenroll HoloLens from Intune

Depending on the enrollment method, unenrolling your device may not be available.

If your device was enrolled with an Azure AD account or Autopilot, it can’t be unenrolled from Intune. If you wish to unjoin HoloLens from Azure AD or rejoin it to a different to Azure AD tenant, you must [reset/reflash](hololens-recovery.md#restart-the-device) the device.

If your device was enrolled from an MSA account that added a work account or from a Local account that enrolled only in device management, then you may unenroll the device. Open the Start menu and then select **Settings App** -> **Access Work or School** -> *YourAccount* -> **Disconnect** button.

## Enrollment troubleshooting

### Ensure device is successfully connected to Internet before attempting enrollment post OOBE

Once user has signed-in, ensure internet connection by browsing to any internet facing website on device.

### Ensure that Azure Active Directory (Azure AD) join is not disabled in your Azure AD tenant

Refer to [Configure your device settings](/azure/active-directory/devices/azureadjoin-plan#configure-your-device-settings) for information about the available options in Azure portal.

### Ensure valid license is assigned to the user

Refer to [Troubleshoot Windows device enrollment problems in Microsoft Intune](/troubleshoot/mem/intune/troubleshoot-windows-enrollment-errors) specifically following sections, that is, [Check device type restrictions](/troubleshoot/mem/intune/troubleshoot-windows-enrollment-errors#check-device-type-restrictions) and [Assign a valid license to the user.](/troubleshoot/mem/intune/troubleshoot-windows-enrollment-errors#assign-a-valid-license-to-the-user)

### Ensure that MDM enrollment isn't blocked for Windows devices

In order for enrollment to succeed you'll need to make sure that your HoloLens devices can enroll. Since HoloLens is considered a Windows device, there will need to be no enrollment restrictions that could block your deployment. [Review this list of restrictions](/mem/intune/enrollment/enrollment-restrictions-set) and ensure you'll be able to enroll your devices.
Currently, the enrollment restriction is not applyed to Hololens devices by default.
If your tenant need to apply enrollment restriction for Hololens devices, you will need to contact Microsoft support to apply the enrollment restriction.
