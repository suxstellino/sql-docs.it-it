---
description: CREATE MESSAGE TYPE (Transact-SQL)
title: CREATE MESSAGE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_MESSAGE_TSQL
- MESSAGE_TSQL
- MESSAGE
- CREATE MESSAGE
- CREATE_MESSAGE_TYPE_TSQL
- MESSAGE TYPE
- MESSAGE_TYPE_TSQL
- CREATE MESSAGE TYPE
dev_langs:
- TSQL
helpviewer_keywords:
- XML [Service Broker]
- validation [Service Broker]
- message types [Service Broker], creating
- empty messages [SQL Server]
- binary [SQL Server], message types
- CREATE MESSAGE TYPE statement
ms.assetid: 98fe0fff-1a2e-4ca2-b37f-83a06fdf098e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b32fe2417a7a9d9d290b72cf3de2dfc325fc0eaf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093450"
---
# <a name="create-message-type-transact-sql"></a>CREATE MESSAGE TYPE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un nuovo tipo di messaggio. Il tipo di messaggio definisce il nome di un messaggio e la convalida che viene eseguita da [!INCLUDE[ssSB](../../includes/sssb-md.md)] nei messaggi con tale nome. In entrambi i lati di una conversazione devono essere definiti gli stessi tipi di messaggio.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE MESSAGE TYPE message_type_name  
    [ AUTHORIZATION owner_name ]  
    [ VALIDATION = {  NONE  
                    | EMPTY   
                    | WELL_FORMED_XML  
                    | VALID_XML WITH SCHEMA COLLECTION schema_collection_name  
                   } ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *message_type_name*  
 Nome del tipo di messaggio da creare. Viene creato un nuovo tipo di messaggio nel database corrente, di proprietà dell'entità specificata nella clausola AUTHORIZATION. Non è possibile specificare i nomi del server, del database e dello schema. *message_type_name* può contenere fino a 128 caratteri.  
  
 AUTHORIZATION *owner_name*  
 Imposta come proprietario del tipo di messaggio l'utente o il ruolo del database specificato. Se l'utente corrente è **dbo** o **sa**, *owner_name* può essere il nome di qualsiasi utente o ruolo valido. In caso contrario, *owner_name* deve essere il nome dell'utente corrente, il nome di un utente per il quale l'utente corrente dispone dell'autorizzazione IMPERSONATE oppure il nome di un ruolo a cui appartiene l'utente corrente. Se questa clausola viene omessa, il tipo di messaggio appartiene all'utente corrente.  
  
 VALIDATION  
 Specifica la modalità con cui [!INCLUDE[ssSB](../../includes/sssb-md.md)] convalida il corpo dei messaggi di questo tipo. Se questa clausola non viene specificata, l'impostazione predefinita per la convalida è NONE.  
  
 NONE  
 Specifica che non viene eseguita alcuna convalida. Il corpo del messaggio può contenere dati oppure può essere NULL.  
  
 EMPTY  
 Specifica che il corpo del messaggio deve essere NULL.  
  
 WELL_FORMED_XML  
 Specifica che il corpo del messaggio deve contenere codice XML in formato corretto.  
  
 VALID_XML WITH SCHEMA COLLECTION *schema_collection_name*  
 Specifica che il corpo del messaggio deve contenere dati XML conformi a uno schema incluso nella raccolta di schemi specificata. *schema_collection_name* deve corrispondere al nome di una raccolta di XML Schema esistente.  
  
## <a name="remarks"></a>Commenti  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] convalida i messaggi in arrivo. Se il corpo di un messaggio non è conforme al tipo di convalida specificato, [!INCLUDE[ssSB](../../includes/sssb-md.md)] elimina il messaggio non valido e restituisce un errore al servizio che ha inviato il messaggio.  
  
 In entrambi i lati di una conversazione deve essere definito lo stesso nome per un tipo di messaggio. Per semplificare la risoluzione dei problemi, per entrambi i lati di una conversazione viene in genere specificata la stessa convalida per il tipo di messaggio, sebbene questo non sia un requisito di [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 Un tipo di messaggio non può essere un oggetto temporaneo. I nomi dei tipi di messaggio che iniziano con **#** sono consentiti, ma sono oggetti permanenti.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per la creazione di un tipo di messaggio viene assegnata per impostazione predefinita ai membri del ruolo predefinito del database **db_ddladmin** o **db_owner** e del ruolo predefinito del server **sysadmin**.  
  
 Per impostazione predefinita l'autorizzazione REFERENCES per un tipo di messaggio viene assegnata al proprietario del tipo di messaggio, ai membri del ruolo predefinito del database **db_owner** e ai membri del ruolo predefinito del server **sysadmin**.  
  
 Se l'istruzione CREATE MESSAGE TYPE specifica una raccolta di schemi, l'utente che esegue l'istruzione deve disporre dell'autorizzazione REFERENCES per la raccolta di schemi specificata.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-message-type-containing-well-formed-xml"></a>R. Creazione di un tipo di messaggio contenente codice XML ben formato  
 Nell'esempio seguente viene creato un nuovo tipo di messaggio contenente XML in formato corretto.  
  
```sql  
CREATE MESSAGE TYPE  
  [//Adventure-Works.com/Expenses/SubmitExpense]  
  VALIDATION = WELL_FORMED_XML ;     
```  
  
### <a name="b-creating-a-message-type-containing-typed-xml"></a>B. Creazione di un tipo di messaggio contenente codice XML tipizzato  
 Nell'esempio seguente viene creato un tipo di messaggio per un rapporto spese in codice XML. In questo esempio viene creata una raccolta di XML Schema che contiene lo schema per un semplice rapporto spese. Viene quindi creato un nuovo tipo di messaggio che convalida i messaggi rispetto allo schema.  
  
```sql  
CREATE XML SCHEMA COLLECTION ExpenseReportSchema AS  
N'<?xml version="1.0" encoding="UTF-16" ?>  
  <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
     targetNamespace="https://Adventure-Works.com/schemas/expenseReport"  
     xmlns:expense="https://Adventure-Works.com/schemas/expenseReport"  
     elementFormDefault="qualified"  
   >   
    <xsd:complexType name="expenseReportType">  
       <xsd:sequence>  
         <xsd:element name="EmployeeName" type="xsd:string"/>  
         <xsd:element name="EmployeeID" type="xsd:string"/>  
         <xsd:element name="ItemDetail"  
           type="expense:ItemDetailType" maxOccurs="unbounded"/>  
      </xsd:sequence>  
    </xsd:complexType>  
  
    <xsd:complexType name="ItemDetailType">  
      <xsd:sequence>  
        <xsd:element name="Date" type="xsd:date"/>  
        <xsd:element name="CostCenter" type="xsd:string"/>  
        <xsd:element name="Total" type="xsd:decimal"/>  
        <xsd:element name="Currency" type="xsd:string"/>  
        <xsd:element name="Description" type="xsd:string"/>  
      </xsd:sequence>  
    </xsd:complexType>  
  
    <xsd:element name="ExpenseReport" type="expense:expenseReportType"/>  
  
  </xsd:schema>' ;  
  
  CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = VALID_XML WITH SCHEMA COLLECTION ExpenseReportSchema ;  
```  
  
### <a name="c-creating-a-message-type-for-an-empty-message"></a>C. Creazione di un tipo di messaggio per un messaggio vuoto  
 Nell'esempio seguente viene creato un nuovo tipo di messaggio con codifica vuota.  
  
```sql  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = EMPTY ;  
```  
  
### <a name="d-creating-a-message-type-containing-binary-data"></a>D. Creazione di un tipo di messaggio contenente dati binari  
 Nell'esempio seguente viene creato un nuovo tipo di messaggio contenente dati binari. Poiché il messaggio conterrà dati non XML, per il tipo di messaggio viene specificato il tipo di convalida `NONE`. In questo caso l'applicazione che riceve un messaggio di questo tipo deve verificare che il messaggio contenga dati e che questi siano del tipo previsto.  
  
```sql  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/ReceiptImage]  
    VALIDATION = NONE ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-message-type-transact-sql.md)   
 [DROP MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-message-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
