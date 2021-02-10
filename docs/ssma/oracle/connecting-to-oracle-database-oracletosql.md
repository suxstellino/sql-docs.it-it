---
title: Connessione a Oracle Database (OracleToSQL) | Microsoft Docs
description: Informazioni su come connettersi al database Oracle per eseguire la migrazione di tale database Oracle a SQL Server. SSMA ottiene e Visualizza i metadati relativi a tutti gli schemi Oracle.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 06/04/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Refreshing Oracle Metadata
ms.assetid: e276cdbf-3ebc-4ba8-b40d-a7a42befa2b6
ms.author: alexiva
ms.openlocfilehash: 4354a4e2726151177c48c27ce37a74e5b4ac335e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058798"
---
# <a name="connecting-to-oracle-database-oracletosql"></a>Connessione a un database Oracle (OracleToSQL)

Per eseguire la migrazione dei database Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario connettersi al database Oracle di cui si desidera eseguire la migrazione. Quando si esegue la connessione, SSMA ottiene i metadati relativi a tutti gli schemi Oracle e quindi li Visualizza nel riquadro Oracle Metadata Explorer. SSMA archivia le informazioni sul server di database, ma non archivia le password.

La connessione al database rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi se si desidera una connessione attiva al database.

I metadati relativi al database Oracle non vengono aggiornati automaticamente. Se invece si desidera aggiornare i metadati in Oracle Metadata Explorer, è necessario aggiornarli manualmente. Per ulteriori informazioni, vedere la sezione "aggiornamento dei metadati Oracle" più avanti in questo argomento.

## <a name="required-oracle-permissions"></a>Autorizzazioni Oracle obbligatorie

L'account utilizzato per la connessione al database Oracle deve disporre almeno delle autorizzazioni seguenti:

- `CONNECT`  
  Obbligatorio per connettersi (creare una sessione) al database.

- `SELECT ANY DICTIONARY`  
  Obbligatorio per eseguire una query sulle tabelle del dizionario di sistema (ad esempio, `SYS.MLOG$` ) per individuare tutti gli oggetti.

Ciò consentirà a SSMA di caricare tutti gli oggetti nello schema di proprietà dell'utente che si connette. Nella maggior parte degli scenari reali sono presenti riferimenti tra schemi tra stored procedure e SSMA deve essere in grado di individuare tutti gli oggetti a cui si fa riferimento per una conversione corretta. Per ottenere i metadati per gli oggetti definiti in altri schemi, l'account deve disporre delle autorizzazioni aggiuntive seguenti:

- `SELECT ANY TABLE`  
  Obbligatorio per individuare tabelle, viste, viste materializzate e sinonimi in altri schemi.

- `SELECT ANY SEQUENCE`  
  Obbligatorio per individuare le sequenze in altri schemi.

- `CREATE ANY PROCEDURE`  
  Necessario per individuare PL/SQL per procedure, funzioni e pacchetti in altri schemi.

- `CREATE ANY TRIGGER`  
  Obbligatorio per individuare le definizioni dei trigger in altri schemi.

- `CREATE ANY TYPE`  
  Obbligatorio per individuare i tipi definiti in altri schemi.

Alcune delle funzionalità di SSMA richiedono autorizzazioni aggiuntive. Se ad esempio si desidera utilizzare la funzionalità di gestione dei [tester](testing-migrated-database-objects-oracletosql.md) e [dei backup](managing-backups-oracletosql.md) , è necessario concedere all'utente che esegue la connessione quanto segue:

- `EXECUTE ANY PROCEDURE`  
  Obbligatorio per eseguire le procedure e le funzioni che si desidera testare in tutti gli schemi.

- `CREATE ANY TABLE` e `ALTER ANY TABLE`  
  Obbligatorio per creare e modificare tabelle temporanee per il rilevamento delle modifiche e i backup.

- `INSERT ANY TABLE` e `UPDATE ANY TABLE`  
  Obbligatorio per inserire i dati di rilevamento delle modifiche e di backup in tabelle temporanee.

- `DROP ANY TABLE`  
  Obbligatorio per eliminare le tabelle temporanee utilizzate per il rilevamento delle modifiche e i backup.

- `CREATE ANY INDEX` e `ALTER ANY INDEX`  
  Obbligatorio per creare e modificare gli indici nelle tabelle temporanee utilizzate per il rilevamento delle modifiche e i backup.

- `DROP ANY INDEX`  
  Obbligatorio per eliminare gli indici nelle tabelle temporanee utilizzate per il rilevamento delle modifiche e i backup.

- `CREATE ANY TRIGGER` e `ALTER ANY TRIGGER`  
  Obbligatorio per creare e modificare i trigger temporanei utilizzati per il rilevamento delle modifiche.

- `DROP ANY TRIGGER`  
  Obbligatorio per eliminare i trigger temporanei utilizzati per il rilevamento delle modifiche.

> [!NOTE]
> Si tratta di un set generico di autorizzazioni necessarie per il corretto funzionamento di SSMA. Se si desidera limitare l'ambito della migrazione a un subset di schemi, è possibile concedere le autorizzazioni sopra indicate al set limitato di oggetti anziché `ALL` . Sebbene sia possibile, potrebbe essere molto difficile identificare correttamente tutte le dipendenze, impedendo in tal modo il corretto funzionamento di SSMA. È consigliabile attenersi al set generico come definito sopra per eliminare i potenziali problemi di autorizzazione durante il processo di migrazione.

## <a name="establishing-a-connection-to-oracle"></a>Stabilire una connessione a Oracle

Quando ci si connette a un database, SSMA legge i metadati del database e quindi aggiunge questi metadati al file di progetto. Questi metadati vengono usati da SSMA quando converte gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi e quando esegue la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile esplorare questi metadati nel riquadro di Esplora metadati Oracle ed esaminare le proprietà dei singoli oggetti di database.

> [!IMPORTANT]
> Prima di provare a connettersi, verificare che il server di database sia in esecuzione e che sia in grado di accettare le connessioni.

**Per connettersi a Oracle**

1. Scegliere **Connetti a Oracle** dal menu **file** .  
   Se in precedenza è stata effettuata la connessione a Oracle, il nome del comando verrà **riconnesso a Oracle**.
  
2. Nella casella **provider** selezionare **provider client Oracle** o **provider OLE DB**, a seconda del provider installato. Il valore predefinito è Oracle client.

3. Nella casella **modalità** selezionare la modalità **standard**, la modalità **TNSNAME** o la **modalità stringa di connessione**.  
   Utilizzare la modalità standard per specificare il nome e la porta del server. Utilizzare la modalità nome servizio per specificare manualmente il nome del servizio Oracle. Utilizzare la modalità stringa di connessione per fornire una stringa di connessione completa.

4. Se si seleziona la **modalità standard**, fornire i valori seguenti:
   1. Nella casella **nome server** immettere o selezionare il nome o l'indirizzo IP del server di database.
   2. Se il server di database non è configurato per accettare le connessioni sulla porta predefinita (1521), immettere il numero di porta utilizzato per le connessioni Oracle nella casella **porta server** .
   3. Nella casella **Oracle SID** immettere l'identificatore di sistema.
   4. Nella casella **nome utente** immettere un account Oracle con le autorizzazioni necessarie.
   5. Nella casella **password** immettere la password per il nome utente specificato.

5. Se si seleziona la **modalità TNSNAME**, specificare i valori seguenti:
   1. Nella casella **identificatore di connessione** immettere identificatore di connessione (ID TNS) del database.
   2. Nella casella **nome utente** immettere un account Oracle con le autorizzazioni necessarie.
   3. Nella casella **password** immettere la password per il nome utente specificato.
  
6. Se si seleziona la **modalità stringa di connessione**, specificare una stringa di connessione nella casella **stringa di connessione** .  
   Nell'esempio seguente viene illustrata una stringa di connessione OLE DB:

   `Provider=OraOLEDB.Oracle;Data Source=MyOracleDB;User Id=myUsername;Password=myPassword;`

   Nell'esempio seguente viene illustrata una stringa di connessione del client Oracle che utilizza la sicurezza integrata:

   `Data Source=MyOracleDB;Integrated Security=yes;`

   Per altre informazioni, vedere [connettersi a Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md).

## <a name="reconnecting-to-oracle"></a>Riconnessione a Oracle

La connessione al server di database rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi se si desidera una connessione attiva al database. È possibile lavorare offline fino a quando non si desidera aggiornare i metadati, caricare oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed eseguire la migrazione dei dati.

## <a name="refreshing-oracle-metadata"></a>Aggiornamento dei metadati Oracle

I metadati relativi al database Oracle non vengono aggiornati automaticamente. I metadati in Oracle Metadata Explorer sono uno snapshot dei metadati al momento della prima connessione oppure l'ultima volta che i metadati sono stati aggiornati manualmente. È possibile aggiornare manualmente i metadati per tutti gli schemi, un singolo schema o singoli oggetti di database.

**Per aggiornare i metadati**

1. Assicurarsi di essere connessi al database.

2. In Esplora metadati Oracle selezionare la casella di controllo accanto a ogni schema o oggetto di database che si desidera aggiornare.

3. Fare clic con il pulsante destro del mouse su **schemi** oppure sul singolo schema o oggetto di database, quindi scegliere **Aggiorna da database**.  
   Se non si dispone di una connessione attiva, in SSMA viene visualizzata la finestra **di dialogo Connetti a Oracle in** cui è possibile connettersi.

4. Nella finestra di dialogo Aggiorna da database specificare gli oggetti da aggiornare.

   - Per aggiornare un oggetto, fare clic sul campo **attivo** adiacente all'oggetto fino a quando non viene visualizzata una freccia.
   - Per impedire l'aggiornamento di un oggetto, fare clic sul campo **attivo** adiacente all'oggetto fino a quando non viene visualizzata una **X** .
   - Per aggiornare o rifiutare una categoria di oggetti, fare clic sul campo **attivo** accanto alla cartella Category.

   Per visualizzare le definizioni della codifica dei colori, fare clic sul pulsante **Legenda** .

5. [!INCLUDE[clickOK](../../includes/clickok-md.md)]

## <a name="next-steps"></a>Passaggi successivi

Il passaggio successivo del processo di migrazione consiste nel [connettersi a un'istanza di SQL Server](connecting-to-sql-server-oracletosql.md).

## <a name="see-also"></a>Vedi anche

[Migrazione di database Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
