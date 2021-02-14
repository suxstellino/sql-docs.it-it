---
title: Modelli di distribuzione dei gruppi di disponibilità - SQL Server in Linux
description: Informazioni sulle configurazioni di distribuzione supportate per i gruppi di disponibilità Always On di SQL Server su server Linux.
ms.custom: seo-lt-2019
ms.date: 04/17/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
ms.assetid: edd75f68-dc62-4479-a596-57ce8ad632e5
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: edeb7ab3de19b50f78f2b4b5bab9c0f8afe80780
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347779"
---
# <a name="high-availability-and-data-protection-for-availability-group-configurations"></a>Disponibilità elevata e protezione dei dati per le configurazioni dei gruppi di disponibilità

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo presenta le configurazioni di distribuzione supportate per i gruppi di disponibilità Always On di SQL Server sui server Linux. Un gruppo di disponibilità offre il supporto per disponibilità elevata e protezione dei dati. Il rilevamento automatico degli errori, il failover automatico e la riconnessione trasparente dopo il failover assicurano una disponibilità elevata. Le repliche sincronizzate garantiscono la protezione dei dati. 

In un cluster WSFC (Windows Server Failover Cluster), una configurazione comune per la disponibilità elevata usa due repliche sincrone e un terzo server o una condivisione file per il quorum. Il server di controllo della condivisione file convalida la configurazione del gruppo di disponibilità, ad esempio lo stato della sincronizzazione e il ruolo della replica. Questa configurazione garantisce che la replica secondaria selezionata come destinazione del failover sia aggiornata con le modifiche più recenti ai dati e alla configurazione del gruppo di disponibilità. 

Il cluster WSFC sincronizza i metadati di configurazione per l'arbitraggio del failover tra le repliche del gruppo di disponibilità e il server di controllo della condivisione file. Quando un gruppo di disponibilità non si trova in un cluster WSFC, le istanze di SQL Server archiviano i metadati di configurazione nel database master.

Ad esempio, un gruppo di disponibilità in un cluster Linux ha l'impostazione `CLUSTER_TYPE = EXTERNAL`. Non è disponibile alcun cluster WSFC per l'arbitraggio del failover. In questo caso, i metadati di configurazione vengono gestiti dalle istanze di SQL Server. Poiché in questo cluster non è presente alcun server di controllo, per archiviare i metadati dello stato di configurazione è necessaria una terza istanza di SQL Server. Tutte e tre le istanze di SQL Server forniscono insieme l'archiviazione dei metadati distribuita per il cluster. 

Il modulo di gestione cluster può eseguire una query sulle istanze di SQL Server nel gruppo di disponibilità e orchestrare il failover per mantenere la disponibilità elevata. In un cluster Linux, il modulo di gestione cluster è Pacemaker. 

SQL Server 2017 CU 1 consente la disponibilità elevata per un gruppo di disponibilità con `CLUSTER_TYPE = EXTERNAL` per due repliche sincrone e una replica di sola configurazione. La replica di sola configurazione può essere ospitata in qualsiasi edizione di SQL Server 2017 CU 1 o di una versione successiva, inclusa l'edizione SQL Server Express. La replica di sola configurazione gestisce le informazioni di configurazione relative al gruppo di disponibilità nel database master, ma non contiene i database utente nel gruppo di disponibilità. 

## <a name="how-the-configuration-affects-default-resource-settings"></a>Effetti della configurazione sulle impostazioni predefinite delle risorse

In SQL Server 2017 è stata introdotta l'impostazione `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` per le risorse cluster. Questa impostazione consente di assicurarsi che nel numero specificato di repliche secondarie i dati delle transazioni vengano scritti nel log prima che la replica primaria esegua il commit di ogni transazione. Quando si usa un modulo di gestione cluster esterno, questa impostazione ha effetto sia sulla disponibilità elevata che sulla protezione dei dati. Il valore predefinito per l'impostazione dipende dall'architettura nel momento in cui viene creata la risorsa cluster. Quando si installa l'agente delle risorse SQL Server, `mssql-server-ha`, e si crea una risorsa cluster per il gruppo di disponibilità, il modulo di gestione cluster rileva la configurazione del gruppo di disponibilità e imposta `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` di conseguenza. 

Se supportato dalla configurazione, il parametro `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` dell'agente delle risorse è impostato sul valore che fornisce disponibilità elevata e protezione dei dati. Per altre informazioni, vedere [Informazioni sull'agente delle risorse SQL Server per Pacemaker](#pacemakerNotify).

Le sezioni seguenti illustrano il comportamento predefinito per la risorsa cluster. 

Scegliere un modello di progettazione del gruppo di disponibilità per soddisfare specifici requisiti aziendali in termini di disponibilità elevata, protezione dei dati e scalabilità in lettura.

Le configurazioni seguenti descrivono i modelli di progettazione del gruppo di disponibilità e le funzionalità di ogni modello. Questi modelli di progettazione si applicano ai gruppi di disponibilità con `CLUSTER_TYPE = EXTERNAL` per le soluzioni a disponibilità elevata. 

- **Tre repliche sincrone**
- **Due repliche sincrone**
- **Due repliche sincrone e una replica di sola configurazione**

<a name="threeSynch"></a>

## <a name="three-synchronous-replicas"></a>Tre repliche sincrone

Questa configurazione è costituita da tre repliche sincrone. Per impostazione predefinita, fornisce disponibilità elevata e protezione dei dati. Può anche fornire scalabilità in lettura.

![Tre repliche][3]

Un gruppo di disponibilità con tre repliche sincrone può fornire scalabilità in lettura, disponibilità elevata e protezione dei dati. La tabella seguente descrive il comportamento della disponibilità. 

|Comportamento della disponibilità |Scalabilità in lettura|Disponibilità elevata e </br> protezione dei dati | Protezione dei dati|
|:---|---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 |1<sup>\*</sup>|2|
|Interruzione primaria |Failover automatico. La nuova replica primaria è L/S. |Failover automatico. La nuova replica primaria è L/S. |Failover automatico. La nuova replica primaria non è disponibile per le transazioni utente finché la replica primaria precedente non viene ripristinata e aggiunta al gruppo di disponibilità come replica secondaria. |
|Interruzione di una replica secondaria  | La replica primaria è L/S. Nessun failover automatico in caso di errore della replica primaria. |La replica primaria è L/S. Nessun failover automatico se si verifica un errore anche sulla replica primaria. | La replica primaria non è disponibile per le transazioni utente. |

<sup>\*</sup> Impostazione predefinita

<a name="twoSynch"></a>

## <a name="two-synchronous-replicas"></a>Due repliche sincrone

Questa configurazione consente la protezione dei dati. Come per le altre configurazioni del gruppo di disponibilità, è possibile abilitare la scalabilità in lettura. La configurazione con due repliche sincrone non fornisce disponibilità elevata automatica. Una configurazione di questo tipo è applicabile solo a SQL Server 2017 RTM e non è più supportata con le versioni successive (CU1 e superiori) di SQL Server 2017.

![Due repliche sincrone][1]

Un gruppo di disponibilità con due repliche sincrone fornisce scalabilità in lettura e protezione dei dati. La tabella seguente descrive il comportamento della disponibilità. 

|Comportamento della disponibilità |Scalabilità in lettura |Protezione dei dati|
|:---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>\*</sup>|1|
|Interruzione primaria | Failover manuale. Potrebbe verificarsi la perdita di dati. La nuova replica primaria è L/S.| Failover automatico. La nuova replica primaria non è disponibile per le transazioni utente finché la replica primaria precedente non viene ripristinata e aggiunta al gruppo di disponibilità come replica secondaria.|
|Interruzione di una replica secondaria  |La replica primaria è L/S, con esecuzione esposta alla perdita di dati. |La replica primaria non è disponibile per le transazioni utente finché la replica secondaria non viene ripristinata.|

<sup>\*</sup> Impostazione predefinita

<a name = "configOnly"></a>

## <a name="two-synchronous-replicas-and-a-configuration-only-replica"></a>Due repliche sincrone e una replica di sola configurazione

Un gruppo di disponibilità con due (o più) repliche sincrone e una replica di sola configurazione fornisce protezione dei dati e può anche offrire disponibilità elevata. Nel diagramma seguente è illustrata questa architettura:

![Gruppo di disponibilità di sola configurazione][2]

1. Replica sincrona dei dati utente nella replica secondaria. Sono inclusi anche i metadati di configurazione del gruppo di disponibilità.
2. Replica sincrona dei metadati di configurazione del gruppo di disponibilità. Non sono inclusi i dati utente.

Nel diagramma del gruppo di disponibilità, una replica primaria inserisce tramite push i dati di configurazione sia nella replica secondaria sia in quella di sola configurazione. La replica secondaria riceve anche i dati utente. La replica di sola configurazione non riceve i dati utente. La replica secondaria è in modalità di disponibilità sincrona. La replica di sola configurazione non contiene i database del gruppo di disponibilità, ma solo i metadati relativi al gruppo di disponibilità. I dati di configurazione nella replica di sola configurazione vengono sottoposti a commit in modo sincrono.

> [!NOTE]
> Un gruppo di disponibilità con replica di sola configurazione rappresenta una novità per SQL Server 2017 CU 1. In tutte le istanze di SQL Server nel gruppo di disponibilità deve essere installato SQL Server 2017 CU 1 o versione successiva. 

Il valore predefinito per `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` è 0. La tabella seguente descrive il comportamento della disponibilità. 

|Comportamento della disponibilità |Disponibilità elevata e </br> protezione dei dati | Protezione dei dati|
|:---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>\*</sup>|1|
|Interruzione primaria | Failover automatico. La nuova replica primaria è L/S. | Failover automatico. La nuova replica primaria non è disponibile per le transazioni utente. |
|Interruzione di una replica secondaria | La replica primaria è L/S, con esecuzione esposta alla perdita di dati (se si verifica un errore sulla replica primaria e questa non può essere ripristinata). Nessun failover automatico se si verifica un errore anche sulla replica primaria. | La replica primaria non è disponibile per le transazioni utente. Nessuna replica in cui eseguire il failover se si verifica un errore anche sulla replica primaria. |
|Interruzione della replica di sola configurazione | La replica primaria è L/S. Nessun failover automatico se si verifica un errore anche sulla replica primaria. | La replica primaria è L/S. Nessun failover automatico se si verifica un errore anche sulla replica primaria. |
|Interruzione di replica secondaria sincrona + replica di sola configurazione| La replica primaria non è disponibile per le transazioni utente. Nessun failover automatico. | La replica primaria non è disponibile per le transazioni utente. Nessuna replica in cui eseguire il failover se si verifica un errore anche sulla replica primaria. |

<sup>\*</sup> Impostazione predefinita

> [!NOTE]
> L'istanza di SQL Server che ospita la replica di sola configurazione può ospitare anche altri database. Può anche partecipare come database di sola configurazione per più gruppi di disponibilità. 

## <a name="requirements"></a>Requisiti

- In tutte le repliche presenti in un gruppo di disponibilità con una replica di sola configurazione deve essere installato SQL Server 2017 CU 1 o versione successiva.
- Una replica di sola configurazione può essere ospitata in qualsiasi edizione di SQL Server, inclusa l'edizione SQL Server Express. 
- Il gruppo di disponibilità richiede almeno una replica secondaria, oltre a quella primaria.
- Le repliche di sola configurazione non vengono incluse nel conteggio del numero massimo di repliche per ogni istanza di SQL Server. In SQL Server Standard Edition sono consentite al massimo tre repliche, mentre in SQL Server Enterprise Edition ne consentite al massimo nove.

## <a name="considerations"></a>Considerazioni

- Non sono consentite più repliche di sola configurazione per un singolo gruppo di disponibilità. 
- Una replica di sola configurazione non può essere una replica primaria.
- Non è possibile modificare la modalità di disponibilità di una replica di sola configurazione. Per passare da una replica di sola configurazione a una replica secondaria sincrona o asincrona, rimuovere la replica di sola configurazione e aggiungere una replica secondaria con la modalità di disponibilità richiesta. 
- Una replica di sola configurazione è sincrona con i metadati del gruppo di disponibilità. Non sono presenti dati utente. 
- Un gruppo di disponibilità con una replica primaria e una replica di sola configurazione, ma nessuna replica secondaria, non è valido. 
- Non è possibile creare un gruppo di disponibilità in un'istanza dell'edizione SQL Server Express. 

<a name="pacemakerNotify"></a>

## <a name="understand-sql-server-resource-agent-for-pacemaker"></a>Informazioni sull'agente delle risorse SQL Server per Pacemaker

In SQL Server 2017 CTP 1.4 è stato aggiunto `sequence_number` a `sys.availability_groups` per consentire a Pacemaker di identificare il livello di aggiornamento delle repliche secondarie rispetto alla replica primaria. `sequence_number` è un valore BIGINT a incremento progressivo costante che rappresenta il livello di aggiornamento della replica del gruppo di disponibilità locale. Pacemaker aggiorna il valore di `sequence_number` con ogni modifica alla configurazione del gruppo di disponibilità. Esempi di modifiche alla configurazione includono il failover e l'aggiunta o la rimozione di repliche. Il numero viene aggiornato nella replica primaria e quindi replicato in quelle secondarie. Una replica secondaria con configurazione aggiornata avrà quindi lo stesso numero di sequenza della replica primaria. 

Quando Pacemaker decide di alzare di livello una replica e impostarla come primaria, invia prima di tutto una notifica *preliminare* a tutte le repliche. Le repliche restituiscono il numero di sequenza. Quando Pacemaker cerca effettivamente di alzare di livello una replica impostandola come primaria, l'operazione viene eseguita solo se il numero di sequenza della replica è il più alto di tutti i numeri di sequenza. Se il numero di sequenza non corrisponde a quello più alto, la replica rifiuta l'operazione. In questo modo, solo la replica con il numero di sequenza più alto può essere alzata di livello e impostata come primaria e non si verifica alcuna perdita dei dati. 

Per questo processo è necessaria almeno una replica disponibile per l'innalzamento di livello con lo stesso numero di sequenza della replica primaria precedente. L'agente delle risorse di Pacemaker imposta `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` in modo che almeno una replica secondaria sincrona sia aggiornata e disponibile a essere la destinazione di un failover automatico per impostazione predefinita. Con ogni azione di monitoraggio, il valore di `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` viene calcolato (e aggiornato, se necessario). Il valore di `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` corrisponde al numero di repliche sincrone diviso per 2. In fase di failover, l'agente delle risorse richiede (`total number of replicas` - `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` repliche) di rispondere alla notifica preliminare all'innalzamento di livello. La replica con il valore di `sequence_number` più elevato verrà alzata di livello come replica primaria. 

Si consideri, ad esempio, il caso di un gruppo di disponibilità con tre repliche sincrone: una replica primaria e due repliche secondarie sincrone.

- `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` è pari a 1 (3 / 2 -> 1).

- Il numero di repliche a cui è richiesto di rispondere all'azione preliminare all'innalzamento di livello è 2 (3 - 1 = 2). 

In questo scenario, per l'attivazione del failover è necessario che due repliche rispondano alla notifica preliminare all'innalzamento di livello. Per una corretta esecuzione del failover automatico dopo un'interruzione della replica primaria, entrambe le repliche secondarie devono essere aggiornate e rispondere alla notifica. Se sono online e sincrone, hanno lo stesso numero di sequenza. Il gruppo di disponibilità alza di livello una delle due. Se solo una delle repliche secondarie risponde all'azione preliminare all'innalzamento di livello, l'agente delle risorse non può garantire che la replica secondaria che ha risposto abbia il valore di sequence_number più elevato e non viene attivato un failover.

> [!IMPORTANT]
> Quando `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` è 0 esiste il rischio di perdita dei dati. Durante un'interruzione della replica primaria, l'agente delle risorse non attiva automaticamente un failover. In tal caso, è possibile attendere il ripristino della replica primaria oppure eseguire manualmente il failover usando `FORCE_FAILOVER_ALLOW_DATA_LOSS`.

Si può scegliere di eseguire l'override del comportamento predefinito e impedire alla risorsa del gruppo di disponibilità di impostare automaticamente `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT`.

Lo script seguente imposta `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` su 0 in un gruppo di disponibilità denominato `<**ag1**>`. Prima di eseguire lo script, sostituire `<**ag1**>` con il nome del gruppo di disponibilità.

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=0
```

Per ripristinare il valore predefinito, in base alla configurazione del gruppo di disponibilità eseguire quanto segue:

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=
```

> [!NOTE]
> Quando si eseguono i comandi precedenti, la replica primaria viene temporaneamente abbassata di livello come replica secondaria e quindi alzata nuovamente di livello. L'aggiornamento della risorsa determina l'arresto e il riavvio di tutte le repliche. Il nuovo valore per `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` viene impostato solo dopo il riavvio delle repliche e non istantaneamente.

## <a name="see-also"></a>Vedere anche

[Gruppi di disponibilità in Linux](sql-server-linux-availability-group-overview.md)

<!--Image references-->
[1]: ./media/sql-server-linux-availability-group-ha/1-read-scale-out.png
[2]: ./media/sql-server-linux-availability-group-ha/2-configuration-only.png
[3]: ./media/sql-server-linux-availability-group-ha/3-three-replica.png
[4]: ./media/sql-server-linux-availability-group-ha/configuration-only-example.png
