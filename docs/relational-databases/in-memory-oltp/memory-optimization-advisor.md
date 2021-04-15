---
title: Ottimizzazione guidata per la memoria | Microsoft Docs
description: Usare l'ottimizzazione guidata per la memoria in SQL Server Management Studio per facilitare la migrazione delle tabelle basate su disco in tabelle ottimizzate per la memoria.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
f1_keywords:
- swb.memoryoptimizationwizard.f1
- sql13.swb.memoryoptimizationwizard.f1
ms.assetid: 181989c2-9636-415a-bd1d-d304fc920b8a
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a4e86c54d339357b7728081f0b7548c875f5bf18
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492539"
---
# <a name="memory-optimization-advisor"></a>Ottimizzazione guidata per la memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  I report di analisi delle prestazioni delle transazioni (vedere [Determinare se una tabella o una stored procedure deve essere trasferita a OLTP in memoria](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)) indicano quali tabelle del database trarranno vantaggio se trasferite per usare OLTP in memoria. Dopo aver identificato la tabella da trasferire per usare OLTP in memoria, è possibile usare l'Ottimizzazione guidata per la memoria per eseguire la migrazione della tabella basata su disco in una tabella ottimizzata per la memoria.  
  
 L'Ottimizzazione guidata per la memoria consente di:  
  
-   Identificare le caratteristiche usate in una tabella basata su disco che non sono supportate per le tabelle ottimizzate per la memoria.  
  
-   Eseguire la migrazione di una tabella e dei dati a una tabella ottimizzata per la memoria (in assenza di funzionalità non supportate).  
    
 Per informazioni sulle metodologie di migrazione, vedere [In-Memory OLTP - Common Workload Patterns and Migration Considerations](/previous-versions/dn673538(v=msdn.10)) (OLTP in memoria: considerazioni sulla migrazione e sui modelli di carico di lavoro comuni).  
  
## <a name="walkthrough-using-the-memory-optimization-advisor"></a>Procedura dettagliata per l'utilizzo di Ottimizzazione guidata per la memoria  
 In **Esplora oggetti** fare clic con il pulsante destro del mouse sulla tabella che si vuole convertire, quindi selezionare **Ottimizzazione guidata per la memoria**. Verrà visualizzata la pagina introduttiva per **Ottimizzazione guidata per la memoria della tabella**.  
  
### <a name="memory-optimization-checklist"></a>Elenco di controllo relativo all'ottimizzazione per la memoria  
 Quando si fa clic su **Avanti** nella pagina introduttiva di **Ottimizzazione guidata per la memoria della tabella**, viene visualizzato l'elenco di controllo relativo all'ottimizzazione per la memoria. Le tabelle con ottimizzazione per la memoria non supportano tutte le funzionalità di una tabella basata su disco. Nell'elenco di controllo relativo all'ottimizzazione per la memoria viene indicato se la tabella basata su disco utilizza qualche funzionalità che non è compatibile con un tabella ottimizzata per la memoria. Poiché l' **Ottimizzazione guidata per la memoria della tabella** non modifica la tabella basata su disco, è possibile effettuare la migrazione della tabella per usare OLTP in memoria. È necessario effettuare queste modifiche manualmente prima di continuare con la migrazione. Per ogni incompatibilità riscontrata, in **Ottimizzazione guidata per la memoria della tabella** viene visualizzato un collegamento a informazioni utili per la modifica delle tabelle basate su disco.  
  
 Se si desidera conservare l'elenco delle incompatibilità, per pianificare la migrazione, fare clic su **Genera report** per generare un elenco HTML.  
  
 Se la tabella non presenta incompatibilità e si è connessi a un'istanza di [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] con OLTP in memoria, fare clic su **Avanti**.  
  
### <a name="memory-optimization-warnings"></a>Avvisi sull'ottimizzazione per la memoria  
 Nella pagina successiva, quella degli avvisi relativi all'ottimizzazione per la memoria, è mostrato un elenco di problemi che non impediscono la migrazione della tabella per l'uso di OLTP in memoria, ma che influiscono sul comportamento di altri oggetti (come le stored procedure o le funzioni CLR) generando errori o comportamenti inaspettati.  
  
 I primi avvisi nell'elenco hanno scopo informativo e possono o meno valere per la tabella. I collegamenti nella colonna di destra della tabella permettono di accedere a ulteriori informazioni.  
  
 Nella tabella degli avvisi vengono inoltre mostrate alcune potenziali condizioni di avviso che non sono presenti nella tabella.  
  
 Gli avvisi correggibili saranno segnalati da un triangolo giallo nella colonna di sinistra. In presenza di avvisi correggibili, si consiglia di uscire dal processo di migrazione, risolvere gli avvisi e riavviare il processo. Se non si risolvono questi avvisi, la tabella potrebbe provocare errori dopo la migrazione.  
  
 Per generare un report HTML di questi avvisi, fare clic su **Genera report** . Fare clic su **Avanti** per continuare.  
  
### <a name="review-optimization-options"></a>Verifica opzioni di ottimizzazione  
 Nella schermata successiva è possibile modificare le opzioni di migrazione a OLTP in memoria:  
  
 Filegroup con ottimizzazione per la memoria  
 Nome per il filegroup ottimizzato per la memoria. Un database deve avere un filegroup ottimizzato per la memoria con almeno un file per poter creare una tabella ottimizzata per la memoria.  
  
 Se non si dispone di un filegroup ottimizzato per la memoria, è possibile modificare il nome predefinito. I filegroup con ottimizzazione per la memoria non possono essere eliminati. La presenza di un filegroup ottimizzato per la memoria può disabilitare alcune funzionalità a livello di database come AUTO CLOSE e il mirroring del database.  
  
 Se il database dispone già di un filegroup ottimizzato per la memoria, questo campo sarà già popolato con il relativo nome e non sarà possibile modificarlo.  
  
 Nome file logico e Percorso file  
 Nome del file che conterrà la tabella ottimizzata per la memoria. Un database deve avere un filegroup ottimizzato per la memoria con almeno un file per poter creare una tabella ottimizzata per la memoria.  
  
 In assenza di un filegroup ottimizzato per la memoria, è possibile modificare il nome predefinito e il percorso del file che sarà creato alla fine del processo di migrazione.  
  
 In presenza di un filegroup ottimizzato per la memoria, questi campi saranno già popolati e non sarà possibile modificarli.  
  
 Rinomina la tabella originale come  
 Al termine del processo di migrazione, verrà creata una nuova tabella ottimizzata per la memoria con il nome corrente della tabella. Per evitare un conflitto di nomi, la tabella corrente deve essere rinominata. Il nome può essere modificato in questo campo.  
  
 Costo memoria corrente stimato (MB)  
 In Ottimizzazione guidata per la memoria viene stimata automaticamente la quantità di memoria che sarà utilizzata dalla nuova tabella ottimizzata per la memoria in base ai metadati della tabella basata su disco. Il calcolo delle dimensioni della tabella è descritto in [Dimensioni di tabelle e righe per le tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md).  
  
 Se non è allocata una quantità di memoria sufficiente, il processo di migrazione potrebbe non riuscire.  
  
 Copia anche i dati della tabella nella nuova tabella con ottimizzazione per la memoria  
 Selezionare questa opzione se si desidera spostare anche i dati della tabella corrente nella tabella ottimizzata per la memoria. Se questa opzione non viene selezionata, la nuova tabella ottimizzata per la memoria non conterrà righe.  
  
 Per impostazione predefinita, verrà eseguita la migrazione della tabella come tabella durevole.  
 OLTP in memoria supporta le tabelle non durevoli con prestazioni superiori rispetto alle tabelle ottimizzate per la memoria durevoli. Tuttavia, i dati in una tabella non durevole verranno persi al riavvio del server.  
  
 Se questa opzione è selezionata, in Ottimizzazione guidata per la memoria verrà creata una tabella non durevole anziché una durevole.  
  
> [!WARNING]  
>  Selezionare questa opzione solo se si comprende il rischio della perdita dei dati con le tabelle non durevoli.  
  
 Fare clic su **Avanti** per continuare.  
  
### <a name="review-primary-key-conversion"></a>Verifica conversione chiave primaria  
 La schermata successiva è denominata **Verifica conversione chiave primaria**. In Ottimizzazione guidata per la memoria viene rilevato se nella tabella sono presenti una o più chiavi primarie e viene popolato l'elenco di colonne in base ai metadati delle chiavi primarie. Se non è presente alcuna chiave primaria, per eseguire la migrazione a una tabella ottimizzata per la memoria durevole, sarà necessario crearne una.  
  
 Se non esiste una chiave primaria e viene eseguita la migrazione della tabella in una tabella non durevole, questa schermata non verrà visualizzata.  
  
 Per le colonne testuali (le colonne con i tipi **char**, **nchar**, **varchar** e **nvarchar**) è necessario selezionare le regole di confronto appropriate. OLTP in memoria supporta solo le regole di confronto BIN2 per le colonne in una tabella ottimizzata per la memoria e non supporta le regole di confronto con i caratteri supplementari. Per informazioni sulle regole di confronto supportate e sull'impatto potenziale di una modifica a una regola di confronto, vedere [Collations and Code Pages](./introduction-to-memory-optimized-tables.md) .  
  
 È possibile configurare i seguenti parametri per la chiave primaria:  
  
 Selezionare un nuovo nome per la chiave primaria  
 Il nome della chiave primaria per questa tabella deve essere univoco all'interno del database. È possibile modificare il nome della chiave primaria qui.  
  
 Selezionare il tipo di chiave primaria  
 OLTP in memoria supporta due tipi di indici in una tabella ottimizzata per la memoria:  
  
-   L'indice HASH NON CLUSTER. Questo indice è ideale per gli indici con ricerche in molti punti. È possibile configurare il conteggio dei bucket per questo indice nel campo **Numero di bucket** .  
  
-   L'indice NON CLUSTER. Questo tipo di indice è ideale per gli indici con molte query di intervallo. È possibile configurare l'ordinamento per ciascuna colonna nell'elenco **Colonna di ordinamento e ordine** .  
  
 Per individuare il tipo di indice più adatto alla chiave primaria, vedere [Indici hash](/previous-versions/sql/sql-server-2016/dn133190(v=sql.130)).  
  
 Una volta selezionate le opzioni per la chiave primaria, fare clic su **Avanti** .  
  
### <a name="review-index-conversion"></a>Verifica conversione indice  
 La pagina successiva è denominata **Verifica conversione indice**. In Ottimizzazione guidata per la memoria viene rilevato se nella tabella sono presenti uno o più indici e vengono popolati l'elenco di colonne e il tipo di dati. I parametri che è possibile configurare nella pagina **Verifica conversione indice** sono simili a quelli della pagina **Verifica conversione chiave primaria** .  
  
 Se la tabella contiene solo una chiave primaria e viene sottoposta a migrazione in una tabella durevole, questa schermata non verrà visualizzata.  
  
 Una volta impostate le opzioni per ciascun indice nella tabella, fare clic su **Avanti**.  
  
### <a name="verify-migration-actions"></a>Verifica azioni di migrazione  
 La pagina successiva è denominata **Verifica azioni di migrazione**. Per generare lo script dell'operazione di migrazione, fare clic su **Script** per generare uno script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Sarà quindi possibile modificare ed eseguire lo script. Fare clic su **Esegui migrazione** per avviare la migrazione della tabella.  
  
 Al termine, aggiornare **Esplora oggetti** per visualizzare la nuova tabella ottimizzata per la memoria e la vecchia tabella basata su disco. La vecchia tabella può essere conservata o eliminata a seconda delle proprie esigenze.  
  
## <a name="see-also"></a>Vedere anche  
 [Migrazione a OLTP in memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
