---
title: Sostituisci nei file
description: Informazioni su come usare la funzionalità Sostituisci nei file della finestra Trova e sostituisci per cercare una stringa o un'espressione nel testo di un set di file e modificare parte o tutto il testo trovato.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Replace in Files dialog box
ms.assetid: 51191c0a-e022-41d6-8473-5cb3c6596862
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5f220d9d64221bb02c577547b645c28a03a5b044
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354562"
---
# <a name="replace-in-files"></a>Sostituisci nei file
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La scheda **Sostituisci nei file** della finestra di dialogo Trova e sostituisci consente di cercare una stringa o un'espressione nel codice di un dato set di file e di modificare alcune o tutte le corrispondenze trovate. L'elenco delle corrispondenze rilevate e delle azioni eseguite viene visualizzato nella finestra per i risultati della ricerca selezionata in **Opzioni risultati**.  
  
 Per visualizzare la finestra di dialogo **Cerca e sostituisci** , sono inoltre disponibili i pulsanti della barra degli strumenti e i tasti di scelta rapida.  
  
## <a name="find-what"></a>Trova  
 Questi controlli della scheda **Sostituisci nei file** consentono di specificare la stringa o l'espressione da trovare.  
  
 **Trova**  
 Digitare il testo che si desidera cercare. Nella finestra di dialogo verrà proposto un testo di ricerca probabile, basato sul testo selezionato con il cursore prima dell'apertura della finestra di dialogo oppure sul testo vicino o su quello cercato in precedenza. È possibile riutilizzare una delle 20 stringhe di ricerca più recenti selezionandola dall'elenco a discesa.  
  
 **[stringa con caratteri jolly]**  
 Se si vuole usare caratteri jolly quali asterischi (`*`) e punti interrogativi (`?`) nella stringa di ricerca, selezionare la casella di controllo **Utilizza** sotto **Opzioni di ricerca** e quindi fare clic su **Caratteri jolly**.  
  
 **[espressione regolare]**  
 Affinché il motore di ricerca interpreti la stringa come espressione regolare, selezionare la casella di controllo **Utilizza** sotto **Opzioni di ricerca** e quindi fare clic su **Espressioni regolari**.  
  
 **Generatore di espressioni**  
 Questo pulsante triangolare accanto alla casella **Trova** è disponibile se in **Opzioni di ricerca** è selezionata la casella di controllo **Utilizza**. Fare clic su questo pulsante per visualizzare un elenco di caratteri jolly o di espressioni regolari, a seconda dell'opzione selezionata nella casella **Utilizza** . Selezionando un elemento in questo elenco lo si aggiunge alla stringa specificata nella casella **Trova** .  
  
## <a name="replace-with"></a>Sostituire con  
 Questi controlli consentono di specificare ciò che verrà inserito al posto della stringa o espressione trovata.  
  
 **Replace with**  
 Per sostituire istanze della stringa specificata nella casella **Trova** con un'altra stringa, immettere la stringa di sostituzione in questo campo. Per eliminare le istanze della stringa specificata nella casella **Trova**, lasciare vuota questa casella. È possibile selezionare uno degli ultimi 20 elementi digitati nell'elenco a discesa. Per includere espressioni regolari nella stringa specificata nella casella **Sostituisci con** , selezionare la casella di controllo **Utilizza** e quindi fare clic sull'opzione **Espressioni regolari** .  
  
 **Generatore di espressioni**  
 Questo pulsante triangolare accanto alla casella **Sostituisci con** diventa disponibile quando è selezionata la casella di controllo **Utilizza** in **Opzioni di ricerca**. Fare clic su questo pulsante per visualizzare un elenco di caratteri jolly o di espressioni regolari, a seconda dell'opzione selezionata nella casella **Utilizza** . Selezionando un elemento nell'elenco lo si aggiunge alla stringa specificata nella casella **Sostituisci con** .  
  
 **Replace**  
 Fare clic su questo pulsante per sostituire l'istanza corrente della stringa specificata nella casella **Trova** con la stringa specificata nella casella **Sostituisci con** e per trovare l'istanza successiva nell'ambito specificato nella casella **Cerca in**.  
  
 **Sostituisci tutto**  
 Fare clic su questo pulsante per sostituire tutte le istanze della stringa specificata nella casella **Trova** con la stringa specificata nella casella **Sostituisci con** in tutti i file dell'ambito specificato nella casella **Cerca in**.  
  
> [!CAUTION]  
>  Verificare che la casella **Cerca in** sia impostata in modo da includere solo i file che si vogliono modificare.  
  
 Viene visualizzato un promemoria che include l'opzione **Non chiudere i file modificati** . Selezionare questa opzione per mantenere l'opzione **Annulla** . **Annulla** è disponibile solo per i file che restano aperti dopo essere stati modificati.  
  
 **Ignora file**  
 Diventa disponibile quando la casella **Cerca in** include più file. Fare clic su questo pulsante se non si desidera cercare nel file corrente o modificare tale file. La ricerca proseguirà nel file successivo dell'elenco **Cerca in**.  
  
## <a name="look-in"></a>Cerca in  
 L'opzione selezionata nell'elenco a discesa **Cerca in** determina se la funzione **Sostituisci nei file** eseguirà la ricerca solo nei file attualmente attivi oppure in tutti i file archiviati all'interno di determinate cartelle. Selezionare un ambito di ricerca dall'elenco, digitare il percorso di una cartella oppure fare clic sul pulsante **Sfoglia** per visualizzare la finestra di dialogo **Seleziona cartelle di ricerca** e selezionare un set di cartelle in cui eseguire la ricerca.  
  
> [!NOTE]  
>  Se l'opzione selezionata in **Cerca in** interessa un file estratto dal controllo del codice sorgente, la ricerca verrà eseguita solo nella versione del file scaricata sul computer locale.  
  
 **Look in**  
 Selezionare un ambito di ricerca predefinito dall'elenco oppure usare la finestra di dialogo **Seleziona cartelle di ricerca** per specificare un set di directory personalizzato.  
  
 **Documento corrente**  
 Questa opzione è disponibile quando un documento è aperto in un editor. La ricerca della stringa specificata nella casella **Trova** viene eseguita solo nel documento attivo.  
  
 **Tutti i documenti aperti**  
 La ricerca viene eseguita in tutti i file aperti attualmente per essere modificati.  
  
 **Progetto corrente**  
 La ricerca viene eseguita in tutti i file del progetto attivo.  
  
 **Intera soluzione**  
 La ricerca viene eseguita in tutti i file della soluzione attiva.  
  
 **Includi sottocartelle**  
 Indica che la ricerca verrà effettuata nelle sottocartelle della cartella specificata in **Cerca in** . È necessario indicare un set di directory personalizzato.  
  
 **Sfoglia (...)**  
 Fare clic su questo pulsante per visualizzare la finestra di dialogo **Seleziona cartelle di ricerca** , in cui è possibile unire, modificare, salvare e selezionare i set denominati di directory da immettere nella casella **Cerca in** .  
  
## <a name="find-options"></a>Opzioni di ricerca  
 È possibile espandere o comprimere la sezione **Opzioni di ricerca** . Le opzioni seguenti possono essere selezionate o deselezionate.  
  
 **Maiuscole/minuscole**  
 Se questa casella di controllo è selezionata, nelle finestre dei risultati di ricerca verranno visualizzate solo le istanze della stringa specificata in **Trova** che corrispondono sia per il contenuto che per l'uso di lettere maiuscole o minuscole. Ad esempio, una ricerca di **MioOggetto** con la casella **Maiuscole/minuscole** selezionata restituirà "MioOggetto" ma non "miooggetto" o "MIOOGGETTO".  
  
 **Parola intera**  
 Se questa casella di controllo è selezionata, nelle finestre dei risultati di ricerca verranno visualizzate solo le istanze della stringa specificata in **Trova** che corrispondono alle parole complete. La ricerca di **MioOggetto** restituirà quindi "MioOggetto", ma non "CMioOggetto" o "MioOggettoC."  
  
 **Uso**  
 Indica come interpretare i caratteri speciali immessi nella casella di testo **Trova** o **Sostituisci con** . Sono disponibili le opzioni **Caratteri jolly** ed **Espressioni regolari**.  
  
 **Regular Expressions**  
 È possibile definire i modelli del testo di cui si desidera ottenere le corrispondenze utilizzando notazioni speciali. Per un elenco, vedere [Trova con espressioni regolari](./search-text-with-regular-expressions.md).  
  
 **Caratteri jolly**  
 I caratteri speciali, quali asterischi (`*`) e punti interrogativi(`?`), rappresentano uno o più caratteri. Per un elenco, vedere [Trova con caratteri jolly](./search-text-with-wildcards.md).  
  
 **Cerca i seguenti tipi di file**  
 In questo elenco vengono indicati i tipi di file da cercare nelle directory specificate in **Cerca in**. Se questa casella viene lasciata vuota, la ricerca verrà eseguita in tutti i file contenuti nelle directory specificate nella casella **Cerca in** .  
  
```  
*.[ext]; *.[ext] (manual)  
```  
  
 Per individuare i file di un determinato tipo, immettere un carattere jolly asterisco (`*`) per il nome del file, seguito da un punto (`.`) e dall'estensione del file. Per individuare più tipi di file, inserire più stringhe di ricerca separate da un punto e virgola (`;`).  
  
```  
*.[ext]; *.[ext] (from list)  
```  
  
 Selezionare un elemento nell'elenco per immettere una stringa di ricerca preconfigurata in base a cui cercare file di determinati tipi.  
  
## <a name="result-options"></a>Opzioni risultati  
 È possibile espandere o comprimere la sezione **Opzioni risultati** . Le opzioni seguenti possono essere selezionate o deselezionate.  
  
 **Finestra Risultati ricerca 1**  
 Se questa casella di controllo è selezionata, i risultati della ricerca corrente verranno aggiunti al contenuto della finestra Risultati ricerca 1. Questa finestra viene aperta automaticamente per visualizzare i risultati della ricerca. Per aprirla manualmente, scegliere **Altre finestre** dal menu **Visualizza** e quindi fare clic su **Risultati ricerca 1**.  
  
 **Finestra Risultati ricerca 2**  
 Se questa casella di controllo è selezionata, i risultati della ricerca corrente verranno aggiunti al contenuto della finestra Risultati ricerca 2. Questa finestra viene aperta automaticamente per visualizzare i risultati della ricerca. Per aprirla manualmente, scegliere **Altre finestre** dal menu **Visualizza** e quindi fare clic su **Risultati ricerca 2**.  
  
 **Mostra solo nomi file**  
 Consente di visualizzare una voce per ogni file contenente una corrispondenza con la stringa anziché una voce per ogni corrispondenza della stringa nella finestra Risultati ricerca 1 o Risultati ricerca 2. Questa opzione non è disponibile in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Non chiudere i file modificati con Sostituisci tutto**  
 Se questa casella di controllo è selezionata, i file in cui sono state eseguite sostituzioni rimangono aperti, in modo che sia possibile annullare o salvare le modifiche. È possibile che la quantità di memoria disponibile limiti il numero di file che possono rimanere aperti dopo un'operazione di sostituzione.  
  
> [!CAUTION]  
>  Il comando **Annulla** può essere usato solo nei file che rimangono aperti per le modifiche. Se questa opzione non è selezionata, i file non ancora aperti per essere modificati rimarranno chiusi e non sarà disponibile alcuna opzione **Annulla** per tali file.  
  
## <a name="find-and-replace-views"></a>Menu Visualizza di Trova e sostituisci  
 Nelle schede sulla parte superiore della finestra Trova e sostituisci sono disponibili i menu **Visualizza** , che consentono di scegliere un set di campi da visualizzare nel riquadro attivo. È possibile lasciare la finestra Trova e sostituisci ancorata in una posizione comoda e quindi passare da una scheda all'altra e da una visualizzazione all'altra per eseguire qualsiasi tipo di operazione di ricerca e sostituzione.  
  
 **Passa a Ricerca veloce**  
 Questa opzione della barra degli strumenti consente di passare dalla finestra di dialogo corrente alla finestra di dialogo **Ricerca veloce** .  
  
 **Passa a Cerca nei file**  
 Questa opzione della barra degli strumenti consente di passare dalla finestra di dialogo corrente alla finestra di dialogo **Cerca nei file** .  
  
 **Passa a Trova simbolo**  
 Questa opzione della barra degli strumenti consente di passare dalla finestra di dialogo corrente alla finestra di dialogo **Trova simbolo** .  
  
## <a name="see-also"></a>Vedere anche  
 [Tasti di scelta rapida di SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)