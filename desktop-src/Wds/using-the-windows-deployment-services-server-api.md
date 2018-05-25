---
title: Using the Windows Deployment Services Server API
description: In environments where the standard Windows Deployment Services (WDS) solution cannot be used, the WDS server exposes an API that enables developers to write plug-ins, referred to as providers, to handle preboot execution environment (PXE) requests.
ms.assetid: '5e25654a-33c6-4c0f-acc3-e938d1f4a4e7'
keywords: ["Windows Deployment Services Windows Deployment Services , using the server API"]
---

# Using the Windows Deployment Services Server API

In environments where the standard Windows Deployment Services (WDS) solution cannot be used, the WDS server exposes an API that enables developers to write plug-ins, referred to as providers, to handle preboot execution environment (PXE) requests. Developers should adhere to the following guidelines when writing PXE providers for WDS.

## Install the WDS Role on the Server

-   Windows Deployment Services (WDS) is the revised version of Remote Installation Services (RIS), you will need the WDS server role to implement the WDS PXE server and providers.
-   WDS replaces RIS as the standard component starting with Windows Server 2008 and Windows Server 2003 with Service Pack 2 (SP2).
-   You must update the RIS server to WDS on Windows Server 2003 with Service Pack 1 (SP1). You can install the WDS server role with the [Windows Automated Installation Kit (WAIK)](http://go.microsoft.com/fwlink/p/?linkid=53552).

## Register Providers

-   Register the provider dynamic-link library (DLL) during its installation and insert the provider into the ordered list of registered providers.
    > [!Note]  
    > When you install a new or modified provider, you will need to restart WDS PXE service for the changes to take effect.

     

-   Use the [**PxeProviderRegister**](pxeproviderregister.md) function to register the provider and add it to the list. Use the [**PxeProviderUnRegister**](pxeproviderunregister.md) function to unregister a registered provider and remove it from the list.
-   Specify the sequence of the provider in the ordered list. The index of a provider in the list cannot be guaranteed because another provider may later be registered before it. To insert the provider in the list before or after another registered provider, first use the [**PxeProviderQueryIndex**](pxeproviderqueryindex.md) function to get the index of the registered provider and then register the new provider while specifying a larger or smaller index value.
-   The installation can store provider configuration information under the registry key returned when the provider is registered. The address of the registry key is received by the *phProviderKey* of [**PxeProviderRegister**](pxeproviderregister.md). The provider receives a handle to this same key as the *hProviderKey* parameter to its [*PxeProviderInitialize*](pxeproviderinitialize.md) callback. The provider should store the address of this key.
-   Always install the provider dynamic-link library (DLL) in the server's Program Files folder.

## Initialize

-   Include a DLL that exports the [*PxeProviderInitialize*](pxeproviderinitialize.md) callback function in the provider. Every provider requires a *PxeProviderInitialize* callback. When WDS loads a provider, it calls the provider's *PxeProviderInitialize* function and passes a handle to the same key that is used to store configuration information during provider registration.
-   When the [*PxeProviderInitialize*](pxeproviderinitialize.md) callback returns and is successful, the provider should be fully initialized and ready to process requests.
-   Register every callback in the provider during the processing of the [*PxeProviderInitialize*](pxeproviderinitialize.md) function. Callbacks should be registered with the [**PxeRegisterCallback**](pxeregistercallback.md) function.
-   Initialize all the provider's internal resources within the processing of its [*PxeProviderInitialize*](pxeproviderinitialize.md) function.

## Shutdown

-   Implement the [*PxeProviderShutdown*](pxeprovidershutdown.md) callback. Every provider is required to have a *PxeProviderShutdown*callback.
-   The [*PxeProviderShutdown*](pxeprovidershutdown.md) callback function should fully shut down the provider and release all its resources.
-   After the [*PxeProviderShutdown*](pxeprovidershutdown.md) callback returns, the *hProvider* handle passed to the [*PxeProviderInitialize*](pxeproviderinitialize.md) function is no longer valid and should not be used to call WDS.
-   Register the [*PxeProviderShutdown*](pxeprovidershutdown.md) callback by calling the [**PxeRegisterCallback**](pxeregistercallback.md) function with **PXE\_CALLBACK\_SHUTDOWN** during the processing of the [*PxeProviderInitialize*](pxeproviderinitialize.md) callback. Do not call the *PxeProviderShutdown* callback if the *PxeProviderInitialize* function fails.

## Handle Request Packet

Implement the [*PxeProviderRecvRequest*](pxeproviderrecvrequest.md) callback for the provider. Every provider is required to have a *PxeProviderRecvRequest* callback. When WDS receives a request, it calls the *PxeProviderRecvRequest* callback for the first provider in the registered provider list.

-   If the provider will process this request synchronously, the [*PxeProviderRecvRequest*](pxeproviderrecvrequest.md) function should return a value of **ERROR\_SUCCESS**. If and only if the provider will process this request asynchronously, the *PxeProviderRecvRequest* callback should return **ERROR\_IO\_PENDING** and call the [**PxeAsyncRecvDone**](pxeasyncrecvdone.md) function when the request has been processed.
-   The [*PxeProviderRecvRequest*](pxeproviderrecvrequest.md) callback and the [**PxeAsyncRecvDone**](pxeasyncrecvdone.md) function returns the address of a **PXE\_BOOT\_ACTION** enum that describes the action taken by the provider to handle the request.

    There are four ways for a provider to handle a request:

    -   The provider replies to the client with a standard DHCP response packet that contains a path to the Network Boot Program. Returning the **PXE\_BA\_NBP** value for the enum means that the provider has successfully processed the request packet and completed the request by sending a response packet by calling the [**PxePacketAllocate**](pxepacketallocate.md) and [**PxeSendReply**](pxesendreply.md) functions.
    -   The provider replies to the client with a custom response packet that does not conform to DHCP. Returning the **PXE\_BA\_CUSTOM** value for the enum means that the provider has successfully processed the request packet and completed the request by sending a custom response packet by calling the [**PxePacketAllocate**](pxepacketallocate.md) and [**PxeSendReply**](pxesendreply.md) functions.
    -   The provider determines the request should be ignored. Returning the **PXE\_BA\_IGNORE** value for the enum means the provider has released all resources associated with the request and the request is not passed to the next provider in the list of registered providers. Providers may use this option if they detect that a request packet is invalid.
    -   The provider declines to service the request. Returning the **PXE\_BA\_REJECT** value for the enum instructs the system to pass the request to the next provider in the list of registered providers. If this were the last provider in the list, this releases all resources associated with the request and the request is ignored.
    -   Register the [*PxeProviderRecvRequest*](pxeproviderrecvrequest.md) callback by calling the [**PxeRegisterCallback**](pxeregistercallback.md) function with **PXE\_CALLBACK\_RECV\_REQUEST** during the processing of the [*PxeProviderInitialize*](pxeproviderinitialize.md) callback.

## Generate Response Packet

-   Use the API to write providers to handle DHCP request and generate response packets.
-   The [**PxeProviderSetAttribute**](pxeprovidersetattribute.md) function specifies the attributes used by the provider to filter packets. The attributes of the provider can be specified so that the provider see all packets, the provider sees only DHCP packets, or the provider sees only DHCP packets that specify the DHCP Vendor Class Identifier option (60) as "PXEClient".
-   The [**PxeDhcpIsValid**](pxedhcpisvalid.md) function checks that a packet is a valid DHCP packet. Providers can use the **PxeDhcpIsValid** function to check whether a packet from the client is a DHCP packet when the filter set with the [**PxeProviderSetAttribute**](pxeprovidersetattribute.md) function is set to receive all packets to determine if a specified packet is a valid DHCP packet.
-   The [**PxeDhcpInitialize**](pxedhcpinitialize.md) function initializes a response packet as a DHCP reply packet that is based on the information in a packet received from the client. The [*PxeProviderInitialize*](pxeproviderinitialize.md) function takes the address of a valid DHCP packet received from the client in the [*PxeProviderRecvRequest*](pxeproviderrecvrequest.md) callback. The **PxeDhcpInitialize** function takes a pointer to a reply packet allocated with the [**PxePacketAllocate**](pxepacketallocate.md) function.
-   The [**PxeDhcpGetOptionValue**](pxedhcpgetoptionvalue.md) function retrieves an option value from a DHCP packet. The [**PxeDhcpGetVendorOptionValue**](pxedhcpgetvendoroptionvalue.md) function retrieves an option value from the Vendor Specific Information field (43) of a DHCP packet.
-   The provider can then populate the reply packet with information and use the [**PxeSendReply**](pxesendreply.md) function to send the reply packet to the client. The [**PxeDhcpAppendOption**](pxedhcpappendoption.md) function appends a DHCP option to the reply packet.
-   A provider that replies to client requests by sending a packet must allocate the response packet using the [**PxePacketAllocate**](pxepacketallocate.md) function. The provider can then populate the reply packet with information and use the [**PxeSendReply**](pxesendreply.md) function to send the reply packet to the client.
-   When the allocated memory is no longer needed, the provider should release it using the [**PxePacketFree**](pxepacketfree.md) function.

## Enumerate Registered Providers

-   Use the API to write providers that enumerate and check other registered providers in the list.
-   The [**PxeGetServerInfo**](pxegetserverinfo.md) function returns information about the PXE server. The **PxeGetServerInfo** function returns a **BOOL** that indicates whether tracing is enabled for the server. **TRUE** indicates that tracing is enabled.
-   The [**PxeProviderEnumFirst**](pxeproviderenumfirst.md) function starts an enumerationof providers in the registered provider list. The **PxeProviderEnumFirst** function starts the enumeration and returns the address of the handle that should be used when calling the [**PxeProviderEnumNext**](pxeproviderenumnext.md) function. The **PxeProviderEnumNext** function returns a [**PXE\_PROVIDER**](pxe-provider.md) structure containing the information about the provider. The [**PxeProviderFreeInfo**](pxeproviderfreeinfo.md) function frees the memory that has been allocated for the **PXE\_PROVIDER** structure by the **PxeProviderEnumNext** function. The [**PxeProviderEnumClose**](pxeproviderenumclose.md) function closes the enumeration of providers in the registered providers list.

## Handle Service Control Codes

-   When a service control message is received, WDS calls the [*PxeProviderServiceControl*](pxeproviderservicecontrol.md) callback.
-   The provider can define a [*PxeProviderServiceControl*](pxeproviderservicecontrol.md) callback function to handle service control messages.
-   Register the [*PxeProviderServiceControl*](pxeproviderservicecontrol.md) callback by calling the [**PxeRegisterCallback**](pxeregistercallback.md) function with **PXE\_CALLBACK\_SERVICE\_CONTROL** during the processing of the [*PxeProviderInitialize*](pxeproviderinitialize.md) callback.

## Add Trace Entries to PXE Log

-   The [**PxeTrace**](pxetrace.md) function adds a trace entry to the PXE log. WDSPXE provides tracing to help administrators in troubleshooting. Providers can log trace entries of different severity levels. The administrators can configure WDSPXE to only log entries for certain severity levels.

## Related topics

<dl> <dt>

[About the Windows Deployment Services API](about-the-windows-deployment-services-api.md)
</dt> <dt>

[Using the Windows Deployment Services Client API](using-the-windows-deployment-services-client-api.md)
</dt> </dl>

 

 



