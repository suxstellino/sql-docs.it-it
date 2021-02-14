---
title: Connessione al database DB2 (DB2ToSQL) | Microsoft Docs
description: Informazioni su come connettersi a un'istanza di destinazione del database DB2 per eseguire la migrazione di database DB2. SSMA ottiene i metadati relativi a tutti gli schemi DB2.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5eb5801d-f0c3-4127-97c0-0b1ef49f4844
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b7a2bd0434e865e3748861b94b21d44615e07ed5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081612"
---
# <a name="connecting-to-db2-database-db2tosql"></a>Connessione al database DB2 (DB2ToSQL)

Per eseguire la migrazione dei database DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario connettersi al database DB2 di cui si desidera eseguire la migrazione. Quando si esegue la connessione, SSMA ottiene i metadati relativi a tutti gli schemi DB2 e quindi li Visualizza nel riquadro DB2 Metadata Explorer. SSMA archivia le informazioni sul server di database, ma non archivia le password.

La connessione al database rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi se si desidera una connessione attiva al database.

I metadati relativi al database DB2 non vengono aggiornati automaticamente. Se invece si desidera aggiornare i metadati in DB2 Metadata Explorer, è necessario aggiornarli manualmente. Per ulteriori informazioni, vedere la sezione "aggiornamento dei metadati DB2" più avanti in questo argomento.

## <a name="required-db2-permissions"></a>Autorizzazioni DB2 obbligatorie

L'autorizzazione utente definisce l'elenco dei comandi e degli oggetti disponibili per un utente. Questo elenco controlla quindi le azioni dell'utente. In DB2 sono disponibili gruppi di privilegi predeterminati per l'autorizzazione, a livello di istanza e a livello di database DB2. Ciò consente a SSMA di ottenere i metadati dagli schemi di proprietà dell'utente che si connette. Per ottenere i metadati per gli oggetti di altri schemi e quindi convertire gli oggetti in tali schemi, l'account deve disporre delle autorizzazioni seguenti:

- L'accesso allo schema per la migrazione dello schema viene generalmente concesso a PUBLIC, a meno che non sia stata utilizzata la parola chiave RESTRICT nella creazione
- L'accesso ai dati per la migrazione dei dati richiede DataAccess

## <a name="establishing-a-connection-to-db2"></a>Stabilire una connessione a DB2

Quando ci si connette a un database, SSMA legge i metadati del database e quindi aggiunge questi metadati al file di progetto. Questi metadati vengono usati da SSMA quando converte gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi e quando esegue la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile esplorare questi metadati nel riquadro DB2 Metadata Explorer ed esaminare le proprietà dei singoli oggetti di database.  

> [!IMPORTANT]
> Prima di provare a connettersi, verificare che il server di database sia in esecuzione e che sia in grado di accettare le connessioni.

**Per connettersi a DB2**

1. Scegliere **Connetti a DB2** dal menu **file** .

   Se in precedenza si è connessi a DB2, il nome del comando verrà **riconnesso a DB2**.

2. Nella casella **provider** verrà visualizzato il **provider OLE DB** che è attualmente l'unico provider di accesso client DB2.

3. Nella casella **Manager** è possibile selezionare **DB2 per zOs**, **DB2 per LUW** o **DB2 per i**

4. Nella casella **modalità** selezionare **modalità standard** o **modalità stringa di connessione**.

   Utilizzare la modalità standard per specificare il nome e la porta del server. Usare la modalità nome servizio per specificare manualmente il nome del servizio DB2. Utilizzare la modalità stringa di connessione per fornire una stringa di connessione completa.

5. Se si seleziona la **modalità standard**, fornire i valori seguenti:

   - Nella casella **nome server** immettere o selezionare il nome o l'indirizzo IP del server di database.
   - Se il server di database non è configurato per accettare le connessioni sulla porta predefinita (1521), immettere il numero di porta usato per le connessioni DB2 nella casella **porta server** .
   - Nella casella **porta server** immettere il numero di porta TCP/IP.
   - Nella casella **catalogo iniziale** immettere il nome del database.
   - Nella casella **nome utente** immettere un account DB2 con le autorizzazioni necessarie.
   - Nella casella **password** immettere la password per il nome utente specificato.

6. Se si seleziona la **modalità stringa di connessione**, specificare una stringa di connessione nella casella **stringa di connessione** .

   Nell'esempio seguente viene illustrata una stringa di connessione OLE DB:

   `Provider=OraOLEDB.DB2;Data Source=MyDB2DB;User Id=myUsername;Password=myPassword;`

   Nell'esempio seguente viene illustrata una stringa di connessione del client DB2 che utilizza la sicurezza integrata:
  
   `Data Source=MyDB2DB;Integrated Security=yes;`

   Per altre informazioni, vedere [connettersi a Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md).
  
## <a name="reconnecting-to-db2"></a>Riconnessione a DB2

La connessione al server di database rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi se si desidera una connessione attiva al database. È possibile lavorare offline fino a quando non si desidera aggiornare i metadati, caricare oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed eseguire la migrazione dei dati.

## <a name="refreshing-db2-metadata"></a>Aggiornamento dei metadati DB2

I metadati relativi al database DB2 non vengono aggiornati automaticamente. I metadati in DB2 Metadata Explorer sono uno snapshot dei metadati al momento della prima connessione o dell'ultima volta in cui sono stati aggiornati manualmente i metadati. È possibile aggiornare manualmente i metadati per tutti gli schemi, un singolo schema o singoli oggetti di database.

**Per aggiornare i metadati**

1. Assicurarsi di essere connessi al database.
2. In DB2 Metadata Explorer selezionare la casella di controllo accanto a ogni schema o oggetto di database che si desidera aggiornare.
3. Fare clic con il pulsante destro del mouse su **schemi** oppure sul singolo schema o oggetto di database, quindi scegliere **Aggiorna da database**.

   Se non si dispone di una connessione attiva, in SSMA viene visualizzata la finestra **di dialogo Connetti a DB2 in** cui è possibile connettersi.
  
4. Nella finestra di dialogo Aggiorna da database specificare gli oggetti da aggiornare.
   - Per aggiornare un oggetto, fare clic sul campo **attivo** adiacente all'oggetto fino a quando non viene visualizzata una freccia.
   - Per impedire l'aggiornamento di un oggetto, fare clic sul campo **attivo** adiacente all'oggetto fino a quando non viene visualizzata una **X** .
   - Per aggiornare o rifiutare una categoria di oggetti, fare clic sul campo **attivo** accanto alla cartella Category.

     Per visualizzare le definizioni della codifica dei colori, fare clic sul pulsante **Legenda** .

5. [!INCLUDE[click OK](../../includes/clickok-md.md)]

## <a name="next-step"></a>passaggio successivo

- Il passaggio successivo del processo di migrazione consiste nel [connettersi a SQL Server](./connecting-to-sql-server-db2tosql.md).

## <a name="see-also"></a>Vedere anche

- [Migrazione di database DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)