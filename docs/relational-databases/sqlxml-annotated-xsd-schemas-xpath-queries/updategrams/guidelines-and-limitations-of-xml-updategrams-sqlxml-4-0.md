---
title: Linee guida e limitazioni degli Updategram (SQLXML)
description: Informazioni sulle linee guida e sulle limitazioni relative all'uso degli updategram XML in SQLXML 4.0.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- updategrams [SQLXML], about updategrams
ms.assetid: b5231859-14e2-4276-bc17-db2817b6f235
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1ed5c30c80728967d86532697be441ef71d8bf57
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491511"
---
# <a name="guidelines-and-limitations-of-xml-updategrams-sqlxml-40"></a>Linee guida e limitazioni per gli updategram XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Quando si utilizzano updategram XML, tenere presenti le considerazioni seguenti:  
  
-   Se si usa un updategram per un'operazione di inserimento con una sola coppia di blocchi e , il **\<before>** **\<after>** blocco può essere **\<before>** omesso. Al contrario, in caso di un'operazione di eliminazione, il **\<after>** blocco può essere omesso.  
  
-   Se si usa un updategram con più blocchi e nel tag , è necessario specificare sia blocchi che blocchi **\<before>** **\<after>** per **\<sync>** **\<before>** **\<after>** **\<before>** formare e **\<after>** coppie.  
  
-   Gli aggiornamenti in un updategram vengono applicati alla vista XML fornita da XML Schema. Ai fini della corretta esecuzione del mapping predefinito, è pertanto necessario specificare il nome del file dello schema nell'updategram o, se il nome di file non viene fornito, i nomi di elemento e di attributo devono corrispondere ai nomi di tabella e di colonna nel database.  
  
-   SQLXML 4.0 richiede che tutti i valori di colonna in un updategram vengano mappati in modo esplicito nello schema (XDR o XSD) fornito per creare la vista XML per i relativi elementi figlio. Questo comportamento è diverso dalle versioni precedenti di SQLXML, che consente un valore per una colonna non mappata nello schema se è stato implicito come parte della chiave esterna in un'annotazione **sql:relationship.** Si noti che questa modifica non influisce sulla propagazione dei valori di chiave primaria negli elementi figlio, che si verifica tuttora per SQLXML 4.0 se non viene specificato in modo esplicito alcun valore per l'elemento figlio.  
  
-   Se si usa un updategram per modificare i dati in una colonna binaria ,ad esempio il tipo di dati image, è necessario specificare uno schema di mapping in cui devono essere specificati il tipo di dati (ad esempio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **sql:datatype="image"**) e il tipo di dati XML (ad esempio **dt:type="binhex"** o **dt:type="binbase64).** I dati per la colonna binaria devono essere specificati nell'updategram. **L'annotazione sql:url-encode** specificata nello schema di mapping viene ignorata dall'updategram.  
  
-   Quando si scrive uno schema XSD, se il valore specificato per l'annotazione **sql:relation** o **sql:field** include un carattere speciale, ad esempio uno spazio (ad esempio, nel nome della tabella "Dettagli ordine"), questo valore deve essere racchiuso tra parentesi quadre (ad esempio, "[Dettagli ordine]").  
  
-   Quando si utilizzano updategram, le relazioni a catena non sono supportate. Se, ad esempio, le tabelle A e C sono correlate tramite una relazione a catena che utilizza la tabella B, si verifica l'errore seguente quando si tenta di eseguire l'updategram:  
  
    ```  
    There is an inconsistency in the schema provided.  
    ```  
  
     Anche se lo schema e l'updategram sono entrambi altrimenti corretti e hanno un formato valido, l'errore si verifica se è presente una relazione a catena.  
  
-   Gli updategram non consentono il passaggio di **dati** di tipo immagine come parametri durante gli aggiornamenti.  
  
-   I tipi BLOB (Binary Large Object) come **text/ntext** e images non devono essere usati nel blocco in quando si usano gli updategram, perché verranno inclusi per l'uso nel controllo della **\<before>** concorrenza. Ciò può provocare problemi con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a causa delle limitazioni applicate al confronto per i tipi BLOB. Ad esempio, la parola chiave LIKE viene usata nella clausola WHERE per il confronto tra colonne con tipo **di** dati text. Tuttavia, i confronti avranno esito negativo nel caso di tipi BLOB in cui le dimensioni dei dati sono maggiori di 8 KB.  
  
-   I caratteri speciali nei **dati ntext** possono causare problemi con SQLXML 4.0 a causa delle limitazioni relative al confronto per i tipi BLOB. Ad esempio, l'uso di "[Serializable]" nel blocco di un updategram quando viene usato nel controllo della concorrenza di una colonna di tipo ntext avrà esito negativo con la descrizione dell'errore **\<before>** SQLOLEDB  seguente:  
  
    ```  
    Empty update, no updatable rows found   Transaction aborted  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Considerazioni sulla sicurezza degli updategram &#40;sqlxml 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
