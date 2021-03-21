---
title: IBCPSession::BCPColFmt (OLE DB Driver) | Microsoft Docs
description: "Informazioni sul modo in cui il metodo IBCPSession:: BCPColFmt crea un'associazione tra le variabili di programma e le colonne di SQL Server in OLE DB Driver per SQL Server."
ms.custom: ''
ms.date: 05/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPColFmt (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPColFmt method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8eba3778f18ec1ec37f95abd087670d16f6da0d0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748751"
---
# <a name="ibcpsessionbcpcolfmt-ole-db"></a>IBCPSession::BCPColFmt (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Crea un'associazione tra variabili di programma e colonne di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT BCPColFmt(   
      DBORDINAL idxUserDataCol,  
      int eUserDataType,  
      int cbIndicator,  
      int cbUserData,  
      BYTE *pbUserDataTerm,  
      int cbUserDataTerm,  
      DBORDINAL idxServerCol);  
```  
  
## <a name="remarks"></a>Osservazioni  
 Il metodo **BCPColFmt** viene usato per creare un'associazione tra i campi del file di dati BCP e le colonne di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Accetta come parametri la lunghezza, il tipo, il carattere di terminazione e la lunghezza del prefisso di una colonna e imposta ciascuna di queste proprietà per i singoli campi.  
  
 Se l'utente sceglie la modalità interattiva, questo metodo viene chiamato due volte, una volta per impostare il formato della colonna in base ai valori predefiniti (che dipendono dal tipo di colonna del server) e una volta per impostare il formato in base al tipo di client selezionato durante la modalità interattiva, per ogni colonna.  
  
 Nelle modalità non interattive, viene chiamato una sola volta per colonna per impostare il tipo di ogni colonna sul carattere o sul tipo nativo e impostare i caratteri di terminazione di colonna e riga.  
  
 Il metodo **BCPColFmt** consente di specificare il formato del file utente per le copie bulk. Per la copia bulk, un formato contiene le parti seguenti:  
  
-   Un mapping dai campi del file utente alle colonne del database.  
  
-   Il tipo di dati di ogni campo del file utente.  
  
-   La lunghezza dell'indicatore facoltativo per ogni campo.  
  
-   La lunghezza massima dei dati per ogni campo del file utente.  
  
-   La sequenza di byte di terminazione facoltativa per ogni campo <a href="#terminator_note"><sup>**1**</sup></a>.  
  
-   La lunghezza della sequenza di byte di terminazione facoltativa <a href="#terminator_note"><sup>**1**</sup></a>.  
  

> [!IMPORTANT]
> <b id="terminator_note">[1]:</b> L'uso della sequenza di caratteri di terminazione in scenari in cui la tabella codici del file di dati è impostata su UTF-8 non è supportata. In questi scenari è necessario impostare **pbUserDataTerm** su `nullptr` e **cbUserDataTerm** su `0`.

 Ogni chiamata a **BCPColFmt** specifica il formato per un campo del file utente. Per modificare ad esempio le impostazioni predefinite per tre campi in un file di dati dell'utente con cinque campi, chiamare prima `BCPColumns(5)` e quindi chiamare **BCPColFmt** cinque volte, con tre di queste chiamate che impostano il formato personalizzato. Per le due chiamate rimanenti, impostare *eUserDataType* su BCP_TYPE_DEFAULT e *cbIndicator*, *cbUserData* e *cbUserDataTerm* su 0, BCP_VARIABLE_LENGTH e 0 rispettivamente. Questa procedura consente di copiare tutte e cinque le colonne, tre con il formato personalizzato e due con il formato predefinito.  
  
> [!NOTE]  
>  Il metodo [IBCPSession::BCPColumns](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md) deve essere chiamato prima di qualsiasi chiamata a **BCPColFmt**. È necessario chiamare **BCPColFmt** una volta per ogni colonna del file utente. Se **BCPColFmt** viene chiamato più di una volta per qualsiasi colonna del file utente, viene generato un errore.  
  
 Non è necessario copiare tutti i dati di un file utente in una tabella [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per ignorare una colonna, specificare il formato dei dati per la colonna impostando il parametro idxServerCol su 0. Per ignorare un campo, è necessario comunque disporre di tutte le informazioni affinché il metodo possa funzionare correttamente.  
  
 **Nota** La funzione [IBCPSession::BCPWriteFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md) può essere usata per rendere persistente la specifica del formato fornita tramite **BCPColFmt**.  
  
## <a name="arguments"></a>Argomenti  
 *idxUserDataCol*[in]  
 Indice del campo nel file di dati dell'utente.  
  
 *eUserDataType*[in]  
 Il tipo di dati del campo nel file di dati dell'utente. I tipi di dati disponibili sono elencati nel file di intestazione di OLE DB Driver per SQL Server (msoledbsql.h) con il formato BCP_TYPE_XXX, ad esempio BCP_TYPE_SQLINT4. Se viene specificato il valore BCP_TYPE_DEFAULT, il provider tenta di utilizzare lo stesso tipo della colonna della tabella o della vista. Per le operazioni di copia bulk da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in un file quando l'argomento **eUserDataType** è BCP_TYPE_SQLDECIMAL o BCP_TYPE_SQLNUMERIC:  
  
-   Se la colonna di origine non è decimal o numeric, vengono utilizzate la precisione e la scala predefinite.  
  
-   Se la colonna di origine è decimal o numeric, vengono utilizzate la precisione e la scala della colonna di origine.  
  
 *cbIndicator*[in]  
 Lunghezza del prefisso per il campo. Il prefisso predefinito è BCP_PREFIX_DEFAULT. Le lunghezze valide per il prefisso sono 0, 1, 2, 4 e 8. Una dimensione del prefisso pari a 8 in genere viene utilizzata per indicare che il campo è Chunked e consente di eseguire in modo efficace la copia bulk di colonne del tipo valore di grandi dimensioni.  
  
 *cbUserData*[in]  
 Lunghezza massima, espressa in byte, dei dati del campo nel file utente, senza includere la lunghezza di un carattere di terminazione o di un indicatore di lunghezza.  
  
 L'impostazione di **cbUserData** su BCP_LENGTH_NULL indica che tutti i valori nei campi del file di dati sono o dovrebbero essere impostati su NULL. L'impostazione di **cbUserData** su BCP_LENGTH_VARIABLE indica che la lunghezza dei dati per ogni campo deve essere determinata dal sistema. Per alcuni campi, questo potrebbe indicare che viene generato un indicatore di lunghezza o Null da anteporre ai dati in una copia da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o che l'indicatore è previsto nei dati copiati in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Per i dati di tipo carattere e binario di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], **cbUserData** può essere BCP_LENGTH_VARIABLE, BCP_LENGTH_NULL, 0 oppure un valore positivo. Se **cbUserData** è BCP_LENGTH_VARIABLE, il sistema usa l'indicatore di lunghezza, se disponibile, o una sequenza di caratteri di terminazione per determinare la lunghezza dei dati. Se vengono specificati sia un indicatore di lunghezza che una sequenza di caratteri di terminazione, la copia bulk utilizza la modalità che comporta la copia del minor numero di dati. Se **cbUserData** è BCP_LENGTH_VARIABLE, i dati sono di tipo carattere o binario di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e se non viene specificato né un indicatore di lunghezza né una sequenza di caratteri di terminazione, il sistema restituisce un messaggio di errore.  
  
 Se **cbUserData** è 0 o un valore positivo, il sistema usa **cbUserData** come lunghezza massima dei dati. Se, tuttavia, oltre a un **cbUserData** positivo, viene specificato un indicatore di lunghezza o una sequenza di caratteri di terminazione, il sistema determina la lunghezza dei dati usando il metodo che comporta la copia della quantità minima di dati.  
  
 Il valore **cbUserData** rappresenta il numero di byte dei dati. Se i dati di tipo carattere sono rappresentati da caratteri wide Unicode, un valore positivo per il parametro **cbUserData** rappresenta il numero di caratteri moltiplicato per la dimensione, espressa in byte, di ogni carattere.  
  
 *pbUserDataTerm*[size_is][in]  
 Sequenza di caratteri di terminazione da utilizzare per il campo. Questo parametro risulta particolarmente utile per i dati di tipo carattere, in quanto tutti gli altri tipi hanno una lunghezza fissa o, nel caso dei dati binari, richiedono un indicatore di lunghezza per registrare in modo accurato il numero di byte presenti.  
  
 Per evitare di terminare i dati estratti o per indicare che i dati di un file utente non devono essere terminati, impostare questo parametro su NULL.  
  
 Se si utilizzano più modalità per definire la lunghezza delle colonne di un file utente, ad esempio un carattere di terminazione e un indicatore di lunghezza o un carattere di terminazione e una lunghezza di colonna massima, la copia bulk sceglierà quella che comporta la copia del minor numero di dati.  
  
 L'API della copia bulk esegue la conversione dei caratteri da Unicode a MBCS in base alle necessità. Verificare attentamente che la stringa di byte del carattere di terminazione e la lunghezza della stringa di byte siano impostate correttamente. Per informazioni sulle limitazioni relative alla codifica UTF-8, vedere la sezione [Commenti](#remarks).

 *cbUserDataTerm*[in]  
 Lunghezza, espressa in byte, della sequenza di caratteri di terminazione da utilizzare per la colonna. Se non sono presenti caratteri di terminazione nei dati o non si desidera includerli, impostare questo valore su 0. Per informazioni sulle limitazioni relative alla codifica UTF-8, vedere la sezione [Commenti](#remarks).

 *idxServerCol*[in]  
 Posizione ordinale della colonna nella tabella del database. Il numero della prima colonna è 1. La posizione ordinale di una colonna viene indicata da **IColumnsInfo::GetColumnInfo** o da metodi simili. Se questo valore è 0, la copia bulk ignora il campo nel file di dati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo è riuscito.  
  
 E_FAIL  
 Si è verificato un errore specifico del provider. Per informazioni dettagliate, usare l'interfaccia [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 La chiamata al metodo non era prevista. Non è stato ad esempio chiamato il metodo [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) prima della chiamata a questo metodo.  
  
 E_INVALIDARG  
 L'argomento non è valido.  
  
 E_OUTOFMEMORY  
 Errore di memoria insufficiente.  
  
## <a name="see-also"></a>Vedere anche  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Esecuzione di operazioni di copia bulk](../../oledb/features/performing-bulk-copy-operations.md)  
  
