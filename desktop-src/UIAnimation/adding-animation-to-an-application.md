---
title: Create the Main Animation Objects
description: To use Windows Animation in your application, the first step is to create a small set of main animation objects.
ms.assetid: '4005819e-482c-4052-89f8-b8e457c0c3dc'
keywords: ["animation manager objects Windows Animation ,creating", "animation timer objects Windows Animation ,creating", "transition library objects Windows Animation ,creating"]
---

# Create the Main Animation Objects

To use Windows Animation in your application, the first step is to create a small set of main animation objects.

## Overview

Use the [**CoCreateInstance**](https://msdn.microsoft.com/library/windows/desktop/ms686615) function to create the animation manager, animation timer, and transition library objects.

These objects will be needed to create and display animations, so they usually should not be released until the application is shutting down. If there is no chance that any registered callbacks could have created a reference cycle, releasing the objects is sufficient for a proper cleanup. Otherwise, the application can clean up by clearing the callbacks (passing **NULL** in the place of each) or by calling the animation manager's [**Shutdown**](iuianimationmanager-shutdown.md) method.

## Example Code

The following example code is taken from MainWindow.cpp in the Windows Animation samples; see the CMainWindow::InitializeAnimation method.


```C++
// Create the animation manager object

HRESULT hr = CoCreateInstance(
    CLSID_UIAnimationManager,
    NULL,
    CLSCTX_INPROC_SERVER,
    IID_PPV_ARGS(&amp;m_pAnimationManager)
    );

if (SUCCEEDED(hr))
{
    // Create the animation timer object

    hr = CoCreateInstance(
        CLSID_UIAnimationTimer,
        NULL,
        CLSCTX_INPROC_SERVER,
        IID_PPV_ARGS(&amp;m_pAnimationTimer)
        );

    if (SUCCEEDED(hr))
    {
        // Create the transition library object

        hr = CoCreateInstance(
            CLSID_UIAnimationTransitionLibrary,
            NULL,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&amp;m_pTransitionLibrary)
            );

        ...

    }

    ...

}
```



Note the following definitions from MainWindow.h.


```
class CMainWindow
{

    ...

private:

    // Animation components

    IUIAnimationManager *m_pAnimationManager;
    IUIAnimationTimer *m_pAnimationTimer;
    IUIAnimationTransitionLibrary *m_pTransitionLibrary;

    ...

};
```



## Next Step

After completing this step, the next step is: [Create Animation Variables](create-animation-variables.md).

## Related topics

<dl> <dt>

[**CoCreateInstance**](https://msdn.microsoft.com/library/windows/desktop/ms686615)
</dt> <dt>

[**IUIAnimationManager**](iuianimationmanager.md)
</dt> <dt>

[**IUIAnimationTimer**](iuianimationtimer.md)
</dt> <dt>

[**IUIAnimationTransitionLibrary**](iuianimationtransitionlibrary.md)
</dt> <dt>

[Windows Animation Overview](scenic-animation-api-overview.md)
</dt> </dl>

 

 



