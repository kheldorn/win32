---
Description: Changes the security permissions for the logical paging file specified in the object path.
ms.assetid: 3abdfa1d-49d8-4676-b7a5-3be528938ec4
ms.tgt_platform: multiple
title: ChangeSecurityPermissions method of the Win32_PageFile class
ms.topic: reference
ms.date: 05/31/2018
topic_type: 
- APIRef
- kbSyntax
api_name: 
- Win32_PageFile.ChangeSecurityPermissions
api_type: 
- COM
api_location: 
- CIMWin32.dll
---

# ChangeSecurityPermissions method of the Win32\_PageFile class

The **ChangeSecurityPermissions** [WMI class](/windows/desktop/WmiSdk/retrieving-a-class) method changes the security permissions for the logical paging file specified in the object path. If the logical file is a directory, then **ChangeSecurityPermissions** is recursive, and changes the security permissions of all of the files and subdirectories that the directory contains.

This topic uses Managed Object Format (MOF) syntax. For more information about using this method, see [Calling a Method](/windows/desktop/WmiSdk/calling-a-method).

## Syntax


```mof
uint32 ChangeSecurityPermissions(
  [in] Win32_SecurityDescriptor SecurityDescriptor,
  [in] uint32                   Option
);
```



## Parameters

<dl> <dt>

*SecurityDescriptor* \[in\]
</dt> <dd>

Expression that resolves to an instance of [**Win32\_SecurityDescriptor**](/previous-versions/windows/desktop/secrcw32prov/win32-securitydescriptor). This descriptor contains new security permissions for the instance of [**Win32\_PageFile**](win32-pagefile.md).

</dd> <dt>

*Option* \[in\]
</dt> <dd>

Security privilege to be modified. For example, to change the owner and discretionary access control list (DACL) security, use:

`Option = 1 + 4`

-or-

`Option = CHANGE_OWNER_SECURITY_INFORMATION | CHANGE_DACL_SECURITY_INFORMATION`

<dt>

<span id="CHANGE_OWNER_SECURITY_INFORMATION"></span><span id="change_owner_security_information"></span>

<span id="CHANGE_OWNER_SECURITY_INFORMATION"></span><span id="change_owner_security_information"></span>**CHANGE\_OWNER\_SECURITY\_INFORMATION** (1)


</dt> <dd>

Change the owner of the logical file.

</dd> <dt>

<span id="CHANGE_GROUP_SECURITY_INFORMATION"></span><span id="change_group_security_information"></span>

<span id="CHANGE_GROUP_SECURITY_INFORMATION"></span><span id="change_group_security_information"></span>**CHANGE\_GROUP\_SECURITY\_INFORMATION** (2)


</dt> <dd>

Change the group of the logical file.

</dd> <dt>

<span id="CHANGE_DACL_SECURITY_INFORMATION"></span><span id="change_dacl_security_information"></span>

<span id="CHANGE_DACL_SECURITY_INFORMATION"></span><span id="change_dacl_security_information"></span>**CHANGE\_DACL\_SECURITY\_INFORMATION** (4)


</dt> <dd>

Change the DACL of the logical file.

</dd> <dt>

<span id="CHANGE_SACL_SECURITY_INFORMATION"></span><span id="change_sacl_security_information"></span>

<span id="CHANGE_SACL_SECURITY_INFORMATION"></span><span id="change_sacl_security_information"></span>**CHANGE\_SACL\_SECURITY\_INFORMATION** (8)


</dt> <dd>

Change the system access control list (SACL) of the logical file.

</dd> </dl> </dd> </dl>

## Return value

Returns a value of 0 (zero) if the permissions are changed, and a different number to indicate an error.

<dl> <dt>

**Success**
</dt> <dd>

0

The request is successful.

</dd> <dt>

**Access Denied**
</dt> <dd>

2

Access is denied.

</dd> <dt>

**Unspecified failure**
</dt> <dd>

8

An unspecified failure occurred.

</dd> <dt>

**Invalid object**
</dt> <dd>

9

The specified name is not valid.

</dd> <dt>

**Object already exists**
</dt> <dd>

10

The specified object already exists.

</dd> <dt>

**File system not NTFS**
</dt> <dd>

11

The file system is not an NTFS file system.

</dd> <dt>

**Platform not NT/Windows 2000**
</dt> <dd>

12

The platform is not Windows.

</dd> <dt>

**Drive not the same**
</dt> <dd>

13

The drive is not the same.

</dd> <dt>

**Directory not empty**
</dt> <dd>

14

The directory is not empty.

</dd> <dt>

**Sharing violation**
</dt> <dd>

15

There is a sharing violation.

</dd> <dt>

**Invalid start file**
</dt> <dd>

16

The specified start file is not valid.

</dd> <dt>

**Privilege not held**
</dt> <dd>

17

A privilege required for the operation is missing.

</dd> <dt>

**Invalid parameter**
</dt> <dd>

21

A specified parameter is not valid.

</dd> </dl>

## Requirements



|                                     |                                                                                         |
|-------------------------------------|-----------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows Vista<br/>                                                                |
| Minimum supported server<br/> | Windows Server 2008<br/>                                                          |
| Namespace<br/>                | Root\\CIMV2<br/>                                                                  |
| MOF<br/>                      | <dl> <dt>CIMWin32.mof</dt> </dl> |
| DLL<br/>                      | <dl> <dt>CIMWin32.dll</dt> </dl> |



## See also

<dl> <dt>

[Operating System Classes](/previous-versions//aa392727(v=vs.85))
</dt> <dt>

[**Win32\_PageFile**](win32-pagefile.md)
</dt> </dl>

 

