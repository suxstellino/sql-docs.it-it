---
title: MSSQLSERVER_208 | Microsoft Docs
description: Poiché è impossibile trovare l'oggetto specificato, verrà generato un messaggio di nome di oggetto non valido. Vedere la descrizione dell'errore e delle possibili soluzioni.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 208 (Database Engine error)
ms.assetid: 4b1895f5-3197-4da1-af86-954c93507956
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 55b31cdba6f7a96fb8eff7fba2974608b72675d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196403"
---
# <a name="mssqlserver_208"></a>MSSQLSERVER_208
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|208|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|SQ_BADOBJECT|  
|Testo del messaggio|Il nome di oggetto '%.*ls' non è valido.|  
  
## <a name="explanation"></a>Spiegazione  
Non è possibile individuare l'oggetto specificato.  
  
### <a name="possible-causes"></a>Possibili cause  
L'errore può essere causato da uno dei problemi seguenti:  
  
-   L'oggetto non è stato specificato correttamente.  
  
-   L'oggetto non esiste nel database corrente oppure in quello specificato.  
  
-   L'oggetto esiste, ma potrebbe non essere esposto all'utente. È possibile ad esempio che l'utente non disponga di autorizzazioni sull'oggetto oppure l'oggetto è stato creato all'interno di un'istruzione EXECUTE, ma l'accesso è stato eseguito al di fuori dell'ambito dell'istruzione EXECUTE.  
  
## <a name="user-action"></a>Azione dell'utente  
Verificare le informazioni seguenti e correggere l'istruzione in base alle esigenze.  
  
-   Correttezza dell'ortografia dell'oggetto.  
  
-   Correttezza del contesto di database corrente. Se per l'oggetto non è specificato un nome di database, l'oggetto deve esistere nel database corrente. Per altre informazioni sull'impostazione del contesto di database, vedere [USE &#40;Transact-SQL&#41;](~/t-sql/language-elements/use-transact-sql.md).  
  
-   Esistenza dell'oggetto nelle tabelle di sistema. Per verificare l'esistenza di una tabella o un altro oggetto con ambito schema, eseguire una query nella vista del catalogo **sys.objects**. Se l'oggetto non è presente nelle tabelle di sistema, significa che è stato eliminato o l'utente non dispone delle autorizzazioni necessarie per visualizzare i metadati dell'oggetto. Per altre informazioni sulle autorizzazioni per visualizzare i metadati degli oggetti, vedere [Configurazione della visibilità dei metadati](~/relational-databases/security/metadata-visibility-configuration.md).  
  
-   Presenza dell'oggetto nello schema predefinito dell'utente. Se questa condizione non si verifica, l'oggetto deve essere specificato nel formato in due parti *schema_name.object_name*. Si noti che è sempre necessario richiamare funzioni a valori scalari utilizzando almeno un nome in due parti.  
  
-   Distinzione tra maiuscole e minuscole delle regole di confronto del database.  
  
    Quando in un database vengono utilizzate regole di confronto con distinzione tra maiuscole e minuscole, il nome dell'oggetto deve corrispondere alla combinazione di maiuscole e minuscole dell'oggetto nel database. Ad esempio, se un oggetto è specificato come **MiaTabella** in un database con regole di confronto che rispettano la distinzione tra maiuscole e minuscole, le query che si riferiscono all'oggetto come **miatabella** o **Miatabella** restituiranno l'errore 208 poiché i nomi degli oggetti non corrispondono.  
  
    Per verificare le regole di confronto del database, eseguire l'istruzione seguente.  
  
    ```  
    SELECT collation_name FROM sys.databases WHERE name = 'database_name';  
    ```  
  
    L'abbreviazione CS nel nome delle regole di confronto indica che le regole di confronto rispettano la distinzione tra maiuscole e minuscole. Ad esempio, Latin1_General_CS_AS è una regola di confronto con distinzione tra maiuscole e minuscole e distinzione tra caratteri accentati e non accentati, mentre CI indica una regola di confronto senza distinzione tra maiuscole e minuscole.  
  
-   Autorizzazioni di accesso all'oggetto concesse all'utente. Per verificare le autorizzazioni di cui l'utente dispone sull'oggetto, usare la funzione di sistema **Has_Perms_By_Name**.  
  
## <a name="see-also"></a>Vedere anche  
[USE &#40;Transact-SQL&#41;](~/t-sql/language-elements/use-transact-sql.md)  
[Configurazione della visibilità dei metadati](~/relational-databases/security/metadata-visibility-configuration.md)  
[HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](~/t-sql/functions/has-perms-by-name-transact-sql.md)  
  
