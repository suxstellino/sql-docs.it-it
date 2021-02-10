---
description: Informazioni sui cursori
title: Informazioni sui cursori | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], about cursors
ms.assetid: 596eb4b6-c22f-4cde-b23f-172dd66c3161
author: rothja
ms.author: jroth
ms.openlocfilehash: 915d41f78de6c789c82bca3d729aa2c28ff26f27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032263"
---
# <a name="what-is-a-cursor"></a>Informazioni sui cursori
Nei database relazionali le operazioni vengono eseguite su set di righe completi. Il set di righe restituito da un'istruzione SELECT include tutte le righe che soddisfano le condizioni specificate nella clausola WHERE dell'istruzione. Il set di righe completo restituito dall'istruzione è noto come set di risultati. Le applicazioni, in particolare quelle interattive e online, non possono sempre funzionare in modo efficace con l'intero set di risultati come unità. In tali applicazioni deve essere pertanto disponibile un meccanismo per l'elaborazione di una riga singola o di un blocco di righe di dimensioni ridotte. I cursori sono un'estensione dei set di risultati che implementano appunto tale meccanismo.  
  
 Un cursore viene implementato da una libreria di cursori. Una libreria di cursori è un software, spesso implementato come parte di un sistema di database o di un'API di accesso ai dati, usato per gestire gli attributi dei dati restituiti da un'origine dati (un set di risultati). Questi attributi includono la gestione della concorrenza, la posizione nel set di risultati, il numero di righe restituite e la possibilità di spostarsi avanti o indietro (o entrambi) tramite il set di risultati (scorrevolezza).  
  
 Un cursore tiene traccia della posizione nel set di risultati e consente di eseguire più operazioni riga per riga rispetto a un set di risultati, con o senza tornare alla tabella originale. In altre parole, i cursori restituiscono concettualmente un set di risultati basato sulle tabelle all'interno dei database. Il nome del cursore è il seguente, in quanto indica la posizione corrente nel set di risultati, così come il cursore sullo schermo di un computer indica la posizione corrente.  
  
 È importante acquisire familiarità con il concetto di cursori prima di procedere per apprendere le specifiche del loro utilizzo in ADO.  
  
 Utilizzando i cursori, è possibile:  
  
-   Consente di specificare il posizionamento in righe specifiche del set di risultati.  
  
-   Recuperare una riga o un blocco di righe in base alla posizione del set di risultati corrente.  
  
-   Modificare i dati nelle righe in corrispondenza della posizione corrente nel set di risultati.  
  
-   Definire diversi livelli di sensibilità per le modifiche dei dati apportate da altri utenti.  
  
 Si consideri, ad esempio, un'applicazione che visualizza un elenco di prodotti disponibili per un potenziale acquirente. Il compratore scorre l'elenco per visualizzare i dettagli e i costi del prodotto e infine seleziona un prodotto per l'acquisto. Lo scorrimento e la selezione aggiuntivi vengono eseguite per il resto dell'elenco. Per quanto riguarda l'acquirente, i prodotti vengono visualizzati uno alla volta, ma l'applicazione utilizza un cursore scorrevole per scorrere verso l'alto e verso il basso il set di risultati.  
  
 È possibile utilizzare i cursori in diversi modi:  
  
-   Senza alcuna riga.  
  
-   Con alcune o tutte le righe in una singola tabella.  
  
-   Con alcune o tutte le righe delle tabelle unite logicamente.  
  
-   Come di sola lettura o aggiornabile a livello di cursore o di campo.  
  
-   Come di sola trasmissione o completamente scorrevole.  
  
-   Con il keyset del cursore posizionato sul server.  
  
-   Sensibili alle modifiche apportate alla tabella sottostante causate da altre applicazioni, ad esempio appartenenza, ordinamento, inserimenti, aggiornamenti ed eliminazioni.  
  
-   Esistente sul server o sul client.  
  
 I cursori di sola lettura consentono agli utenti di esplorare il set di risultati e i cursori di lettura/scrittura possono implementare singoli aggiornamenti delle righe. I cursori complessi possono essere definiti con i set di tasti che puntano alle righe della tabella di base. Sebbene alcuni cursori siano di sola lettura in una direzione successiva, altri possono spostarsi avanti e indietro e fornire un aggiornamento dinamico del set di risultati in base alle modifiche apportate da altre applicazioni al database.  
  
 Non tutte le applicazioni devono utilizzare i cursori per accedere ai dati o aggiornarli. Alcune query non richiedono semplicemente l'aggiornamento diretto delle righe tramite un cursore. I cursori devono essere una delle ultime tecniche scelte per recuperare i dati e quindi scegliere il cursore di effetto più basso possibile. Quando si crea un set di risultati utilizzando un stored procedure, il set di risultati non è aggiornabile mediante i metodi di modifica o di aggiornamento del cursore.  
  
## <a name="concurrency"></a>Concorrenza  
 In alcune applicazioni multiutente è molto importante che i dati presentati all'utente finale siano i più aggiornati possibili. Un esempio classico di un sistema di questo tipo è un sistema di prenotazione delle compagnie aeree, in cui molti utenti potrebbero tendersi per la stessa sede in un determinato volo (e quindi un singolo record). In un caso di questo tipo, la progettazione dell'applicazione deve gestire operazioni simultanee in un singolo record.  
  
 In altre applicazioni, la concorrenza non è altrettanto importante. In questi casi, non è possibile giustificare le spese necessarie per mantenere aggiornati i dati in qualsiasi momento.  
  
## <a name="position"></a>Posizione  
 Un cursore tiene inoltre traccia della posizione corrente in un set di risultati. Si pensi alla posizione del cursore come puntatore al record corrente, in modo analogo al modo in cui un indice della matrice punta al valore in corrispondenza della posizione specifica nella matrice.  
  
## <a name="scrollability"></a>Scorrimento  
 Il tipo di cursore utilizzato dall'applicazione influisca anche sulla possibilità di spostarsi avanti e indietro nelle righe di un set di risultati. Questa operazione viene a volte definita scorrimento. La possibilità di spostarsi avanti *e* indietro tramite un set di risultati aggiunge alla complessità del cursore ed è quindi più onerosa da implementare. Per questo motivo, è consigliabile richiedere un cursore con questa funzionalità solo quando è necessario.
