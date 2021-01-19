---
title: Pool di risorse di Resource Governor
description: SQL Server Resource Governor specifica i limiti per la quantità di CPU, I/O fisico e memoria che le richieste dell'applicazione in ingresso possono usare nel pool di risorse.
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, resource pool
- resource pool [SQL Server], overview
- resource pool [SQL Server]
ms.assetid: 306b6278-e54f-42e6-b746-95a9315e0cbe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4028f19dc9d7ac319b083b5e6077021da59e7a9b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172313"
---
# <a name="resource-governor-resource-pool"></a>Pool di risorse di Resource Governor
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  In Resource Governor di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , un pool di risorse rappresenta un subset delle risorse fisiche di un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Resource Governor permette di specificare i limiti sulla quantità di CPU, I/O fisico e memoria che le richieste dell'applicazione in ingresso possono utilizzare nel pool di risorse. Ogni pool di risorse può contenere uno o più gruppi di carico di lavoro. Una volta avviata, la sessione viene assegnata a un gruppo di carico di lavoro specifico tramite la funzione di classificazione di Resource Governor e deve essere eseguita utilizzando le risorse assegnate a tale gruppo.  
  
## <a name="resource-pool-concepts"></a>Concetti relativi al pool di risorse  
 Un pool di risorse, o pool, rappresenta le risorse fisiche del server. Il pool può essere paragonato a un'istanza virtuale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un pool è composto da due parti. Una parte non si sovrappone agli altri pool, abilitando così la prenotazione delle risorse minime. L'altra parte è condivisa con gli altri pool e supporta l'utilizzo massimo possibile delle risorse. Le risorse del pool vengono definite specificando una o più delle seguenti impostazioni per ciascuna risorsa (CPU, memoria e I/O fisico):  
  
-   **MIN_CPU_PERCENT e MAX_CPU_PERCENT**  
  
     Queste impostazioni rappresentano il valore minimo e massimo della larghezza di banda media garantita della CPU per tutte le richieste nel pool in caso di contesa delle risorse della CPU. È possibile utilizzare queste impostazioni per determinare l'utilizzo previsto delle risorse della CPU per più carichi di lavoro in base alle esigenze di ciascun carico di lavoro. Si supponga ad esempio che i reparti Vendita e Marketing di un'azienda condividano lo stesso database. Il reparto Vendita ha un carico di lavoro ad uso intensivo della CPU con query ad alta priorità. Anche il reparto Marketing ha un carico di lavoro ad uso intensivo della CPU, ma la priorità delle query è inferiore. Se si crea un pool di risorse distinto per ciascun reparto, è possibile assegnare una percentuale di CPU *minima* pari a 70 per il pool di risorse del reparto Vendita e una percentuale *massima* pari a 30 per il pool di risorse del reparto Marketing. In questo modo si garantisce che il carico di lavoro del reparto Vendita riceva le risorse della CPU necessarie e che il carico di lavoro del reparto Marketing sia isolato dalle richieste di CPU del carico di lavoro dell'altro reparto. Notare che la percentuale di CPU massima è un valore massimo opportunistico. Se disponibile, il carico di lavoro utilizza fino al 100% della capacità della CPU. Il valore massimo viene applicato solo in caso di contesa delle risorse della CPU. In questo esempio, se il carico di lavoro del reparto Vendita è disattivato, per il carico di lavoro del reparto Marketing se necessario può essere utilizzato il 100% della CPU.  
  
-   **CAP_CPU_PERCENT**  
  
     L'impostazione CAP_CPU_PERCENT impone un limite rigido sulla larghezza di banda della CPU per tutte le richieste nel pool di risorse. I carichi di lavoro associati al pool possono utilizzare la capacità della CPU oltre il valore di MAX_CPU_PERCENT se è disponibile, ma non oltre il valore di CAP_CPU_PERCENT. Utilizzando l'esempio precedente, si supponga che al reparto Marketing sia fatturato l'utilizzo delle risorse. Il reparto chiede di ricevere una fatturazione prevedibile e non desidera pagare più del 30 percento della CPU. Questa richiesta può essere soddisfatta impostando CAP_CPU_PERCENT su 30 per il pool di risorse del reparto Marketing.  
  
-   **MIN_MEMORY_PERCENT e MAX_MEMORY_PERCENT**  
  
     Queste impostazioni rappresentano il valore minimo e massimo della quantità di memoria riservata al pool di risorse che non può essere condivisa con altri pool di risorse. Per i database senza tabelle ottimizzate per la memoria, la memoria a cui viene fatto riferimento qui è la memoria concessa per l'esecuzione delle query, non la memoria del pool di buffer (ad esempio le pagine di dati e di indice). Impostando un valore di memoria minimo per un pool, si garantisce la disponibilità della percentuale di memoria specificata per qualsiasi richiesta che possa essere eseguita nel pool di risorse. Questo rappresenta un'importante differenza rispetto a MIN_CPU_PERCENT, perché in questo caso la memoria può rimanere nel pool di risorse in questione anche se non esistono richieste nei gruppi di carico di lavoro appartenenti a esso. Occorre quindi prestare molta attenzione nell'uso di questa impostazione, perché la memoria specificata non sarà disponibile per gli altri pool, anche in assenza di richieste attive. Se si imposta un valore di memoria massimo per un pool significa che le richieste in esecuzione in questo pool non otterranno mai più di questa percentuale di memoria complessiva.

     Per gestire la memoria per le tabelle ottimizzate per la memoria con Resource Governor, è necessario associare il database a un pool di risorse distinto. Per altre informazioni, vedere [Associare un database con tabelle con ottimizzazione per la memoria a un pool di risorse](../in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md).
  
-   **AFFINITY**  
  
     Con questa impostazione è possibile impostare l'affinità di un pool di risorse a una o più unità di pianificazione o nodi NUMA per un maggiore isolamento delle risorse della CPU. Nello scenario precedente dei reparti Vendita e Marketing, si supponga che il reparto Vendita necessiti di un ambiente più isolato e richieda costantemente il 100% di un core CPU. Tramite l'uso dell'opzione AFFINITY è possibile pianificare i carichi di lavoro dei reparti Vendita e Marketing su CPU differenti. Supponendo che il valore di CAP_CPU_PERCENT nel pool del reparto Marketing sia invariato, il carico di lavoro del reparto Marketing continua a utilizzare un massimo del 30 percento di un core, mentre il carico di lavoro del reparto Vendita utilizza il 100 percento dell'altro core. Per quanto concerne i carichi di lavoro dei reparti Vendita e Marketing, essi sono in esecuzione su due computer isolati.  
  
-   **MIN_IOPS_PER_VOLUME e MAX_IOPS_PER_VOLUME**  
  
     Queste impostazioni rappresentano il valore minimo e massimo delle operazioni di I/O fisico al secondo (IOPS) per volume di disco per un pool di risorse. È possibile utilizzare queste impostazioni per controllare gli I/O fisici per i thread di utente per un determinato pool di risorse. Il reparto Vendita genera ad esempio numerosi report di fine mese in grandi batch. Le query in questi batch possono generare operazioni di I/O tali da saturare il volume del disco e influire sulle prestazioni di altri carichi di lavoro nel database che hanno una priorità più elevata. Per isolare questo carico di lavoro, MIN_IOPS_PER_VOLUME viene impostato su 20 e MAX_IOPS_PER_VOLUME viene impostato su 100 per il pool di risorse del reparto Vendita, in modo da controllare il livello di I/O che può essere emesso per il carico di lavoro.  
  
Quando si configura la CPU o la memoria, la somma dei valori MIN di tutti i pool non può superare il 100 percento delle risorse del server. Inoltre, quando si configura la CPU o la memoria, i valori MAX e CAP possono essere impostati dovunque nell'intervallo tra MIN e 100 percento inclusi.  
  
Se per un pool è definito un valore MIN diverso da zero, il valore MAX effettivo degli altri pool viene nuovamente regolato. Il valore minimo del valore MAX configurato di un pool e la somma dei valori MIN degli altri pool vengono sottratti dal 100 percento.  
  
Nella seguente tabella vengono illustrati alcuni concetti precedenti. Nella tabella vengono mostrate le impostazioni per il pool interno, il pool predefinito e due pool definiti dall'utente. 
  
|Nome pool|Impostazione MIN%|Impostazione MAX%|Valore MAX% effettivo calcolato|Percentuale condivisa calcolata|Comment|  
|---------------|-------------------|-------------------|--------------------------------|-------------------------|-------------|  
|interno|0|100|100|0|I valori MAX% effettivo e % condivisa non sono applicabili al pool interno.|  
|default|0|100|30|30|Il valore MAX effettivo viene calcolato come: min(100,100-(20+50)) = 30. La percentuale condivisa calcolata è il valore effettivo MAX - MIN = 30.|  
|Pool 1|20|100|50|30|Il valore MAX effettivo viene calcolato come: min(100,100-50) = 50. La percentuale condivisa calcolata è il valore effettivo MAX - MIN = 30.|  
|Pool 2|50|70|70|20|Il valore MAX effettivo viene calcolato come: min(70,100-20) = 70. La percentuale condivisa calcolata è il valore effettivo MAX - MIN = 20.|  

Le seguenti formule vengono utilizzate per calcolare il valore MAX% effettivo e la percentuale condivisa nella tabella precedente:  
  
-   Min(X,Y) indica il valore più piccolo di X e Y.  
  
-   Sum(X) indica la somma del valore X in tutti i pool.  
  
-   Totale percentuale condivisa = 100 - sum(MIN%).  
  
-   Valore effettivo MAX% = Min(X,Y).  
  
-   Percentuale condivisa = Valore effettivo MAX% - MIN%.  

Utilizzando la tabella precedente come esempio, è possibile descrivere ulteriormente le regolazioni che si verificano quando viene creato un altro pool. Il pool descritto è il pool 3 e ha una impostazione MIN% di 5.  
  
|Nome pool|Impostazione MIN%|Impostazione MAX%|Valore MAX% effettivo calcolato|Percentuale condivisa calcolata|Comment|  
|---------------|-------------------|-------------------|--------------------------------|-------------------------|-------------|  
|interno|0|100|100|0|I valori MAX% effettivo e % condivisa non sono applicabili al pool interno.|  
|default|0|100|25|25|Il valore MAX effettivo viene calcolato come: min(100,100-(20+50+5) = 25. La percentuale condivisa calcolata è il valore effettivo MAX - MIN = 25.|  
|Pool 1|20|100|45|25|Il valore MAX effettivo viene calcolato come: min(100,100-55) = 45. La percentuale condivisa calcolata è il valore effettivo MAX - MIN = 25.|  
|Pool 2|50|70|70|20|Il valore MAX effettivo viene calcolato come: min(70,100-25) = 70. La percentuale condivisa calcolata è il valore effettivo MAX - MIN = 20.|  
|Pool 3|5|100|30|25|Il valore MAX effettivo viene calcolato come: min(100,100-70) = 30. La percentuale condivisa calcolata è il valore effettivo MAX - MIN = 25.|  
  
 La parte condivisa del pool viene utilizzata per indicare la destinazione possibile delle risorse, se disponibili. Tuttavia, quando le risorse vengono utilizzate vengono destinate al pool specificato e non vengono condivise. Ciò può migliorare l'utilizzo delle risorse nei casi in cui non sono presenti richieste in un pool specificato e le risorse configurate per il pool possono essere liberate per gli altri pool.  
  
 Alcuni casi estremi di configurazione del pool sono:  
  
-   Tutti i pool definiscono valori minimi che in totale rappresentano il 100 percento delle risorse del server. In questo caso i valori massimi effettivi equivalgono ai minimi. Ciò equivale a dividere le risorse del server in parti non sovrapposte indipendentemente dalle risorse utilizzate in ciascun pool.  
  
-   Tutti i pool hanno valori minimi uguali a zero. Tutti i pool si contendono le risorse disponibili e le dimensioni finali sono basate sull'utilizzo delle risorse in ciascun pool. Altri fattori quali i criteri svolgono un ruolo nella determinazione delle dimensioni finali del pool.  
  
In Resource Governor sono disponibili due pool di risorse, il pool interno e il pool predefinito. È possibile aggiungere altri pool.  
  
**Pool interno**  
  
Il pool interno rappresenta le risorse utilizzate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questo pool contiene sempre soltanto il gruppo interno e non può essere modificato in alcun modo. L'utilizzo delle risorse da parte del pool interno non è limitato. I carichi di lavoro nel pool sono considerati critici per la funzione del server e tramite Resource Governor il pool interno richiede memoria agli altri pool anche se questo comporta la violazione dei limiti impostati per gli altri pool.  
  
> [!NOTE]  
>  L'utilizzo del pool interno e delle risorse del gruppo interno non viene sottratto dall'utilizzo complessivo delle risorse. Le percentuali vengono calcolate dalle risorse complessive disponibili.  
  
**Pool predefinito**  
  
Il pool predefinito è il primo pool utente predefinito. Prima della configurazione il pool predefinito contiene solo il gruppo predefinito. Il pool predefinito non può essere creato o eliminato ma può essere modificato. Il pool predefinito può contenere gruppi definiti dall'utente oltre al gruppo predefinito. A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] è presente un pool di risorse predefinito per le operazioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di routine e un pool di risorse esterno predefinito per i processi esterni, ad esempio l'esecuzione di script R.  
  
> [!NOTE]  
>  Il gruppo predefinito può essere modificato ma non può essere spostato all'esterno del pool predefinito.  
  
**Pool esterno**  
  
Gli utenti possono definire un pool esterno per definire le risorse per i processi esterni. Per R Services, questo pool determina `rterm.exe`, `BxlServer.exe` e altri processi derivati.  
  
**Pool di risorse definiti dall'utente**  
  
I pool di risorse definiti dall'utente sono quelli che si creano per carichi di lavoro specifici in un ambiente. Resource Governor fornisce istruzioni DDL per la creazione, la modifica e l'eliminazione dei pool di risorse.  
  
## <a name="resource-pool-tasks"></a>Attività relative ai pool di risorse  
  
|Descrizione dell'attività|Articolo|  
|----------------------|-----------|  
|Viene descritto come creare un pool di risorse.|[Creare un pool di risorse](../../relational-databases/resource-governor/create-a-resource-pool.md)|  
|Viene descritto come modificare le impostazioni del pool di risorse.|[Modificare le impostazioni del pool di risorse](../../relational-databases/resource-governor/change-resource-pool-settings.md)|  
|Viene descritto come eliminare un pool di risorse.|[Eliminare un pool di risorse](../../relational-databases/resource-governor/delete-a-resource-pool.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Gruppo di carico di lavoro di Resource Governor](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Funzione di classificazione di Resource Governor](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [Configurare Resource Governor utilizzando un modello](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)   
 [Visualizzare proprietà di Resource Governor](../../relational-databases/resource-governor/view-resource-governor-properties.md)  
  
  
