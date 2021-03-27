---
title: Chiamata di un stored procedure (OLE DB)
description: Informazioni su come chiamare una stored procedure in OLE DB Driver per SQL Server, inclusa la procedura per passare i valori dei parametri.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- calling stored procedures
- RPC escape sequence
- OLE DB, stored procedures
- parameters [OLE DB Driver for SQL Server], OLE DB
- ODBC CALL escape sequence
- stored procedures [OLE DB], calling
- OLE DB Driver for SQL Server, stored procedures
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 601e2c07e7b2abb188e75ace02d1856945e0d689
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633300"
---
# <a name="calling-a-stored-procedure-ole-db"></a>Chiamata di un stored procedure (OLE DB)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Una stored procedure può avere zero o più parametri. Può inoltre restituire un valore. Quando si usa OLE DB Driver per SQL Server, è possibile passare i parametri in una stored procedure nel modo seguente:  
  
- Specificando il valore dei dati a livello di codice.
- Utilizzando un marcatore di parametro (?) per specificare i parametri, associare una variabile di programma all'marcatore di parametro e quindi inserire il valore dei dati nella variabile di programma.

> [!NOTE]
> Quando si esegue una chiamata alle stored procedure [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usando parametri denominati con OLE DB, i nomi dei parametri devono iniziare con il carattere "\@". Si tratta di una restrizione specifica di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. OLE DB Driver per SQL Server applica questa limitazione in modo più restrittivo rispetto a MDAC.
  
Per supportare i parametri, l'interfaccia **ICommandWithParameters** viene esposta nell'oggetto comando. Per usare i parametri, il consumer descrive prima i parametri al provider chiamando il metodo **ICommandWithParameters::SetParameterInfo** (o facoltativamente prepara un'istruzione di chiamata che chiama il metodo **GetParameterInfo**). Il consumer crea quindi una funzione di accesso che specifica la struttura di un buffer e inserisce i valori dei parametri in questo buffer. Infine, passa l'handle della funzione di accesso e un puntatore al buffer a **Execute**. Nelle chiamate successive a **Execute**, il consumer inserisce i nuovi valori dei parametri nel buffer e chiama **Execute** con l'handle della funzione di accesso e il puntatore al buffer.

Un comando che chiama un stored procedure temporaneo usando i parametri deve prima chiamare **ICommandWithParameters:: separameterinfo** per definire le informazioni sui parametri. Questa chiamata deve essere eseguita prima che il comando possa essere preparato correttamente. Questo requisito è dovuto al fatto che il nome interno di una stored procedure temporanea è diverso dal nome esterno usato da un client. MSOLEDBSQL non è in grado di eseguire query sulle tabelle di sistema per determinare le informazioni sui parametri per un stored procedure temporaneo.

Gli elementi seguenti sono i passaggi del processo di associazione dei parametri:

1. Completare le informazioni sui parametri in una matrice di strutture DBPARAMBINDINFO, ovvero nome del parametro, nome specifico del provider per il tipo di dati del parametro o nome del tipo di dati standard e così via. Ogni struttura della matrice descrive un solo parametro. Questa matrice viene quindi passata al metodo **SetParameterInfo**.
2. Chiamare il metodo **ICommandWithParameters::SetParameterInfo** per descrivere i parametri al provider. **SetParameterInfo** specifica il tipo di dati nativo di ogni parametro. Gli argomenti di **SetParameterInfo** sono:
    - Numero di parametri per i quali impostare le informazioni sul tipo.
    - Matrice di numeri ordinali dei parametri per i quali impostare le informazioni sul tipo.
    - Matrice di strutture DBPARAMBINDINFO.
3. Creare una funzione di accesso ai parametri usando il comando **IAccessor::CreateAccessor**. La funzione di accesso specifica la struttura di un buffer e inserisce i valori dei parametri nel buffer. Il comando **CreateAccessor** crea una funzione di accesso da un set di associazioni. Queste associazioni vengono descritte dal consumer mediante una matrice di strutture DBBINDING. Ogni associazione associa un singolo parametro al buffer del consumer e contiene le seguenti informazioni:
    - Numero ordinale del parametro a cui viene applicata l'associazione.
    - Elemento da associare (valore dei dati, lunghezza e stato).
    - Offset nel buffer per ognuna di queste parti.
    - Lunghezza e tipo del valore dei dati esattamente come riportati nel buffer del consumer.

    Una funzione di accesso viene identificata dal relativo handle di tipo HACCESSOR. Questo handle viene restituito dal metodo **CreateAccessor**. Al termine dell'uso di una funzione di accesso, il consumer deve chiamare il metodo **ReleaseAccessor** per rilasciarne la memoria.  
  
    Quando il consumer chiama un metodo, ad esempio **ICommand::Execute**, passa l'handle a una funzione di accesso e a un puntatore a un buffer. Il provider utilizza questa funzione di accesso per determinare il modo in cui trasferire i dati contenuti nel buffer.
4. Completare la struttura DBPARAMS. Le variabili del consumer, da cui vengono acquisiti i valori dei parametri di input e in cui vengono scritti i valori dei parametri di output, vengono passate in fase di esecuzione a **ICommand::Execute** nella struttura DBPARAMS. La struttura DBPARAMS include tre elementi:
    - Puntatore al buffer da cui il provider recupera i dati dei parametri di input e in cui il provider restituisce i dati dei parametri di output, in base alle associazioni specificate dall'handle della funzione di accesso.
    - Numero di set di parametri nel buffer.
    - Handle della funzione di accesso creato nel passaggio 3.
5. Eseguire il comando usando **ICommand::Execute**.

## <a name="methods-of-calling-a-stored-procedure"></a>Metodi di chiamata di un stored procedure

Quando si esegue una stored procedure in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], OLE DB Driver per SQL Server supporta gli elementi seguenti:

- Sequenza di escape ODBC CALL.
- Sequenza di escape RPC (Remote Procedure Call).
- Istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)]EXECUTE.

### <a name="odbc-call-escape-sequence"></a>sequenza di escape ODBC CALL  

Se si conoscono le informazioni sui parametri, chiamare il metodo **ICommandWithParameters::SetParameterInfo** per descrivere i parametri al provider. In caso contrario, quando viene utilizzata la sintassi ODBC CALL per eseguire la chiamata a una stored procedure, il provider chiama una funzione di supporto per trovare le informazioni sui parametri della stored procedure.

Se non si è certi delle informazioni sui parametri (metadati del parametro), è consigliabile usare la sintassi ODBC CALL.

La sintassi generale per la chiamata a una stored procedure mediante la sequenza di escape ODBC CALL è:

{[ **? =** ]**chiamare**_procedure\_nome_[ **(** [*parametro*] [ **,** [_parametro_]]... **)** ]}

Ad esempio:

```sql
{call SalesByCategory('Produce', '1995')}
```

### <a name="rpc-escape-sequence"></a>sequenza di escape RPC

La sequenza di escape RPC è simile alla sintassi ODBC CALL della chiamata a una stored procedure. Quando si chiama la stored procedure più volte, la sequenza di escape RPC è la migliore delle tre modalità di chiamata di un stored procedure.

Quando viene utilizzata la sequenza di escape RPC per eseguire una stored procedure, il provider non chiama alcuna funzione di supporto per determinare le informazioni sui parametri (come avviene con la sintassi ODBC CALL). La sintassi RPC è più semplice della sintassi ODBC CALL, pertanto il comando viene analizzato più velocemente, migliorando le prestazioni. In questo caso, è necessario fornire le informazioni sui parametri eseguendo **ICommandWithParameters::SetParameterInfo**.

Per utilizzare la sequenza di escape RPC è necessario disporre di un valore restituito. Se il stored procedure non restituisce un valore, il server restituisce 0 per impostazione predefinita. Inoltre, non è possibile aprire un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cursore sul stored procedure. La stored procedure viene preparata in modo implicito e la chiamata a **ICommandPrepare::Prepare** avrà esito negativo. A causa dell'impossibilità di preparare una chiamata RPC, non è possibile eseguire query sui metadati della colonna. IColumnsInfo:: GetColumnInfo e IColumnsRowset:: GetColumnsRowset restituiranno DB_E_NOTPREPARED.

Se si conoscono tutti i metadati dei parametri, la sequenza di escape RPC è la modalità consigliata per eseguire le stored procedure.

Questo SQL è un esempio di sequenza di escape RPC per la chiamata di un stored procedure:

```sql
{rpc SalesByCategory}
```

Per un'applicazione di esempio che illustra una sequenza di escape RPC, vedere [Eseguire una stored procedure &#40;con la sintassi RPC&#41; ed elaborare i codici restituiti e i parametri di output &#40;OLE DB&#41;](../ole-db-how-to/results/execute-stored-procedure-with-rpc-and-process-output.md).

### <a name="transact-sql-execute-statement"></a>Transact-SQL EXECUTE-istruzione

La sequenza di escape ODBC CALL e la sequenza di escape RPC rappresentano i metodi preferiti per la chiamata a una stored procedure rispetto all'istruzione [EXECUTE](../../../t-sql/language-elements/execute-transact-sql.md). OLE DB Driver per SQL Server usa il meccanismo RPC di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per ottimizzare l'elaborazione del comando. Il protocollo RPC aumenta le prestazioni eliminando la maggior parte dell'elaborazione dei parametri e l'analisi delle istruzioni eseguite sul server.

Il codice SQL seguente è un esempio dell' [!INCLUDE[tsql](../../../includes/tsql-md.md)] istruzione **Execute** :

```sql
EXECUTE SalesByCategory 'Produce', '1995'
```

## <a name="see-also"></a>Vedi anche

[Stored procedure](stored-procedures.md)
