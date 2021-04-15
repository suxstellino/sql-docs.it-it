---
title: Utilizzare il provider PowerShell per eventi estesi
description: Usare il provider SQL Server PowerShell per gestire SQL Server eventi estesi. Questo articolo illustra esempi di creazione, modifica e gestione di sessioni.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- PowerShell [SQL Server], xevent
- extended events [SQL Server], PowerShell
- PowerShell [SQL Server], extended events
ms.assetid: 0b10016f-a479-4444-a484-46cb4677cf64
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2305c8e5e98b4f050779930be16ecae60dec1990
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492357"
---
# <a name="use-the-powershell-provider-for-extended-events"></a>Utilizzare il provider PowerShell per eventi estesi

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  È possibile gestire eventi estesi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il provider PowerShell per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La sottocartella XEvent è disponibile all'interno dell'unità SQLSERVER. È possibile accedere alla cartella utilizzando uno dei metodi seguenti:  
  
-   Al prompt dei comandi digitare **sqlps**, quindi premere INVIO. Digitare **cd xevent**, quindi premere INVIO. È quindi possibile usare i comandi **cd** e **dir** o i cmdlet **Set-Location** e **Get-Childitem** per passare al nome del server e al nome dell'istanza.  
  
-   In Esplora oggetti espandere il nome dell'istanza, espandere **Gestione**, fare clic con il pulsante destro del mouse su **Eventi estesi**, quindi scegliere **Avvia PowerShell**. Verrà avviato PowerShell nel percorso seguente:  
  
     PS SQLSERVER:\XEvent \\ *ServerName* \\ *InstanceName*>  
  
    > [!NOTE]  
    >  È possibile avviare PowerShell da qualsiasi nodo all'interno di **Eventi estesi**. È possibile, ad esempio, fare clic con il pulsante destro del mouse su **Sessioni** e quindi scegliere **Avvia PowerShell**. Verrà avviato PowerShell a un livello più interno, nella cartella Sessioni.  
  
 È possibile esplorare l'albero delle cartelle XEvent per visualizzare sessioni di eventi estesi esistenti e i relativi eventi, database di destinazione e predicati associati. Ad esempio, dal percorso PS SQLSERVER:\XEvent \\  \\ *ServerName InstanceName*>, se si digita cd sessions , premere INVIO, digitare **dir** e quindi premere INVIO, è possibile visualizzare l'elenco delle sessioni archiviate in tale istanza. È inoltre possibile visualizzare se la sessione è in esecuzione, e in tal caso per quanto tempo, e se la sessione è configurata per l'avvio all'avvio dell'istanza.  
  
 Per visualizzare gli eventi, i relativi predicati e i database di destinazione associati a una sessione, è possibile passare alla directory con il nome della sessione e quindi visualizzare la cartella degli eventi o dei database di destinazione. Ad esempio, per visualizzare gli eventi e i relativi predicati associati alla sessione di integrità del sistema predefinita, dal percorso PS SQLSERVER:\XEvent \\  \\ *ServerName InstanceName*\Sessions> digitare **cd system_health\events,** premere INVIO, digitare **dir** e quindi premere INVIO.  
  
 Il provider PowerShell per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è uno strumento potente che consente di creare, modificare e gestire sessioni di eventi estesi. Nella sezione seguente vengono forniti alcuni esempi di base dell'utilizzo di script di PowerShell con eventi estesi.  
  
## <a name="examples"></a>Esempio  
 Negli esempi seguenti notare quanto segue:  
  
-   È necessario eseguire gli script dal prompt PS SQLSERVER:\\>, disponibile digitando **sqlps** al prompt dei comandi.  
  
-   Gli script utilizzano l'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   È necessario salvare gli script con estensione ps1.  
  
-   I criteri di esecuzione di PowerShell devono consentire l'esecuzione dello script. Per impostare i criteri di esecuzione, usare il cmdlet **Set-Executionpolicy** Per altre informazioni, digitare **get-help set-executionpolicy -detailed**, quindi premere INVIO.  
  
 Nello script seguente viene creata una nuova sessione denominata 'TestSession'.  
  
```  
#Script for creating a session.  
cd XEvent  
$h = hostname  
cd $h  
  
#Use the default instance.  
$store = dir | where {$_.DisplayName -ieq 'default'}  
$session = new-object Microsoft.SqlServer.Management.XEvent.Session -argumentlist $store, "TestSession"  
$event = $session.AddEvent("sqlserver.file_written")  
$event.AddAction("package0.callstack")  
$session.Create()  
```  
  
 Nello script seguente viene aggiunta la destinazione del buffer circolare alla sessione creata nell'esempio precedente. Questo esempio illustra l'uso del metodo **Alter** Tenere presente che è possibile aggiungere la destinazione quando si crea la sessione per la prima volta.  
  
```  
#Script to alter a session.  
cd XEvent  
$h = hostname  
cd $h  
cd DEFAULT\Sessions  
  
#Used to find the specified session.  
$session = dir|where {$_.Name -eq 'TestSession'}  
  
#Add the ring buffer target and call the Alter method.  
$session.AddTarget("package0.ring_buffer")  
$session.Alter()  
```  
  
 Nello script seguente viene creata una nuova sessione in cui è utilizzata un'espressione di predicato. In questo caso la sessione raccoglie informazioni per il momento in cui verrà scritto il file c:\temp.log tramite l'evento sqlserver.file_written.  
  
```  
#Script for creating a session.  
cd XEvent  
$h = hostname  
cd $h  
  
#Use the default instance.  
$store = dir | where {$_.DisplayName -ieq 'default'}  
$session = new-object Microsoft.SqlServer.Management.XEvent.Session -argumentlist $store, "TestSession2"  
$event = $session.AddEvent("sqlserver.file_written")  
  
#Construct a predicate "equal_i_unicode_string(path, N'c:\temp.log')".  
$column = $store.SqlServerPackage.EventInfoSet["file_written"].DataEventColumnInfoSet["path"]  
$operand = new-object Microsoft.SqlServer.Management.XEvent.PredOperand -argumentlist $column  
$value = new-object Microsoft.SqlServer.Management.XEvent.PredValue -argumentlist "c:\temp.log"  
$compare = $store.Package0Package.PredCompareInfoSet["equal_i_unicode_string"]  
$predicate = new-object Microsoft.SqlServer.Management.XEvent.PredFunctionExpr -argumentlist $compare, $operand, $value  
$event.SetPredicate($predicate)  
$session.Create()  
```  
  
## <a name="security"></a>Sicurezza  
 Per creare, modificare o eliminare una sessione di eventi estesi, è necessario disporre dell'autorizzazione ALTER ANY EVENT SESSION.  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server PowerShell](../../powershell/sql-server-powershell.md)   
 [Usare la system_health sessione](../../relational-databases/extended-events/use-the-system-health-session.md)   
 [Strumenti degli eventi estesi](../../relational-databases/extended-events/extended-events-tools.md)  
  
