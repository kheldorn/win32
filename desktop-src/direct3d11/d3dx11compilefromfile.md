---
title: D3DX11CompileFromFile function (D3DX11async.h)
description: Note The D3DX (D3DX 9, D3DX 10, and D3DX 11) utility library is deprecated for Windows 8 and is not supported for Windows Store apps. Note Instead of using this function, we recommend that you compile offline by using the Fxc.exe command-line compiler or use one of the HLSL compile APIs, like the D3DCompileFromFile API. Compile a shader or an effect from a file.
ms.assetid: 91a1a339-50da-4f86-9b55-6af246a60482
keywords:
- D3DX11CompileFromFile function Direct3D 11
topic_type:
- apiref
api_name:
- D3DX11CompileFromFile
api_location:
- D3DX11.lib
- D3DX11.dll
api_type:
- LibDef
ms.topic: reference
ms.date: 05/31/2018
---

# D3DX11CompileFromFile function

> [!Note]  
> The D3DX (D3DX 9, D3DX 10, and D3DX 11) utility library is deprecated for Windows 8 and is not supported for Windows Store apps.

 

> [!Note]  
> Instead of using this function, we recommend that you compile offline by using the Fxc.exe command-line compiler or use one of the HLSL compile APIs, like the [**D3DCompileFromFile**](/windows/desktop/direct3dhlsl/d3dcompilefromfile) API.

 

Compile a shader or an effect from a file.

## Syntax


```C++
HRESULT D3DX11CompileFromFile(
  _In_        LPCTSTR            pSrcFile,
  _In_  const D3D10_SHADER_MACRO *pDefines,
  _In_        LPD3D10INCLUDE     pInclude,
  _In_        LPCSTR             pFunctionName,
  _In_        LPCSTR             pProfile,
  _In_        UINT               Flags1,
  _In_        UINT               Flags2,
  _In_        ID3DX11ThreadPump  *pPump,
  _Out_       ID3D10Blob         **ppShader,
  _Out_       ID3D10Blob         **ppErrorMsgs,
  _Out_       HRESULT            *pHResult
);
```



## Parameters

<dl> <dt>

*pSrcFile* \[in\]
</dt> <dd>

Type: **[**LPCTSTR**](/windows/desktop/WinProg/windows-data-types)**

The name of the file that contains the shader code. If the compiler settings require Unicode, the data type LPCTSTR resolves to LPCWSTR. Otherwise, the data type resolves to LPCSTR.

</dd> <dt>

*pDefines* \[in\]
</dt> <dd>

Type: **const [**D3D10\_SHADER\_MACRO**](/windows/desktop/api/d3dcommon/ns-d3dcommon-d3d_shader_macro)\***

Optional. Pointer to an array of macro definitions (see [**D3D10\_SHADER\_MACRO**](/windows/desktop/api/d3dcommon/ns-d3dcommon-d3d_shader_macro)). The last structure in the array serves as a terminator and must have all members set to 0. If not used, set *pDefines* to **NULL**.

</dd> <dt>

*pInclude* \[in\]
</dt> <dd>

Type: **[**LPD3D10INCLUDE**](/previous-versions/windows/desktop/legacy/bb173775(v=vs.85))**

Optional. Pointer to an interface for handling include files. Setting this to **NULL** will cause a compile error if a shader contains a \#include.

</dd> <dt>

*pFunctionName* \[in\]
</dt> <dd>

Type: **[**LPCSTR**](/windows/desktop/WinProg/windows-data-types)**

Name of the shader-entry point function where shader execution begins. When you compile an effect, **D3DX11CompileFromFile** ignores *pFunctionName*; we recommend that you set *pFunctionName* to **NULL** because it is good programming practice to set a pointer parameter to **NULL** if the called function will not use it.

</dd> <dt>

*pProfile* \[in\]
</dt> <dd>

Type: **[**LPCSTR**](/windows/desktop/WinProg/windows-data-types)**

A string that specifies the shader model; can be any profile in shader model 2, shader model 3, shader model 4, or shader model 5. The profile can also be for effect type (for example, fx\_4\_1).

</dd> <dt>

*Flags1* \[in\]
</dt> <dd>

Type: **[**UINT**](/windows/desktop/WinProg/windows-data-types)**

Shader [**compile flags**](/windows/desktop/direct3dhlsl/d3dcompile-constants).

</dd> <dt>

*Flags2* \[in\]
</dt> <dd>

Type: **[**UINT**](/windows/desktop/WinProg/windows-data-types)**

Effect [**compile flags**](/windows/desktop/direct3dhlsl/d3dcompile-effect-constants). When you compile a shader and not an effect file, **D3DX11CompileFromFile** ignores *Flags2*; we recommend that you set *Flags2* to zero because it is good programming practice to set a nonpointer parameter to zero if the called function will not use it.

</dd> <dt>

*pPump* \[in\]
</dt> <dd>

Type: **[**ID3DX11ThreadPump**](id3dx11threadpump.md)\***

A pointer to a thread pump interface (see [**ID3DX11ThreadPump Interface**](id3dx11threadpump.md)). Use **NULL** to specify that this function should not return until it is completed.

</dd> <dt>

*ppShader* \[out\]
</dt> <dd>

Type: **[**ID3D10Blob**](/windows/desktop/api/d3dcommon/nn-d3dcommon-id3d10blob)\*\***

A pointer to memory which contains the compiled shader, as well as any embedded debug and symbol-table information.

</dd> <dt>

*ppErrorMsgs* \[out\]
</dt> <dd>

Type: **[**ID3D10Blob**](/windows/desktop/api/d3dcommon/nn-d3dcommon-id3d10blob)\*\***

A pointer to memory which contains a listing of errors and warnings that occurred during compilation. These errors and warnings are identical to the debug output from a debugger.

</dd> <dt>

*pHResult* \[out\]
</dt> <dd>

Type: **[**HRESULT**](https://msdn.microsoft.com/library/Bb401631(v=MSDN.10).aspx)\***

A pointer to the return value. May be **NULL**. If *pPump* is not **NULL**, then *pHResult* must be a valid memory location until the asynchronous execution completes.

</dd> </dl>

## Return value

Type: **[**HRESULT**](https://msdn.microsoft.com/library/Bb401631(v=MSDN.10).aspx)**

The return value is one of the values listed in [Direct3D 11 Return Codes](d3d11-graphics-reference-returnvalues.md).

**D3DX11CompileFromFile** returns E\_INVALIDARG if you supply a non-**NULL** value to the *pHResult* parameter when you supply **NULL** to the *pPump* parameter. For more information about this situation, see Remarks.

## Remarks

For more information about **D3DX11CompileFromFile**, see [**D3DCompile**](/windows/desktop/direct3dhlsl/d3dcompile).

You must supply **NULL** to the *pHResult* parameter if you also supply **NULL** to the *pPump* parameter. Otherwise, you cannot create a shader by using the compiled shader code that **D3DX11CompileFromFile** returns in the memory that the *ppShader* parameter points to. To create a shader from complied shader code, you call one of the following [**ID3D11Device**](/windows/desktop/api/D3D11/nn-d3d11-id3d11device) interface methods:

-   [**CreateComputeShader**](/windows/desktop/api/D3D11/nf-d3d11-id3d11device-createcomputeshader)
-   [**CreateDomainShader**](/windows/desktop/api/D3D11/nf-d3d11-id3d11device-createdomainshader)
-   [**CreateGeometryShader**](/windows/desktop/api/D3D11/nf-d3d11-id3d11device-creategeometryshader)
-   [**CreateGeometryShaderWithStreamOutput**](/windows/desktop/api/D3D11/nf-d3d11-id3d11device-creategeometryshaderwithstreamoutput)
-   [**CreateHullShader**](/windows/desktop/api/D3D11/nf-d3d11-id3d11device-createhullshader)
-   [**CreatePixelShader**](/windows/desktop/api/D3D11/nf-d3d11-id3d11device-createpixelshader)
-   [**CreateVertexShader**](/windows/desktop/api/D3D11/nf-d3d11-id3d11device-createvertexshader)

In addition, if you supply a non-**NULL** value to *pHResult* when you supply **NULL** to *pPump*, **D3DX11CompileFromFile** returns the E\_INVALIDARG error code.

## Requirements



|                    |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Header<br/>  | <dl> <dt>D3DX11async.h</dt> </dl> |
| Library<br/> | <dl> <dt>D3DX11.lib</dt> </dl>    |



## See also

<dl> <dt>

[D3DX Functions](d3d11-graphics-reference-d3dx11-functions.md)
</dt> </dl>

 

