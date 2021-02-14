---
description: Estrarre un'applicazione livello dati da un database
title: Estrarre un'applicazione livello dati da un database | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.extractdacwizard.validationandsummary.f1
- sql13.swb.extractdacwizard.introduction.f1
- sql13.swb.extractdacwizard.selectdatapage.f1
- sql13.swb.extractdacwizard.setdacproperties.f1
- sql13.swb.extractdacwizard.buildandsave.f1
helpviewer_keywords:
- extract DAC
- How to [DAC], extract
- data-tier application [SQL Server], extract
- wizard [DAC], extract
ms.assetid: ae52a723-91c4-43fd-bcc7-f8de1d1f90e5
author: stevestein
ms.author: sstein
ms.openlocfilehash: 5091c3437d0e74f3c108e13efb3789997cadb2d3
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489465"
---
# <a name="extract-a-dac-from-a-database"></a>Estrarre un'applicazione livello dati da un database
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Usare la **procedura guidata Estrai applicazione livello dati** o uno script di Windows PowerShell per estrarre un pacchetto di applicazione livello dati da un database di SQL Server esistente. Il processo di estrazione crea un file di pacchetto DAC che contiene le definizioni degli oggetti di database e i relativi elementi a livello di istanza. Ad esempio, un file di pacchetto di applicazione livello dati contiene tutte le tabelle di database, le stored procedure, le viste e gli utenti, nonché gli account di accesso che eseguono il mapping agli utenti del database.  
  
 
## <a name="before-you-begin"></a>Prima di iniziare  
 È possibile estrarre un'applicazione livello dati dai database che risiedono in istanze di [!INCLUDE[ssSDS](../../includes/sssds-md.md)], o [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 4, o versioni successive. Se il processo di estrazione viene eseguito rispetto a un database distribuito da un'applicazione livello dati, vengono estratte solo le definizioni degli oggetti nel database. Il processo non fa riferimento all'applicazione livello dati registrata in **msdb** (**master** in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]). Il processo di estrazione non consente di registrare la definizione DAC nell'istanza corrente del motore di database. Per ulteriori informazioni sulla registrazione di un'applicazione livello dati, vedere [Register a Database As a DAC](../../relational-databases/data-tier-applications/register-a-database-as-a-dac.md).  
  
##  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> Limitazioni e restrizioni  
 Un'applicazione livello dati può essere estratta solo da un database in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]o [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) o versioni successive. Non è possibile estrarre un'applicazione livello dati se il database include oggetti non supportati nell'applicazione livello dati o utenti contenuti. Per ulteriori informazioni sui tipi di oggetti supportati in un'applicazione livello dati, vedere [DAC Support For SQL Server Objects and Versions](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110)).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 L'estrazione di un'applicazione livello dati richiede almeno le autorizzazioni ALTER ANY LOGIN e VIEW DEFINITION nell'ambito del database, oltre alle autorizzazioni SELECT su **sys.sql_expression_dependencies**. L'estrazione di un'applicazione del livello dati può essere effettuata da membri del ruolo predefinito del server securityadmin che sono anche membri del ruolo predefinito del database database_owner nel database dal cui viene estratta l'applicazione del livello dati. Possono estrarre un'applicazione livello dati anche i membri del ruolo predefinito del server sysadmin o dell'account amministratore di sistema SQL Server predefinito denominato **sa** .  
  
##  <a name="using-the-extract-data-tier-application-wizard"></a><a name="UsingDACExtractWizard"></a> Utilizzo della Procedura guidata Estrai applicazione livello dati  
 **Per estrarre un'applicazione livello dati utilizzando una procedura guidata**  
  
1.  In **Esplora oggetti**, espandere il nodo dell'istanza contenente il database da cui è necessario estrarre l'applicazione livello dati.  
  
2.  Espandere il nodo di **Database** .  
  
3.  Fare clic con il pulsante destro del mouse sul nodo del database da cui si deve estrarre l'applicazione livello dati, scegliere **Attività** e quindi selezionare **Estrai applicazione livello dati**.  
  
4.  Completare le finestre di dialogo della procedura guidata.  
  
    1.  [Pagina Introduzione](#Introduction)  
  
    2.  [Pagina Seleziona dati](#SelectData)  
  
    3.  [Pagina Imposta proprietà](#SetProperties)  
  
    4.  [Pagina Convalida e riepilogo](#ValidateSummary)  
  
    5.  [Pagina Compila pacchetto](#BuildPackage)  
  
###  <a name="wizard-introduction-page"></a><a name="Introduction"></a> Pagina introduttiva della procedura guidata  
 In questa pagina vengono descritti i passaggi per l'estrazione di un'applicazione livello dati.  
  
 **Non visualizzare più questa pagina** - Fare clic sulla casella di controllo per evitare che la pagina venga visualizzata nuovamente in futuro.  
  
 **Avanti >**: consente di passare alla pagina **Seleziona metodo**.  
  
 **Annulla** : termina la procedura guidata senza estrarre un'applicazione livello dati dal database.  
  
 [&#91;Estrazione guidata&#93;](#UsingDACExtractWizard)  
  
###  <a name="select-data-page"></a><a name="SelectData"></a> Select data page  
Selezionare i dati di riferimento che si vuole includere nel file del pacchetto di applicazione livello dati. L'inserimento di dati nel pacchetto di applicazione livello dati è facoltativo. Il pacchetto di applicazione livello dati include già lo schema di tutti gli oggetti di database e gli oggetti istanza supportati correlati al database.  
  
 È possibile includere un massimo di 10 MB di dati di riferimento nel file di pacchetto di applicazione livello dati. Per poter essere incluse nell'applicazione livello dati, tuttavia, le tabelle non devono contenere tipi di dati BLOB (Binary Large Object, oggetto binario di grandi dimensioni), ad esempio **image** o **varchar(max)**. Per estrarre maggiori quantità di dati per il trasferimento a un altro database, utilizzare SQL Server Integration Services, l'utilità di copia bulk o una delle numerose altre tecniche di migrazione dei dati.  
  
 **Tabella di database** : selezionare la casella di controllo accanto alle tabelle di database contenenti i dati da includere nel pacchetto di applicazione livello dati. È possibile selezionare fino a dieci tabelle contenenti un massimo di 10.000 righe.  
  
 [&#91;Estrazione guidata&#93;](#UsingDACExtractWizard)  
  
###  <a name="set-properties-page"></a><a name="SetProperties"></a> Set properties page  
 Utilizzare questa pagina della procedura guidata per descrivere l'applicazione livello dati. Queste proprietà vengono utilizzate per identificare l'applicazione livello dati e distinguerla dalle altre.  
  
 **Nome** identifica l'applicazione livello dati. Può essere diverso dal nome del file del pacchetto di applicazione livello dati e deve descrivere l'applicazione. Ad esempio, se il database viene utilizzato per un'applicazione finanziaria, si può chiamare DAC Finanza.  
  
 **Versione (usare xx.xx.xx.xx, dove x è un numero)** : valore numerico che identifica la versione dell'applicazione livello dati. La versione DAC viene utilizzata in Visual Studio per identificare la versione della DAC alla quale stanno lavorando gli sviluppatori. Quando si distribuisce un'applicazione livello dati, la versione viene archiviata nel database **msdb** e può essere visualizzata successivamente nel nodo **Applicazioni livello dati** di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Descrizione** : facoltativa. Viene descritta l'applicazione livello dati. Quando si distribuisce un'applicazione livello dati, la descrizione viene archiviata nel database **msdb** e può essere visualizzata successivamente nel nodo **Applicazioni livello dati** di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 **Salva nel file del pacchetto di applicazione livello dati (nome file con estensione dacpac):** salva l'applicazione livello dati in un file del pacchetto di applicazione livello dati, con estensione dacpac. Fare clic sul pulsante **Sfoglia** per specificare il nome e il percorso del file.  
  
 **Sovrascrivi file esistente** : selezionare questa casella di controllo per sostituire il file del pacchetto di applicazione livello dati se ne esiste già uno con lo stesso nome.  
  
###  <a name="validation-and-summary-page"></a><a name="ValidateSummary"></a> Validation and summary page  
 In questa pagina della procedura guidata viene verificato che tutti gli oggetti di database siano supportati da un'applicazione livello dati. Controlla inoltre le dipendenze attraverso gli oggetti di database per determinare il set di oggetti che possono essere inclusi correttamente nella DAC. Successivamente, visualizza il report di convalida e riepiloga le opzioni selezionate nella procedura guidata. Per modificare un'opzione, fare clic su **Indietro**. Per avviare l'estrazione di un'applicazione livello dati, fare clic su **Avanti**.  
  
> **NOTA.** Se uno o più oggetti non sono supportati da un'applicazione livello dati, il pulsante **Avanti** è disabilitato e il processo di estrazione non può continuare. In tal caso, è consigliabile rimuovere gli oggetti non supportati ed eseguire nuovamente la procedura guidata.  
  
 **Riepilogo** n riepilogo delle opzioni selezionate viene visualizzato in **Proprietà DAC**. I risultati della convalida vengono elencati in **Oggetti DAC**. La convalida può dare tre tipi di risultati:  
  
-   **Oggetti inclusi correttamente in DAC**- Questi oggetti e le loro dipendenze sono supportati e possono essere inclusi correttamente nella DAC.  
  
-   **Oggetti inclusi in DAC con avvisi**- Questi oggetti sono supportati, ma dipendono da altri oggetti che non sono supportati in una DAC.  
  
-   **Oggetti non inclusi in DAC**- Questi oggetti non sono supportati e devono essere rimossi dal database prima di estrarre correttamente una DAC.  
  
 Il processo di convalida controlla più livelli delle dipendenze. Ad esempio, se una stored procedure dipende da una tabella che utilizza il tipo di dati CLR non supportato, la stored procedure sarà elencata sotto **Oggetti inclusi in DAC con avvisi**.  
  
 Se uno o più oggetti non sono supportati da una DAC, il pulsante **Avanti** è disabilitato e il processo di estrazione non può continuare. In tal caso, è consigliabile rimuovere gli oggetti non supportati ed eseguire nuovamente questa procedura guidata.  
  
 **Salva report** : consente di salvare nel riepilogo un file basato su HTML che elenca tutti gli oggetti sotto il nodo **Oggetti DAC** . Questo report può essere utile quando alcuni oggetti di database non sono supportati in una DAC. Utilizzare il report per modificare o rimuovere gli oggetti che non sono supportati, prima di tentare di estrarre nuovamente la DAC.  
  
 ###  <a name="build-package-page"></a><a name="BuildPackage"></a> Build package page  
 Utilizzare questa pagina per monitorare l'avanzamento della procedura guidata durante l'estrazione dell'applicazione livello dati.  
  
 **Azione** : durante l'azione **Crea e salva il file del pacchetto di applicazione livello dati** , la procedura guidata estrae un'applicazione livello dati dal database SQL Server. Quindi, nella memoria viene creato un pacchetto di applicazione livello dati che viene salvato nel percorso specificato. Fare clic sui collegamenti nella colonna **Risultato** per vedere il risultato del passaggio corrispondente.  
  
 **Salva report** fare clic per salvare i risultati dello stato della procedura guidata in un file.  
  
 **Fine** fare clic per chiudere la procedura guidata una volta completata o se si verifica un errore.  
   
##  <a name="extract-a-dac-using-powershell"></a><a name="ExtractDACPowerShell"></a> Estrarre un'applicazione livello dati con PowerShell  
 **Per estrarre un'applicazione livello dati da un database con il metodo Extract() in uno script di PowerShell**  
  
1.  Creare un oggetto server SMO e impostarlo sull'istanza contenente il database da cui si desidera estrarre l'applicazione livello dati.  
  
2.  Aggiungere una variabile che specifichi il nome del database.  
  
3.  Specificare i metadati per l'applicazione livello dati, quali nome dell'applicazione livello dati, versione e descrizione.  
  
4.  Specificare il percorso e il nome per il file del pacchetto di applicazione livello dati.  
  
5.  Eseguire il metodo Extract con le informazioni specificate in precedenza.  
  
### <a name="example-powershell"></a>Esempio (PowerShell)  
 L'esempio seguente estrae un'applicazione livello dati denominata MyApplication da un database denominato MyDB.  
  
```  
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$srv = get-item .  
  
## Specify the database to extract to a DAC.  
$dbname = "MyDB"  
  
## Specify the DAC metadata.  
$applicationname = "MyApplication"  
$version = "1.0.0.0"  
$description = "This DAC defines the database used by my application."  
  
## Specify the location and name for the extracted DAC package.  
$dacpacPath = "C:\MyDACs\MyApplication.dacpac"  
  
## Extract the DAC.  
$extractionunit = New-Object Microsoft.SqlServer.Management.Dac.DacExtractionUnit($srv, $dbname, $applicationname, $version)  
$extractionunit.Description = $description  
$extractionunit.Extract($dacpacPath)  
```  
  
## <a name="see-also"></a>Vedere anche  
 - [Applicazioni livello dati](../../relational-databases/data-tier-applications/data-tier-applications.md) 
 - [SqlPackage nelle pipeline di sviluppo](../../tools/sqlpackage/sqlpackage-pipelines.md)