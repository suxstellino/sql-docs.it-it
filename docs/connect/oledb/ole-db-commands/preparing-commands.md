---
title: Preparazione dei comandi (OLE DB Driver)
description: Per un singolo comando eseguito più volte, OLE DB Driver per SQL Server supporta la preparazione dei comandi per migliorare le prestazioni.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, commands
- prepared statements [OLE DB Driver for SQL Server]
- commands [OLE DB]
- command preparation [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6b682c54374bed8645ff45f7f4fadc59b9d0f828
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735515"
---
# <a name="preparing-commands"></a>Preparazione dei comandi
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Il driver OLE DB per SQL Server supporta la preparazione dei comandi per l'esecuzione multipla ottimizzata di un singolo comando. La preparazione dei comandi genera tuttavia un overhead e un consumer non ha la necessità di preparare un comando per eseguirlo più volte. In genere, un comando deve essere preparato se verrà eseguito più di tre volte.  
  
 Per motivi relativi alle prestazioni, la preparazione dei comandi viene rinviata fino all'esecuzione del comando. Questo è il comportamento predefinito. Eventuali errori nel comando da preparare verranno rilevati solo dopo l'esecuzione del comando o dell'operazione di metaproprietà. L'impostazione della proprietà di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] SSPROP_DEFERPREPARE su FALSE può determinare la disattivazione del comportamento predefinito.  
  
 In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], quando un comando viene eseguito direttamente (senza preparazione iniziale), viene creato un piano di esecuzione che viene memorizzato nella cache. Se l'istruzione SQL viene eseguita nuovamente, in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è disponibile un algoritmo efficiente che consente di mettere in corrispondenza la nuova istruzione con il piano di esecuzione esistente nella cache e di riutilizzare il piano di esecuzione per l'istruzione in questione.  
  
 Per i comandi preparati [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fornisce supporto nativo per la preparazione e l'esecuzione delle relative istruzioni. Quando si prepara un'istruzione, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] consente di creare un piano di esecuzione, di memorizzarlo nella cache e di restituire al provider un handle al piano di esecuzione. Il provider quindi utilizza l'handle per eseguire ripetutamente l'istruzione. Non vengono create stored procedure. Poiché l'handle identifica direttamente il piano di esecuzione per un'istruzione SQL, anziché mettere in corrispondenza l'istruzione al piano di esecuzione nella cache (come nel caso dell'esecuzione diretta), è più efficiente preparare un'istruzione piuttosto che eseguirla direttamente, se è noto che l'istruzione verrà eseguita più volte.  
  
 In [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] le istruzioni preparate non possono essere utilizzate per creare oggetti temporanei e non possono fare riferimento a stored procedure di sistema che creano oggetti temporanei, ad esempio tabelle temporanee. Tali procedure devono essere eseguite in modo diretto.  
  
 Alcuni comandi non devono essere mai preparati, ad esempio quelli che specificano l'esecuzione di stored procedure o includono testo non valido per la creazione di stored procedure di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Se viene creata una stored procedure temporanea, il driver OLE DB per SQL Server la esegue restituendo i risultati come se fosse stata eseguita l'istruzione stessa.  
  
 La creazione di stored procedure temporanee viene controllata dalla proprietà di inizializzazione specifica del driver OLE DB per SQL Server denominata SSPROP_INIT_USEPROCFORPREP. Se il valore della proprietà è SSPROPVAL_USEPROCFORPREP_ON o SSPROPVAL_USEPROCFORPREP_ON_DROP, il driver OLE DB per SQL Server tenta di creare una stored procedure quando viene preparato un comando. La creazione della stored procedure riesce se l'utente dell'applicazione dispone di autorizzazioni sufficienti per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Per i consumer che si disconnettono raramente, la creazione di stored procedure temporanee può richiedere risorse significative di **tempdb**, ovvero il database di sistema di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel quale vengono creati gli oggetti temporanei. Quando il valore di SSPROP_INIT_USEPROCFORPREP è SSPROPVAL_USEPROCFORPREP_ ON, le stored procedure temporanee create dal driver OLE DB per SQL Server vengono eliminate solo quando la sessione che ha creato il comando perde la connessione all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se la connessione in oggetto è quella predefinita, creata al momento dell'inizializzazione dell'origine dati, la stored procedure temporanea viene eliminata solo quando l'origine dati diventa non inizializzata.  
  
 Quando il valore di SSPROP_INIT_USEPROCFORPREP è SSPROPVAL_USEPROCFORPREP_ON_DROP, le store procedure temporanee del driver OLE DB per SQL Server vengono eliminate quando si verifica una delle situazioni seguenti:  
  
-   Il consumer usa **ICommandText::SetCommandText** per indicare un nuovo comando.  
  
-   Il consumer usa **ICommandPrepare::Unprepare** per indicare che il testo del comando non è più necessario.  
  
-   Il consumer rilascia tutti i riferimenti all'oggetto comando utilizzando la stored procedure temporanea.  
  
 Un oggetto comando include al massimo una stored procedure temporanea in **tempdb**. Le stored procedure temporanee esistenti rappresentano il testo del comando corrente di un oggetto comando specifico.  
  
## <a name="see-also"></a>Vedere anche  
 [Comandi](../../oledb/ole-db-commands/commands.md)  
  
  
