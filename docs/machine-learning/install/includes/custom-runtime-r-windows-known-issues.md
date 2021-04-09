---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/07/2021
ms.topic: include
author: anmunde
ms.author: anmunde
ms.reviewer: dphansen
ms.openlocfilehash: dbfc7de86a0ecf06c2eca0c6d49e96ca8e3ea39e
ms.sourcegitcommit: 09122d02fc3d86c6028366653337c083da8a3f4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107072377"
---
## <a name="known-issues"></a>Problemi noti

Se si usa il runtime di R fornito come parte di [SQL Server Machine Learning Services](../../sql-server-machine-learning-services.md) impostando `R_HOME` su `C:\Program Files\Microsoft SQL Server\MSSQL15.<INSTANCE_NAME>\R_SERVICES` quando si registra l'estensione del linguaggio, è possibile che venga visualizzato l'errore seguente durante l'esecuzione di qualsiasi script R personalizzato esterno con [sp_execute_external script](../../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*Errore: memoria svantaggi esaurita (limite raggiunto?)*

Per risolvere il problema:
 1. Impostare la variabile di ambiente `R_NSIZE` che indica il numero di oggetti a dimensione fissa ( `cons cells` ) su un valore ragionevole, ad esempio `200000` .
 1. Riavviare il servizio **Launchpad** e ritentare l'esecuzione dello script.