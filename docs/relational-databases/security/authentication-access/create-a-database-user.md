---
title: Creazione di un utente di database | Microsoft Docs
description: Informazioni su come creare i tipi più comuni di utenti di database usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 04/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.user.securables.f1
- SQL13.SWB.DATABASEUSER.GENERAL.F1
helpviewer_keywords:
- database users, creating
- creating users with Management Studio
- mapping users
- users [SQL Server], creating
- database user additions [SQL Server]
- database users, mapping
- CREATE USER [Management Studio]
- users [SQL Server], adding
- mapping database users
ms.assetid: 782798d3-9552-4514-9f58-e87be4b264e4
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5088cb94c4c4387cd77835bbe02502764d3c2384
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171703"
---
# <a name="create-a-database-user"></a>Creazione di un utente di database
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Questo argomento descrive come creare i tipi più comuni di utenti di database. Esistono undici tipi di utenti. L'elenco completo è disponibile nell'argomento [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md). Tutte le varianti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] supportano gli utenti di database, ma non necessariamente tutti i tipi di utenti.  
  
 È possibile creare un utente del database con [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
##  <a name="understanding-the-types-of-users"></a><a name="Understanding"></a> Informazioni sui tipi di utenti  
 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] offre 6 opzioni per la creazione di un utente di database. Nella figura seguente le 6 opzioni sono indicate nel riquadro verde insieme a informazioni su cosa rappresentano.  
  
 ![Diagramma che illustra i diversi tipi di utenti.](../../../relational-databases/security/authentication-access/media/typesofusers.png "TypesOfUsers")  
  
### <a name="selecting-the-type-of-user"></a>Selezione del tipo di utente  
 **Account di accesso o utente non mappato a un account di accesso**  
  
 Per coloro che non hanno familiarità con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], può essere difficile determinare il tipo di utente che si vuole creare. Per prima cosa, chiedersi se la persona o il gruppo con l'esigenza di accedere al database ha un account di accesso. Gli account di accesso nel database master sono comuni per le persone che gestiscono [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e per quelle che devono accedere a molti o a tutti i database nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per questa situazione, creare un **utente SQL con account di accesso**. L'utente del database è l'identità dell'account di accesso quando è connesso a un database. Può utilizzare lo stesso nome dell'account, ma non si tratta di una condizione obbligatoria. In questo argomento si presuppone che esista già un account di accesso in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per informazioni su come creare un account di accesso, vedere [Creare un account di accesso](../../../relational-databases/security/authentication-access/create-a-login.md).  
  
 Se la persona o il gruppo che deve accedere al database non ha un account di accesso e deve accedere solo a uno o alcuni database, creare un **utente di Windows** o un **utente SQL con password**. Noto anche come utente di database indipendente, questo tipo di utente non è associato a un account di accesso nel database master. È un'ottima scelta se si vuole avere la possibilità di spostare facilmente il database tra istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per usare questa opzione in [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)], un amministratore deve prima abilitare i database indipendenti per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]e il database deve essere abilitato per l'indipendenza. Per altre informazioni, vedere [Utenti di database indipendente: rendere portabile un database](../../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
> **IMPORTANTE** Per la connessione come utente di database indipendente, è necessario specificare il nome del database indipendente come parte della stringa di connessione. Per specificare il database in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], nella finestra di dialogo **Connetti a** fare clic su **Opzioni** e quindi sulla scheda **Proprietà connessione** .  
  
 Quando la persona che deve connettersi non può eseguire l'autenticazione con Windows, selezionare **Utente SQL con password** o **Utente SQL con account di accesso** in base a un **account di accesso con autenticazione di SQL Server**. Si tratta di una situazione comune quando persone esterne all'organizzazione, ad esempio i clienti, si connettono a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
> **SUGGERIMENTO** Per le persone all'interno dell'organizzazione, l'autenticazione di Windows è una scelta migliore, perché non dovranno ricordare un'altra password e perché l'autenticazione di Windows offre funzionalità di sicurezza aggiuntive, come Kerberos.  
  
##  <a name="background"></a><a name="Restrictions"></a> Background  
 Un utente è un'entità di sicurezza a livello di database. Affinché gli account di accesso possano eseguire la connessione al database, è necessario eseguirne il mapping a un utente di database. È possibile eseguire il mapping di un account di accesso a database diversi come utenti diversi, ma come un singolo utente per ogni database. In un database parzialmente indipendente, può essere creato un utente che non dispone di un account di accesso. Per altre informazioni sugli utenti di database indipendenti, vedere [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md). Se è abilitato l'utente guest in un database, un account di accesso di cui non è stato eseguito il mapping a un utente del database può accedere al database come utente guest.  
  
> **IMPORTANTE** L'utente guest è in genere disabilitato. Non abilitarlo, a meno che non sia strettamente necessario.  
  
 È possibile concedere autorizzazioni agli utenti, in quanto entità di sicurezza. L'ambito di un utente è il database. Affinché un account di accesso possa eseguire la connessione a un database specifico nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], è necessario eseguirne il mapping a un utente del database. Le autorizzazioni all'interno del database vengono concesse e negate all'utente del database, non all'account di accesso.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Richiede l'autorizzazione **ALTER ANY USER** per il database.  
  
##  <a name="create-a-user-with-ssms"></a><a name="SSMSProcedure"></a> Creare un utente con SSMS  
  
 
1.  In Esplora oggetti espandere la cartella **Database** .  
  
2.  Espandere il database in cui si desidera creare il nuovo utente del database.  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella **Sicurezza**, scegliere **Nuovo** e quindi **Utente...** .  
  
4.  Nella finestra di dialogo **Utente database - Nuovo** nella pagina **Generale** selezionare uno dei tipi di utente seguenti nell'elenco **Tipo utente**:  
  
    -   **utente SQL con account di accesso**  
  
    -   **utente SQL con password**  
  
    -   **Utente SQL senza account di accesso**  
  
    -   **Utente con mapping eseguito a un certificato**  
  
    -   **Utente con mapping eseguito a una chiave asimmetrica**  
  
    -   **utente di Windows**  
  
5.  Quando si seleziona un'opzione, le restanti opzioni nella finestra di dialogo potrebbero cambiare. Alcune opzioni si applicano solo a tipi specifici di utenti di database. Alcune opzioni possono essere lasciate vuote e in questo caso verrà usato il valore predefinito.  
  
     **Nome utente**  
     Immettere un nome per il nuovo utente. Se si è scelto **Utente di Windows** nell'elenco **Tipo utente**, è anche possibile fare clic sui puntini di sospensione **(...)** per aprire la finestra di dialogo **Seleziona utente o gruppo**.  
  
     **Nome account di accesso**  
     Immettere l'account di accesso per l'utente. In alternativa, fare clic sui puntini di sospensione **(...)** per aprire la finestra di dialogo **Seleziona account di accesso**. È disponibile **Nome account di accesso** se si seleziona **Utente SQL con account di accesso** o **Utente di Windows** dall'elenco **Tipo di utente** .  
  
     **Password** e **Conferma password**  
     Immettere una password per gli utenti che eseguono l'autenticazione nel database.  
  
     **Lingua predefinita**  
     Immettere la lingua predefinita dell'utente.  
  
     **Schema predefinito**  
     Immettere lo schema che diventerà proprietario degli oggetti creati da questo utente. In alternativa, fare clic sui puntini di sospensione **(...)** per aprire la finestra di dialogo **Seleziona schema**. È disponibile **Schema predefinito** se si seleziona **Utente SQL con account di accesso**, **Utente SQL senza account di accesso** o **Utente di Windows** nell'elenco **Tipo di utente** .  
  
     **Nome certificato**  
     Immettere il certificato da usare per l'utente del database. In alternativa, fare clic sui puntini di sospensione **(...)** per aprire la finestra di dialogo **Seleziona certificato**. È disponibile **Nome certificato** se si seleziona **Utente con mapping eseguito a un certificato** dall'elenco **Tipo di utente** .  
  
     **Nome chiave asimmetrica**  
     Immettere la chiave da usare per l'utente del database. In alternativa, fare clic sui puntini di sospensione **(...)** per aprire la finestra di dialogo **Seleziona chiave asimmetrica**. È disponibile **Nome chiave asimmetrica** se si seleziona **Utente con mapping eseguito a una chiave asimmetrica** dall'elenco **Tipo di utente** .  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="additional-options"></a>Opzioni aggiuntive  
 Nella finestra di dialogo **Utente di database - Nuovo** sono disponibili opzioni anche in altre quattro pagine: **Schemi di proprietà**, **Appartenenza**, **Entità a protezione diretta** e **Proprietà estese**.  
  
-   Nella pagina **Schemi di proprietà** sono elencati tutti i possibili schemi che possono essere di proprietà del nuovo utente del database. Per aggiungere schemi o rimuoverli da un utente del database, in **Schemi di proprietà di questo utente** selezionare o deselezionare le caselle di controllo accanto agli schemi.  
  
-   Nella pagina **Appartenenza** sono elencati tutti i possibili ruoli di appartenenza al database che possono essere di proprietà del nuovo utente del database. Per aggiungere ruoli o rimuoverli da un utente del database, in **Appartenenza a ruoli del database** selezionare o deselezionare le caselle di controllo accanto ai ruoli.  
  
-   Nella pagina **Entità a protezione diretta** sono elencate tutte le possibili entità a protezione diretta e le autorizzazioni su quelle entità a protezione diretta che possono essere concesse all'account di accesso.  
  
-   La pagina **Proprietà estese** consente di aggiungere proprietà personalizzate a utenti di database. In questa pagina sono disponibili le opzioni seguenti.  
  
     **Database**  
     Consente di visualizzare il nome del database selezionato. Questo campo è di sola lettura.  
  
     **Regole di confronto**  
     Consente di visualizzare le regole di confronto utilizzate per il database selezionato. Questo campo è di sola lettura.  
  
     **Proprietà**  
     Consente di visualizzare o specificare le proprietà estese relative all'oggetto. Ogni proprietà estesa è composta da una coppia nome/valore di metadati associati all'oggetto.  
  
     **Puntini di sospensione (...)**  
     Fare clic sui puntini di sospensione **(…)** dopo **Valore** per visualizzare la finestra di dialogo **Valore per Proprietà estesa**. Digitare o visualizzare il valore della proprietà estesa in questa finestra di dimensioni maggiori. Per ulteriori informazioni, vedere [Finestra di dialogo Valore per proprietà estesa](../../databases/value-for-extended-property-dialog-box.md).  
  
     **Elimina**  
     Consente di eliminare la proprietà estesa selezionata.  
  
##  <a name="create-a-user-using-t-sql"></a><a name="TsqlProcedure"></a> Creare un utente mediante T-SQL  
    
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Sulla barra **Standard** fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- Creates the login AbolrousHazem with password '340$Uuxwp7Mcxo7Khy'.  
    CREATE LOGIN AbolrousHazem   
        WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
    GO  
  
    -- Creates a database user for the login created above.  
    CREATE USER AbolrousHazem FOR LOGIN AbolrousHazem;  
    GO  
    ```  
  
 Per altre informazioni, vedere [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md) che contiene molti altri esempi per [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Entità &#40;motore di database&#41;](../../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Creare un account di accesso](../../../relational-databases/security/authentication-access/create-a-login.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
  
