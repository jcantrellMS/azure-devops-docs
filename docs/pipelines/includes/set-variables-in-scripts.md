---
ms.topic: include
ms.technology: devops-cicd
ms.manager: mijacobs
ms.author: jukullam
author: juliakm
ms.date: 08/19/2020
---

<h3 id="set-in-script">Define and modify your variables in a script</h3>

To define or modify a variable from a script, use the `task.setvariable` logging command.
Note that the updated variable value is scoped to the job being executed, and does not flow across jobs or stages.
Variable names are transformed to uppercase, and the characters "." and " " are replaced by "_".

For example, `Agent.WorkFolder` becomes `AGENT_WORKFOLDER`.
On Windows, you access this as `%AGENT_WORKFOLDER%` or `$env:AGENT_WORKFOLDER`.
On Linux and macOS, you use `$AGENT_WORKFOLDER`.

> [!TIP]
> 
> You can run a script on a:
> 
> * [Windows agent](../agents/v2-windows.md) using either a [Batch script task](/azure/devops/pipelines/tasks/reference/batch-script-v1) or [PowerShell script task](/azure/devops/pipelines/tasks/reference/powershell-v2).
> * [macOS](../agents/v2-osx.md) or [Linux](../agents/v2-linux.md) agent using a [Shell script task](/azure/devops/pipelines/tasks/reference/shell-script-v2).



# [Batch](#tab/batch)

**Batch script**

:::image type="icon" source="../tasks/utility/media/batch-script.png" border="false"::: Set the `sauce` and `secret.Sauce` variables

```bat
@echo ##vso[task.setvariable variable=sauce]crushed tomatoes
@echo ##vso[task.setvariable variable=secret.Sauce;issecret=true]crushed tomatoes with garlic
```

:::image type="icon" source="../tasks/utility/media/batch-script.png" border="false"::: Read the variables

Arguments

```arguments
"$(sauce)" "$(secret.Sauce)"
```

Script

 ```bat
@echo off
set sauceArgument=%~1
set secretSauceArgument=%~2
@echo No problem reading %sauceArgument% or %SAUCE%
@echo But I cannot read %SECRET_SAUCE%
@echo But I can read %secretSauceArgument% (but the log is redacted so I do not spoil
      the secret)
```

# [PowerShell](#tab/powershell)

**PowerShell script**

:::image type="icon" source="../tasks/utility/media/powershell.png" border="false"::: Set the `sauce` and `secret.Sauce` variables

```powershell
Write-Host "##vso[task.setvariable variable=sauce]crushed tomatoes"
Write-Host "##vso[task.setvariable variable=secret.Sauce;issecret=true]crushed tomatoes with
            garlic"
```

:::image type="icon" source="../tasks/utility/media/powershell.png" border="false"::: Read the variables

Arguments

```arguments
-sauceArgument "$(sauce)" -secretSauceArgument "$(secret.Sauce)"
```

Script

```powershell
Param(
   [string]$sauceArgument,
   [string]$secretSauceArgument
)
Write-Host No problem reading $env:SAUCE or $sauceArgument
Write-Host But I cannot read $env:SECRET_SAUCE
Write-Host But I can read $secretSauceArgument "(but the log is redacted so I do not
           spoil the secret)"
```

# [Shell](#tab/shell)

:::image type="icon" source="../tasks/utility/media/shell-script.png" border="false"::: Set the `sauce` and `secret.Sauce` variables

```bash
#!/bin/bash
echo "##vso[task.setvariable variable=sauce]crushed tomatoes"
echo "##vso[task.setvariable variable=secret.Sauce;issecret=true]crushed tomatoes with garlic"
```

:::image type="icon" source="../tasks/utility/media/shell-script.png" border="false"::: Read the variables

Arguments

```arguments
"$(sauce)" "$(secret.Sauce)"
```

Script

```bash
#!/bin/bash
echo "No problem reading $SAUCE"
echo "But I cannot read $SECRET_SAUCE"
```

---

Console output from reading the variables:

```output
No problem reading crushed tomatoes or crushed tomatoes
But I cannot read 
But I can read ******** (but the log is redacted so I do not spoil the secret)
```
