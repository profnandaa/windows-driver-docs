---
title: Introduction to NDIS Protocol Drivers
description: Introduction to NDIS Protocol Drivers
ms.assetid: 398a1cf1-9bf8-45a5-9b6d-65467d061e99
ms.date: 04/20/2017
ms.localizationpriority: medium
---

# Introduction to NDIS Protocol Drivers


An NDIS protocol driver exports a set of *ProtocolXxx* functions at its lower edge. Such a protocol driver communicates with NDIS to send and receive network data. The protocol driver binds to an underlying miniport driver or intermediate driver that exports a *MiniportXxx* interface at its upper edge.

**Note**  The miniport driver upper edge of an intermediate driver (virtual miniport) does not manage physical devices. Underlying miniport drivers manage physical devices.

 

Protocol drivers always use NDIS-provided functions to communicate with underlying NDIS drivers to send and receive network data. For example, a protocol driver that has a connectionless lower-edge (which communicates with underlying drivers for connectionless media, such as Ethernet) must call [**NdisSendNetBufferLists**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ndis/nf-ndis-ndissendnetbufferlists) to send network data to an underlying NDIS driver. The protocol driver can call [**NdisOidRequest**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ndis/nf-ndis-ndisoidrequest) to query or set OIDs that underlying connectionless drivers support. A protocol driver that has a connection-oriented lower edge (which communicates with underlying drivers for connection-oriented media, such as ISDN) must call [**NdisCoSendNetBufferLists**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ndis/nf-ndis-ndiscosendnetbufferlists) to send network data to a lower-level NDIS driver. It can also call [**NdisCoOidRequest**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ndis/nf-ndis-ndiscooidrequest) to query or set OIDs that are supported by underlying connection-oriented drivers.

NDIS also provides a set of **Ndis*Xxx*** functions that hide the details of the underlying operating system. For example, a protocol driver can call [**NdisInitializeEvent**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ndis/nf-ndis-ndisinitializeevent) to create an event for synchronization purposes and [**NdisInitializeListHead**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ndis/nf-ndis-ndisinitializelisthead) to create a linked list. Protocol drivers that use the NDIS versions of such functions are more portable across Microsoft operating systems. However, protocol drivers can also call kernel-mode support routines, such as [**IoCreateDevice**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdm/nf-wdm-iocreatedevice). For more information, see [Summary of Kernel-Mode Support Routines](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/index).

Developers of protocol drivers should use the same [programming considerations](network-driver-programming-considerations.md) that are applied to other NDIS drivers.

 

 





