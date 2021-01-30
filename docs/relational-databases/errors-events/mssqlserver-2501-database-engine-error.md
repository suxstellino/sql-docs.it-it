---
description: MSSQLSERVER_2501
title: MSSQLSERVER_2501 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2501 (Database Engine error)
ms.assetid: 895aafe3-a4e7-4ed8-acc5-93be76ef3664
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3f5f698928b3df5abc1c927ec926b24e2ba853f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208473"
---
# <a name="mssqlserver_2501"></a>MSSQLSERVER_2501
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|2501|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC_NO_SUCH_TABLE_NAME|  
|Testo del messaggio|Impossibile trovare un oggetto o una tabella con il nome 'NAME'. Controllare il catalogo di sistema.|  
  
## <a name="explanation"></a>Spiegazione  
Non è possibile individuare l'oggetto specificato.  
  
### <a name="possible-causes"></a>Possibili cause  
L'errore può essere causato da uno dei problemi seguenti:  
  
-   L'oggetto non è stato specificato correttamente.  
  
-   L'oggetto non esiste o è stato rimosso prima di essere utilizzato in un'istruzione.  
  
-   È possibile che l'oggetto esista ma non possa essere esposto all'utente. Ad esempio, l'utente potrebbe non disporre di autorizzazioni sufficienti per visualizzare l'oggetto oppure l'oggetto è costituito da una tabella interna che non può essere visualizzata dall'utente.  
  
## <a name="user-action"></a>Azione dell'utente  
  
-   Verificare la correttezza del contesto di database corrente. Per altre informazioni, vedere [USE &#40;Transact-SQL&#41;](~/t-sql/language-elements/use-transact-sql.md).  
  
-   Verificare che il nome della tabella o dell'oggetto sia stato specificato correttamente.  
  
-   Verificare il nome dello schema contenente l'oggetto. Se l'oggetto appartiene a uno schema diverso da quello predefinito, ovvero **dbo**, è necessario specificare il nome della tabella o dell'oggetto usando il formato in due parti *schema_name.object_name*.  
  
-   Verificare l'esistenza dell'oggetto nelle tabelle di sistema. Per verificare l'esistenza di una tabella o un altro oggetto con ambito schema, eseguire una query nella vista del catalogo [sys.objects](~/relational-databases/system-catalog-views/sys-objects-transact-sql.md). Se l'oggetto non è presente nelle tabelle di sistema, significa che è stato eliminato o l'utente non dispone delle autorizzazioni necessarie per visualizzare i metadati dell'oggetto. Per altre informazioni su come visualizzare i metadati degli oggetti, vedere [Configurazione della visibilità dei metadati](~/relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
[Viste del catalogo &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
