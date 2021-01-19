---
title: Configurare le transazioni distribuite per un gruppo di disponibilità
description: "Spiega come configurare le transazioni distribuite per i database all'interno di un gruppo di disponibilità Always On. "
ms.custom: seodec18
ms.date: 02/06/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5587622f0f61b7b7063b246d0599d46cc8c16f0c
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170783"
---
# <a name="configure-distributed-transactions-for-an-always-on-availability-group"></a>Configurare le transazioni distribuite per un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] supporta tutte le transazioni distribuite incluse nei database di un gruppo di disponibilità. Questo articolo illustra come configurare un gruppo di disponibilità per le transazioni distribuite.  

Per garantire le transazioni distribuite, il gruppo di disponibilità deve essere configurato in modo da registrare i database come strumenti di gestione delle risorse delle transazioni distribuite.  

>[!NOTE]
>[!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] Nel Service Pack 2 e nelle versioni successive è disponibile un supporto completo per le transazioni distribuite in gruppi di disponibilità. Nelle versioni di [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] precedenti a Service Pack 2 le transazioni distribuite tra database (ovvero le transazioni che usano database nella stessa istanza di SQL Server) che coinvolgono un database in un gruppo di disponibilità non sono supportate. [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] non presenta questa limitazione. 
>
>In [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] i passaggi di configurazione sono uguali a quelli di [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)].

In una transazione distribuita, le applicazioni client funzionano con Microsoft Distributed Transaction Coordinator (MS DTC o DTC) per garantire la coerenza transazionale tra più origini dati. DTC è un servizio disponibile nei sistemi operativi basati su Windows Server supportati. Per una transazione distribuita, DTC è il *coordinatore di transazioni*. In genere, un'istanza di SQL Server è lo *strumento di gestione delle risorse*. Quando un database è in un gruppo di disponibilità, ogni database deve essere il proprio strumento di gestione delle risorse. 

[!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] non impedisce le transazioni distribuite per i database in un gruppo di disponibilità, anche quando il gruppo di disponibilità non è configurato per le transazioni distribuite. Tuttavia quando un gruppo di disponibilità non è configurato per le transazioni distribuite, in alcuni casi il failover potrebbe non riuscire. In particolare, l'istanza di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] della nuova replica primaria potrebbe non essere in grado di ottenere il risultato della transazione da DTC. Per consentire all'istanza di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] di ottenere il risultato delle transazioni in dubbio da DTC dopo il failover, configurare il gruppo di disponibilità per le transazioni distribuite. 

DTC non è coinvolto nell'elaborazione del gruppo di disponibilità a meno che un database non sia anche membro di un cluster di failover. All'interno di un gruppo di disponibilità, la coerenza tra le repliche viene gestita dalla logica del gruppo di disponibilità: Il database primario non completa il commit né conferma il commit al chiamante fino a quando il database secondario non ha riconosciuto che i record del log sono stati salvati in modo permanente in un archivio durevole. Solo a questo punto il database primario dichiara il completamento della transazione. In modalità asincrona, non è prevista l'attesa del riconoscimento da parte della replica secondaria ed esiste esplicitamente la possibilità che una piccola quantità di dati vada persa.

## <a name="prerequisites"></a>Prerequisiti

Prima di configurare un gruppo di disponibilità per supportare le transazioni distribuite, è necessario soddisfare i prerequisiti seguenti:

* La versione di tutte le istanze di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] che fanno parte della transazione distribuita devono essere [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] o versioni successive.

* Gruppi di disponibilità deve essere in esecuzione in Windows Server 2012 R2 o Windows Server 2016. Per Windows Server 2012 R2, è necessario installare l'aggiornamento in KB3090973 disponibile all'indirizzo [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  

## <a name="create-an-availability-group-for-distributed-transactions"></a>Creare un gruppo di disponibilità per le transazioni distribuite

Configurare un gruppo di disponibilità per supportare le transazioni distribuite. Impostare il gruppo di disponibilità per consentire a ogni database di essere registrato come strumento di gestione delle risorse. Questo articolo illustra come configurare un gruppo di disponibilità in modo che ogni database possa essere uno strumento di gestione delle risorse in DTC.



È possibile creare un gruppo di disponibilità per le transazioni distribuite in [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] o versioni successive. Per creare un gruppo di disponibilità per le transazioni distribuite, includere `DTC_SUPPORT = PER_DB` nella definizione del gruppo di disponibilità. Lo script seguente crea un gruppo di disponibilità per le transazioni distribuite. 

```sql
CREATE AVAILABILITY GROUP MyAG
   WITH (
      DTC_SUPPORT = PER_DB  
      )
   FOR DATABASE DB1, DB2
   REPLICA ON
      'Server1' WITH (
         ENDPOINT_URL = 'TCP://SERVER1.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         ),
      'Server2' WITH (
         ENDPOINT_URL = 'TCP://SERVER2.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         )
```

>[!NOTE]
>Lo script precedente è un esempio semplice di un gruppo di disponibilità e non è progettato per un ambiente di produzione specifico. 

## <a name="alter-an-availability-group-for-distributed-transactions"></a>Modificare un gruppo di disponibilità per le transazioni distribuite

È possibile modificare un gruppo di disponibilità per le transazioni distribuite in [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] o versioni successive. Per modificare un gruppo di disponibilità per le transazioni distribuite includere `DTC_SUPPORT = PER_DB` nello script `ALTER AVAILABILITY GROUP`. Lo script di esempio modifica il gruppo di disponibilità per supportare le transazioni distribuite. 

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = PER_DB  
      );
```

>[!NOTE]
>A partire da [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] Service Pack 2 è possibile modificare un gruppo di disponibilità per le transazioni distribuite. Per le versioni di [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] precedenti al Service Pack 2 è necessario eliminare e ricreare il gruppo di disponibilità con l'impostazione `DTC_SUPPORT = PER_DB`. 

Per disabilitare le transazioni distribuite, usare il comando Transact-SQL seguente:

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = NONE  
      );
```

## <a name="distributed-transactions---technical-concepts"></a><a name="distTran"/>Transazioni distribuite - concetti tecnici

Una transazione distribuita si estende su due o più database. DTC in qualità di strumento di gestione transazioni coordina la transazione tra le istanze di SQL Server e altre origini dati. Ogni istanza del motore di database di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] può operare come strumento di gestione delle risorse. Quando un gruppo di disponibilità è configurato con `DTC_SUPPORT = PER_DB`, i database possono operare come strumenti di gestione delle risorse. Per ulteriori informazioni, vedere la documentazione di MS DTC.

Una transazione con due o più database in una singola istanza del motore di database è in effetti una transazione distribuita. Nell'istanza le transazioni distribuite vengono gestite internamente e appaiono all'utente come transazioni locali. [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] alza di livello tutte le transazioni tra database a DTC quando i database si trovano in un gruppo di disponibilità configurato con `DTC_SUPPORT = PER_DB`, anche all'interno di una singola istanza di SQL Server. 

A livello dell'applicazione le transazioni distribuite vengono gestite in modo simile alle transazioni locali. Al termine della transazione l'applicazione ne richiede il commit o il rollback. Il commit di transazioni distribuite deve essere gestito dal gestore delle transazioni in modo diverso per evitare che, in seguito a un errore della rete, alcuni strumenti di gestione delle risorse eseguano il commit correttamente mentre altri eseguano il rollback della transazione. A tale scopo il processo di commit viene gestito in due fasi, ovvero una fase preparatoria e la fase di commit effettivo. Questo tipo di commit è noto come protocollo 2PC.

- **Fase preparatoria**
   
   Quando il gestore delle transazioni riceve una richiesta di commit, invia un comando di preparazione a tutti gli strumenti di gestione delle risorse coinvolti nella transazione. Ogni strumento esegue quindi tutte le operazioni necessarie per rendere la transazione durevole. Tutti i buffer contenenti immagini del log relative alla transazione vengono inoltre trasferiti su disco. Dopo avere completato la fase preparatoria, lo strumento di gestione delle risorse comunica al gestore delle transazioni l'esito, positivo o negativo, della preparazione.

- **Fase di commit**
   
   Se la fase preparatoria ha esito positivo in tutti gli strumenti di gestione delle risorse, il gestore delle transazioni invia comandi di commit agli strumenti di gestione, i quali possono quindi completare il commit. Se il commit viene eseguito correttamente da parte di tutti gli strumenti di gestione delle risorse, il gestore delle transazioni invia all'applicazione una notifica di operazione riuscita. Se viene segnalato un tentativo di preparazione non riuscito in uno strumento di gestione delle risorse, il gestore delle transazioni invia un comando di rollback a tutti gli strumenti di gestione e segnala all'applicazione che il commit ha avuto esito negativo.

### <a name="detailed-steps"></a>Procedura dettagliata

L'elenco seguente illustra il funzionamento dell'applicazione con DTC per completare le transazioni distribuite.

1. L'istanza di SQL Server viene integrata nella transazione DTC. Questa situazione può verificarsi quando nella transazione ci sono più strumenti di gestione delle risorse o se il client richiede che una transazione venga alzata di livello a transazione DTC.
2. Il client esegue alcune operazioni nell'istanza di SQL Server nella transazione DTC.
3. Il client esegue le operazioni di commit e interruzione per la transazione DTC.
    - Se il client esegue l'operazione di interruzione, la transazione viene interrotta immediatamente.
    - Se il client esegue l'operazione di commit, DTC avvia il protocollo 2PC chiedendo a tutti gli strumenti di gestione delle risorse nella transazione di preparare la transazione.
4. DTC segnala a tutti gli strumenti di gestione delle risorse di eseguire il commit della transazione dopo la conferma della corretta esecuzione della fase preparatoria. Se non si verificano problemi che impediscono la conferma della corretta esecuzione, DTC interrompe la transazione. 

### <a name="effects-of-configuring-an-availability-group-for-distributed-transactions"></a>Effetti della configurazione di un gruppo di disponibilità per le transazioni distribuite

Ogni entità che fa parte di una transazione distribuita viene chiamata strumento di gestione delle risorse. Esempi di strumenti di gestione delle risorse includono:

* Istanza di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]. 
* Database in un gruppo di disponibilità che è stato configurato per le transazioni distribuite.
* Servizio DTC, può essere anche uno strumento di gestione transazioni.
* Altre origini dati. 

Per partecipare alle transazioni distribuite, un'istanza di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] viene integrata con un controllo DTC. In genere, l'istanza di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] viene integrata con il controllo DTC nel server locale. Ogni istanza di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] crea uno strumento di gestione delle risorse con un identificatore di gestione delle risorse univoco (RMID) e lo registra con DTC. Nella configurazione predefinita tutti i database in un'istanza di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] usano lo stesso RMID. 

Quando un database è in un gruppo di disponibilità, la copia di lettura e scrittura del database, o replica primaria, può essere spostata in un'istanza diversa di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]. Per supportare le transazioni distribuite durante questo spostamento, ogni database deve agire da strumento di gestione delle risorse separato e deve avere un RMID univoco. Quando un gruppo di disponibilità ha `DTC_SUPPORT = PER_DB`, [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] crea uno strumento di gestione delle risorse per ogni database e lo registra con DTC usando un RMID univoco. In questa configurazione il database è uno strumento di gestione delle risorse per le transazioni DTC.

Per altre informazioni sulle transazioni distribuite in [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)], vedere [Transazioni distribuite](#distTran).

## <a name="manage-unresolved-transactions"></a>Gestire le transazioni non risolte

Il risultato delle transazioni attive presente durante la modifica del RMID non può essere recuperato dopo un failover perché l'RMID usato da SQL Server per l'integrazione e l'RMID usato da SQL Server per il recupero sono diversi. La modifica del RMID può verificarsi nei casi seguenti:

* Modifica di `DTC_SUPPORT` per un gruppo di disponibilità. 
* Aggiunta o rimozione di un database da un gruppo di disponibilità. 
* Eliminazione di un gruppo di disponibilità.

Nei casi precedenti, se la replica primaria effettua il failover in una nuova istanza di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)], l'istanza prova a contattare il controllo DTC per identificare il risultato della transazione. DTC non può restituire il risultato perché l'RMID usato dal database per ottenere il risultato delle transazioni in dubbio durante il ripristino non è stato integrato in precedenza. Pertanto, lo stato del database diventa SUSPECT.

Il nuovo log degli errori di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] conterrà una voce simile a quella dell'esempio seguente:

```
Microsoft Distributed Transaction Coordinator (MS DTC) 
failed to reenlist citing that the database RMID does 
not match the RMID [xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx] 
associated with the transaction.  Please manually resolve
the transaction.
    
SQL Server detected a DTC/KTM in-doubt transaction with UOW 
{yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy}.Please resolve it 
following the guideline for Troubleshooting DTC Transactions.
```

L'esempio precedente mostra che il controllo DTC non è riuscito a reintegrare il database dalla nuova replica primaria nella transazione creata dopo il failover. L'istanza di [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] non può determinare il risultato della transazione distribuita, pertanto contrassegna il database come sospetto. La transazione viene contrassegnata come un'unità di lavoro (UOW) a cui fa riferimento un GUID. Per ripristinare il database, eseguire manualmente il commit o il rollback della transazione. 

>[!WARNING]
>Quando si esegue manualmente il commit o il rollback di una transazione, tale operazione può influire su un'applicazione. Verificare che l'azione di commit o rollback sia coerente con i requisiti dell'applicazione. 

Eseguire solo uno degli script seguenti:

   * Per eseguire il commit della transazione, aggiornare ed eseguire lo script seguente: sostituire `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` con l'unità di lavoro della transazione in dubbio del messaggio di errore precedente ed eseguire:

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH COMMIT
   ```

   * Per eseguire il rollback della transazione, aggiornare ed eseguire lo script seguente: sostituire `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` con l'unità di lavoro della transazione in dubbio del messaggio di errore precedente ed eseguire:

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH ROLLBACK
   ```

Dopo il commit o il rollback della transazione, è possibile usare `ALTER DATABASE` per impostare il database online. Aggiornare ed eseguire lo script seguente: impostare il nome del database per il nome del database sospetto:

   ```sql
   ALTER DATABASE [DB1] SET ONLINE
   ```

Per altre informazioni sulla risoluzione delle transazioni in dubbio, vedere [Risolvere le transazioni manualmente](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754134(v=ws.10)).

## <a name="next-steps"></a>Passaggi successivi  

[Transazioni distribuite](/dotnet/framework/data/adonet/distributed-transactions)

[Gruppi di disponibilità Always On: Interoperabilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
[Transactions - Always On Availability Groups and Database Mirroring](transactions-always-on-availability-and-database-mirroring.md) (Transazioni: gruppi di disponibilità Always On e mirroring del database)  

[Supporting XA Transactions](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753563(v=ws.10)) (Supporto delle transazioni XA)

[How It Works: Session/SPID (-2) for DTC Transactions](/archive/blogs/bobsql/how-it-works-sessionspid-2-for-dtc-transactions) (Come funziona: sessione/SPID (-2) per transazioni DTC)