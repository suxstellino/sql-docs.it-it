---
title: Inserimento di frammenti Transact-SQL racchiusi
description: Informazioni su come inserire uno snippet di inclusione che funga da punto di partenza per inserire le istruzioni nei blocchi di codice.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- snippets [Transact-SQL], surround with
- IntelliSense [SQL Server], surround with snippets
- Transact-SQL snippets, surround with
ms.assetid: 5b5a8c6c-968e-4361-a7f5-9e2ac186d927
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a62f995d8f75eef26c6e27afdfac06f365251796
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354635"
---
# <a name="insert-surround-with-transact-sql-snippets"></a>Inserimento di frammenti Transact-SQL racchiusi
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Uno snippet di inclusione è un modello che è possibile utilizzare come punto iniziale per includere un set di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] in un blocco BEGIN, IF o WHILE.  
  
## <a name="inserting-surround-with-snippets"></a>Inserimento di snippet di inclusione  
 È possibile avviare snippet di inclusione in tre diverse modalità: tramite una scelta rapida da tastiera, dal menu **Modifica** o dal menu di scelta rapida.  
  
 Dopo aver inserito il frammento, è necessario modificare il testo di sostituzione per formare un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] valida. Per altre informazioni, vedere [Completare frammenti di Transact-SQL](./complete-transact-sql-snippets.md).  
  
#### <a name="to-insert-a-surround-with-snippet"></a>Per inserire uno snippet di inclusione  
  
1.  Nella finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] selezionare il set di istruzioni da includere nel blocco.  
  
2.  Utilizzare uno dei tre metodi seguenti per visualizzare l'elenco di snippet di inclusione:  
  
    -   Digitare CTRL+K, CTRL+S.  
  
    -   Scegliere **IntelliSense** dal menu **Modifica** e quindi selezionare il comando **Racchiudi tra** .  
  
    -   Fare clic con il pulsante destro del mouse sul testo selezionato, quindi scegliere **Racchiudi tra** dal menu di scelta rapida.  
  
3.  Selezionare il nome del frammento (BEGIN, IF o WHILE) nell'elenco utilizzando il mouse o digitando il nome del frammento e quindi premendo TAB o INVIO.  
  
## <a name="see-also"></a>Vedere anche  
 [Inserire frammenti Transact-SQL](./insert-transact-sql-snippets.md)  
  
