---
description: Destinazione di File flessibili
title: Destinazione di File flessibili | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpextfiledest.f1
- sql14.dts.designer.afpextfiledest.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d331004e82b155a1c101a31253a82e029ec4a31b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091651"
---
# <a name="flexible-file-destination"></a>Destinazione di File flessibili

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Il componente **Destinazione di File flessibili** consente a un pacchetto SSIS di scrivere dati in vari servizi di archiviazione supportati.

I servizi di archiviazione attualmente supportati sono:

- [Archiviazione BLOB di Azure](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-introduction)
   
Trascinare e rilasciare **Destinazione di File flessibili** nella finestra di progettazione del flusso di dati e fare doppio clic su di esso per visualizzare l'editor.
  
**Destinazione di File flessibili** è un componente del [Feature Pack di SQL Server Integration Services (SSIS) per Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  

Le seguenti proprietà sono disponibili nell'**Editor Destinazione di File flessibili**.

- **Tipo di gestione connessione file:** specifica il tipo di gestione connessione di origine. Sceglierne una esistente del tipo specificato oppure crearne una nuova.
- **Percorso cartella:** specifica il percorso della cartella di destinazione.
- **Nome file:** specifica il nome file di destinazione.
- **Formato di file:** specifica il formato del file di destinazione. I formati supportati sono **Testo**, **Avro**, **ORC**, **Parquet**. Per ORC/Parquet è necessario Java. Per informazioni dettagliate, vedere [qui](../../integration-services/azure-feature-pack-for-integration-services-ssis.md#dependency-on-java).
- **Carattere delimitatore di colonna:** specifica il carattere da usare come delimitatore di colonna (i delimitatori composti da più caratteri non sono supportati).
- **Prima riga come nome di colonna:** specifica se scrivere i nomi di colonna nella prima riga.
- **Comprimi file:** specifica se comprimere il file.
- **Tipo di compressione:** specifica il formato di compressione da usare. I formati supportati sono **GZIP**, **DEFLATE**, **BZIP2**.
- **Livello di compressione:** specifica il livello di compressione da usare.

Le seguenti proprietà sono disponibili nell'**Editor avanzato**.

- **rowDelimiter:** carattere usato per separare le righe in un file. È consentito un solo carattere. Il valore **predefinito** è \r\n.
- **escapeChar:** carattere speciale usato per eseguire l'escape di un delimitatore di colonna nel contenuto del file di input. Non è possibile specificare sia escapeChar sia quoteChar per una tabella. È consentito un solo carattere. Nessun valore predefinito.
- **quoteChar:** carattere usato per inserire un valore stringa tra virgolette. I delimitatori di riga e colonna all'interno delle virgolette sono considerati come parte del valore della stringa. Questa proprietà è applicabile ai set di dati di input e di output. Non è possibile specificare sia escapeChar sia quoteChar per una tabella. È consentito un solo carattere. Nessun valore predefinito.
- **nullValue:** uno o più caratteri usati per rappresentare un valore null. Il valore **predefinito** è \N.
- **encodingName:** specifica il nome della codifica. Vedere la proprietà [Encoding.EncodingName](/dotnet/api/system.text.encoding).
- **skipLineCount:**  indica il numero di righe non vuote da ignorare durante la lettura dei dati dai file di input. Se vengono specificati sia skipLineCount che firstRowAsHeader, le righe vengono ignorate e quindi le informazioni dell'intestazione vengono lette dal file di input.
- **treatEmptyAsNull:** specifica se considerare una stringa null o vuota come valore null durante la lettura dei dati da un file di input. Il valore **predefinito** è True.

Dopo aver specificato le informazioni di connessione, passare alla pagina **Colonne** per eseguire il mapping delle colonne di origine alle colonne di destinazione per il flusso di dati SSIS.

**Note sulla configurazione delle autorizzazioni dell'entità servizio**

Per il funzionamento di **Test connessione** (sia per archiviazione BLOB sia per Azure Data Lake Storage Gen2), all'entità servizio deve essere assegnato almeno il **Ruolo con autorizzazioni di lettura per i dati dei BLOB di archiviazione** per l'account di archiviazione.
Questa operazione viene eseguita con [Controllo degli accessi in base al ruolo](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal).

Per l'archiviazione BLOB, l'autorizzazione di scrittura viene concessa assegnando almeno il ruolo **Collaboratore ai dati dei BLOB di archiviazione**.

Per Data Lake Storage Gen2, l'autorizzazione è determinata sia da Controllo degli accessi in base al ruolo che dagli [elenchi di controllo di accesso (ACL)](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer).
Verificare che gli ACL siano configurati usando l'ID oggetto (OID) dell'entità servizio per la registrazione dell'app, come descritto [qui](/azure/storage/blobs/data-lake-storage-access-control#how-do-i-set-acls-correctly-for-a-service-principal).
Questo ID è diverso dall'ID applicazione (client) usato con la configurazione di Controllo degli accessi in base al ruolo.
Quando a un'entità di sicurezza vengono concesse le autorizzazioni per i dati di Controllo degli accessi in base al ruolo tramite un ruolo predefinito o tramite un ruolo personalizzato, queste autorizzazioni vengono valutate per prime all'autorizzazione di una richiesta.
Se l'operazione richiesta è autorizzata dalle assegnazioni di Controllo degli accessi in base al ruolo dell'entità di sicurezza, l'autorizzazione viene immediatamente risolta e non vengono eseguiti controlli ACL aggiuntivi.
In alternativa, se l'entità di sicurezza non dispone di un'assegnazione di Controllo degli accessi in base al ruolo o se l'operazione della richiesta non corrisponde all'autorizzazione assegnata, vengono eseguiti i controlli ACL per determinare se l'entità di sicurezza è autorizzata a eseguire l'operazione richiesta.
Per l'autorizzazione di scrittura, concedere almeno l'autorizzazione di **esecuzione** partendo dal file system del sink insieme all'autorizzazione di **scrittura** per la cartella del sink.
In alternativa, concedere almeno il ruolo **Collaboratore ai dati dei BLOB di archiviazione** con Controllo degli accessi in base al ruolo.
Per informazioni dettagliate, vedere [questo articolo](/azure/storage/blobs/data-lake-storage-access-control).