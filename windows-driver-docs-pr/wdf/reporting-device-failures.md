---
title: Reporting Device Failures
description: Reporting Device Failures
ms.assetid: ca536547-d51a-4450-8a83-19aac67aab92
keywords:
- PnP WDK KMDF , device failures
- Plug and Play WDK KMDF , device failures
- device failures WDK KMDF
- failed devices WDK KMDF
- WdfDeviceFailedAttemptRestart
- WdfDeviceFailedNoRestart
- reporting device failures WDK KMDF
ms.date: 04/20/2017
ms.localizationpriority: medium
---

# Reporting Device Failures


There are two ways to report device failures:

-   When returning from [device object callback functions](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/#device-callbacks), the driver can supply a return value for which [NT\_SUCCESS](https://docs.microsoft.com/windows-hardware/drivers/kernel/using-ntstatus-values)(*status*) equals **FALSE**.

-   The driver can call [**WdfDeviceSetFailed**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nf-wdfdevice-wdfdevicesetfailed).

For both methods, the framework effectively removes the device. If the device's drivers are not supporting other devices on the system, the I/O manager unloads the drivers.

If a driver's device object callback function returns a value for which NT\_SUCCESS(*status*) equals **FALSE**, the framework notifies the PnP manager, which then attempts to restart the device by requesting the bus driver to reenumerate its devices. Your driver will be reloaded, if it was unloaded.

If your driver calls [**WdfDeviceSetFailed**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nf-wdfdevice-wdfdevicesetfailed), it supplies an input argument that determines whether the device will be restarted. The argument values are **WdfDeviceFailedAttemptRestart** and **WdfDeviceFailedNoRestart**.

**UMDF** A UMDF driver must set this value to **WdfDeviceFailedNoRestart**.

For more information about these argument values, see [**WDF\_DEVICE\_FAILED\_ACTION**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/ne-wdfdevice-_wdf_device_failed_action).
Before a driver's device object callback function returns with a value for which NT\_SUCCESS(*status*) equals **FALSE**, the callback function can prevent restarts by calling [**WdfDeviceSetFailed**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nf-wdfdevice-wdfdevicesetfailed) with an input argument of **WdfDeviceFailedNoRestart**. Otherwise, these callback functions do not have to call **WdfDeviceSetFailed**.

If, within a short period of time, several consecutive restart attempts fail (because the restarted driver again reports an error), the framework stops trying to restart the device.

If a bus driver's [*EvtDeviceD0Entry*](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nc-wdfdevice-evt_wdf_device_d0_entry) function returns a value for which NT\_SUCCESS(*status*) equals **FALSE**, the framework might still call the *EvtDeviceD0Entry* functions of drivers associated with the bus driver's child devices.

 

 





