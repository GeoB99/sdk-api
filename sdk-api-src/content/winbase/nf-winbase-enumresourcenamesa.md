---
UID: NF:winbase.EnumResourceNamesA
title: EnumResourceNamesA function (winbase.h)
description: Enumerates resources of a specified type within a binary module.
helpviewer_keywords: ["EnumResourceNames","EnumResourceNames function [Menus and Other Resources]","EnumResourceNamesA","EnumResourceNamesW","_win32_EnumResourceNames","_win32_enumresourcenames_cpp","menurc.enumresourcenames","winbase/EnumResourceNames","winbase/EnumResourceNamesA","winbase/EnumResourceNamesW","winui._win32_enumresourcenames"]
old-location: menurc\enumresourcenames.htm
tech.root: menurc
ms.assetid: VS|winui|~\winui\windowsuserinterface\resources\introductiontoresources\resourcereference\resourcefunctions\enumresourcenames.htm
ms.date: 09/24/2020
ms.keywords: EnumResourceNames, EnumResourceNames function [Menus and Other Resources], EnumResourceNamesA, EnumResourceNamesW, _win32_EnumResourceNames, _win32_enumresourcenames_cpp, menurc.enumresourcenames, winbase/EnumResourceNames, winbase/EnumResourceNamesA, winbase/EnumResourceNamesW, winui._win32_enumresourcenames
req.header: winbase.h
req.include-header: Windows.h
req.target-type: Windows
req.target-min-winverclnt: Windows 2000 Professional [desktop apps only]
req.target-min-winversvr: Windows 2000 Server [desktop apps only]
req.kmdf-ver: 
req.umdf-ver: 
req.ddi-compliance: 
req.unicode-ansi: EnumResourceNamesW (Unicode) and EnumResourceNamesA (ANSI)
req.idl: 
req.max-support: 
req.namespace: 
req.assembly: 
req.type-library: 
req.lib: Kernel32.lib
req.dll: Kernel32.dll
req.irql: 
targetos: Windows
req.typenames: 
req.redist: 
ms.custom: 19H1
f1_keywords:
 - EnumResourceNamesA
 - winbase/EnumResourceNamesA
dev_langs:
 - c++
topic_type:
 - APIRef
 - kbSyntax
api_type:
 - DllExport
api_location:
 - Kernel32.dll
 - API-MS-Win-Core-LibraryLoader-L1-2-2.dll
 - KernelBase.dll
api_name:
 - EnumResourceNames
 - EnumResourceNamesA
 - EnumResourceNamesW
---

# EnumResourceNamesA function

## -description

Enumerates resources of a specified type within a binary module. For Windows Vista and later, this is typically a [language-neutral Portable Executable](/windows/desktop/Intl/mui-resource-management) (LN file), and the enumeration will also include resources from the corresponding language-specific resource files (.mui files) that contain localizable language resources. It is also possible for *hModule* to specify an .mui file, in which case only that file is searched for resources.

## -parameters

### -param hModule [in, optional]

Type: **HMODULE**

A handle to a module to be searched. Starting with Windows Vista, if this is an LN file, then appropriate .mui files (if any exist) are included in the search.

If this parameter is **NULL**, that is equivalent to passing in a handle to the module used to create the current process.

### -param lpType [in]

Type: **LPCTSTR**

The type of the resource for which the name is being enumerated. Alternately, rather than a pointer, this parameter can be [MAKEINTRESOURCE](/windows/desktop/api/winuser/nf-winuser-makeintresourcea)(ID), where ID is an integer value representing a predefined resource type. For a list of predefined resource types, see [Resource Types](/windows/win32/menurc/resource-types). For more information, see the [Remarks](#remarks) section below.

### -param lpEnumFunc [in]

Type: **ENUMRESNAMEPROC**

A pointer to the callback function to be called for each enumerated resource name or ID. For more information, see [ENUMRESNAMEPROC](/windows/win32/api/libloaderapi/nc-libloaderapi-enumresnameproca).

### -param lParam [in]

Type: **LONG_PTR**

An application-defined value passed to the callback function. This parameter can be used in error checking.

## -returns

Type: **BOOL**

The return value is **TRUE** if the function succeeds or **FALSE** if the function does not find a resource of the type specified, or if the function fails for another reason. To get extended error information, call [GetLastError](/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror).

## -remarks

If [IS_INTRESOURCE](/windows/desktop/api/winuser/nf-winuser-is_intresource)(*lpszType*) is **TRUE**, then *lpszType* specifies the integer identifier of the given resource type. Otherwise, it is a pointer to a null-terminated string. If the first character of the string is a pound sign (#), then the remaining characters represent a decimal number that specifies the integer identifier of the resource type. For example, the string "#258" represents the identifier 258.

For each resource found, **EnumResourceNames** calls an application-defined callback function *lpEnumFunc*, passing the name or the ID of each resource it finds, as well as the various other parameters that were passed to **EnumResourceNames**.

Alternately, applications can call [EnumResourceNamesEx](/windows/desktop/api/libloaderapi/nf-libloaderapi-enumresourcenamesexw), which provides more precise control of what resources are enumerated.

If a resource has an ID, the ID is passed to the callback function; otherwise the resource name is passed to the callback function. For more information, see [ENUMRESNAMEPROC](/windows/win32/api/libloaderapi/nc-libloaderapi-enumresnameproca).

The **EnumResourceNames** function continues to enumerate resources until the callback function returns **FALSE** or all resources have been enumerated.

Starting with Windows Vista, if *hModule* specifies an LN file, then the resources enumerated can reside either in the LN file or in a .mui file associated with it. If no .mui files are found, only resources from the LN file are returned. The order in which .mui files are searched is the usual Resource Loader search order; see [User Interface Language Management](/windows/desktop/Intl/user-interface-language-management) for details. Once one appropriate .mui file is found, the .mui file search stops. Because all .mui files that correspond to a single LN file have the same resource types, only the resources in the found .mui file need to be enumerated.

The enumeration never includes duplicates: if resources with the same name are contained in both the LN file and in an .mui file, the resource will only be enumerated once.

#### Examples

For an example, see [Creating a Resource List](/windows/desktop/menurc/using-resources#creating-a-resource-list).

## -see-also

### Conceptual

- [ENUMRESNAMEPROC](/windows/win32/api/libloaderapi/nc-libloaderapi-enumresnameproca)
- [EnumResourceLanguages](/windows/desktop/api/winbase/nf-winbase-enumresourcelanguagesa)
- [EnumResourceNamesEx](/windows/desktop/api/libloaderapi/nf-libloaderapi-enumresourcenamesexw)
- [EnumResourceTypes](/windows/desktop/api/winbase/nf-winbase-enumresourcetypesa)

### Reference

- [Menus and Other Resources](/windows/desktop/menurc/resources)
