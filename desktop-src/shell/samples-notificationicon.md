---
Description: 'Demonstrates how to use the Shell\_NotifyIcon and Shell\_NotifyIconGetRect APIs to display a notification icon.'
title: NotificationIcon Sample
---

# NotificationIcon Sample

Demonstrates how to use the [**Shell\_NotifyIcon**](shell-notifyicon.md) and [**Shell\_NotifyIconGetRect**](shell-notifyicongetrect.md) APIs to display a notification icon.

This topic contains the following sections.

-   [Description](#description)
-   [Requirements](#requirements)
-   [Downloading the Sample](#downloading-the-sample)
-   [Building the Sample](#building-the-sample)
-   [Running the Sample](#running-the-sample)

## Description

In addition to the use of [**Shell\_NotifyIcon**](shell-notifyicon.md) and [**Shell\_NotifyIconGetRect**](shell-notifyicongetrect.md) to display a notification icon, this sample also demonstrates how to display a rich flyout window, context menu, and balloon notification.

> [!Note]  
> [**Shell\_NotifyIconGetRect**](shell-notifyicongetrect.md) is only available on Windows 7 and later versions.

 

## Requirements



| Product                                | Minimum Product Version |
|----------------------------------------|-------------------------|
| Windows                                | Windows 7               |
| Windows Software Development Kit (SDK) | 7.0                     |



 

## Downloading the Sample

This sample is available in the following locations.



| Location      | Path URL                                                                                             |
|---------------|------------------------------------------------------------------------------------------------------|
| Code Gallery  | [Windows Shell Integration Samples on Code Gallery](http://go.microsoft.com/fwlink/p/?linkid=155659) |
| Windows 7 SDK | [Download Windows 7 SDK](http://go.microsoft.com/fwlink/p/?linkid=129787)                            |



 

In the case of the Windows SDK, once you have downloaded and installed it, you will find the samples in the installed directory. For example, use of the default installation path for the Windows 7 software development kit (SDK) results in the samples being placed under `C:\Program Files\Microsoft SDKs\Windows\v7.0\Samples\`.

## Building the Sample

To build the sample from the command prompt:

1.  Open the command prompt window and navigate to the **NotificationIcon** project directory.
2.  Enter `msbuild NotificationIcon.sln`.

To build the sample using Microsoft Visual Studio (preferred):

1.  Open Windows Explorer and navigate to the **NotificationIcon** project directory.
2.  Double-click the icon for the NotificationIcon.sln file to open the project in Visual Studio.
3.  From the **Build** menu, select **Build Solution**.

## Running the Sample

1.  Navigate to the directory that contains the new executable, using the command prompt or Windows Explorer.
2.  At the command line, enter `NotificationIcon.exe`. Alternatively, from Windows Explorer double-click the icon for NotificationIcon.exe.

> [!Note]  
> Notification icons specified with a GUID are protected against spoofing by validating that only a single application registers them. This registration is performed the first time you call Shell\_NotifyIcon(NIM\_ADD, ...) and the full path name of the calling application is stored. If you later move your binary file to a different location, the system will not allow the icon to be added again. Please see [**Shell\_NotifyIcon**](shell-notifyicon.md) for more information.

 

 

 


