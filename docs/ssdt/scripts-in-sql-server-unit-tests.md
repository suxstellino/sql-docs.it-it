---
title: Script in unit test di SQL Server
description: Acquisire familiarità con gli script per gli unit test di SQL Server. Informazioni sugli script di pre-test, test e post-test e sugli script comuni TestInitialize e TestCleanup.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 80c5cf62-a9c9-4e9d-8c6f-8eed50a595a7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: eb725a491eb87a0116413a786c514a0ede98361f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066946"
---
# <a name="scripts-in-sql-server-unit-tests"></a>Script in unit test di SQL Server

In ogni unit test di SQL Server sono contenuti una singola azione di pre-test, di test e di post-test. In ognuna di queste azioni sono contenuti a loro volta gli elementi seguenti:  
  
-   Script Transact\-SQL da eseguire in un database.  
  
-   Nessuna o più condizioni di test tramite cui vengono valutati i risultati restituiti dall'esecuzione di script.  
  
Lo script di test Transact\-SQL nell'azione di test è l'unico componente che è necessario includere in ogni unit test di SQL Server. Oltre allo script di test stesso, è inoltre possibile specificare le condizioni di test per verificare se tramite lo script di test è stato restituito il valore o il set di valori previsto. Tramite l'azione di test viene verificato il comportamento di un determinato oggetto del database oppure viene modificato e, successivamente, viene valutata tale modifica.  
  
Per ogni azione di test, è possibile includere un'azione di pre-test e una di post-test. Analogamente all'azione di test, in ogni azione di pre-test e di post-test sono contenuti uno script Transact\-SQL e nessuna o più condizioni di test. È possibile utilizzare un'azione di pre-test per assicurarsi che lo stato in cui si trova il database consenta l'esecuzione dell'azione di test e la restituzione di risultati significativi. È ad esempio possibile utilizzare un'azione di pre-test per verificare che in una tabella siano contenuti dati prima che su questi ultimi venga eseguita un'operazione tramite lo script di test. Dopo la preparazione del database tramite l'azione di pre-test e la restituzione di risultati significativi tramite l'azione di test, è possibile utilizzare l'azione di post-test per ripristinare lo stato del database precedente all'esecuzione dell'azione di pre-test. In alternativa, in alcuni casi, è possibile utilizzare l'azione di post-test per convalidare i risultati dell'azione di test. Questa situazione si verifica perché l'azione di post-test può disporre di privilegi superiori rispetto all'azione di test. Per altre informazioni, vedere [Panoramica delle stringhe di connessione e delle autorizzazioni](../ssdt/overview-of-connection-strings-and-permissions.md).  
  
Oltre a queste tre azioni, sono disponibili anche due script di test, definiti script comuni, eseguiti prima e dopo l'esecuzione di ogni unit test di SQL Server. Di conseguenza, durante l'esecuzione di un singolo unit test di SQL Server è possibile che vengano eseguiti fino a cinque script Transact\-SQL. Solo lo script Transact\-SQL contenuto all'interno dell'azione di test è obbligatorio, mentre gli script comuni e gli script delle azioni di pre-test e di post-test sono facoltativi.  
  
Nella tabella seguente viene fornito un elenco completo degli script associati a qualsiasi unit test di SQL Server.  
  
|**Azione**|**Tipo di script**|**Descrizione**|  
|--------------|-------------------|-------------------|  
|TestInitialize|Script comune (inizializzazione)|(Facoltativo) Questo script precede tutte le azioni di pre-test e le azioni di test nello unit test. Lo script TestInitialize viene eseguito prima di ogni unit test in una determinata classe di test utilizzando il contesto autorizzato.|  
|Pre-test|Script di test|(Facoltativo) Questo script fa parte dello unit test. Lo script di pre-test viene eseguito prima dell'azione di test all'interno di uno unit test utilizzando il contesto autorizzato.|  
|Test|Script di test|(Obbligatorio) Questo script fa parte dello unit test. Tramite questo script si potrebbe, ad esempio, eseguire una stored procedure mediante la quale è possibile ottenere, inserire o aggiornare i valori di tabella. Viene eseguito utilizzando il contesto di esecuzione.|  
|Post-test|Script di test|(Facoltativo) Questo script fa parte dello unit test. Lo script di post-test viene eseguito dopo un singolo unit test utilizzando il contesto autorizzato.|  
|TestCleanup|Script comune (disinstallazione)|(Facoltativo) Questo script segue lo unit test. Lo script TestCleanup viene eseguito dopo tutti gli unit test in una determinata classe di test utilizzando il contesto autorizzato.|  
  
Per altre informazioni sui diversi contesti di sicurezza in cui viene eseguito ognuno di questi script, vedere [Panoramica delle stringhe di connessione e delle autorizzazioni](../ssdt/overview-of-connection-strings-and-permissions.md) e la sezione dedicata alle autorizzazioni per gli unit test di SQL Server in [Autorizzazioni necessarie per SQL Server Data Tools](../ssdt/required-permissions-for-sql-server-data-tools.md).  
  
## <a name="order-in-which-scripts-are-run"></a>Ordine in cui vengono eseguiti gli script  
L'ordine di esecuzione di ogni script è un aspetto fondamentale da considerare. Anche se l'ordine non può essere modificato, è possibile decidere quali script eseguire. La figura seguente include la selezione degli script che è possibile usare in un'esecuzione di test contenente due unit test di SQL Server e viene indicato l'ordine in cui vengono eseguiti:  
  
![Due unit test del database](../ssdt/media/twodatabaseunittests.png "Due unit test del database")  
  
> [!NOTE]  
> Se è stata configurata la distribuzione del progetto di database di SQL Server, questa operazione viene eseguita all'avvio dell'esecuzione del test, usando la stringa di connessione del contesto autorizzato. Per altre informazioni, vedere [Procedura: Configurare l'esecuzione di unit test di SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
## <a name="initialization-and-cleanup-scripts"></a>Script di inizializzazione e di disinstallazione  
Nella finestra di progettazione unit test di SQL Server gli script TestInitialize e TestCleanup vengono definiti script comuni. Nell'esempio precedente si presuppone che i due unit test facciano parte della stessa classe di test. Di conseguenza, condividono gli stessi script TestInitialize e TestCleanup. Questa situazione si verifica sempre per tutti gli unit test all'interno di una singola classe di test. Tuttavia, se nell'esecuzione di test sono contenuti unit test da classi di test differenti, gli script comune per la classe di test associata vengono eseguiti prima e dopo l'esecuzione degli unit test.  
  
Se si scrivono unit test usando solo la finestra di progettazione unit test di SQL Server, è possibile che non si abbia familiarità con il concetto di classe di test. Ogni volta che si crea uno unit test scegliendo **Nuovo test** dal menu **Test**, SQL Server Data Tools genera una classe di test. Le classi di test vengono visualizzate in **Esplora soluzioni** con il nome di test specificato, seguito dall'estensione cs o vb. All'interno di ogni classe di test i singoli unit test vengono memorizzati come metodi di test. Tuttavia, indipendentemente dal numero di metodi di test, vale a dire unit test, in ogni classe di test possono essere contenuti nessuno o uno script TestInitialize e TestCleanup.  
  
È possibile utilizzare lo script TestInitialize per preparare il database di test e lo script TestCleanup per ripristinare uno stato noto del database di test. Ad esempio, è possibile utilizzare TestInitialize per creare una stored procedure di supporto da eseguire in seguito nello script di test per testare una stored procedure diversa.  
  
## <a name="pre-test-and-post-test-scripts"></a>Script di pre-test e di post-test  
Gli script associati alle azioni di pre-test e di post-test variano probabilmente da uno unit test a quello successivo. È possibile utilizzare questi script per stabilire modifiche incrementali nel database e successivamente eliminarle.  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Uso di condizioni di test in unit test di SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
  
