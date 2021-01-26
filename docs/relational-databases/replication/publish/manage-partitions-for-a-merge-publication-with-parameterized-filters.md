---
title: Gestire la partizione dei filtri con parametri (merge)
description: Gestire le partizioni con filtri con parametri per la replica di tipo merge di SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- partitions [SQL Server replication]
- merge replication partitions [SQL Server replication], SQL Server Management Studio
- parameterized filters [SQL Server replication], partition management
ms.assetid: fb5566fe-58c5-48f7-8464-814ea78e6221
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0b24e2e404dbea75d77cdfc8acdfa50189db2286
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765723"
---
# <a name="manage-partitions-for-a-merge-publication-with-parameterized-filters"></a>Gestione delle partizioni di una pubblicazione di tipo merge con filtri con parametri
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come gestire le partizioni per una pubblicazione di tipo merge con i filtri con parametri in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o Replication Management Objects (RMO). È possibile utilizzare i filtri di riga con parametri per generare partizioni non sovrapposte. È possibile limitare tali partizioni in modo che solo una sottoscrizione riceva una determinata partizione. In questi casi, la presenza di un numero elevato di Sottoscrittori comporta un numero elevato di partizioni, che richiedono anche un numero uguale di snapshot partizionati. Per altre informazioni sui filtri di riga con parametri, vedere [Filtri di riga con parametri](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Indicazioni](#Recommendations)  
  
-   **Per gestire le partizioni di una pubblicazione di tipo merge con filtri con parametri, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Se si crea, come consigliato, uno script per la topologia di replica, gli script di pubblicazione contengono le chiamate di stored procedure necessarie per creare le partizioni di dati. Lo script offre un riferimento per le partizioni create e un modo per ricreare, se necessario, una o più partizioni. Per altre informazioni, vedere [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
-   Se una pubblicazione contiene filtri con parametri che producono sottoscrizioni con partizioni non sovrapposte ed è necessario ricreare un'eventuale sottoscrizione persa, rimuovere la partizione sottoscritta, ricreare la sottoscrizione, quindi ricreare la partizione. Per altre informazioni sui filtri di riga con parametri, vedere [Filtri di riga con parametri](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md). La replica genera script di creazione per le partizioni del Sottoscrittore esistenti al momento della generazione di uno script per la creazione della pubblicazione. Per altre informazioni, vedere [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Gestire le partizioni nella pagina **Partizioni dati** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per ulteriori informazioni sull'accesso a questa finestra di dialogo, vedere [View and Modify Publication Properties](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md). In questa pagina è possibile creare ed eliminare partizioni, consentire ai Sottoscrittori di avviare la generazione e il recapito di snapshot, generare snapshot per una o più partizioni ed eliminare snapshot.  
  
#### <a name="to-create-a-partition"></a>Per creare una partizione  
  
1.  Nella pagina **Partizioni dati** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **Aggiungi**.  
  
2.  Nella finestra di dialogo **Aggiungi partizione dati** immettere un valore per **HOST_NAME()** e/o un valore **SUSER_SNAME()** associato alla partizione che si desidera creare.  
  
3.  Facoltativamente, specificare una pianificazione per l'aggiornamento degli snapshot:  
  
    1.  Selezionare **Usa la pianificazione seguente per l'esecuzione dell'agente snapshot per questa partizione**.  
  
    2.  Accettare la pianificazione predefinita per l'aggiornamento degli snapshot oppure fare clic su **Cambia** per specificare una pianificazione diversa.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

#### <a name="to-delete-a-partition"></a>Per eliminare una partizione  
  
1.  Nella pagina **Partizioni dati** selezionare una partizione della griglia.  
  
2.  Fare clic su **Elimina**.  
  
#### <a name="to-allow-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Per consentire ai Sottoscrittori di avviare la generazione e il recapito di snapshot  
  
1.  Nella pagina **Partizioni dati** selezionare **Definisci automaticamente una partizione e genera uno snapshot, se necessario, quando un nuovo Sottoscrittore cerca di eseguire la sincronizzazione**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-generate-a-snapshot-for-a-partition"></a>Per generare lo snapshot di una partizione  
  
1.  Nella pagina **Partizioni dati** selezionare una partizione della griglia.  
  
2.  Fare clic su **Genera gli snapshot selezionati adesso**.  
  
#### <a name="to-clean-up-a-snapshot-for-a-partition"></a>Per eliminare lo snapshot di una partizione  
  
1.  Nella pagina **Partizioni dati** selezionare una partizione della griglia.  
  
2.  Fare clic su **Elimina gli snapshot esistenti**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 Per migliorare la gestione di una pubblicazione con filtri con parametri, è possibile enumerare le partizioni esistenti a livello di programmazione, utilizzando stored procedure di replica. È inoltre possibile creare ed eliminare le partizioni esistenti. È possibile ottenere le informazioni seguenti sulle partizioni esistenti:  
  
-   Modalità in cui una partizione viene filtrata (tramite [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md) o [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md)).  
  
-   Nome del processo che genera uno snapshot partizionato.  
  
-   Ora dell'ultima esecuzione di un processo di snapshot partizionato.  
  
 Mentre la seconda parte dello snapshot a due parti può essere generata su richiesta quando viene inizializzata una nuova sottoscrizione, le procedure descritte di seguito consentono di controllare il modo in cui tale snapshot viene generato e di effettuare la pregenerazione dello snapshot nel momento più appropriato. Per altre informazioni, vedere [Snapshots for Merge Publications with Parameterized Filters](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
#### <a name="to-view-information-on-existing-partitions"></a>Per visualizzare informazioni sulle partizioni esistenti  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_helpmergepartition &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md). Specificare il nome della pubblicazione per `@publication`. (Facoltativo) Specificare `@suser_sname` o `@host_name` per ottenere esclusivamente informazioni basate su un solo criterio di filtro.  
  
#### <a name="to-define-a-new-partition-and-generate-a-new-partitioned-snapshot"></a>Per definire una nuova partizione e generare un nuovo snapshot partizionato  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergepartition &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md). Specificare il nome della pubblicazione per `@publication` e il valore con parametri che definisce la partizione per uno degli elementi seguenti:  
  
    -   `@suser_sname`: se il filtro con parametri è definito dal valore restituito da [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md).  
  
    -   `@host_name`: se il filtro con parametri è definito dal valore restituito da [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md).  
  
2.  Creare e inizializzare lo snapshot con parametri per la nuova partizione. Per altre informazioni, vedere [Creazione di uno snapshot per una pubblicazione di tipo merge con filtri con parametri](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
#### <a name="to-delete-a-partition"></a>Per eliminare una partizione  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_dropmergepartition &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropmergepartition-transact-sql.md). Specificare il nome della pubblicazione per `@publication` e il valore con parametri che definisce la partizione per uno degli elementi seguenti:  
  
    -   `@suser_sname`: se il filtro con parametri è definito dal valore restituito da [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md).  
  
    -   `@host_name`: se il filtro con parametri è definito dal valore restituito da [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md).  
  
     Viene inoltre effettuata la rimozione del processo di snapshot e degli eventuali file di snapshot per la partizione.  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 Per migliorare la gestione di una pubblicazione con filtri con parametri, è possibile creare nuove partizioni del Sottoscrittore a livello di programmazione, enumerare le partizioni del Sottoscrittore esistenti ed eliminare quelle desiderate utilizzando oggetti RMO (Replication Management Objects). Per informazioni sulla creazione di partizioni del Sottoscrittore, vedere [Creazione di uno snapshot per una pubblicazione di tipo merge con filtri con parametri](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md). È possibile ottenere le informazioni seguenti sulle partizioni esistenti:  
  
-   Valore e funzione di filtro su cui si basa la partizione.  
  
-   Nome del processo che genera uno snapshot con parametri per il Sottoscrittore.  
  
-   Ora dell'ultima esecuzione di un processo di snapshot con parametri.  
  
#### <a name="to-view-information-on-existing-partitions"></a>Per visualizzare informazioni sulle partizioni esistenti  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication>. Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione, quindi impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sull'oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> creato nel passaggio 1.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà della pubblicazione sono state definite in modo non corretto nel passaggio 2 oppure la pubblicazione non esiste.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergePartitions%2A> e passare il risultato a una matrice di oggetti <xref:Microsoft.SqlServer.Replication.MergePartition> .  
  
5.  Per ogni oggetto <xref:Microsoft.SqlServer.Replication.MergePartition> nella matrice, ottenere le proprietà desiderate.  
  
#### <a name="to-delete-existing-partitions"></a>Per eliminare partizioni esistenti  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication>. Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione, quindi impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sull'oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> creato nel passaggio 1.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà della pubblicazione sono state definite in modo non corretto nel passaggio 2 oppure la pubblicazione non esiste.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergePartitions%2A> e passare il risultato a una matrice di oggetti <xref:Microsoft.SqlServer.Replication.MergePartition> .  
  
5.  Per ogni oggetto <xref:Microsoft.SqlServer.Replication.MergePartition> nella matrice, determinare se la partizione deve essere eliminata. Questa decisione si basa in genere sul valore della proprietà <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterLogin%2A> o <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterHostName%2A> .  
  
6.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergePublication.RemoveMergePartition%2A> sull'oggetto <xref:Microsoft.SqlServer.Replication.MergePublication> indicato nel passaggio 2. Passare l'oggetto <xref:Microsoft.SqlServer.Replication.MergePartition> indicato nel passaggio 5.  
  
7.  Ripetere il passaggio 6 per ogni partizione eliminata.  
  
## <a name="see-also"></a>Vedere anche  
 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Snapshot per pubblicazioni di tipo merge con filtri con parametri](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  
  
  
  
