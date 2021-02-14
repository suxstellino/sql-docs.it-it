---
title: Risoluzione dei problemi relativi a unit test del database di SQL Server
description: Visualizzare suggerimenti per la risoluzione dei problemi che possono verificarsi con gli unit test di SQL Server, ad esempio errori di timeout e di distribuzione di database a destinazioni impreviste.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cf4c9cd1-7e73-4c3b-922a-68b9247e7b33
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0bd87bb75255ca50ceaed8e46c356555cdedf799
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066866"
---
# <a name="troubleshooting-sql-server-database-unit-testing-issues"></a>Risoluzione dei problemi relativi a unit test del database di SQL Server

Quando si eseguono unit test di SQL Server è possibile che si verifichino i problemi descritti in questo argomento:  
  
-   [Modifiche a unit test e App.Config ignorate quando si eseguono unit test](#UnitTestingAndAppConfigChanges)  
  
-   [Distribuzione del database a una destinazione imprevista quando si eseguono unit test](#DatabaseDeploymentInUnitTests)  
  
-   [Timeout quando si eseguono unit test di database](#TimeoutsDuringUnitTests)  
  
## <a name="unit-testing-and-appconfig-changes-ignored-when-you-run-unit-tests"></a><a name="UnitTestingAndAppConfigChanges"></a>Modifiche a unit test e App.Config ignorate quando si eseguono unit test  
Se si modifica il file App.Config nel progetto di test, è necessario ricompilare tale progetto prima che le modifiche abbiano effetto. Queste modifiche includono quelle apportate al file App.Config usando la finestra di dialogo **Configurazione di test di SQL Server**. Se il progetto di test non viene ricompilato, le modifiche non verranno applicate quando si eseguono gli unit test.  
  
## <a name="database-deployment-to-unexpected-target-when-you-run-unit-tests"></a><a name="DatabaseDeploymentInUnitTests"></a>Distribuzione del database a una destinazione imprevista quando si eseguono unit test  
Se si distribuisce un database da un progetto di database quando si eseguono gli unit test, il database verrà distribuito utilizzando le informazioni sulla stringa di connessione specificate nella configurazione degli unit test. Le informazioni sulla connessione specificate nelle proprietà di debug del progetto di database non vengono usate per questa attività, quindi è possibile eseguire unit test di SQL Server su istanze diverse dello stesso database.  
  
## <a name="timeouts-when-you-run-database-unit-tests"></a><a name="TimeoutsDuringUnitTests"></a>Timeout quando si eseguono unit test di database  
Se gli unit test del database non riescono a causa di un timeout, è possibile aumentare il periodo di timeout aggiornando il file app.config nel progetto di test. Il timeout della connessione, definito nella stringa di connessione, specifica il periodo di attesa per la connessione al server dello unit test. Il timeout del comando, che deve essere definito direttamente nel file app.config, specifica il periodo di attesa per l'esecuzione dello script Transact\-SQL da parte dello unit test. In caso di problemi con gli unit test a esecuzione prolungata, provare ad aumentare il valore di timeout del comando nell'elemento di contesto appropriato. Ad esempio, per specificare un timeout del comando di 120 secondi per l'elemento **PrivilegedContext**, aggiornare il file app.config come segue:  
  
```  
<SqlUnitTesting_VS2010>  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\..\..\..\Visual Studio 2010\Projects\Database10\Database10\AdventureWorks.sqlproj"  
        Configuration="Debug" />  
    <DataGeneration ClearDatabase="true" />  
    <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(LocalDB)\Projects;Initial Catalog=AdventureWorks_Test;Integrated Security=True;Pooling=False"  
        CommandTimeout="30" />  
    <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(LocalDB)\Projects;Initial Catalog=AdventureWorks_Test;Integrated Security=True;Pooling=False"  
        CommandTimeout="120" />  
</SqlUnitTesting_VS2010>  
```  
  
## <a name="see-also"></a>Vedere anche  
[Procedura: Creare unit test di SQL Server per funzioni, trigger e stored procedure](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)  
[Procedura: Configurare l'esecuzione di unit test di SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md)  
  
