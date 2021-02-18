---
title: Suggerimenti per l'uso di pacchetti R
titleSuffix: SQL machine learning
description: Suggerimenti utili per l'uso di pacchetti R in SQL Server destinati a sviluppatori che non hanno familiarità con R o SQL Server.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/06/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e3dd91e29d88fc2ab6f7216f0bac38632caa7b25
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352509"
---
# <a name="tips-for-using-r-packages"></a>Suggerimenti per l'uso di pacchetti R

[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Questo articolo offre suggerimenti utili per l'uso di pacchetti R in SQL Server. Questi suggerimenti sono destinati ad amministratori di database che non hanno familiarità con R e a sviluppatori esperti di linguaggio R che non hanno familiarità con l'accesso ai pacchetti in un'istanza di SQL Server.

## <a name="if-youre-new-to-r"></a>Se non si ha familiarità con R

Gli amministratori che installano pacchetti R per la prima volta possono trovare utili alcune nozioni di base sulla gestione dei pacchetti R.

### <a name="package-dependencies"></a>Dipendenze dei pacchetti

I pacchetti R spesso dipendono da altri pacchetti, alcuni dei quali potrebbero non essere disponibili nella libreria R predefinita usata dall'istanza. A volte un pacchetto richiede una versione diversa di un pacchetto dipendente rispetto a quello già installato. Le dipendenze dei pacchetti sono riportate in un file DESCRIPTION incorporato nel pacchetto, ma talvolta sono incomplete. Per elaborare il grafico completo delle dipendenze è possibile usare un pacchetto denominato [iGraph](https://igraph.org/r/).

Se si devono installare più pacchetti o si vuole essere sicuri che tutti gli utenti dell'organizzazione ottengano il tipo di pacchetto e la versione corretti, è consigliabile usare [miniCRAN](https://mran.microsoft.com/package/miniCRAN) per analizzare la catena delle dipendenze completa. miniCRAN crea un repository locale che può essere condiviso tra più utenti o computer. Per altre informazioni, vedere [Creare un repository di pacchetti locale usando miniCRAN](create-a-local-package-repository-using-minicran.md).

### <a name="package-sources-versions-and-formats"></a>Origini, versioni e formati dei pacchetti

I pacchetti R possono avere più origini, ad esempio [CRAN](https://cran.r-project.org/) e [Bioconductor](https://www.bioconductor.org/). Sul sito ufficiale per il linguaggio R (<https://www.r-project.org/>) sono elencate molte di queste risorse. Microsoft offre [MRAN](https://mran.microsoft.com/) per la propria distribuzione di R open source ([MRO](https://mran.microsoft.com/open)) e per altri pacchetti. Molti pacchetti sono pubblicati su GitHub, dove gli sviluppatori possibile ottenere il codice sorgente.

::: moniker range=">=sql-server-2016"
I pacchetti R vengono eseguiti su più piattaforme di elaborazione. Assicurarsi che le versioni installate siano file binari per Windows.
::: moniker-end

::: moniker range=">=sql-server-linux-ver15"
I pacchetti R vengono eseguiti su più piattaforme di elaborazione. Assicurarsi che le versioni installate siano file binari per Linux.
::: moniker-end

### <a name="know-which-library-youre-installing-to-and-which-packages-are-already-installed"></a>Individuare la libreria in cui si esegue l'installazione e i pacchetti già installati

Se in precedenza l'ambiente R è stato modificato nel computer, prima di procedere all'installazione assicurarsi che la variabile di ambiente R `.libPath` usi un solo percorso,

che deve puntare alla cartella R_SERVICES per l'istanza. Per altre informazioni, tra cui il modo in cui determinare quali pacchetti sono già installati, vedere [Ottenere informazioni sui pacchetti R](../package-management/r-package-information.md).

## <a name="if-youre-new-to-sql-server"></a>Se non si ha familiarità con SQL Server

Gli sviluppatori in ambiente R che lavorano sul codice in esecuzione su SQL Server possono avere difficoltà a controllare l'ambiente R a causa dei criteri di sicurezza che proteggono il server. I suggerimenti seguenti descrivono situazioni tipiche e forniscono suggerimenti per lavorare in questo ambiente.

### <a name="r-user-libraries-not-supported-on-sql-server"></a>Librerie utente R: non supportate in SQL Server

Gli sviluppatori in ambiente R che devono installare nuovi pacchetti R sono abituati a installare i pacchetti a propria discrezione, usando una libreria utente privata ogni volta che la libreria predefinita non è disponibile oppure quando non sono amministratori del computer. In un tipico ambiente di sviluppo R, l'utente aggiungerebbe il percorso alla variabile di ambiente R `libPath` o farebbe riferimento al percorso completo del pacchetto, come nell'esempio seguente:

```R
library("c:/Users/<username>/R/win-library/packagename")
```

Questo non è possibile quando si eseguono soluzioni R in SQL Server perché i pacchetti R devono essere installati in una libreria predefinita specifica associata all'istanza. Se un pacchetto non è installato nella libreria predefinita, quando si prova a chiamare il pacchetto viene restituito un errore simile al seguente:

*Errore nella libreria (xxx): nessun pacchetto denominato 'nome-pacchetto'*

Per informazioni su come installare i pacchetti R in SQL Server, vedere [Installare nuovi pacchetti R in Machine Learning Services per SQL Server o R Services per SQL Server](install-additional-r-packages-on-sql-server.md).

### <a name="how-to-avoid-package-not-found-errors"></a>Come evitare errori di tipo "pacchetto non trovato"

Le linee guida seguenti consentono di evitare che vengano restituiti errori di tipo "pacchetto non trovato".

+ Eliminare le dipendenze dalle librerie utente.

    L'installazione dei pacchetti R richiesti in una libreria utente personalizzata non è una pratica di sviluppo corretta. Se una soluzione viene eseguita da un altro utente che non ha accesso al percorso della libreria, è possibile che vengano restituiti errori.

    Inoltre, se un pacchetto è installato nella libreria predefinita, il runtime R lo caricherà dalla libreria predefinita, anche se si specifica una versione diversa nel codice R.

+ Verificare che il codice possa essere eseguito in un ambiente condiviso.

+ Evitare di includere l'installazione di pacchetti in una soluzione. Se non si hanno le autorizzazioni per installare i pacchetti, il codice restituirà un errore. Anche se si dispone di tali autorizzazioni, è necessario eseguire l'installazione separatamente dall'altro codice da eseguire.

+ Controllare il codice per assicurarsi che non ci siano chiamate a pacchetti non installati.

+ Aggiornare il codice per rimuovere i riferimenti diretti ai percorsi dei pacchetti o delle librerie R.

+ Individuare la libreria di pacchetti associata all'istanza. Per altre informazioni, vedere [Ottenere informazioni sui pacchetti R](../package-management/r-package-information.md).

## <a name="see-also"></a>Vedere anche

::: moniker range="<=sql-server-2017"
+ [Installare pacchetti con gli strumenti R](install-r-packages-standard-tools.md)
::: moniker-end
::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Installare nuovi pacchetti R con sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end
