---
description: Come contribuire alla documentazione di SQL Server
title: Come contribuire alla documentazione di SQL Server | Microsoft Docs
ms.date: 08/13/2018
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: a0f3f665421c04208af1123e659aadcfe513d407
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080312"
---
# <a name="how-to-contribute-to-sql-server-documentation"></a>Come contribuire alla documentazione di SQL Server

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Chiunque può contribuire alla documentazione di SQL Server. Si può contribuire, ad esempio, correggendo gli errori tipografici, suggerendo spiegazioni più chiare e migliorando la precisione dal punto di vista tecnico. Questo articolo illustra come iniziare a contribuire al contenuto e come funziona il processo.

Si può contribuire alla documentazione usando due flussi di lavoro principali:

|Flusso di lavoro|Descrizione|
|---|---|
| [Apportare modifiche nel browser](#githubui) | Adatto per piccole e rapide modifiche degli articoli. |
| [Apportare modifiche a livello locale con gli strumenti](#tools) | Adatto per modifiche più complesse, modifiche che interessano più articoli e contributi frequenti a docs.microsoft.com. |

Tutti i contenuti pubblici vengono convalidati dal team responsabile dei contenuti relativi a SQL per garantirne la coerenza e l'accuratezza tecnica. 

## <a name="edit-in-your-browser"></a><a id="githubui"></a> Apportare modifiche nel browser

È possibile apportare semplici modifiche al contenuto SQL Server nel browser e quindi inviarle a Microsoft. Per altre informazioni, vedere la [panoramica della guida per i collaboratori di Microsoft Docs](/contribute/#quick-edits-to-existing-documents). 

La procedura seguente sintetizza il processo: 

1. Nella pagina per cui si vogliono inviare commenti e suggerimenti selezionare il collegamento **Modifica** in alto a destra.
1. Nella pagina successiva selezionare l'icona **Matita** in alto a destra.
1. Nella finestra **Edit file text** (Modifica testo file) nella pagina successiva apportare le modifiche direttamente al testo che si vuole modificare.
    Se è necessaria assistenza per la formattazione del testo nuovo o modificato, vedere [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) (Scheda di riferimento rapido per il markdown).
1. Dopo aver apportato le modifiche, in **Commit changes** (Conferma modifiche):
    1. Nella prima casella di testo immettere una breve descrizione della modifica apportata.
    1. Nella casella **Add an optional extended description** (Aggiungi descrizione estesa facoltativa) immettere una breve descrizione della modifica.
1. Selezionare **Propose file change** (Proponi modifica file).
1. Nella pagina **Comparing changes** (Confronto modifiche) selezionare **Create pull request** (Crea richiesta pull). 
1. Nella pagina **Open a pull request** (Apri richiesta pull) selezionare **Create pull request** (Crea richiesta pull). 

Il video seguente illustra la procedura completa per l'invio delle modifiche nel browser:

![Modificare la documentazione SQL](media/sql-server-docs-navigation-guide/edit-sql-docs.gif)

## <a name="edit-locally-with-tools"></a><a id="tools"></a> Apportare modifiche a livello locale con gli strumenti

Un'altra opzione di modifica è creare un fork del repository **sql-docs** o **azure-docs** e clonarlo nel computer locale. È quindi possibile usare un editor di Markdown e un client Git per inviare le modifiche. Questo flusso di lavoro è ottimale per le modifiche più complesse o che interessano più file. È ideale inoltre per i collaboratori che inviano spesso contributi a docs.microsoft.com.

Per contribuire con questo metodo, vedere gli articoli seguenti:

- [Creare un account GitHub](/contribute/get-started-setup-github)
- [Installare gli strumenti di creazione del contenuto](/contribute/get-started-setup-tools)
- [Configurare un repository Git locale](/contribute/get-started-setup-local)
- [Usare gli strumenti per collaborare](/contribute/how-to-write-workflows-major)

Se si invia una richiesta pull con modifiche significative alla documentazione, verrà visualizzato un commento in GitHub che richiede di inviare un **contratto di licenza per contributi (CLA)** online. È necessario completare il modulo online affinché la richiesta pull possa essere accettata.

## <a name="recognition"></a>Riconoscimento

Se le modifiche vengono accettate, l'utente viene riconosciuto come collaboratore all'inizio dell'articolo.

![Riconoscimento del contributo al contenuto](./media/sql-server-docs-contribute/contribution-recognition.png)

## <a name="sql-docs-overview"></a>Panoramica di sql-docs

Questa sezione contiene informazioni aggiuntive sull'uso del repository **sql-docs**.

> [!IMPORTANT]
> Le informazioni di questa sezione sono specifiche per **sql-docs**. Se si modifica un articolo su SQL nella documentazione di Azure, vedere [il file Leggimi relativo al repository azure-docs su GitHub](https://github.com/MicrosoftDocs/azure-docs/blob/master/README.md).

Il repository [sql-docs](https://github.com/MicrosoftDocs/sql-docs) usa alcune cartelle standard per organizzare il contenuto.

| Cartella | Descrizione |
|---|---|
| [docs](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs) | Contiene tutto il contenuto pubblicato su SQL Server. Le diverse aree del contenuto sono organizzate logicamente in sottocartelle. |
| [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) | Contiene i file di inclusione. Questi file sono blocchi di contenuto che si possono includere in uno o più argomenti diversi. |
| **./media** | Ogni cartella può avere una sottocartella **media** per le immagini degli articoli. La cartella **media** a sua volta contiene sottocartelle con lo stesso nome degli argomenti in cui appare l'immagine. Le immagini devono essere file PNG con tutte le lettere minuscole e senza spazi. |
| **TOC.MD** | Un file di sommario. Ogni sottocartella può usare un file TOC.MD. |

#### <a name="applies-to-includes"></a>Inclusioni applies-to

Ogni articolo su SQL Server contiene un file di inclusione **applies-to** dopo il titolo. Indica a quali aree o versioni di SQL Server si riferisce l'articolo.

Si consideri l'esempio di Markdown seguente che esegue il pull nel file di inclusione **appliesto-ss-asdb-asdw-pdw-md.md**.

```Markdown
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
```

Questo comando aggiunge il testo seguente nella parte superiore dell'articolo:

![Testo applies-to](./media/sql-server-docs-contribute/applies-to.png)

Per trovare il file di inclusione applies-to corretto per l'articolo, usare i suggerimenti seguenti:

- Per un elenco delle inclusioni di uso comune, vedere [File di inclusione per il controllo delle versioni e Si applica a in SQL Server](applies-to-includes.md).
- Consultare un altro articolo che analizza la stessa funzionalità o un'attività correlata. Se si modifica quell'articolo, è possibile copiare il Markdown per il collegamento al file di inclusione applies-to (è possibile annullare la modifica senza inviarla).
- Cercare nella directory [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) i file che contengono il testo "applies-to". È possibile usare il pulsante **Find** in GitHub per filtrare rapidamente. Fare clic sul file per vedere come viene visualizzato.
- Prestare attenzione alla convenzione di denominazione. Se il nome contiene delle x, in genere sono segnaposto che indicano la mancanza di supporto per un servizio. Ad esempio, **appliesto-xx-xxxx-asdw-xxx-md.md** indica che esiste solo il supporto per Azure Synapse Analytics, poiché nel nome appare solo **asdw**, mentre gli altri campi contengono solo x.
- Alcune inclusioni specificano un numero di versione, ad esempio **tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md**. Usare questi file di inclusione solo quando si sa cha la funzionalità è stata introdotta con una versione specifica di SQL Server.

## <a name="contributor-resources"></a>Risorse per i collaboratori

- [Guida per il collaboratore per docs.microsoft.com](/contribute/)
- [Guida di stile Microsoft](/teamblog/style-guide)
- [Informazioni di base su Markdown](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/)

> [!TIP]
> Per inviare commenti e suggerimenti relativi al prodotto anziché alla documentazione, [inserire qui eventuali suggerimenti per il prodotto SQL Server](https://feedback.azure.com/forums/908035-sql-server).

## <a name="next-steps"></a>Passaggi successivi

Esplorare il [repository sql-docs](https://github.com/MicrosoftDocs/sql-docs) su GitHub.

Trovare un articolo, inviare una modifica e collaborare con la community di SQL Server. 

Grazie!