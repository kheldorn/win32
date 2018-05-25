﻿---
Description: 'Closes the OC manager.'
ms.assetid: 'feba9954-03b2-4b57-b7ba-933e171751ff'
title: OcTerminate function
---

# OcTerminate function

Closes the OC manager.

## Syntax


```C++
VOID OcTerminate(
  _Inout_ PVOID *OcManagerContext
);
```



## Parameters

<dl> <dt>

*OcManagerContext* \[in, out\]
</dt> <dd>

On input, contains the OC manager context pointer returned by [**OcInitialize**](ocinitialize.md). On output, receives **NULL**.

</dd> </dl>

## Return value

This function does not return a value.

## Remarks

This function has no associated import library or header file; you must call it using the [**LoadLibrary**](base.loadlibrary) and [**GetProcAddress**](base.getprocaddress) functions.

## Requirements



|                |                                                                                         |
|----------------|-----------------------------------------------------------------------------------------|
| DLL<br/> | <dl> <dt>OcManage.dll</dt> </dl> |



## See also

<dl> <dt>

[**OcInitialize**](ocinitialize.md)
</dt> </dl>

 

 



