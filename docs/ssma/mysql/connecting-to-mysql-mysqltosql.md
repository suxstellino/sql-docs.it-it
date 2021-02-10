---
title: Connessione a MySQL (MySQLToSQL) | Microsoft Docs
description: Informazioni su come connettersi a un database di iMySQL di destinazione per eseguire la migrazione di un database MySQL. SSMA ottiene i metadati sui database nel database SQL di Azure.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to MySQL, MySQL permission
- Connecting to MySQL,reconnecting
ms.assetid: 084c7020-f729-4f91-90e0-143f85fa68d1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d7f3f46a58fccafa68164d9335c66df10c5b7487
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068896"
---
# <a name="connecting-to-mysql-mysqltosql"></a>Connessione a MySQL (MySQLToSQL)
Per eseguire la migrazione di database MySQL a SQL Server o SQL Azure, è necessario connettersi al database MySQL di cui si vuole eseguire la migrazione. Quando si esegue la connessione, SSMA ottiene i metadati relativi a tutti gli schemi MySQL e quindi li Visualizza nel riquadro MySQL Metadata Explorer. SSMA archivia le informazioni sul server di database, ma non archivia le password.  
  
La connessione al database rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi se si desidera una connessione attiva al database.  
  
I metadati relativi al database MySQL non vengono aggiornati automaticamente. Se invece si desidera aggiornare i metadati in MySQL Metadata Explorer, è necessario aggiornarli manualmente. Per ulteriori informazioni, vedere la sezione "aggiornamento dei metadati MySQL" più avanti in questo argomento.  
  
## <a name="required-mysql-permissions"></a>Autorizzazioni MySQL obbligatorie  
L'account usato per connettersi al database MySQL deve avere almeno le autorizzazioni **Connect** . Ciò consente a SSMA di ottenere i metadati dagli schemi di proprietà dell'utente che si connette. Per ottenere i metadati per gli oggetti di altri schemi e quindi convertire gli oggetti in tali schemi, l'account deve disporre delle autorizzazioni seguenti:  
  
-   Privilegi "Mostra" per gli oggetti di database  
  
-   Privilegio ' SELECT ' in ' Information_schema '  
  
-   Privilegio ' SELECT ' in MySQL (per UDF)  
  
## <a name="establishing-a-connection-to-mysql"></a>Stabilire una connessione a MySQL  
Quando ci si connette a un database, SSMA legge i metadati del database e quindi aggiunge questi metadati al file di progetto. Questi metadati vengono usati da SSMA quando converte gli oggetti in SQL Server o SQL Azure sintassi e quando esegue la migrazione dei dati a SQL Server o SQL Azure. È possibile esplorare questi metadati nel riquadro MySQL Metadata Explorer ed esaminare le proprietà dei singoli oggetti di database.  
  
> [!IMPORTANT]  
> Prima di provare a connettersi, verificare che il server di database sia in esecuzione e che sia in grado di accettare le connessioni.  
  
**Per eseguire la connessione a MySQL**  
  
1.  Scegliere **Connetti a MySQL** dal menu **file** . questa opzione verrà abilitata dopo la creazione del progetto.  
  
    Se in precedenza si è connessi a MySQL, il nome del comando verrà **riconnesso a MySQL**.  
  
2.  Nella casella **provider** selezionare MySQL ODBC 5,1 driver (Trusted). Si tratta del provider predefinito in modalità standard.  
  
3.  Nella casella **modalità** selezionare **modalità standard**. Si tratta della modalità predefinita.  
  
    Utilizzare la modalità standard per specificare il nome e la porta del server.  
  
4.  In **modalità standard** specificare i valori seguenti:  
  
    1.  Nella casella **nome server** immettere il nome del server MySQL. Nella casella **porta server** immettere il numero di porta che deve essere 3306. Si tratta della porta predefinita.  
  
    2.  Nella casella **nome utente** immettere un account MySQL con le autorizzazioni necessarie.  
  
    3.  Nella casella **password** immettere la password per il nome utente specificato.  
  
5.  **SSL:** Se si desidera connettersi in modo sicuro a MySQL, utilizzare Secure Socket Layer (SSL) selezionando la casella di controllo **SSL** .  
  
6.  **Configurare:** Offre un'opzione per configurare la connessione a MySQL tramite Secure Socket Layer (SSL).  
  
    > [!NOTE]  
    > Per abilitare la **configurazione**, è necessario che SSL sia impostato su **true**.  
  
    Quando si fa clic sul pulsante "Configura", viene visualizzata una finestra di dialogo. Per usare la crittografia durante la connessione al database MySQL, è necessario definire il percorso dei seguenti tre file di certificato presenti nella finestra di dialogo [Privacy Enhanced Mail Certificates (PEM)]:  
  
    -   **Autorità di certificazione SSL:** Specifica il percorso di un file con un elenco di CA SSL attendibili.  
  
    -   **Certificato SSL:** Specifica il nome del file di certificato SSL da usare per stabilire una connessione protetta.  
  
    -   **chiave SSL:** Specifica il nome del file di chiave SSL da usare per stabilire una connessione protetta.  
  
    > [!NOTE]  
    > -   Il pulsante **OK** è abilitato quando sono state fornite le informazioni necessarie. Se uno dei percorsi di file non è valido, il pulsante "OK" rimarrà disabilitato.  
    > -   Il pulsante **Annulla** chiude la finestra di dialogo e **Disattiva** l'opzione SSL dal form principale della connessione.  
  
7.  Per altre informazioni, vedere [connettersi a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connect-to-mysql-mysqltosql.md)  
  
## <a name="reconnecting-to-mysql"></a>Riconnessione a MySQL  
La connessione al server di database rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi se si desidera una connessione attiva al database. È possibile lavorare offline fino a quando non si desidera aggiornare i metadati, caricare oggetti di database in SQL Server o SQL Azure ed eseguire la migrazione dei dati.  
  
## <a name="refreshing-mysql-metadata"></a>Aggiornamento dei metadati MySQL  
I metadati relativi al database MySQL non vengono aggiornati automaticamente. I metadati in MySQL Metadata Explorer sono uno snapshot dei metadati al momento della prima connessione o dell'ultima volta in cui sono stati aggiornati manualmente i metadati. È possibile aggiornare manualmente i metadati per tutti gli schemi, un singolo schema o singoli oggetti di database.  
  
**Per aggiornare i metadati**  
  
1.  Assicurarsi di essere connessi al database.  
  
2.  In MySQL Metadata Explorer selezionare la casella di controllo accanto a ogni schema o oggetto di database che si desidera aggiornare.  
  
3.  Fare clic con il pulsante destro del mouse su **schemi** oppure sul singolo schema o oggetto di database, quindi scegliere **Aggiorna da database**.  
  
    Se non si dispone di una connessione attiva, SSMA visualizzerà la finestra **di dialogo Connetti a MySQL in** modo che sia possibile connettersi.  
  
4.  Nella finestra di dialogo Aggiorna da database specificare gli oggetti da aggiornare.  
  
    -   Per aggiornare un oggetto, fare clic sul campo **attivo** adiacente all'oggetto fino a quando non viene visualizzata una freccia.  
  
    -   Per impedire l'aggiornamento di un oggetto, fare clic sul campo **attivo** adiacente all'oggetto fino a quando non viene visualizzata una **X** .  
  
    -   Per aggiornare o rifiutare una categoria di oggetti, fare clic sul campo **attivo** accanto alla cartella Category.  
  
    -   Per visualizzare le definizioni della codifica dei colori, fare clic sul pulsante **Legenda** .  
  
5.  Fare clic su **OK**.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione è la [connessione a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database MySQL a SQL Server-database SQL di Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
