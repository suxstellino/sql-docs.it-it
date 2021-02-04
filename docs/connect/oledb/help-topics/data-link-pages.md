---
title: Configurazione di UDL (Universal Data Link) | Microsoft Docs
description: Informazioni su come usare la scheda Connessione per specificare come connettersi ai dati tramite OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.author: v-beaziz
author: bazizi
ms.openlocfilehash: 52089c92af76611fe8ab60b53dc7cf2848fb31c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195311"
---
# <a name="universal-data-link-udl-configuration"></a>Configurazione di UDL (Universal Data Link)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="connection-tab"></a>Scheda Connessione
Usare la scheda Connessione per specificare come connettersi ai dati tramite Microsoft OLE DB Driver per SQL Server.

![Screenshot delle pagine OLE DB Data Link - Scheda Connessione](../media/data-link-pages-connection-tab.png)

Nella scheda Connessione, specifica del provider, vengono visualizzate solo le proprietà di connessione richieste da Microsoft OLE DB Driver for SQL Server.

|Opzione|Descrizione|
|---   |---        |
|Selezionare o immettere il nome di un server|Selezionare un nome di server dall'elenco a discesa oppure digitare il percorso del server in cui si trova il database a cui si desidera accedere. La selezione del database nel server è un'azione distinta. Aggiornare l'elenco facendo clic su "Aggiorna".
|Immettere le informazioni per l'accesso al server|Da questo elenco a discesa è possibile selezionare le opzioni di autenticazione seguenti: <ul><li>`Windows Authentication:` Autenticazione per SQL Server con le credenziali dell'account di Windows dell'utente attualmente connesso.</li><li>`SQL Server Authentication:` Autenticazione con ID di accesso e password.</li><li>`Active Directory - Integrated:` Autenticazione integrata con un'identità di Azure Active Directory. Questa modalità può essere usata anche per l'autenticazione di Windows per SQL Server.</li><li>`Active Directory - Password:` Autenticazione di ID utente e password con un'identità di Azure Active Directory.</li><li>`Active Directory - Universal with MFA support:` Autenticazione interattiva con un'identità di Azure Active Directory. Questa modalità supporta l'autenticazione a più fattori (MFA) di Azure.</li><li>`Active Directory - Service Principal:` Autenticazione con entità servizio di Azure Active Directory. Il **nome utente** deve essere impostato sull'ID applicazione (client). La **password** deve essere impostata sul segreto client dell'applicazione.</li></ul>|
|SPN server|Se si utilizza una connessione trusted, è possibile specificare un nome dell'entità servizio (SPN) per il server.|
|Nome utente|Digitare l'ID utente da usare per l'autenticazione quando si accede all'origine dati.|
|Password|Digitare la password da usare per l'autenticazione quando si accede all'origine dati.|
|Nessuna password|Se è selezionata, questa opzione consente al provider specificato di usare una password vuota nella stringa di connessione.|
|Consenti salvataggio password|Se è selezionata, questa opzione consente di salvare la password con la stringa di connessione. L'eventuale inclusione della password nella stringa di connessione dipende dalle funzionalità dell'applicazione chiamante. <br/><br/>**NOTA:** se salvata, la password viene restituita e salvata non mascherata e non crittografata.|
|Usa crittografia avanzata per i dati|Se questa opzione è selezionata, i dati passati attraverso la connessione verranno crittografati.|
|Certificato server attendibile|Se questa opzione è selezionata, il certificato del server verrà convalidato. Il certificato del server deve avere il nome host corretto del server e deve essere emesso da un'autorità di certificazione attendibile.|
|Selezionare il database|Selezionare o digitare il nome del database a cui si vuole accedere.|
|Associa file di database con nome|Indica il nome del file primario di un database a cui è possibile collegarsi. Questo database viene associato e utilizzato come database predefinito per l'origine dati. Nella prima casella di testo di questa sezione digitare il nome di database da usare per il file di database associato.<br/><br/>Digitare il percorso completo del file di database da associare nella casella di testo con l'etichetta `Using the filename` oppure fare clic sul pulsante `...` per cercare il file di database.|
|Comando Cambia password|Visualizza la finestra di dialogo Modifica password SQL Server. |
|Test della connessione|Fare clic per tentare una connessione all'origine dati specificata. Se la connessione non riesce, verificare che le impostazioni siano corrette. Ad esempio, è possibile che le connessioni non vengano eseguite correttamente se sono presenti errori ortografici o non viene rispettata la distinzione tra maiuscole e minuscole.|

## <a name="advanced-tab"></a>Avanzate - scheda
Usare la scheda Avanzate per visualizzare e impostare proprietà di inizializzazione aggiuntive.

![Screenshot delle pagine OLE DB Data Link - Scheda Avanzate](../media/data-link-pages-advanced-tab.png)

|Opzione|Descrizione|
|---   |---        |
| Connect timeout | Specifica il numero di secondi di attesa di Microsoft OLE DB Driver per SQL Server per il completamento dell'inizializzazione. Se l'inizializzazione scade, viene restituito un errore e la connessione non viene creata.|


> [!NOTE]  
>  Per informazioni più generali sulla connessione Data Link, vedere [Panoramica dell'API Data Link](/previous-versions/windows/desktop/ms718102(v=vs.85)).

## <a name="next-steps"></a>Passaggi successivi
- [Eseguire l'autenticazione per Azure Active Directory](../features/using-azure-active-directory.md) usando il driver OLE DB.

- [Richiedere all'utente le credenziali di autenticazione](../help-topics/sql-server-login-dialog.md) usando il driver OLE DB.