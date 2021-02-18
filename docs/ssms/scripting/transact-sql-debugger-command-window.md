---
title: Finestra di comando
description: Informazioni su come usare la finestra di comando del debugger Transact-SQL per eseguire i comandi di debug e per modificare i comandi nel codice di cui si esegue il debug.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Command Window [Transact-SQL]
ms.assetid: e567ebf9-0793-451b-92c7-26193a02d9da
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1921e37b51728d9d460082ecbd73643ef80f1a92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339377"
---
# <a name="transact-sql-debugger---command-window"></a>Debugger Transact-SQL - Finestra di comando

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Usare la **Finestra di comando** per eseguire comandi, ad esempio i comandi debug o di modifica, sul codice incluso nella finestra dell'editor di query del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] di cui è in corso il debug. Per usare la funzionalità **Finestra di comando**, è necessario usare la modalità di debug. Il debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] supporta molti dei comandi supportati anche nella finestra **Comando** di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Per altre informazioni, vedere [Visual Studio Command Window](/previous-versions/visualstudio/visual-studio-2015/ide/reference/command-window)(Finestra di comando di Visual Studio).  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>Elenco attività

**Per accedere alla funzionalità Finestra di comando**

- Scegliere **Avvia debug** dal menu **Debug**.

**Per stampare il valore di una variabile**

- In **Finestra di comando** digitare **Debug.Print\<VariableName>** e quindi premere INVIO.

**Per elencare le informazioni sul thread corrente**

- In **Finestra di comando** digitare **Debug.ListThread**, quindi premere INVIO.

**Per aggiungere una variabile alla finestra Controllo immediato**

- In **Finestra di comando** digitare **Debug.QuickWatch \<VariableName>** e quindi premere INVIO.

## <a name="see-also"></a>Vedere anche

[Debugger Transact-SQL](./transact-sql-debugger.md)