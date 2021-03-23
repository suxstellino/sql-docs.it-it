---
description: Utilizzo di Copia guidata database
title: Usare Copia guidata database | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.cdw.packageconfiguration.f1
- sql13.swb.cdw.schedule.f1
- sql13.swb.cdw.transfermethod.f1
- sql13.swb.cdw.databases.f1
- sql13.swb.cdw.destinationconfiguration.f1
- sql13.swb.cdw.destination.f1
- sql13.swb.cdw.locationofsourcedbfiles.f1
- sql13.swb.cdw.source.f1
- sql13.swb.cdw.selectdatabaseobjects.f1
- sql13.swb.cdw.complete.f1
- sql13.swb.cdw.welcome.f1
helpviewer_keywords:
- Copy Database Wizard
- starting Copy Database Wizard
ms.assetid: 7a999fc7-0a26-4a0d-9eeb-db6fc794f3cb
author: stevestein
ms.author: sstein
ms.openlocfilehash: 5f07a7966c7f4384292e49c10645332d0844c9c6
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890690"
---
# <a name="use-the-copy-database-wizard"></a>Utilizzo di Copia guidata database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Copia guidata database consente di spostare o copiare facilmente database e determinati oggetti server da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  a un'altra istanza, senza tempi di inattività del server. Mediante questa procedura guidata è possibile effettuare le operazioni seguenti: 
  
-   Selezionare un server di origine e uno di destinazione.  
  
-   Selezionare i database da spostare o copiare.  
  
-   Specificare il percorso di file per i database.  
  
-   Copiare account di accesso nel server di destinazione.  
  
-   Copiare ulteriori oggetti di supporto, processi, stored procedure definite dall'utente e messaggi di errore.  
  
-   Pianificare lo spostamento o la copia dei database.  
  

##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   La Copia guidata database non è disponibile nell'edizione Express.  
  
-   Non è possibile usare Copia guidata database per copiare o spostare i database che:
  
    -   Sono sistemi.
  
    -   Sono contrassegnati per la replica.
  
    -   Sono contrassegnati come inaccessibili, offline o sospetti o in modalità di emergenza.
    
    -   Hanno file di dati o di log archiviati in Archiviazione di Microsoft Azure.
  
-   Se si usano [FileTable](../../relational-databases/blob/filetables-sql-server.md), non è possibile usare la Copia guidata database nello stesso server, perché la procedura guidata usa lo stesso nome di directory.

-   Un database non può essere spostato o copiato in una versione precedente di SQL Server.
  
-   Se si seleziona l'opzione **Sposta** , il database di origine verrà automaticamente eliminato dopo lo spostamento del database. Se si seleziona l'opzione **Copia** , il database di origine non verrà eliminato.  Inoltre, gli oggetti server selezionati vengono copiati anziché spostati nella destinazione; il database è l'unico oggetto che viene spostato.
  
-   Se si usa il metodo SMO ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects) per spostare il catalogo full-text, è necessario ripopolare l'indice dopo lo spostamento.  
  
-   Il metodo di **collegamento e scollegamento** consente di scollegare il database, spostare o copiare i file con estensione MDF, NDF e LDF del database e ricollegare quest'ultimo nella nuova posizione. Per **questo metodo** non è possibile collegare sessioni attive al database da spostare o copiare per evitare la perdita o l'incoerenza dei dati. Nel caso del metodo SMO ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Object), l'utilizzo delle sessioni attive è consentito, poiché il database non viene mai portato offline.  

-    Il trasferimento dei processi di SQL Server Agent che fanno riferimento a database non ancora esistenti nel server di destinazione causa l'esito negativo dell'intera operazione.  La procedura guidata prova a creare un processo di SQL Server Agent prima di creare il database.  Soluzione alternativa:
     1. Nel server di destinazione creare uno scheletro di database con lo stesso nome del database da copiare o spostare.  Vedere [Creare un database](../../relational-databases/databases/create-a-database.md).
     
     2. Dalla pagina **Configurare il database di destinazione** selezionare **Elimina il database con lo stesso nome nel server di destinazione, quindi continua il trasferimento sovrascrivendo i file di database esistenti**.

> **IMPORTANTE** Se si usa il metodo di **collegamento e scollegamento** , la proprietà dei database di origine e di destinazione viene impostata sui dati di accesso che eseguono **Copia guidata database**.  Vedere [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md) per modificare la proprietà di un database.
  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
-   Assicurarsi che SQL Server Agent sia stato avviato nel server di destinazione.  

-   Verificare che le directory dei dati e dei log nel server di origine siano raggiungibili dal server di destinazione.

-   Nel metodo **di collegamento scollegamento** un proxy di SQL Server Agent per il sottosistema SSIS deve esistere nel server di destinazione con credenziali che possono accedere al file system dei server di origine e destinazione. Per altre informazioni sui proxy, vedere [Creare un proxy di SQL Server Agent](~/ssms/agent/create-a-sql-server-agent-proxy.md).

> **IMPORTANTE** Nel metodo **di collegamento e scollegamento** il processo di copia o spostamento avrà esito negativo se non viene usato un account proxy di Integration Services.  In alcuni casi il database di origine non verrà riassociato al server di origine e tutte le autorizzazioni di protezione NTFS verranno rimosse dai file di dati e di log.  In questo caso, passare ai file, riapplicare le autorizzazioni rilevanti e quindi ricollegare il database all'istanza di SQL Server.
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Per garantire prestazioni ottimali del database aggiornato, eseguire [sp_updatestats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md) (aggiornamento delle statistiche) sul database aggiornato.  
  
-   Quando si sposta o si copia un database in un'altra istanza del server, per garantire un uso coerente a utenti e applicazioni, potrebbe essere necessario ricreare nell'altra istanza del server alcuni o tutti i metadati per il database, ad esempio account di accesso e processi. Per altre informazioni, vedere [Gestione dei metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  

  
###  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario essere membri del ruolo predefinito del server **sysadmin** sia nel server di origine sia in quello di destinazione.  
  
##  <a name="the-copy-database-wizard-pages"></a><a name="Overview"></a> Pagine della Copia guidata database 
Avviare **Copia guidata database** in SQL Server Management Studio da **Esplora oggetti** ed espandere **Database**.  Quindi, fare clic con il pulsante destro del mouse su un database, scegliere **Attività** e quindi **Copia database**.  Se viene visualizzata la pagina iniziale **Copia guidata database** , fare clic su **Avanti**.


### <a name="select-a-source-server"></a>Selezionare un server di origine
Consente di specificare il server in cui si trova il database da spostare o copiare e di immettere le informazioni di accesso.  Dopo aver selezionato il metodo di autenticazione e immesso le informazioni per l'accesso, fare clic su **Avanti** per stabilire la connessione al server di origine.  La connessione rimane aperta durante tutta la sessione.

-    **Server di origine**  
Consente di identificare il nome del server in cui si trovano i database che si vuole spostare o copiare.  Immettere manualmente oppure fare clic sui puntini di sospensione per passare al server desiderato.  La versione del server deve essere almeno SQL Server 2005.

-    **Usa autenticazione di Windows**  
Consente a un utente di connettersi usando un account utente di Microsoft Windows.

-    **Usa autenticazione di SQL Server**  
Consente all'utente di connettersi specificando una password e un nome utente di Autenticazione di SQL Server.

     -    **Nome utente**  
Consente di immettere il nome utente per la connessione. Questa opzione è disponibile solo se si è scelto di usare **Autenticazione di SQL Server** per la connessione.

     -    **Password**  
Consente di immettere la password per l'accesso. Questa opzione è disponibile solo se si è scelto di usare **Autenticazione di SQL Server** per la connessione.

###  <a name="select-a-destination-server"></a>Selezionare un server di destinazione
Consente di specificare il server in cui il database verrà spostato o copiato.  Se si impostano i server di origine e di destinazione sulla stessa istanza del server, verrà eseguita una copia del database.  In questo caso, è necessario rinominare il database in un punto successivo della procedura guidata.  È possibile utilizzare il nome del database di origine per il database copiato o spostato solo se non si verificano conflitti di nome nel server di destinazione.  Se sono presenti conflitti di nome, è necessario risolverli manualmente nel server di destinazione per potervi utilizzare il nome del database di origine.
  
-    **Server di destinazione**  
Consente di identificare il nome del server in cui verranno i database verranno spostati o copiati.  Immettere manualmente oppure fare clic sui puntini di sospensione per passare al server desiderato.  La versione del server deve essere almeno SQL Server 2005.

     >**NOTA:** è possibile usare una destinazione costituita da un server di cluster. La Copia guidata database permette di selezionare solo unità condivise in un server di destinazione di cluster.

-    **Usa autenticazione di Windows**  
Consente a un utente di connettersi usando un account utente di Microsoft Windows.

-    **Usa autenticazione di SQL Server**  
Consente all'utente di connettersi specificando una password e un nome utente di Autenticazione di SQL Server.

     -    **Nome utente**  
Consente di immettere il nome utente per la connessione. Questa opzione è disponibile solo se si è scelto di usare **Autenticazione di SQL Server** per la connessione.

     -    **Password**  
Consente di immettere la password per l'accesso. Questa opzione è disponibile solo se si è scelto di usare **Autenticazione di SQL Server** per la connessione.

###  <a name="select-the-transfer-method"></a>Selezionare il metodo di trasferimento
 
-    **Usa metodo di collegamento e scollegamento**  
Consente di scollegare il database dal server di origine, copiare i file di database (con estensione mdf, ndf e ldf) nel server di destinazione e collegare il database nel server di destinazione. Questo è in genere il metodo più rapido poiché il lavoro principale è rappresentato dalla lettura del disco di origine e dalla scrittura del disco di destinazione. Per creare oggetti strutture di archiviazione dei dati o oggetti all'interno del database non è necessaria alcuna logica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se il database contiene molto spazio allocato ma inutilizzato, questo metodo può risultare più lento. Ad esempio, nel caso di un database nuovo e quasi vuoto creato allocando 100 MB, vengono copiati tutti i 100 MB anche se nel database sono effettivamente utilizzati solo 5 MB.

     > **NOTA:** questo metodo rende il database non disponibile per gli utenti durante il trasferimento.

     -    **In caso di errore, ricollega il database di origine**  
     Quando un database viene copiato, i rispettivi file originali vengono sempre ricollegati al server di origine. Utilizzare questa casella per ricollegare i file originali al database di origine se non è possibile completare lo spostamento di un database. 
  
-    **Usa metodo SMO (SQL Management Objects)**  
     Questo metodo consente di leggere le definizioni di ogni oggetto di database nel database di origine e di creare ciascun oggetto nel database di destinazione. Consente quindi di trasferire i dati dalle tabelle di origine a quelle di destinazione, ricreando gli indici e i metadati.
  
     > [!NOTE]
     >  Gli utenti del database possono continuare ad accedere al database durante il trasferimento. 
  
###  <a name="select-database"></a>Seleziona database
Selezionare i database che si vuole spostare o copiare dal server di origine al server di destinazione.  Vedere [Limitazioni e restrizioni](#Restrictions) all'inizio dell'argomento.  
  
-    **Spostamento**  
Consente di spostare il database nel server di destinazione.

-    **Copia**  
Consente di copiare il database nel server di destinazione.

-    **Origine**  
Consente di visualizzare i database disponibili nel server di origine.

-    **Status**  
Visualizza varie informazioni del database di origine.

-    **Aggiorna**  
Consente di aggiornare l'elenco dei database.
  
### <a name="configure-destination-database"></a>Configurare il database di destinazione
Modificare il nome del database, se necessario, e specificare il percorso e i nomi dei file di database.  Questa pagina viene visualizzata solo una volta per ogni database spostato o copiato.

-    **Database di origine**  
Nome del database di origine.  Questa casella di testo non è modificabile.

-    **Database di destinazione**  
Il nome del database di destinazione da creare; modificare in base alle esigenze.

-    **File di database di destinazione:**  
     
     -    **Filename**  
Il nome del file di database di destinazione da creare; modificare in base alle esigenze.

     -    **Dimensioni (MB)**  
Dimensioni del file di database di destinazione in megabyte.

     -    **Cartella di destinazione**  
La cartella nel server di destinazione in cui ospitare il file di database di destinazione; modificare in base alle esigenze.

     -    **Status**  
Stato

-    **Se il database di destinazione esiste già:**  
     Scegliere l'azione da eseguire se il database di destinazione esiste già.

     -    **Arresta il trasferimento se nella destinazione esiste un database o un file con lo stesso nome**

     -    **Elimina il database con lo stesso nome nel server di destinazione, quindi continua il trasferimento sovrascrivendo i file di database esistenti**

###  <a name="select-server-objects"></a>Selezionare gli oggetti server 
Questa pagina è disponibile solo se l'origine e la destinazione si trovano in server diversi.  
  
-    **Oggetti correlati disponibili**  
Elenca gli oggetti disponibili per il trasferimento al server di destinazione.  Per includere un oggetto, fare clic sul relativo nome nella casella **Oggetti correlati disponibili** , quindi fare clic sul pulsante **>>** per spostare l'oggetto nella casella **Oggetti correlati selezionati** . 

-    **Oggetti correlati selezionati**  
Elenca gli oggetti che verranno trasferiti al server di destinazione.  Per escludere un oggetto, fare clic sul relativo nome nella casella **Oggetti correlati selezionati** , quindi fare clic sul pulsante **<\<** per spostare l'oggetto nella casella **Oggetti correlati disponibili** .  Per impostazione predefinita, vengono trasferiti tutti gli oggetti di ogni tipo selezionato, ad eccezione degli account di accesso. Per scegliere singoli oggetti di un tipo, fare clic sul pulsante con i puntini di sospensione accanto al tipo di oggetti nella casella **Oggetti correlati selezionati** .  Verrà aperta una finestra di dialogo in cui è possibile selezionare i singoli oggetti.

-    **Elenco di oggetti server** 

      -    **Account di accesso (l'opzione è selezionata per impostazione predefinita).** 
  
     -    **SQL Server Agent - processi**  
  
     -    **Messaggi di errore definiti dall'utente**  
  
     -    **Endpoints**  
  
     -    **Catalogo full-text** 
  
     -    **Pacchetto SSIS**  
     
     -    **Stored procedure dal database master** 
          >**NOTA:** le stored procedure estese e le rispettive DLL associate non sono idonee per la copia automatica.  
    
  
###   <a name="location-of-source-database-files"></a>Percorso dei file di database di origine
Questa pagina è disponibile solo se l'origine e la destinazione si trovano in server diversi.  Specificare una condivisione del file system contenente i file di database nel server di origine.
  
-    **Database**  
     Consente di visualizzare il nome di ogni database da spostare.  
  
-    **Percorso cartella**  
Il percorso della cartella dei file di database nel server di origine.
Ad esempio: `C:\Program Files\Microsoft SQL Server\MSSQL110.MSSQLSERVER\MSSQL\DATA`.

  
-    **Condivisione file nel server di origine**  
La condivisione file contenente i file di database nel server di origine.  Immettere manualmente la condivisione oppure fare clic sui puntini di sospensione per passare alla condivisione desiderata.
Ad esempio: `\\server_name\C$\Program Files\Microsoft SQL Server\MSSQL110.MSSQLSERVER\MSSQL\Data`.

### <a name="configure-the-package"></a>Configurare il pacchetto
Copia guidata database crea un pacchetto SSIS per trasferire il database.

-    **Posizione pacchetto**  
Visualizza il punto in cui verrà scritto il pacchetto SSIS.

-    **Nome pacchetto**  
Consente di creare un nome predefinito per il pacchetto SSIS; modificare in base alle esigenze.

-    **Opzioni di registrazione**  
Consente di indicare se le informazioni di registrazione devono essere archiviate nel registro eventi di Windows o in un file di testo.

-    **Percorso file log degli errori**  
Questa opzione è disponibile solo se è selezionata l'opzione per la registrazione di file di testo.  Consente di fornire un percorso per il file di log. 

### <a name="schedule-the-package"></a>Pianificazione pacchetto
Consente di specificare il momento in cui si vuole avviare l'operazione di spostamento o copia.  Se non si è un amministratore di sistema, è necessario specificare un account proxy di SQL Server Agent con accesso al sottosistema di esecuzione del pacchetto SQL Server Integration Services (SSIS).

> **IMPORTANTE** Un account proxy di Integration Services deve essere usato nel metodo di **collegamento e scollegamento** .  

-    **Run immediately**  
     Il pacchetto SSIS verrà eseguito dopo aver completato la procedura guidata.
  
-    **Pianificare**  
     Il pacchetto SSIS verrà eseguito in base a una pianificazione. 
  
     -    **Cambia pianificazione**   
Apre la finestra di dialogo **Nuova pianificazione processo** .  Configurare in base alle esigenze.  Al termine, fare clic su **OK**.


-    **Account proxy Integration Services** Selezionare un account proxy disponibile dall'elenco a discesa.  Per pianificare il trasferimento è necessario che sia disponibile almeno un account proxy per l'utente e che questo sia configurato con l'autorizzazione al **sottosistema di esecuzione del pacchetto SSIS**.

        Per creare un account proxy per l'esecuzione del pacchetto SSIS, in **Esplora oggetti** espandere **SQL Server Agent**, espandere **Proxy**, fare clic con il pulsante destro del mouse su **Esecuzione pacchetto SSIS** e quindi fare clic su **Nuovo proxy**.

### <a name="complete-the-wizard"></a>Completamento procedura guidata
Visualizza un riepilogo delle opzioni selezionate.  È possibile fare clic su **Indietro** per modificare le opzioni  Fare clic su **Fine** per creare il pacchetto SSIS. La pagina **Esecuzione dell'operazione** monitora le informazioni sullo stato di esecuzione di **Copia guidata database**.

-    **Azione**  
 Vengono elencate tutte le azioni eseguite.

-    **Status**  
 Viene indicato se l'azione è stata completata correttamente o meno.

-    **Message**  
Viene fornito qualsiasi messaggio restituito a ogni passaggio.

##  <a name="examples"></a><a name="Examples"></a> Esempi
### <a name="common-steps"></a>**Passaggi comuni** 
Indipendentemente dall'operazione scelta tra **spostamento** o **copia**, **collegamento e scollegamento** o **SMO**, i cinque passaggi elencati di seguito saranno uguali.  Per brevità, i passaggi sono elencati in questa pagina una sola volta e tutti gli esempi inizieranno al **passaggio 6**.

1.  In **Esplora oggetti** connettersi a un'istanza del motore di database di SQL Server e, successivamente, espanderla.

2.  Espandere **Database**, fare clic con il pulsante destro del mouse sul database desiderato, scegliere **Attività** e quindi fare clic su **Copia database...**

3.  Se viene visualizzata la pagina iniziale **Copia guidata database** , fare clic su **Avanti**.

4.  Nella pagina **Selezionare un server di origine** specificare il server in cui si trova il database da spostare o copiare e immettere le informazioni relative all'account di accesso.  Selezionare il metodo di autenticazione.  Se si sceglie **Autenticazione di SQL Server** è necessario immettere le credenziali di accesso.  Fare clic su **Avanti** per stabilire la connessione al server di origine.  La connessione rimane aperta durante tutta la sessione.

5.  Nella pagina **Selezionare un server di destinazione** specificare il server in cui verrà spostato o copiato il database.  Selezionare il metodo di autenticazione.  Se si sceglie **Autenticazione di SQL Server** è necessario immettere le credenziali di accesso.  Fare clic su **Avanti** per stabilire la connessione al server di origine.  La connessione rimane aperta durante tutta la sessione.

     > **NOTA:** è possibile avviare Copia guidata database da qualsiasi database.  È possibile usare Copia guidata database dal server di origine o di destinazione.
  
### <a name="a--move-database-using-detach-and-attach-method-to-an-instance-on-a-different-physical-server--a-login-and-sql-server-agent-job-will-be-moved-as-well"></a>**A.  Usando un metodo di collegamento e scollegamento, spostare il database in un'istanza su un server fisico diverso.  Verranno spostati anche un account di accesso e il processo di SQL Server Agent.**  
Nell'esempio seguente vengono spostati il database `Sales` , un account di accesso di Windows denominato `contoso\Jennie` e un processo di SQL Server Agent denominato `Jennie's Report` da un'istanza di SQL Server 2008 su `Server1` a un'istanza di SQL Server 2016 su `Server2`.  `Jennie's Report` usa il database `Sales` .  `Sales` non esiste ancora nel server di destinazione, `Server2`.  `Server1` verrà riassegnato a un team diverso dopo lo spostamento del database.
  
6.  Come indicato nella sezione [Limitazioni e restrizioni](#Restrictions)precedente, sarà necessario creare uno scheletro di database nel server di destinazione durante il trasferimento di un processo di SQL Server Agent che fa riferimento a un database non ancora esistente nel server di destinazione.  Creare uno scheletro di database denominato `Sales` nel server di destinazione. 

7.  Nella pagina **Procedura guidata**, **Selezionare il metodo di trasferimento** esaminare e gestire i valori predefiniti.  Fare clic su **Avanti**.
  
8.  Nella pagina **Selezionare i database** selezionare la casella di controllo **Sposta** per il database desiderato, `Sales`.  Fare clic su **Avanti**.
  
9.  Nella pagina **Configurare il database di destinazione** la **Procedura guidata** ha rilevato che `Sales` esiste già nel server di destinazione, perché è stato creato nel **passaggio 6** precedente, e ha aggiunto `_new` al nome del **Database di destinazione** .  Eliminare `_new` dalla casella di testo **Database di destinazione** .  Facoltativamente, modificare il **Nome file** e la **Cartella di destinazione**.  Selezionare **Elimina il database con lo stesso nome nel server di destinazione, quindi continua il trasferimento sovrascrivendo i file di database esistenti**.  Fare clic su **Avanti**.
  
10. Nel pannello **Oggetti correlati selezionati** della pagina **Selezionare gli oggetti server** fare clic sul pulsante con puntini di sospensione relativo a **Object name Logins**(Account di accesso nome oggetto).  In **Opzioni copia** selezionare **Copia solo gli account di accesso selezionati:**.  Selezionare la casella relativa a **Mostra tutti gli account di accesso al server**.  Controllare la casella **Account di accesso** per `contoso\Jennie`.  Fare clic su **OK**.  Nel pannello **Oggetti correlati disponibili:** selezionare **Processi di SQL Server Agent** e quindi fare clic sul pulsante **>** .  Nel pannello **Oggetti correlati selezionati:** fare clic sul pulsante con puntini di sospensione relativo a **Processi di SQL Server Agent**.  In **Opzioni copia** selezionare **Copia solo i processi selezionati:**.  Selezionare la casella per `Jennie's Report`.  Fare clic su **OK**.  Fare clic su **Avanti**.  
  
11. Nella pagina **Percorso dei file di database di origine** fare clic sul pulsante con puntini di sospensione relativo a **Condivisione file nel server di origine** e passare al percorso per il percorso della cartella specificato.  Ad esempio, per il percorso di cartella `D:\MSSQL13.MSSQLSERVER\MSSQL\DATA` usare `\\Server1\D$\MSSQL13.MSSQLSERVER\MSSQL\DATA` per **Condivisione file nel server di origine**.  Fare clic su **Avanti**.
  
12. Nella casella di testo **Nome pacchetto:** della pagina **Configurare il pacchetto** pagina immettere `SalesFromServer1toServer2_Move`.  Selezionare la casella **Salva log di trasferimento** .  Nell'elenco a discesa **Opzioni di registrazione** selezionare **File di testo**.  Modificare il **Percorso file di log degli errori** in base alle esigenze.  Fare clic su **Avanti**.  
  
     > **NOTA:****Percorso file di log degli errori** è il percorso nel server di destinazione.
  
13. Nella pagina **Pianificare il pacchetto** selezionare il proxy rilevante dall'elenco a discesa **Account proxy di Integration Services** .  Fare clic su **Avanti**.

14. Nella pagina **Completare la procedura guidata** rivedere il riepilogo delle opzioni selezionate.  È possibile fare clic su **Indietro** per modificare le opzioni  Fare clic su **Fine** per eseguire l'attività.  Durante il trasferimento, nella pagina **Esecuzione dell'operazione** vengono monitorate le informazioni sullo stato di esecuzione della **Procedura guidata**.

15. Nella pagina **Esecuzione dell'operazione** se l'operazione ha esito positivo, fare clic su **Chiudi**.  Se l'operazione ha esito negativo, esaminare il log degli errori ed eventualmente selezionare **Indietro** per un ulteriore esame.  In caso contrario, fare clic su **Chiudi**.
  
16. **Passaggi successivi allo spostamento** : considerare la possibilità di eseguire le istruzioni T-SQL seguenti nel nuovo host, `Server2`:
  
     ~~~ tsql 
     ALTER AUTHORIZATION ON DATABASE::Sales TO sa;

     ALTER DATABASE Sales 
     SET COMPATIBILITY_LEVEL = 130;

     USE Sales
     GO

     EXEC sp_updatestats;
     ~~~
 
17. **Operazioni di pulizia dei passaggi successivi allo spostamento**  
Considerato che `Server1` verrà spostato in un team diverso e che l'operazione di **spostamento** non verrà ripetuta, considerare la possibilità di eseguire i passaggi seguenti:
     -    Eliminazione del pacchetto SSIS `SalesFromServer1toServer2_Move` su `Server2`.
     -    Eliminazione del processo di SQL Server Agent `SalesFromServer1toServer2_Move` su `Server2`.
     -    Eliminazione del processo di SQL Server Agent `Jennie's Report` su `Server1`.
     -    Eliminazione dell'account di accesso `contoso\Jennie` su `Server1`.


### <a name="b-----copy-database-using-detach-and-attach-method-to-the-same-instance-and-set-recurring-schedule"></a>**B.     Copiare il database usando il metodo di collegamento e scollegamento nella stessa istanza e impostare una pianificazione ricorrente.**  
In questo esempio il database `Sales` verrà copiato e creato come `SalesCopy` nella stessa istanza.  Successivamente, `SalesCopy`verrà ricreato con cadenza settimanale.

6.  Nella pagina **Selezionare il metodo di trasferimento** esaminare e gestire i valori predefiniti.  Fare clic su **Avanti**.

7.  Nella pagina **Selezionare i database** selezionare la casella di controllo **Copia** per il database `Sales` .  Fare clic su **Avanti**.

8.  Nella pagina **Configurare il database di destinazione** impostare il nome di **Database di destinazione** su `SalesCopy`.  Facoltativamente, modificare il **Nome file** e la **Cartella di destinazione**.  Selezionare **Elimina il database con lo stesso nome nel server di destinazione, quindi continua il trasferimento sovrascrivendo i file di database esistenti**.  Fare clic su **Avanti**.

9.  Nella casella di testo **Nome pacchetto:** della pagina **Configurare il pacchetto** pagina immettere `SalesCopy Weekly Refresh`.  Selezionare la casella **Salva log di trasferimento** .  Fare clic su **Avanti**.

10. Nella pagina **Pianificare il pacchetto** fare clic sul pulsante di opzione **Pianifica** e quindi fare clic sul pulsante **Cambia pianificazione** . 
 
    1. Nella casella di testo **Nome** della pagina **Nuova pianificazione processo** immettere `Weekly on Sunday`. 
          
    2. Fare clic su **OK**.

11. Selezionare il proxy rilevante dall'elenco a discesa **Account proxy di Integration Services** .  Fare clic su **Avanti**.

12. Nella pagina **Completare la procedura guidata** rivedere il riepilogo delle opzioni selezionate.  È possibile fare clic su **Indietro** per modificare le opzioni  Fare clic su **Fine** per eseguire l'attività.  Durante la creazione del pacchetto, nella pagina **Esecuzione dell'operazione** vengono monitorate le informazioni sullo stato di esecuzione della **Procedura guidata**.

13. Nella pagina **Esecuzione dell'operazione** se l'operazione ha esito positivo, fare clic su **Chiudi**.  Se l'operazione ha esito negativo, esaminare il log degli errori ed eventualmente selezionare **Indietro** per un ulteriore esame.  In caso contrario, fare clic su **Chiudi**.

14. Avviare manualmente il processo di SQL Server Agent appena creato `SalesCopy weekly refresh`.  Esaminare la cronologia processo e verificare che `SalesCopy` ora esista nell'istanza.

  
##  <a name="follow-up-after-upgrading-a-database"></a><a name="FollowUp"></a> Completamento: Dopo l'aggiornamento di un database  
 Dopo aver utilizzato Copia guidata database per aggiornare un database da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], il database viene reso immediatamente disponibile e aggiornato automaticamente. Se il database include indici full-text, questi vengono importati, reimpostati o ricompilati dal processo di aggiornamento, a seconda dell'impostazione della proprietà del server **Opzione di aggiornamento full-text** . Se l'opzione di aggiornamento è impostata su **Importa** o **Ricompila**, gli indici full-text non saranno disponibili durante l'aggiornamento. A seconda della quantità di dati indicizzati, l'importazione può richiedere diverse ore, mentre la ricompilazione può risultare dieci volte più lunga. Si noti inoltre che, quando l'opzione di aggiornamento è impostata su **Importa** e un catalogo full-text non è disponibile, gli indici full-text associati vengono ricompilati. Per informazioni sulla visualizzazione o sulla modifica dell'impostazione della proprietà **Opzione di aggiornamento full-text** , vedere [Gestione e monitoraggio della ricerca full-text per un'istanza del server](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md).  
  
Dopo l'aggiornamento, il livello di compatibilità del database rimane a livello di compatibilità prima dell'aggiornamento, a meno che il livello di compatibilità precedente non sia supportato nella nuova versione. In questo caso, il livello di compatibilità del database aggiornato è impostato sul livello di compatibilità supportato più basso.

Se, ad esempio, si collega un database con livello di compatibilità 90 prima di collegarlo a un'istanza di [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] , dopo l'aggiornamento il livello di compatibilità viene impostato su 100, che corrisponde al livello di compatibilità supportato più basso in [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] . Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).
 
 ## <a name="post-copy-or-move-considerations"></a><a name="Post"></a> Considerazioni successive alla copia o allo spostamento
 Considerare la possibilità di seguire questa procedura dopo un'operazione di **copia** o **spostamento**:
-    Modifica della proprietà dei database durante l'uso del metodo di collegamento e scollegamento.
-    Eliminazione di oggetti server nel server di origine dopo uno **spostamento**.
-    Eliminazione del pacchetto SSIS creato dalla procedura guidata nel server di destinazione.
-    Eliminazione del processo di SQL Server Agent creato usando la procedura guidata nel server di destinazione.

  
## <a name="more-information"></a>Altre informazioni! 
 [Aggiornamento di un database utilizzando le operazioni di scollegamento e collegamento &#40;Transact-SQL&#41;](../../relational-databases/databases/upgrade-a-database-using-detach-and-attach-transact-sql.md)   
 [Creazione di un proxy di SQL Server Agent](../../ssms/agent/create-a-sql-server-agent-proxy.md)  
  
  

