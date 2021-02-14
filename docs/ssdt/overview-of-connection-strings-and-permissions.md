---
title: Stringhe di connessione e autorizzazioni
description: Informazioni sulle stringhe di connessione, gli account e le autorizzazioni necessarie per eseguire unit test di SQL Server. Vedere come configurare le stringhe di connessione.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ceff114e-a738-46ad-9785-b6647a2247f9
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 70387a9e18888a47522613b7a76a2483aec0d287
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080812"
---
# <a name="overview-of-connection-strings-and-permissions"></a>Panoramica delle stringhe di connessione e delle autorizzazioni

Per l'esecuzione di unit test di SQL Server è necessario connettersi a un server di database usando una o due stringhe di connessione specifiche. Ogni stringa di connessione rappresenta un account con le autorizzazioni specifiche necessarie per eseguire l'attività o il set di attività in uno script specifico come parte del test. È possibile specificare queste stringhe nella finestra di dialogo **Configurazione test di SQL Server** o modificando manualmente il file app.config per il progetto di test.  
  
## <a name="connection-strings"></a>Stringhe di connessione  
Nella finestra di dialogo **Configurazione test di SQL Server** è possibile specificare le stringhe di connessione per ognuno degli account riportati di seguito.  
  
> [!NOTE]  
> Il contesto di esecuzione e quello autorizzato sono diversi solo se si usa l'autenticazione di SQL Server. Se si utilizza l'autenticazione di Windows, verranno utilizzate le stesse credenziali per entrambe le stringhe di connessione.  
  
-   Contesto di esecuzione (obbligatorio): account utente per l'esecuzione dello script di test. Questa stringa di connessione deve disporre delle stesse credenziali che dovrebbero avere anche gli utenti. Ciò è importante perché assicura che siano state applicate al database le autorizzazioni appropriate. Per altre informazioni, vedere [Procedura: Configurare l'esecuzione di unit test di SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
    Nel file app.config per il progetto di test, si tratta dell'elemento `ExecutionContext`.  
  
-   Contesto autorizzato (facoltativo): account nello stesso database con autorizzazioni superiori per l'esecuzione delle azioni pre-test e post-test e degli script TestInitialize e TestCleanup. Questi script impostano lo stato del database e, per l'azione post-test, possono essere utilizzati per convalidare oggetti nel database. Questa stringa di connessione viene inoltre utilizzata per distribuire le modifiche del database e generare dati.  
  
    Nel file app.config per il progetto di test, si tratta dell'elemento `PrivilegedContext`. Se tramite gli unit test di SQL Server viene eseguito solo lo script di test, non è necessario specificare un contesto autorizzato.  
  
Le stringhe specificate nella finestra di dialogo di configurazione progetto vengono archiviate nel file app.config del progetto di test. È inoltre possibile modificare direttamente il file e ricompilare il progetto. I nuovi valori verranno quindi visualizzati nella finestra di dialogo.  
  
## <a name="windows-authentication-versus-sql-server-authentication"></a>Autenticazione di Windows e autenticazione di SQL Server  
Quando si specificano le stringhe di connessione, è necessario scegliere tra l'utilizzo dell'autenticazione di Windows e l'autenticazione di SQL server. Un motivo per scegliere l'autenticazione di Windows è il fatto che supporta l'uso dei test in team meglio dell'autenticazione di SQL Server. Se si sceglie l'autenticazione di SQL Server, le stringhe di connessione vengono crittografate usando le API Data Protection (DPAPI), in base alle credenziali utente. Ciò significa che i test in questo progetto di test verranno eseguiti solo per l'utente corrente, non per i membri del team, i quali ottengono i test tramite il sistema di controllo del codice sorgente dopo che l'utente li ha archiviati. Per eseguire test in questo progetto di test, gli altri membri del team dovranno riconfigurare il progetto di test usando le proprie credenziali. A tale scopo, dovranno modificare la propria copia del file app.config o utilizzare la finestra di dialogo di configurazione progetto.  
  
## <a name="permissions"></a>Autorizzazioni  
Lo script di test viene eseguito al livello di autorizzazione del contesto di esecuzione, ovvero lo stesso livello di autorizzazione che sarebbe attivo per i comandi dell'utente eseguiti sul database secondo l'utilizzo tipico. Le azioni pre-test e post-test e gli script TestInitialize e TestCleanup vengono eseguiti nel livello di autorizzazione del contesto autorizzato.  
  
Poiché per lo script dell'azione post-test viene utilizzata la connessione con autorizzazione superiore, è possibile eseguirne la convalida. In questo script è inoltre possibile eseguire i comandi script per il test delle autorizzazioni. Per altre informazioni sulle autorizzazioni, vedere la sezione sugli unit test di SQL Server in [Autorizzazioni necessarie per SQL Server Data Tools](../ssdt/required-permissions-for-sql-server-data-tools.md).  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Script in unit test di SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md)  
[File di unit test di SQL Server](../ssdt/sql-server-unit-test-files.md)  
  
