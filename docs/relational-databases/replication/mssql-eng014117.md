---
description: MSSQL_ENG014117
title: MSSQL_ENG014117 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014117 error
ms.assetid: e5906a76-9511-4c47-8826-8c765b58a39d
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 573e93f00f496b99bd685d28bf16ef985077a22f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198822"
---
# <a name="mssql_eng014117"></a>MSSQL_ENG014117
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|14117|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|'%s' non è configurato come database di distribuzione.|  
  
## <a name="explanation"></a>Spiegazione  
 Questo errore si può verificare in presenza di una o entrambe le condizioni seguenti:  
  
-   La voce per il database di distribuzione specificato non è presente in **msdb..MSdistributiondbs**.  
  
-   Nel database **master** non è presente una voce per il server locale oppure la voce contenuta non è corretta.  
  
     La replica prevede che tutti i server di una topologia vengano registrati utilizzando il nome del computer con il nome di un'istanza opzionale (nel caso di un'istanza cluster il nome del server virtuale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con il nome dell'istanza opzionale). Per un corretto funzionamento della replica il valore restituito da `SELECT @@SERVERNAME` per ogni server nella topologia deve far corrispondere al nome dell'istanza opzionale il nome del computer o il nome del server virtuale.  
  
     Non sarà possibile eseguire la replica, se una qualsiasi delle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene registrata utilizzando l'indirizzo IP o il nome di dominio completo (FQDN, Fully Qualified Domain Name). Se durante la configurazione della replica una delle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è stata registrata utilizzando l'indirizzo IP o il nome di dominio completo in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , è possibile che venga generato questo errore.  
  
## <a name="user-action"></a>Azione dell'utente  
 Verificare la corretta registrazione dell'istanza del server di distribuzione. Se il nome di rete del computer e il nome dell'istanza di SQL Server sono diversi, procedere in uno dei modi seguenti:  
  
-   Aggiungere il nome dell'istanza di SQL Server come nome di rete valido. Uno dei metodi disponibili per impostare un nome di rete alternativo consiste nell'aggiungerlo al file hosts locale. Il file hosts locale è disponibile per impostazione predefinita nella cartella WINDOWS\system32\drivers\etc o WINNT\system32\drivers\etc. Per ulteriori informazioni, vedere la documentazione di Windows.  
  
     Ad esempio, se il nome del computer è comp1, l'indirizzo IP del computer è 10.193.17.129 e il nome dell'istanza è inst1/instname, aggiungere la voce seguente al file hosts:  
  
     10.193.17.129 inst1  
  
-   Disabilitare la distribuzione, registrare l'istanza e quindi riattivare la distribuzione. Se il valore di @@SERVERNAME non è corretto per un'istanza non cluster, eseguire questa procedura:  
  
    ```  
    sp_dropserver '<old_name>', 'droplogins'  
    go  
    sp_addserver '<new_name>', 'local'  
    go  
    ```  
  
     Dopo l'esecuzione della stored procedure [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md), è necessario riavviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per rendere effettiva la modifica apportata a @@SERVERNAME.  
  
     Se il valore di @@SERVERNAME non è corretto per un'istanza cluster, è necessario modificare il nome tramite Amministrazione cluster. Per altre informazioni, vedere [Istanze del cluster di failover Always On (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).  
  
 Dopo la verifica della corretta registrazione dell'istanza del server di distribuzione, verificare che il database di distribuzione sia elencato in **msdb..MSdistributiondbs**. In caso contrario:  
  
1.  Inserire nello script la configurazione di distribuzione. Per altre informazioni, vedere [Scripting Replication](../../relational-databases/replication/scripting-replication.md).  
  
2.  Disabilitare la distribuzione e quindi attivarla nuovamente. Per altre informazioni, vedere [Configure Distribution](../../relational-databases/replication/configure-distribution.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
