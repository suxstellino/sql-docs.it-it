---
title: Generazione dei comandi con CommandBuilders
description: Questo articolo illustra come usare i compilatori di comandi per generare automaticamente i comandi INSERT, UPDATE e DELETE per un `DataAdapter` che include un comando SELECT a tabella singola.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 6e3fb8b5-373b-4f9e-ab03-a22693df8e91
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: d738650db9dbb7b0b2ed3f9411945173f73f748f
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771318"
---
# <a name="generating-commands-with-commandbuilders"></a>Generazione dei comandi con CommandBuilders

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Quando la proprietà `SelectCommand` dell'oggetto <xref:System.Data.Common.DbDataAdapter> viene specificata in modo dinamico in fase di esecuzione, ad esempio tramite uno strumento di query che accetta un comando testuale dall'utente, potrebbe non essere possibile specificare gli `InsertCommand`, `UpdateCommand` o `DeleteCommand` appropriati in fase di progettazione. Se <xref:System.Data.DataTable> esegue il mapping o viene generato da una singola tabella di database, è possibile usare l'oggetto <xref:System.Data.Common.DbCommandBuilder> per generare automaticamente gli oggetti `DeleteCommand`, `InsertCommand` e `UpdateCommand` di <xref:System.Data.Common.DbDataAdapter>.

> [!NOTE]
> Nel provider di dati Microsoft SqlClient per SQL Server la classe <xref:Microsoft.Data.SqlClient.SqlDataAdapter> è derivata dalla classe <xref:System.Data.Common.DbDataAdapter> e la classe <xref:Microsoft.Data.SqlClient.SqlCommandBuilder> è derivata dalla classe <xref:System.Data.Common.DbCommandBuilder>.

Come requisito minimo, è necessario impostare la proprietà `SelectCommand` affinché la generazione automatica del comando venga eseguita correttamente. Lo schema della tabella recuperato dalla proprietà `SelectCommand` determina la sintassi delle istruzioni INSERT, UPDATE e DELETE generate automaticamente.

<xref:System.Data.Common.DbCommandBuilder> deve eseguire `SelectCommand` in modo da restituire i metadati necessari per creare i comandi SQL INSERT, UPDATE e DELETE. Di conseguenza, è necessario un ulteriore percorso all'origine dati che può ridurre le prestazioni. Per ottenere prestazioni ottimali, specificare i comandi in modo esplicito anziché usare <xref:System.Data.Common.DbCommandBuilder>.

> [!NOTE]
> È inoltre necessario che `SelectCommand` restituisca almeno una chiave primaria o una colonna univoca. In caso contrario, viene generata un'opzione `InvalidOperation` e i comandi non vengono generati.

Quando è associato a un `DataAdapter`, <xref:System.Data.Common.DbCommandBuilder> genera automaticamente le proprietà `InsertCommand`, `UpdateCommand` e `DeleteCommand` del `DataAdapter`, se queste sono riferimenti Null. Se esiste già un oggetto `Command` per una proprietà, viene usato l'oggetto `Command` esistente.

Le visualizzazioni di database create dall'unione di due o più tabelle non vengono considerate un'unica tabella di database. In questa istanza, non è possibile usare <xref:System.Data.Common.DbCommandBuilder> per generare i comandi automaticamente. È necessario specificarli in modo esplicito. Per informazioni sull'impostazione esplicita dei comandi per applicare gli aggiornamenti a `DataSet` nell'origine dati, vedere [Aggiornare origini dati con DataAdapter](update-data-sources-with-dataadapters.md).

È possibile eseguire il mapping dei parametri di output alla riga aggiornata di un `DataSet`. Un'attività comune sarebbe il recupero del valore di un campo identità generato automaticamente o del timestamp da un'origine dati. Per impostazione predefinita, <xref:System.Data.Common.DbCommandBuilder> non esegue il mapping dei parametri di output sulle colonne in una riga aggiornata. In questo caso, è necessario specificare il comando in modo esplicito. Per un esempio di mapping di un campo identità generato automaticamente su una colonna di una riga inserita, vedere [Recuperare l’identità o valori numerati automaticamente](retrieve-identity-or-autonumber-values.md).

## <a name="rules-for-automatically-generated-commands"></a>Regole per i comandi generati automaticamente

Nella tabella seguente vengono illustrate le regole per generare automaticamente i comandi.

|Comando|Regola|  
|-------------|----------|  
|`InsertCommand`|Consente di inserire una riga nell'origine dati per ogni riga della tabella con una proprietà <xref:System.Data.DataRow.RowState%2A> uguale a <xref:System.Data.DataRowState.Added>. I valori vengono inseriti in tutte le colonne aggiornabili, quindi non nelle colonne di identità, espressioni o timestamp.|  
|`UpdateCommand`|Aggiorna le righe nell'origine dati per tutte le righe della tabella con `RowState` uguale a <xref:System.Data.DataRowState.Modified>. I valori vengono aggiornati in tutte le colonne aggiornabili, quindi non nelle colonne di identità o di espressioni. Vengono aggiornate tutte le righe in cui i valori delle colonne nell'origine dati corrispondono ai valori delle colonne della chiave primaria della riga e in cui le restanti colonne dell'origine dati corrispondono ai valori originali della riga. Per altre informazioni, vedere la sezione "Modello di concorrenza ottimistica per aggiornamenti ed eliminazioni" contenuta in questo argomento.|  
|`DeleteCommand`|Elimina le righe nell'origine dati per tutte le righe della tabella con `RowState` uguale a <xref:System.Data.DataRowState.Deleted>. Vengono eliminate tutte le righe in cui i valori delle colonne corrispondono ai valori delle colonne della chiave primaria della riga e in cui le restanti colonne dell'origine dati corrispondono ai valori originali della riga. Per altre informazioni, vedere [Modello di concorrenza ottimistica per gli aggiornamenti e le eliminazioni](#optimistic-concurrency-model-for-updates-and-deletes), di seguito in questo argomento.|

## <a name="optimistic-concurrency-model-for-updates-and-deletes"></a>Modello di concorrenza ottimistica per gli aggiornamenti e le eliminazioni

La logica per generare automaticamente i comandi per le istruzioni UPDATE e DELETE si basa sulla *concorrenza ottimistica*. In altre parole, i record non vengono bloccati per la modifica e altri utenti o processi possono modificarli in qualsiasi momento. Poiché un record può essere modificato dopo che è stato restituito dall'istruzione SELECT, ma prima che sia eseguita l'istruzione UPDATE o DELETE, nell'istruzione UPDATE o DELETE generata automaticamente è contenuta una clausola WHERE, tale che una riga viene aggiornata solo se contiene tutti i valori originali e non è stata eliminata dall'origine dati. In questo modo si evita di sovrascrivere i nuovi dati.
 
> [!NOTE]
> Nei casi in cui un comando Update generato automaticamente tenta di aggiornare una riga che è stata eliminata o che non contiene i valori originali rilevati in <xref:System.Data.DataSet>, il comando non ha alcun effetto sui record e viene generata un'eccezione <xref:System.Data.DBConcurrencyException>.

Se si desidera che le istruzioni UPDATE o DELETE vengano completate a prescindere dai valori originali, è necessario impostare in modo esplicito `UpdateCommand` per il `DataAdapter` e non usare la generazione automatica dei comandi.

## <a name="limitations-of-automatic-command-generation-logic"></a>Limiti della logica della generazione automatica dei comandi

I limiti che seguono si riferiscono alla generazione automatica dei comandi.

### <a name="unrelated-tables-only"></a>Solo tabelle non correlate

La logica per la generazione automatica dei comandi genera un'istruzione INSERT, UPDATE o DELETE per le tabelle autonome senza tenere conto delle relazioni con altre tabelle nell'origine dati. Di conseguenza, è possibile che venga rilevato un errore quando si chiama `Update` per inviare le modifiche a una colonna che fa parte di un vincolo di chiave esterna nel database. Per evitare questa eccezione, non usare <xref:System.Data.Common.DbCommandBuilder> per aggiornare le colonne incluse in un vincolo di chiave esterna e specificare in modo esplicito le istruzioni usate per eseguire l'operazione.

### <a name="table-and-column-names"></a>Nomi di tabella e colonne

La logica per la generazione automatica dei comandi potrebbe non essere eseguita correttamente se i nomi delle colonne o delle tabelle contengono caratteri speciali quali spazi, punti, virgolette o altri caratteri non alfanumerici, anche se racchiusi tra parentesi. A seconda del provider, l'impostazione dei parametri QuoteSuffix e QuotePrefix può consentire alla logica di generazione di elaborare gli spazi, ma non i caratteri speciali di escape. Sono supportati i nomi di tabella completi nel formato *catalog.schema.table*.

## <a name="use-the-commandbuilder-to-automatically-generate-an-sql-statement"></a>Usare CommandBuilder per generare automaticamente un'istruzione SQL

Per generare automaticamente le istruzioni SQL per un oggetto `DataAdapter`, impostare innanzitutto la proprietà `SelectCommand` di `DataAdapter`. Creare quindi un oggetto `CommandBuilder` e specificare come argomento l'oggetto `DataAdapter` per il quale l'oggetto `CommandBuilder` genererà automaticamente le istruzioni SQL.

[!code-csharp[SqlCommandBuilder_Create#1](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#1)]

## <a name="modify-the-selectcommand"></a>Modificare SelectCommand

Se si modifica `CommandText` di `SelectCommand` dopo la generazione automatica di un comando INSERT, UPDATE o DELETE, è possibile che venga generata un'eccezione. Se l'oggetto `SelectCommand.CommandText` modificato contiene informazioni sullo schema che non sono coerenti con l'oggetto `SelectCommand.CommandText` usato durante la generazione automatica del comando di inserimento, aggiornamento o eliminazione, nelle future chiamate al metodo `DataAdapter.Update` potrebbe essere eseguito un tentativo di accesso a colonne che non esistono più nella tabella corrente a cui `SelectCommand` fa riferimento e verrà generata un'eccezione.

È possibile aggiornare le informazioni sullo schema usate da `CommandBuilder` per generare automaticamente i comandi chiamando il metodo `RefreshSchema` di `CommandBuilder`.

Se si desidera conoscere il comando che è stato generato automaticamente, è possibile ottenere un riferimento ai comandi generati automaticamente tramite i metodi `GetInsertCommand`, `GetUpdateCommand` e `GetDeleteCommand` dell'oggetto `CommandBuilder` e selezionare la proprietà `CommandText`del comando associato.

Nell'esempio di codice seguente viene scritto sulla console il comando di aggiornamento che è stato generato automaticamente.

[!code-csharp[SqlCommandBuilder_Create#2](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#2)]

Nell'esempio seguente viene ricreata la tabella nel set di dati. Il metodo **RefreshSchema** viene chiamato per aggiornare i comandi generati automaticamente con le informazioni della nuova colonna.

[!code-csharp[SqlCommandBuilder_Create#3](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#3)]

## <a name="see-also"></a>Vedere anche

- [Comandi e parametri](commands-parameters.md)
- [Esecuzione di un comando](execute-command.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
