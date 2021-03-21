---
description: Preparazione di comandi in SQL Server Native Client
title: Preparazione di comandi (provider OLE DB di Native Client)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, commands
- prepared statements [SQL Server Native Client]
- commands [OLE DB]
- command preparation [SQL Server Native Client]
ms.assetid: 09ec0c6c-0a44-4766-b9b7-5092f676ee54
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 485c7d55a528e3fb8709afb4243b17b5d2b9346e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749011"
---
# <a name="preparing-commands-in-sql-server-native-client"></a>Preparazione di comandi in SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client supporta la funzione di preparazione dei comandi per l'esecuzione multipla ottimizzata di un solo comando. La preparazione dei comandi genera tuttavia overhead e per un consumer non esiste la necessità di preparare un comando per eseguirlo più volte. In genere, un comando deve essere preparato se verrà eseguito più di tre volte.  
  
 Per motivi relativi alle prestazioni, la preparazione dei comandi viene rinviata fino all'esecuzione del comando. Questo è il comportamento predefinito. Eventuali errori nel comando da preparare verranno rilevati solo dopo l'esecuzione del comando o dell'operazione di metaproprietà. L'impostazione della proprietà di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SSPROP_DEFERPREPARE su FALSE può determinare la disattivazione del comportamento predefinito.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], quando un comando viene eseguito direttamente (senza preparazione iniziale), viene creato un piano di esecuzione che viene memorizzato nella cache. Se l'istruzione SQL viene eseguita nuovamente, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è disponibile un algoritmo efficiente che consente di mettere in corrispondenza la nuova istruzione con il piano di esecuzione esistente nella cache e di riutilizzare il piano di esecuzione per l'istruzione in questione.  
  
 Per i comandi preparati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce supporto nativo per la preparazione e l'esecuzione delle relative istruzioni. Quando si prepara un'istruzione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di creare un piano di esecuzione, di memorizzarlo nella cache e di restituire al provider un handle al piano di esecuzione. Il provider quindi utilizza l'handle per eseguire ripetutamente l'istruzione. Non vengono create stored procedure. Poiché l'handle identifica direttamente il piano di esecuzione per un'istruzione SQL, anziché mettere in corrispondenza l'istruzione al piano di esecuzione nella cache (come nel caso dell'esecuzione diretta), è più efficiente preparare un'istruzione piuttosto che eseguirla direttamente, se è noto che l'istruzione verrà eseguita più volte.  
  
 In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] le istruzioni preparate non possono essere utilizzate per creare oggetti temporanei e non possono fare riferimento a stored procedure di sistema che creano oggetti temporanei, ad esempio tabelle temporanee. Tali procedure devono essere eseguite in modo diretto.  
  
 Alcuni comandi non devono essere mai preparati, ad esempio quelli che specificano l'esecuzione di stored procedure o includono testo non valido per la creazione di stored procedure di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se viene creata una stored procedure temporanea, il provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client la esegue, restituendo i risultati come se fosse stata eseguita l'istruzione stessa.  
  
 La creazione di stored procedure temporanee viene controllata dalla proprietà di inizializzazione specifica del provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client denominata SSPROP_INIT_USEPROCFORPREP. Se il valore della proprietà è SSPROPVAL_USEPROCFORPREP_ON o SSPROPVAL_USEPROCFORPREP_ON_DROP, il provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client tenta di creare una stored procedure quando viene preparato un comando. La creazione della stored procedure riesce se l'utente dell'applicazione dispone di autorizzazioni sufficienti per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per i consumer che si disconnettono raramente, la creazione di stored procedure temporanee può richiedere risorse significative di **tempdb**, ovvero il database di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel quale vengono creati gli oggetti temporanei. Quando il valore di SSPROP_INIT_USEPROCFORPREP è SSPROPVAL_USEPROCFORPREP_ ON, le stored procedure temporanee create dal provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client vengono eliminate solo quando la sessione che ha creato il comando perde la connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se la connessione in oggetto è quella predefinita, creata al momento dell'inizializzazione dell'origine dati, la stored procedure temporanea viene eliminata solo quando l'origine dati diventa non inizializzata.  
  
 Quando il valore di SSPROP_INIT_USEPROCFORPREP è SSPROPVAL_USEPROCFORPREP_ON_DROP, le stored procedure temporanee del provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client vengono eliminate quando si verifica una delle situazioni seguenti:  
  
-   Il consumer usa **ICommandText::SetCommandText** per indicare un nuovo comando.  
  
-   Il consumer usa **ICommandPrepare::Unprepare** per indicare che il testo del comando non è più necessario.  
  
-   Il consumer rilascia tutti i riferimenti all'oggetto comando utilizzando la stored procedure temporanea.  
  
 Un oggetto comando include al massimo una stored procedure temporanea in **tempdb**. Le stored procedure temporanee esistenti rappresentano il testo del comando corrente di un oggetto comando specifico.  
  
## <a name="see-also"></a>Vedere anche  
 [Comandi](../../relational-databases/native-client-ole-db-commands/commands.md)  
  
  
