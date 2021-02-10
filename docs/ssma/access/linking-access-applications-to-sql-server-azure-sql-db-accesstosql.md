---
title: Collegare le applicazioni ad SQL Server-database SQL di Azure | Microsoft Docs
description: Informazioni su come collegare le tabelle di accesso alle tabelle migrate in modo da poter usare le applicazioni di accesso esistenti con SQL Server o il database SQL di Azure.
ms.prod: sql
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, linking to SQL Azure
- Access databases, linking to SQL Server
- auto-increment columns
- data types, unsupported
- hyperlink columns
- linking tables
- migrating databases, post-migration issues
- post-migration issues
- reference, post-migration issues
- refreshing linked tables
- slow performance
- unlinking tables
ms.assetid: 82374ad2-7737-4164-a489-13261ba393d4
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 53fbbd5a865594f46eacb75c9799b9c956efb2da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059042"
---
# <a name="linking-access-applications-to-sql-server---azure-sql-database-accesstosql"></a>Collegamento di applicazioni di accesso a SQL Server-database SQL di Azure (AccessToSQL)
Se si desidera utilizzare le applicazioni di accesso esistenti con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile collegare le tabelle di accesso originali alle tabelle migrate [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Il collegamento modifica il database di Access in modo che le query, i form, i report e le pagine di accesso ai dati usino i dati nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database di o SQL di Azure anziché i dati nel database di Access.  
  
> [!NOTE]  
> Le tabelle di accesso rimangono in accesso, ma non vengono aggiornate insieme a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure aggiornamenti. Dopo aver collegato le tabelle e verificato la funzionalità, potrebbe essere necessario eliminare le tabelle di accesso.  
  
## <a name="linking-access-and-sql-server-tables"></a>Collegamento di accesso e tabelle di SQL Server  
Quando si collega una tabella di accesso a una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabella o SQL Azure, il motore di database Jet archivia le informazioni di connessione e i metadati della tabella, ma i dati vengono archiviati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Questo collegamento consente alle applicazioni di accesso di operare sulle tabelle di accesso anche se le tabelle e i dati effettivi sono in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
> [!NOTE]  
> Se si usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di, la password viene archiviata in testo non crittografato nelle tabelle di accesso collegato. Si consiglia di usare l'autenticazione di Windows.  
  
**Per collegare tabelle**  
  
1.  In Esplora metadati di Access selezionare le tabelle che si desidera collegare.  
  
2.  Fare clic con il pulsante destro del mouse su **tabelle**, quindi scegliere **collega**.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) per l'accesso esegue il backup della tabella di accesso originale e crea una tabella collegata.  
  
Dopo aver collegato le tabelle, le tabelle in SSMA vengono visualizzate con un'icona di collegamento di piccole dimensioni. In Access, le tabelle vengono visualizzate con un'icona "collegata", che è un globo con una freccia che punta a essa.  
  
Quando si apre una tabella in Access, i dati vengono recuperati utilizzando un cursore keyset. Di conseguenza, per le tabelle di grandi dimensioni tutti i dati non vengono recuperati contemporaneamente. Tuttavia, quando si Esplora la tabella, Access recupera i dati aggiuntivi, se necessario.  
  
> [!IMPORTANT]  
> Per collegare le tabelle di accesso a un database di Azure, è necessario SQL Server Native Client (SNAC) versione 10,5 o successiva.   
> È possibile ottenere la versione più recente di SNAC da [Microsoft® SQL Server® 2008 R2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=44272).  
  
## <a name="unlinking-access-tables"></a>Scollegamento di tabelle di accesso  
Quando si scollega una tabella di accesso da una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabella o SQL Azure, SSMA ripristina la tabella di accesso originale e i relativi dati.  
  
**Per scollegare le tabelle**  
  
1.  In Esplora metadati di Access selezionare le tabelle che si desidera scollegare.  
  
2.  Fare clic con il pulsante destro del mouse su **tabelle**, quindi scegliere **Scollega**.  
  
## <a name="linking-tables-to-a-different-server"></a>Collegamento di tabelle a un server diverso  
Se le tabelle di accesso sono state collegate a un'istanza di SQL Server e successivamente si desidera modificare i collegamenti a un'altra istanza, è necessario ricollegare le tabelle.  
  
**Per collegare le tabelle a un server diverso**  
  
1.  In Esplora metadati di Access selezionare le tabelle che si desidera scollegare.  
  
2.  Fare clic con il pulsante destro del mouse su **tabelle** , quindi scegliere **Scollega**.  
  
3.  Fare clic sul pulsante **Riconnetti a SQL Server** .  
  
4.  Connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure a cui si desidera collegare le tabelle di accesso.  
  
5.  In Esplora metadati di Access selezionare le tabelle che si desidera collegare.  
  
6.  Fare clic con il pulsante destro del mouse su **tabelle**, quindi scegliere **collega**.  
  
## <a name="updating-linked-tables"></a>Aggiornamento delle tabelle collegate  
Se le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definizioni di tabella o SQL Azure vengono modificate, è possibile scollegare e quindi collegare nuovamente le tabelle in SSMA utilizzando le procedure illustrate in precedenza in questo argomento. È anche possibile aggiornare le tabelle usando Access.  
  
**Per aggiornare le tabelle collegate tramite Access**  
  
1.  Aprire il database di Access.  
  
2.  Nell'elenco **oggetti** fare clic su **tabelle**.  
  
3.  Fare clic con il pulsante destro del mouse su una tabella collegata, quindi scegliere **gestore tabelle collegate**.  
  
4.  Selezionare la casella di controllo accanto a ogni tabella collegata che si desidera aggiornare, quindi fare clic su **OK**.  
  
## <a name="possible-post-migration-issues"></a>Possibili problemi post-migrazione  
Nelle sezioni seguenti sono elencati i problemi che possono verificarsi nelle applicazioni di accesso esistenti dopo aver eseguito la migrazione dei database dall'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure e quindi collegato alle tabelle, insieme alle cause e alle risoluzioni.  
  
### <a name="slow-performance-with-linked-tables"></a>Rallentamento delle prestazioni con le tabelle collegate  
**Motivo:** Alcune query potrebbero essere lente dopo il ridimensionamento per i motivi seguenti:  
  
-   L'applicazione dipende da funzioni che non esistono in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, che fa in modo che Jet esegua il pull delle tabelle localmente per eseguire una query SELECT.  
  
-   Le query che aggiornano o eliminano molte righe vengono inviate da Jet come una query con parametri per ogni riga.  
  
**Risoluzione:** Convertire le query con esecuzione lenta in query pass-through, stored procedure o viste. La conversione in query pass-through presenta i seguenti problemi:  
  
-   Non è possibile modificare le query pass-through. La modifica del risultato della query o l'aggiunta di nuovi record deve essere eseguita in modo alternativo, ad esempio tramite l' **aggiunta di pulsanti** espliciti o di **modifica** nel form associati alla query.  
  
-   Per alcune query è richiesto l'input dell'utente, ma le query pass-through non supportano l'input dell'utente. L'input dell'utente può essere ottenuto dal codice Visual Basic, Applications Edition (VBA) che richiede parametri o da un modulo usato come controllo di input. In entrambi i casi, il codice VBA invia la query con l'input dell'utente al server.  
  
### <a name="auto-increment-columns-are-not-updated-until-the-record-is-updated"></a>Le colonne con incremento automatico non vengono aggiornate fino a quando il record non viene aggiornato  
**Motivo:** Dopo aver chiamato RecordSet. AddNew in Jet, la colonna incremento automatico è disponibile prima dell'aggiornamento del record. Questo non è vero in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Il nuovo valore del nuovo valore della colonna Identity è disponibile solo dopo aver salvato il nuovo record.  
  
**Risoluzione:** Eseguire il seguente codice di Visual Basic, Applications Edition (VBA) prima di accedere al campo Identity:  
  
```  
Recordset.Update  
Recordset.Move 0,  
Recordset.LastModified  
```  
  
### <a name="new-records-are-not-available"></a>I nuovi record non sono disponibili  
**Motivo:** Quando si aggiunge un record a una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabella o SQL Azure usando VBA, se il campo indice univoco della tabella ha un valore predefinito e non si assegna un valore a tale campo, il nuovo record non viene visualizzato fino a quando non si riapre la tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Se si tenta di ottenere un valore dal nuovo record, viene visualizzato il messaggio di errore seguente:  
  
`Run-time error '3167' Record is deleted.`  
  
**Risoluzione:** Quando si apre la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabella o SQL Azure usando il codice VBA, includere l' `dbSeeChanges` opzione, come nell'esempio seguente:  
  
`Set rs = db.OpenRecordset("TestTable", dbOpenDynaset, dbSeeChanges)`  
  
### <a name="after-migration-some-queries-will-not-allow-the-user-to-add-a-new-record"></a>Dopo la migrazione, alcune query non consentiranno all'utente di aggiungere un nuovo record  
**Motivo:** Se una query non include tutte le colonne incluse in un indice univoco, non è possibile aggiungere nuovi valori utilizzando la query.  
  
**Risoluzione:** Verificare che tutte le colonne incluse in almeno un indice univoco facciano parte della query.  
  
### <a name="you-cannot-modify-a-linked-table-schema-with-access"></a>Non è possibile modificare uno schema di tabella collegato con Access  
**Motivo:** Dopo la migrazione dei dati e il collegamento delle tabelle, l'utente non può modificare lo schema di una tabella in Access.  
  
**Risoluzione:** Modificare lo schema della tabella utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , quindi aggiornare il collegamento in Access.  
  
### <a name="hyperlink-functionality-is-lost-after-migrating-data"></a>La funzionalità Hyperlink viene persa dopo la migrazione dei dati  
**Motivo:** Dopo la migrazione dei dati, i collegamenti ipertestuali nelle colonne perdono la loro funzionalità e diventano semplici colonne **nvarchar (max)** .  
  
**Risoluzione:** No.  
  
### <a name="some-sql-server-data-types-are-not-supported-by-access"></a>Alcuni tipi di dati SQL Server non sono supportati da Access  
**Motivo:** Se successivamente si aggiornano le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabelle o SQL Azure in modo che contengano tipi di dati non supportati da Access, non è possibile aprire la tabella in Access.  
  
**Risoluzione:** È possibile definire una query di accesso che restituisce solo le righe con i tipi di dati supportati.  
  
## <a name="see-also"></a>Vedi anche  
[Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
