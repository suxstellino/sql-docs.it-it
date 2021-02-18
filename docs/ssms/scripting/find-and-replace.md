---
title: Trova e sostituisci
description: Informazioni su come usare la finestra di dialogo Trova e sostituisci per trovare e sostituire testo.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Find and Replace dialog box
ms.assetid: 09297893-d80b-4c88-86b4-52bfb639e521
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bd45603e4254be161b6e80a59e969f61784cc987
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354666"
---
# <a name="find-and-replace"></a>Trova e sostituisci
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Usare la finestra di dialogo **Trova e sostituisci** per trovare testo all'interno di un file e facoltativamente sostituirlo con altro testo. Possono essere visualizzate versioni della finestra **Trova e sostituisci** con opzioni leggermente diverse, a seconda di come è stata aperta la finestra di dialogo. Scegliere **Trova e sostituisci** dal menu **Modifica** e quindi fare clic su **Ricerca veloce** per aprire la finestra di dialogo con le opzioni di ricerca ma senza le opzioni di sostituzione. Scegliere **Trova e sostituisci** dal menu **Modifica** e quindi fare clic su **Sostituzione veloce** per aprire la finestra di dialogo con le opzioni di ricerca e sostituzione.  
  
 Per visualizzare la finestra di dialogo **Cerca e sostituisci** , sono inoltre disponibili i pulsanti della barra degli strumenti e i tasti di scelta rapida.  
  
## <a name="find-what"></a>Trova  
 Questi controlli consentono di specificare la stringa o l'espressione da cercare.  
  
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
 Per sostituire istanze della stringa specificata nella casella **Trova** con un'altra stringa, immettere la stringa di sostituzione in questo campo. Per eliminare le istanze del testo specificato nella casella **Trova**, lasciare vuoto questo campo. È possibile selezionare uno degli ultimi 20 elementi digitati nell'elenco a discesa. Per includere espressioni regolari nella stringa specificata nella casella **Sostituisci con** , selezionare la casella di controllo **Utilizza** e quindi fare clic su **Espressioni regolari**. Questa casella viene visualizzata solo se la finestra di dialogo è stata aperta facendo clic su **Sostituzione veloce**.  
  
 **Replace with**  
 Per sostituire istanze della stringa specificata nella casella **Trova** con un'altra stringa, immettere la stringa di sostituzione in questo campo. Per eliminare le istanze della stringa specificata nella casella **Trova** , lasciare vuoto questo campo. È possibile selezionare uno degli ultimi 20 elementi digitati nell'elenco a discesa. Per includere espressioni regolari nella stringa specificata nella casella **Sostituisci con** , selezionare la casella di controllo **Utilizza** e quindi fare clic su **Espressioni regolari**.  
  
 **Generatore di espressioni**  
 Questo pulsante triangolare accanto alla casella **Sostituisci con** diventa disponibile quando è selezionata la casella di controllo **Utilizza** in **Opzioni di ricerca**. Fare clic su questo pulsante per visualizzare un elenco di caratteri jolly o di espressioni regolari, a seconda dell'opzione selezionata nella casella **Utilizza** . Selezionando un elemento nell'elenco lo si aggiunge alla stringa specificata nella casella **Sostituisci con** .  
  
 **Replace**  
 Fare clic su questo pulsante per sostituire l'istanza corrente della stringa specificata nella casella **Trova** con la stringa specificata nella casella **Sostituisci con** e per trovare l'istanza successiva nell'ambito specificato nella casella **Cerca in**.  
  
 **Sostituisci tutto**  
 Fare clic su questo pulsante per sostituire tutte le istanze della stringa specificata nella casella **Trova** con la stringa specificata nella casella **Sostituisci con** in tutti i file dell'ambito specificato nella casella **Cerca in** .  
  
> [!CAUTION]  
>  Verificare che la casella **Cerca in** sia impostata in modo da includere solo i file che si vogliono modificare.  
  
 Viene visualizzato un promemoria che include l'opzione **Non chiudere i file modificati** . Selezionare questa opzione per mantenere l'opzione **Annulla** . **Annulla** è disponibile solo per i file che restano aperti dopo essere stati modificati.  
  
 **Ignora file**  
 Diventa disponibile quando il valore specificato in **Cerca in** include più file. Fare clic su questo pulsante se non si desidera cercare nel file corrente o modificare tale file. La ricerca proseguirà nel file successivo dell'elenco **Cerca in**.  
  
## <a name="look-in"></a>Cerca in  
 **Look in**  
 Selezionare il percorso in cui cercare il testo specificato nella casella **Trova**. Le opzioni disponibili sono **Documento corrente**, che consente di eseguire la ricerca nella finestra del documento attivo al momento dell'apertura della finestra di dialogo, e **Tutti i documenti aperti**, che consente di eseguire la ricerca in tutte le finestre dei documenti attualmente aperti in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="find-options"></a>Opzioni di ricerca  
 È possibile espandere o comprimere la sezione **Opzioni di ricerca** . Le opzioni seguenti possono essere selezionate o deselezionate.  
  
 **Maiuscole/minuscole**  
 Se questa casella di controllo è selezionata, nelle finestre dei risultati di ricerca verranno visualizzate solo le istanze della stringa specificata in **Trova** che corrispondono sia per il contenuto che per l'uso di lettere maiuscole o minuscole. Ad esempio, una ricerca di "**MioOggetto**" con la casella **Maiuscole/minuscole** selezionata restituirà "MioOggetto" ma non "miooggetto" o "MIOOGGETTO".  
  
 **Parola intera**  
 Se questa casella di controllo è selezionata, nelle finestre Risultati ricerca saranno visualizzate solo istanze della stringa specificata nella casella **Trova** con parola intera corrispondente. La ricerca di **MioOggetto** restituirà quindi "MioOggetto", ma non "CMioOggetto" o "MioOggettoC."  
  
 **Cerca in alto**  
 Esegue una ricerca dalla posizione del cursore verso l'inizio del documento.  
  
 **Cerca nel testo nascosto**  
 Consente di trovare istanze nascoste del testo e all'interno di testo nascosto.  
  
 **Uso**  
 Indica come interpretare i caratteri speciali immessi nella casella di testo **Trova** o **Sostituisci con** . Sono disponibili le opzioni **Caratteri jolly** ed **Espressioni regolari**.  
  
 **Regular Expressions**  
 È possibile definire i modelli del testo di cui si desidera ottenere le corrispondenze utilizzando notazioni speciali. Per un elenco, vedere [Trova con espressioni regolari](./search-text-with-regular-expressions.md).  
  
 **Caratteri jolly**  
 I caratteri speciali, quali asterischi (`*`) e punti interrogativi(`?`), rappresentano uno o più caratteri. Per un elenco, vedere [Trova con caratteri jolly](./search-text-with-wildcards.md).  
  
 **Trova successivo**  
 Consente di iniziare la ricerca del testo all'interno della casella **Trova** .  
  
 **Replace**  
 Fare clic su questo pulsante per sostituire l'istanza corrente della stringa specificata nella casella **Trova** con la stringa specificata nella casella **Sostituisci con** e per trovare l'istanza successiva nell'ambito specificato nella casella **Cerca in**.  
  
 **Replace All**  
 Fare clic su questo pulsante per sostituire tutte le istanze della stringa specificata nella casella **Trova** con la stringa specificata nella casella **Sostituisci con** in tutti i file dell'ambito specificato nella casella **Cerca in**.  
  
> [!CAUTION]  
>  Verificare che la casella **Cerca in** sia impostata in modo da includere solo i file che si vogliono modificare.  
  
## <a name="find-and-replace-views"></a>Menu Visualizza di Trova e sostituisci  
 Fra le schede nella parte superiore della finestra Trova e Sostituisci sono disponibili i menu **Visualizza** , che consentono di scegliere un set di campi da visualizzare nel riquadro attivo. È possibile ancorare la finestra **Trova e sostituisci** in una posizione appropriata e quindi spostarsi tra le varie schede e viste per eseguire qualsiasi tipo di operazione di ricerca o sostituzione.  
  
 **Ricerca veloce**  
 Questa opzione della barra degli strumenti consente di passare dalla finestra di dialogo corrente alla finestra di dialogo **Ricerca veloce** .  
  
 **Cerca nei file**  
 Questa opzione della barra degli strumenti consente di passare dalla finestra di dialogo corrente alla finestra di dialogo **Cerca nei file** .  
  
 **Sostituzione veloce**  
 Questa opzione della barra degli strumenti consente di passare dalla finestra di dialogo corrente alla finestra di dialogo **Sostituzione veloce** .  
  
 **Sostituisci nei file**  
 Questa opzione della barra degli strumenti consente di passare dalla finestra di dialogo corrente alla finestra di dialogo **Sostituisci nei file** .  
  
## <a name="see-also"></a>Vedere anche  
 [Tasti di scelta rapida di SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)