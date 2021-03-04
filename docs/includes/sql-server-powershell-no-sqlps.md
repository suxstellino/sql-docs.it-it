---
title: Uso di NOSQLPS per SQL Server Agent con PowerShell
description: Messaggio in cui viene illustrato come utilizzare il cmdlet di PowerShell per SqlServer anziché il cmdlet sqlps con SQL Server Agent
ms.topic: include
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier
ms.openlocfilehash: bf161bd583f3d48dc389cea6b69f55c32e2cce59
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839406"
---
A partire da SQL Server 2019, è possibile disabilitare SQLPS. Nella prima riga di un passaggio del processo di tipo PowerShell è possibile aggiungere `#NOSQLPS` , che interrompe l'esecuzione del caricamento automatico del modulo sqlps da parte di SQL Agent. A questo punto il processo di SQL Agent esegue la versione di PowerShell installata nel computer, quindi è possibile usare qualsiasi altro modulo di PowerShell.

Per usare il [**modulo SqlServer**](https://www.powershellgallery.com/packages/Sqlserver/21.1.18235) nel passaggio del processo di SQL Agent, è possibile inserire questo codice nelle prime due righe dello script.

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```