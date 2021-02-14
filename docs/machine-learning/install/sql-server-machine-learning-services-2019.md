---
title: Modifiche al meccanismo di isolamento per Windows
description: Questo articolo descrive le modifiche apportate al meccanismo di isolamento di Machine Learning Services per SQL Server 2019 in Windows. Queste modifiche hanno effetto su SQLRUserGroup, sulle regole del firewall, sulle autorizzazioni per i file e sull'autenticazione implicita.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/05/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: ace298ff0e69ef3e3ee964c15d894280d6ef1b99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082389"
---
# <a name="sql-server-2019-on-windows-isolation-changes-for-machine-learning-services"></a>SQL Server 2019 in Windows: Modifiche al meccanismo di isolamento per Machine Learning Services
[!INCLUDE [SQL Server 2019 - Windows only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Questo articolo descrive le modifiche apportate al meccanismo di isolamento di Machine Learning Services per SQL Server 2019 in Windows. Queste modifiche hanno effetto su **SQLRUserGroup**, sulle regole del firewall, sulle autorizzazioni per i file e sull'autenticazione implicita.

Per altre informazioni, vedere le istruzioni per l'installazione di [Machine Learning Services per SQL Server in Windows](sql-machine-learning-services-windows-install.md).

## <a name="changes-to-isolation-mechanism"></a>Modifiche al meccanismo di isolamento

In Windows, il programma di installazione di SQL Server 2019 introduce una modifica al meccanismo di isolamento per i processi esterni. Questa modifica sostituisce gli account di lavoro locali con [AppContainer](/windows/desktop/secauthz/appcontainer-isolation), una tecnologia di isolamento per le applicazioni client in esecuzione su Windows. 

Non sono richieste azioni specifiche dell'amministratore in seguito a questa modifica. In un server nuovo o aggiornato, tutti gli script esterni e il codice eseguito da [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) seguono automaticamente il nuovo modello di isolamento. 

In sintesi, le principali differenze di questa versione sono le seguenti:

+ Gli account utente locali nel **gruppo di utenti con restrizioni SQL (SQLRUserGroup)** non vengono più creati o usati per eseguire processi esterni. In alternativa a questi vengono usati ambienti AppContainer.
+ L'appartenenza a **SQLRUserGroup** è cambiata. Invece di più account utente locali, solo l'account del servizio Launchpad di SQL Server appartiene a questo gruppo. I processi R e Python vengono ora eseguiti con l'identità del servizio Launchpad, isolati tramite AppContainer.

Anche se il modello di isolamento è cambiato, l'Installazione guidata e i parametri della riga di comando rimangono gli stessi in SQL Server 2019. Per informazioni sull'installazione, vedere [Installare Machine Learning Services per SQL Server](sql-machine-learning-services-windows-install.md).

## <a name="about-appcontainer-isolation"></a>Informazioni sull'isolamento tramite AppContainer

Nelle versioni precedenti, **SQLRUserGroup** contiene un pool di account utente di Windows locali (MSSQLSERVER00-MSSQLSERVER20) che vengono usati per isolare ed eseguire processi esterni. Quando è necessario un processo esterno, il servizio Launchpad di SQL Server acquisisce un account disponibile e lo usa per eseguire un processo. 

In SQL Server 2019, il programma di installazione non crea più account di lavoro locali. L'isolamento viene invece ottenuto tramite [AppContainer](/windows/desktop/secauthz/appcontainer-isolation). In fase di esecuzione, quando viene rilevato uno script o del codice incorporato in una stored procedure o in una query, SQL Server chiama Launchpad con una richiesta di un'utilità di avvio specifica dell'estensione. Launchpad richiama l'ambiente di runtime appropriato in un processo con la propria identità e crea un'istanza di un ambiente AppContainer per contenerlo. Questa modifica è utile perché la gestione degli account locali e delle password non è più necessaria. Inoltre, nelle installazioni in cui non sono consentiti account utente locali, l'eliminazione della dipendenza dall'account utente locale significa che è ora possibile usare questa funzionalità.

In base all'implementazione in SQL Server, gli ambienti AppContainer sono meccanismi interni. Anche se non si può avere una prova fisica degli ambienti AppContainer nel monitoraggio dei processi, questi sono presenti nelle regole del firewall in uscita create dal programma di installazione per impedire ai processi di eseguire chiamate di rete.

## <a name="firewall-rules-created-by-setup"></a>Regole del firewall create dal programma di installazione

Per impostazione predefinita, SQL Server disabilita le connessioni in uscita creando regole del firewall. Nelle versioni precedenti, queste regole sono basate sugli account utente locali, in cui il programma di installazione ha creato una regola in uscita per **SQLRUserGroup** che nega l'accesso alla rete ai propri membri. Ogni account di lavoro è elencato come principio locale soggetto alla regola. 

In seguito al passaggio alla tecnologia AppContainer, sono disponibili nuove regole del firewall basate sui SID di AppContainer: una per ognuno dei 20 AppContainer creati dal programma di installazione di SQL Server. Per il nome della regola del firewall viene usata la convenzione di denominazione **Blocca accesso alla rete per AppContainer-00 nell'istanza di SQL Server MSSQLSERVER**, dove 00 è il numero dell'ambiente AppContainer (00-20 per impostazione predefinita) e MSSQLSERVER è il nome dell'istanza di SQL Server. 

> [!Note]
> Se sono necessarie chiamate di rete, è possibile disabilitare le regole in uscita in Windows Firewall.

<a name="file-permissions"></a>

## <a name="file-permissions"></a>Autorizzazioni per i file

Per impostazione predefinita, gli script Python e R esterni hanno solo le autorizzazioni di accesso in lettura alle directory di lavoro. 

Se gli script Python o R devono accedere a un'altra directory, è necessario concedere le autorizzazioni **Lettura ed esecuzione** e/o **Scrittura** all'account utente del servizio **NT Service\MSSQLLaunchpad** e a **TUTTI I PACCHETTI APPLICAZIONI** in questa directory.

Seguire questa procedura per concedere l'accesso.

1. In Esplora file fare clic con il pulsante destro del mouse sulla cartella che si vuole usare come directory di lavoro e scegliere **Proprietà**.
1. Selezionare **Sicurezza** e fare clic su **Modifica** per modificare le autorizzazioni.
1. Fare clic su **Aggiungi**.
1. Verificare che **Da questo percorso** sia il nome del computer locale.
1. Immettere **TUTTI I PACCHETTI APPLICAZIONI** in **Immettere i nomi degli oggetti da selezionare** e fare clic su **Controlla nomi**. Fare clic su **OK**.
1. Selezionare **Lettura ed esecuzione** nella colonna **Consenti**.
1. Selezionare **Scrittura** nella colonna **Consenti** se si vogliono concedere le autorizzazioni di scrittura.
1. Fare clic su **OK** e di nuovo su **OK**.

### <a name="program-file-permissions"></a>Autorizzazioni sui file di programma

Come nelle versioni precedenti, **SQLRUserGroup** continua a fornire autorizzazioni di lettura ed esecuzione sui file eseguibili nelle directory di SQL Server **Binn**, **R_SERVICES** e **PYTHON_SERVICES**. In questa versione, l'unico membro di **SQLRUserGroup** è l'account del servizio Launchpad di SQL Server.  Quando questo servizio avvia un ambiente di esecuzione R o Python, il processo viene eseguito come servizio Launchpad.

## <a name="implied-authentication"></a>Autenticazione implicita

Come in precedenza, è ancora necessaria una configurazione aggiuntiva per l'*autenticazione implicita* nei casi in cui lo script o il codice deve riconnettersi a SQL Server usando l'autenticazione trusted per recuperare dati o risorse. Questa configurazione aggiuntiva comporta la creazione di un account di accesso al database per **SQLRUserGroup**, il cui unico membro è ora il singolo account del servizio Launchpad di SQL Server in sostituzione di più account di lavoro. Per altre informazioni su questa attività, vedere [Aggiungere SQLRUserGroup come utente del database](../security/create-a-login-for-sqlrusergroup.md).


## <a name="symbolic-link-created-by-setup"></a>Collegamento simbolico creato dal programma di installazione

Come parte dell'installazione di SQL Server viene creato un collegamento simbolico alle directory predefinite correnti **R_SERVICES** e **PYTHON_SERVICES**. Se non si vuole creare questo collegamento, in alternativa è possibile concedere l'autorizzazione di lettura di "tutti i pacchetti applicazioni" sulla struttura gerarchica fino alla cartella.


## <a name="see-also"></a>Vedere anche

+ [Installare Machine Learning Services per SQL Server in Windows](sql-machine-learning-services-windows-install.md)
+ [Installare Machine Learning Services per SQL Server in Linux](../../linux/sql-server-linux-setup-machine-learning.md)