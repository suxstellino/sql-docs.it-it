---
description: Attività File flessibili
title: Attività File flessibili | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPEXTFILETASK.F1
- SQL14.DTS.DESIGNER.AFPEXTFILETASK.F1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 89cb7e2de8cc4fa78afa93ca35f4eb36dab7136a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345719"
---
# <a name="flexible-file-task"></a>Attività File flessibili

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

L'attività File flessibili consente agli utenti di eseguire operazioni sui file presenti in vari servizi di archiviazione supportati.
I servizi di archiviazione attualmente supportati sono:

- File system locale
- [Archiviazione BLOB di Azure](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-introduction)

L'attività File flessibili è un componente del [Feature Pack di SQL Server Integration Services (SSIS) per Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).

Per aggiungere un'attività File flessibili a un pacchetto, trascinarla dalla casella degli strumenti SSIS nel canvas di progettazione. Fare quindi doppio clic sull'attività oppure clic con il pulsante destro del mouse sull'attività e selezionare **Modifica** per aprire la finestra di dialogo dell'**editor attività File flessibili**.

La proprietà **Operation** specifica l'operazione file da eseguire.
Attualmente sono supportate le operazioni seguenti:
- Operazione **Copia**
- Operazione **Elimina**

Per l'operazione **Copia** sono disponibili le proprietà riportate di seguito.

- **SourceConnectionType:** specifica il tipo di gestione connessione di origine.
- **SourceConnection:** specifica la gestione connessione di origine.
- **SourceFolderPath:** specifica il percorso della cartella di origine.
- **SourceFileName:** specifica il nome file di origine. Se lasciato vuoto, viene copiata la cartella di origine. I caratteri jolly seguenti sono consentiti nel nome del file di origine: `*` (corrisponde a zero o più caratteri), `?` (corrisponde a zero o a un carattere singolo) e `^` (carattere di escape).
- **SearchRecursively:** specifica se le sottocartelle verranno copiate in modo ricorsivo.
- **DestinationConnectionType:** specifica il tipo di gestione connessione di destinazione.
- **DestinationConnection:** specifica la gestione connessione di destinazione.
- **DestinationFolderPath:** specifica il percorso della cartella di destinazione.
- **DestinationFileName:** specifica il nome file di destinazione. Se lasciato vuoto, verranno usati i nomi dei file di origine.

Per l'operazione **Elimina** sono disponibili le proprietà riportate di seguito.
- **ConnectionType:** specifica il tipo di gestione connessione.
- **Connection:** specifica la gestione connessione.
- **FolderPath:** specifica il percorso della cartella.
- **FileName:** specifica il nome file. Se questa proprietà viene lasciata vuota, la cartella viene eliminata. Per Archiviazione BLOB di Azure l'eliminazione di cartelle non è supportata. I caratteri jolly seguenti sono consentiti nel nome del file: `*` (corrisponde a zero o più caratteri), `?` (corrisponde a zero o a un carattere singolo) e `^` (carattere di escape).
- **DeleteRecursively:** specifica se eliminare i file in modo ricorsivo.

***Note sulla configurazione delle autorizzazioni dell'entità servizio***

Per il funzionamento di **Test connessione** (sia per archiviazione BLOB sia per Azure Data Lake Storage Gen2), all'entità servizio deve essere assegnato almeno il **Ruolo con autorizzazioni di lettura per i dati dei BLOB di archiviazione** per l'account di archiviazione.
Questa operazione viene eseguita con [Controllo degli accessi in base al ruolo](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal).

Per l'archiviazione BLOB, le autorizzazioni di lettura e scrittura vengono concesse assegnando almeno il **Ruolo con autorizzazioni di lettura per i dati dei BLOB di archiviazione** e il ruolo **Collaboratore ai dati dei BLOB di archiviazione** rispettivamente.

Per Data Lake Storage Gen2, l'autorizzazione è determinata sia da Controllo degli accessi in base al ruolo che dagli [elenchi di controllo di accesso (ACL)](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer).
Verificare che gli ACL siano configurati usando l'ID oggetto (OID) dell'entità servizio per la registrazione dell'app, come descritto [qui](/azure/storage/blobs/data-lake-storage-access-control#how-do-i-set-acls-correctly-for-a-service-principal).
Questo ID è diverso dall'ID applicazione (client) usato con la configurazione di Controllo degli accessi in base al ruolo.
Quando a un'entità di sicurezza vengono concesse le autorizzazioni per i dati di Controllo degli accessi in base al ruolo tramite un ruolo predefinito o tramite un ruolo personalizzato, queste autorizzazioni vengono valutate per prime all'autorizzazione di una richiesta.
Se l'operazione richiesta è autorizzata dalle assegnazioni di Controllo degli accessi in base al ruolo dell'entità di sicurezza, l'autorizzazione viene immediatamente risolta e non vengono eseguiti controlli ACL aggiuntivi.
In alternativa, se l'entità di sicurezza non dispone di un'assegnazione di Controllo degli accessi in base al ruolo o se l'operazione della richiesta non corrisponde all'autorizzazione assegnata, vengono eseguiti i controlli ACL per determinare se l'entità di sicurezza è autorizzata a eseguire l'operazione richiesta.

- Per l'autorizzazione di lettura, concedere almeno l'autorizzazione di **esecuzione** partendo dal file system di origine insieme all'autorizzazione di **lettura** per i file da copiare. In alternativa, concedere almeno il **Ruolo con autorizzazioni di lettura per i dati dei BLOB di archiviazione** con Controllo degli accessi in base al ruolo.
- Per l'autorizzazione di scrittura, concedere almeno l'autorizzazione di **esecuzione** partendo dal file system del sink insieme all'autorizzazione di **scrittura** per la cartella del sink. In alternativa, concedere almeno il ruolo **Collaboratore ai dati dei BLOB di archiviazione** con Controllo degli accessi in base al ruolo.

Per informazioni dettagliate, vedere [questo articolo](/azure/storage/blobs/data-lake-storage-access-control).