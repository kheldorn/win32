---
title: IAgentCommand GetEnabled
description: IAgentCommand GetEnabled
ms.assetid: 'b4ec25d2-b2e0-4b1b-8dc5-10fbb44149b5'
---

# IAgentCommand::GetEnabled

\[Microsoft Agent is deprecated as of Windows 7, and may be unavailable in subsequent versions of Windows.\]

``` syntax
HRESULT GetEnabled(
   long * pbEnabled  // address of Enabled setting for Command
);
```

Retrieves the value of the [**Enabled**](enabled-property.md) property for a [**Command**](https://msdn.microsoft.com/library/windows/desktop/ms696441).

-   Returns S\_OK to indicate the operation was successful.

<dl> <dt>

<span id="pbEnabled"></span><span id="pbenabled"></span><span id="PBENABLED"></span>*pbEnabled*
</dt> <dd>

The address of a variable that receives **True** if the [**Command**](https://msdn.microsoft.com/library/windows/desktop/ms696441) is enabled, or **False** if it is disabled. A disabled **Command** cannot be selected.

</dd> </dl>

## See Also

[**IAgentCommand::SetCaption**](iagentcommand--setcaption.md), [**IAgentCommand::SetVisible**](iagentcommand--setvisible.md), [**IAgentCommand::SetVoice**](iagentcommand--setvoice.md), [**IAgentCommands::Add**](iagentcommands--add.md), [**IAgentCommands::Insert**](iagentcommands--insert.md)


 

 



