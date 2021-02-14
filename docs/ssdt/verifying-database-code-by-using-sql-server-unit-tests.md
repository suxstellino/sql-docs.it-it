---
title: Verifica del codice di database tramite unit test di SQL Server
description: Informazioni sugli unit test di SQL Server. Scoprire quali edizioni di Visual Studio forniscono supporto per i test e visualizzare risorse sull'uso dei test per verificare il codice del database.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 003713e2-de6b-4277-a0a8-7d1f2f4ffb39
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 4c05b9e3a3330b395ee3cd8c2cb3da5aa7d50508
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066699"
---
# <a name="verifying-database-code-by-using-sql-server-unit-tests"></a>Verifica del codice di database tramite unit test di SQL Server

È possibile usare unit test di SQL Server per definire uno stato di base del database e verificare quindi eventuali modifiche apportate successivamente agli oggetti di database.  
  
Per definire uno stato di base per un database, è possibile creare un progetto di test e scrivere set di istruzioni Transact\-SQL che operano sugli oggetti di database. Con questi test è possibile verificare in un ambiente di sviluppo isolato se gli oggetti funzionano come previsto. Gli unit test di SQL Server sono efficaci in combinazione con lo sviluppo di database offline tramite progetti di database di SQL Server. Per altre informazioni, vedere [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md). Dopo aver definito un set di base di unit test di SQL Server, è possibile usare tali test per verificare il corretto funzionamento del database prima di archiviare le modifiche nel controllo delle versioni.  
  
È possibile creare test per verificare le modifiche apportate a qualsiasi oggetto di database. Inoltre, è possibile generare automaticamente stub di codice Transact\-SQL per testare le funzioni, i trigger e le stored procedure del database.  
  
> [!NOTE]  
> È possibile creare ed eseguire unit test di SQL Server anche se non è stato aperto alcun progetto di database. Tuttavia, se si desidera generare automaticamente script di test per testare oggetti di database specifici del progetto, è necessario aprire il progetto di database contenente gli oggetti che si desidera verificare.  
  
Se un membro del team modifica lo schema del database, è possibile utilizzare questi test per verificare se le modifiche hanno interferito con le funzionalità esistenti. Si possono creare unit test di SQL Server a integrazione degli unit test del software creati dagli sviluppatori del software. È necessario completare entrambi i set di test per verificare il comportamento complessivo dell'applicazione.  
  
Tramite gli unit test è possibile verificare che le procedure vengano eseguite correttamente quando è previsto che abbiano esito positivo e che non vengano completate in caso contrario. Le verifiche della presenza degli errori previsti sono note come test negativi.  
  
## <a name="visual-studio-editions-support-for-sql-server-unit-tests"></a>Edizioni di Visual Studio supportate per gli unit test di SQL Server  
La funzionalità di unit test di SQL Server, aggiunta nell'aggiornamento di dicembre 2012 di SQL Server Data Tools, consente di creare, modificare ed eseguire unit test di SQL Server in Visual Studio 2010 Professional e Visual Studio 2012 Professional ed edizioni superiori.  
  
Per assicurarsi di installare l'aggiornamento più recente di SQL Server Data Tools, accedere alla [finestra di dialogo Verifica disponibilità aggiornamenti](../ssdt/check-for-updates-dialog-box.md).  
  
La shell di SQL Server Data Tools integrata in Visual Studio 2010 e Visual Studio 2012 non supporta unit test di SQL Server.  
  
## <a name="common-tasks"></a>Attività comuni  
Nella tabella seguente sono incluse le descrizioni di attività comuni che supportano questo scenario e vengono forniti i collegamenti a ulteriori informazioni su come completare queste attività.  
  
|Attività comuni|Contenuto di supporto|  
|----------------|----------------------|  
|**Esercitazione pratica:** è possibile seguire una procedura dettagliata introduttiva per acquisire familiarità con le modalità di creazione ed esecuzione di un semplice unit test di SQL Server. Questa procedura dettagliata include un esempio di unit test negativo di SQL Server.|[Procedura dettagliata: Creazione ed esecuzione di uno unit test di SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)|  
|**Definire unit test di SQL Server:** è necessario creare unit test di SQL Server nel proprio progetto. Configurare le impostazioni per il progetto in questione e definire una o più condizioni di test per ogni test.|[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)<br /><br />[Uso di condizioni di test in unit test di SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)|  
|**Eseguire unit test di SQL Server:** dopo aver definito uno o più unit test, eseguirli, eseguire il debug di eventuali problemi ed esaminare i risultati del test.|[Esecuzione di unit test di SQL Server](../ssdt/running-sql-server-unit-tests.md)|  
|**Gestire gruppi di test (Visual Studio 2010):** è possibile organizzare in gruppi i test che in genere è opportuno eseguire contemporaneamente. Gli elenchi di test sono comunque supportati, ma per i nuovi gruppi di test è consigliabile utilizzare invece le categorie di test. È possibile, ad esempio, creare una categoria di test per i test destinati ai trigger o a tutti gli oggetti in un particolare *schema*.|[Definizione di categorie per raggruppare i test](/previous-versions/visualstudio/visual-studio-2010/dd286595(v=vs.100))<br /><br />[Definizione di elenchi di test per raggruppare i test](/previous-versions/visualstudio/visual-studio-2010/dd286584(v=vs.100))|  
|**Archiviare i progetti di test e i test nel controllo delle versioni:** dopo aver eseguito i test e verificato se funzionano correttamente, è consigliabile archiviare il progetto di test e tutti i file associati nel controllo delle versioni, in modo che tutti i membri del team possano eseguire i test. Archiviando il progetto di test nel controllo delle versioni insieme al progetto di database di SQL Server è possibile ripristinare facilmente le versioni compatibili sia del database che dei test per il database.|[Aggiungere file al controllo della versione](/previous-versions/visualstudio/visual-studio-2010/ms181374(v=vs.100))<br /><br />[Uso delle finestre Archivia e Modifiche in sospeso](/previous-versions/visualstudio/visual-studio-2010/ms245462(v=vs.100))|  
|**Definire condizioni di test personalizzate:** è possibile creare condizioni di test personalizzate per poter verificare comportamenti non previsti nel set predefinito di condizioni di test. È necessario distribuire queste condizioni a tutti i membri del team che desiderano eseguire i test in cui vengono utilizzate le nuove condizioni.|[Scenario: Definire condizioni di test personalizzate per unit test di SQL Server](/previous-versions/visualstudio/visual-studio-2010/dd193282(v=vs.100))|  
|**Aggiornare unit test esistenti**: se si dispone di unit test di database creati in una versione precedente di Visual Studio, è necessario aggiornarli prima che vengano compilati ed eseguiti correttamente con questa versione.<br /><br />**NOTA:** se si apre una soluzione contenente sia un progetto di database sia un progetto di unit test di database da una versione precedente di Visual Studio, verrà richiesto di aggiornare il progetto di database. Verrà richiesto di aggiornare i progetti di unit test di database. L'aggiornamento deve essere eseguito manualmente.|[Aggiornare un progetto di test precedente contenente unit test del database](../ssdt/upgrade-an-older-test-project-containing-database-unit-tests.md)|  
|**Estendibilità:** è possibile estendere SQL Server Data Tools tramite la creazione di estensioni di funzionalità.|[Condizioni di test personalizzate per unit test di SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)|  
|**Risolvere i problemi:** è possibile acquisire altre informazioni sulla risoluzione dei problemi comuni relativi agli unit test di SQL Server.|[Risoluzione dei problemi relativi a unit test del database di SQL Server](../ssdt/troubleshooting-sql-server-database-unit-testing-issues.md)|  
  
## <a name="related-scenarios"></a>Scenari correlati  
[Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md)  
Gli unit test di database sono particolarmente efficaci se usati insieme allo sviluppo di progetti offline tramite i progetti di database di SQL Server.  
  
## <a name="see-also"></a>Vedere anche  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
