---
description: Stored procedure-chiamata SQL Server Native Client
title: Chiamata a una stored procedure (OLE DB) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- calling stored procedures
- RPC escape sequence
- OLE DB, stored procedures
- parameters [SQL Server Native Client], OLE DB
- ODBC CALL escape sequence
- stored procedures [OLE DB], calling
- SQL Server Native Client OLE DB provider, stored procedures
ms.assetid: 8e5738e5-4bbe-4f34-bd69-0c0633290bdd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9e640cfb186e29f73f11f48cbd34a9cc4f7866e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744841"
---
# <a name="stored-procedures---calling-in-sql-server-native-client"></a>Stored procedure-chiamata SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Una stored procedure può avere zero o più parametri. Può inoltre restituire un valore. Quando si utilizza il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] provider di OLE DB di Native client, è possibile passare i parametri di un stored procedure:  
  
-   Specificando il valore dei dati a livello di codice.  
  
-   Utilizzando un marcatore di parametro (?) per specificare i parametri, associare una variabile di programma all'marcatore di parametro e quindi inserire il valore dei dati nella variabile di programma.  
  
> [!NOTE]  
>  Quando si esegue una chiamata alle stored procedure [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usando parametri denominati con OLE DB, i nomi dei parametri devono iniziare con il carattere "\@". Si tratta di una restrizione specifica di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il provider OLE DB di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client applica questa limitazione in modo più restrittivo rispetto a MDAC.  
  
 Per supportare i parametri, l'interfaccia **ICommandWithParameters** viene esposta nell'oggetto comando. Per usare i parametri, il consumer descrive prima i parametri al provider chiamando il metodo **ICommandWithParameters::SetParameterInfo** (o facoltativamente prepara un'istruzione di chiamata che chiama il metodo **GetParameterInfo**). Il consumer crea quindi una funzione di accesso che specifica la struttura di un buffer e inserisce i valori dei parametri in questo buffer. Infine, passa l'handle della funzione di accesso e un puntatore al buffer a **Execute**. Nelle chiamate successive a **Execute**, il consumer inserisce i nuovi valori dei parametri nel buffer e chiama **Execute** con l'handle della funzione di accesso e il puntatore al buffer.  
  
 Un comando che chiama una stored procedure temporanea con i parametri deve prima chiamare **ICommandWithParameters::SetParameterInfo** per definire le informazioni sui parametri, perché il comando possa essere preparato correttamente. Questa operazione è necessaria perché il nome interno di una stored procedure temporanea è diverso dal nome esterno utilizzato da un client e SQLOLEDB non può eseguire una query sulle tabelle di sistema per determinare le informazioni sui parametri per una stored procedure temporanea.  
  
 Di seguito sono riportati i passaggi del processo di associazione dei parametri:  
  
1.  Completare le informazioni sui parametri in una matrice di strutture DBPARAMBINDINFO, ovvero nome del parametro, nome specifico del provider per il tipo di dati del parametro o nome del tipo di dati standard e così via. Ogni struttura della matrice descrive un solo parametro. Questa matrice viene quindi passata al metodo **SetParameterInfo**.  
  
2.  Chiamare il metodo **ICommandWithParameters::SetParameterInfo** per descrivere i parametri al provider. **SetParameterInfo** specifica il tipo di dati nativo di ogni parametro. Gli argomenti di **SetParameterInfo** sono:  
  
    -   Numero di parametri per i quali impostare le informazioni sul tipo.  
  
    -   Matrice di numeri ordinali dei parametri per i quali impostare le informazioni sul tipo.  
  
    -   Matrice di strutture DBPARAMBINDINFO.  
  
3.  Creare una funzione di accesso ai parametri usando il comando **IAccessor::CreateAccessor**. La funzione di accesso specifica la struttura di un buffer e inserisce i valori dei parametri nel buffer. Il comando **CreateAccessor** crea una funzione di accesso da un set di associazioni. Queste associazioni vengono descritte dal consumer mediante una matrice di strutture DBBINDING. Ogni associazione associa un singolo parametro al buffer del consumer e contiene le seguenti informazioni:  
  
    -   Numero ordinale del parametro a cui viene applicata l'associazione.  
  
    -   Elemento da associare (valore dei dati, lunghezza e stato).  
  
    -   Offset nel buffer per ognuna di queste parti.  
  
    -   Lunghezza e tipo del valore dei dati esattamente come riportati nel buffer del consumer.  
  
     Una funzione di accesso viene identificata dal relativo handle di tipo HACCESSOR. Questo handle viene restituito dal metodo **CreateAccessor**. Al termine dell'uso di una funzione di accesso, il consumer deve chiamare il metodo **ReleaseAccessor** per rilasciarne la memoria.  
  
     Quando il consumer chiama un metodo, ad esempio **ICommand::Execute**, passa l'handle a una funzione di accesso e a un puntatore a un buffer. Il provider utilizza questa funzione di accesso per determinare il modo in cui trasferire i dati contenuti nel buffer.  
  
4.  Completare la struttura DBPARAMS. Le variabili del consumer, da cui vengono acquisiti i valori dei parametri di input e in cui vengono scritti i valori dei parametri di output, vengono passate in fase di esecuzione a **ICommand::Execute** nella struttura DBPARAMS. La struttura DBPARAMS include tre elementi:  
  
    -   Puntatore al buffer da cui il provider recupera i dati dei parametri di input e in cui il provider restituisce i dati dei parametri di output, in base alle associazioni specificate dall'handle della funzione di accesso.  
  
    -   Numero di set di parametri nel buffer.  
  
    -   Handle della funzione di accesso creato nel passaggio 3.  
  
5.  Eseguire il comando usando **ICommand::Execute**.  

## <a name="methods-of-calling-a-stored-procedure"></a>Metodi per l'esecuzione di una chiamata a una stored procedure  
 Quando si esegue un stored procedure in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] provider OLE DB di Native Client supporta:  
  
-   Sequenza di escape ODBC CALL.  
  
-   Sequenza di escape RPC (Remote Procedure Call).  
  
-   Istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)]EXECUTE.  
  
### <a name="odbc-call-escape-sequence"></a>Sequenza di escape ODBC CALL  
 Se si conoscono le informazioni sui parametri, chiamare il metodo **ICommandWithParameters::SetParameterInfo** per descrivere i parametri al provider. In caso contrario, quando viene utilizzata la sintassi ODBC CALL per eseguire la chiamata a una stored procedure, il provider chiama una funzione di supporto per trovare le informazioni sui parametri della stored procedure.  
  
 Se non si è certi delle informazioni sui parametri (metadati dei parametri), è consigliata la sintassi ODBC CALL.  
  
 La sintassi generale per la chiamata a una stored procedure mediante la sequenza di escape ODBC CALL è:  
  
 {[**? =**]**chiama**_procedure_name_[**(**[*parametro*] [**,**[*parametro*]]... **)**]}  
  
 Ad esempio:  
  
```  
{call SalesByCategory('Produce', '1995')}  
```  
  
### <a name="rpc-escape-sequence"></a>Sequenza di escape RPC  
 La sequenza di escape RPC è simile alla sintassi ODBC CALL della chiamata a una stored procedure. Se si chiama la stored procedure più volte, la sequenza di escape RPC offre le massime prestazioni tra i tre metodi utilizzati per chiamare una stored procedure.  
  
 Quando la sequenza di escape RPC viene utilizzata per eseguire una stored procedure, il provider non chiama la funzione di supporto per determinare le informazioni sui parametri (come avviene nel caso della sintassi ODBC CALL). La sintassi RPC è più semplice della sintassi ODBC CALL, pertanto il comando viene analizzato più velocemente, migliorando le prestazioni. In questo caso, è necessario fornire le informazioni sui parametri eseguendo **ICommandWithParameters::SetParameterInfo**.  
  
 Per utilizzare la sequenza di escape RPC è necessario disporre di un valore restituito. Se la stored procedure non restituisce un valore, il server restituisce per impostazione predefinita il valore 0. Inoltre, non è possibile aprire un cursore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nella stored procedure. La stored procedure viene preparata in modo implicito e la chiamata a **ICommandPrepare::Prepare** avrà esito negativo. A causa dell'impossibilità di preparare una chiamata RPC, non è possibile eseguire una query sui metadati delle colonne. IColumnsInfo::GetColumnInfo e IColumnsRowset::GetColumnsRowset restituiranno DB_E_NOTPREPARED.  
  
 Se si conoscono tutti i metadati dei parametri, la sequenza di escape RPC è la modalità consigliata per eseguire le stored procedure.  
  
 Di seguito viene riportato un esempio di sequenza di escape RPC per la chiamata a una stored procedure:  
  
```  
{rpc SalesByCategory}  
```  
  
 Per un'applicazione di esempio che illustra una sequenza di escape RPC, vedere [Eseguire una stored procedure &#40;con la sintassi RPC&#41; ed elaborare i codici restituiti e i parametri di output &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-how-to/results/execute-stored-procedure-with-rpc-and-process-output.md).  
  
### <a name="transact-sql-execute-statement"></a>Istruzione Transact-SQL EXECUTE  
 La sequenza di escape ODBC CALL e la sequenza di escape RPC rappresentano i metodi preferiti per la chiamata a una stored procedure rispetto all'istruzione [EXECUTE](../../../t-sql/language-elements/execute-transact-sql.md). Il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] provider di OLE DB di Native client utilizza il meccanismo RPC di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per ottimizzare l'elaborazione del comando. Questo protocollo RPC migliora le prestazioni riducendo l'elaborazione dei parametri e l'analisi delle istruzioni eseguite sul server.  
  
 Di seguito è riportato un esempio dell'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)]EXECUTE **di**:  
  
```  
EXECUTE SalesByCategory 'Produce', '1995'  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure](../../../relational-databases/native-client/ole-db/stored-procedures.md)  
  
  
