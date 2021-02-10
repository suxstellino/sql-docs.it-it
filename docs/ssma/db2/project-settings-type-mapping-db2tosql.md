---
description: Impostazioni progetto (mapping dei tipi) (DB2ToSQL)
title: Impostazioni progetto (mapping dei tipi) (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: cf426c69-6a8e-4d19-951d-6661d5ae2562
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b562e886bcb09eae4c02607f17eb7419b313a759
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072035"
---
# <a name="project-settings-type-mapping-db2tosql"></a>Impostazioni progetto (mapping dei tipi) (DB2ToSQL)
La pagina mapping dei tipi della finestra di dialogo **Impostazioni progetto** contiene impostazioni che personalizzano il modo in cui SSMA converte i tipi di dati DB2 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipi di dati.  
  
La pagina mapping dei tipi è disponibile nelle finestre di dialogo **Impostazioni progetto** e **Impostazioni progetto predefinite** .  
  
-   Per specificare le impostazioni per tutti i progetti SSMA futuri, nel menu **strumenti** fare clic su **Impostazioni progetto predefinite**, selezionare tipo di progetto di migrazione per cui è necessario visualizzare o modificare le impostazioni dall'elenco a discesa **versione destinazione migrazione** e quindi fare clic su **mapping dei tipi** nella parte inferiore del riquadro sinistro.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** e quindi fare clic su **mapping dei tipi** nella parte inferiore del riquadro sinistro.  
  
Per specificare le impostazioni per l'oggetto o la classe di oggetti correnti, usare la scheda **mapping dei tipi** nella finestra SSMA primaria.  
  
## <a name="options"></a>Opzioni  
Nella tabella seguente vengono illustrate le opzioni della scheda **mapping dei tipi** :  
  
**Tipo origine**  
Tipo di dati DB2 mappato.  
  
**Tipo destinazione**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Tipo di dati di destinazione per il tipo di dati DB2 specificato.  
  
Vedere le tabelle nella sezione successiva per i mapping dei tipi SSMA predefiniti per DB2.  
  
**Aggiungere**  
Fare clic su questo pulsante per aggiungere un tipo di dati all'elenco di mapping.  
  
**Modifica**  
Fare clic per modificare il tipo di dati selezionato nell'elenco mapping.  
  
**Rimuovi**  
Fare clic per rimuovere il mapping dei tipi di dati selezionati dall'elenco mapping.  
  
**Ripristina predefiniti**  
Fare clic per reimpostare l'elenco dei mapping dei tipi sui valori predefiniti di SSMA.  
  
## <a name="default-type-mappings"></a>Mapping dei tipi predefiniti  
In SSMA per DB2 è possibile impostare mapping di tipi personalizzati per argomenti, colonne, variabili locali e valori restituiti. Il mapping predefinito per gli argomenti e i tipi restituiti è quasi identico.  
  
### <a name="default-argument-type-and-return-value-type-mapping"></a>Tipo di argomento predefinito e mapping del tipo di valore restituito  
La tabella seguente contiene il mapping del tipo di dati predefinito per gli argomenti e i valori restituiti.  
  
|Tipo di dati DB2|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Tipo di dati predefinito|  
|-----------------|-------------------------------------------------------------------------|  
|BFILE|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|binary_integer|INT|  
|blob|varbinary(max)|  
|boolean|bit|  
|char|ntext|  
|char varying|ntext|  
|character|ntext|  
|character varying|ntext|  
|CLOB|ntext|  
|Data|datetime2 [0]|  
|dec|Dec [38] [0]|  
|decimal|float [53]|  
|double precision|float [53]|  
|float|float [53]|  
|INT|INT|  
|numero intero|INT|  
|long|ntext|  
|long raw|varbinary(max)|  
|Long RAW [ \* .. 8000]<sup>\*</sup>|varbinary [ \* ]|  
|Long RAW [8001.. \* ]<sup>\*</sup>|varbinary(max)|  
|carattere nazionale|nvarchar(max)|  
|carattere nazionale varying|nvarchar(max)|  
|carattere nazionale|nvarchar(max)|  
|carattere nazionale variabile<sup>\*\*</sup>|nvarchar(max)|  
|carattere nazionale variabile<sup>\*</sup>|nvarchar(max)|  
|NCHAR|nvarchar(max)|  
|NCLOB|nvarchar(max)|  
|d'acquisto|float [53]|  
|NUMERIC|float [53]|  
|NVARCHAR2|nvarchar(max)|  
|pls_integer|INT|  
|raw|varbinary(max)|  
|real|float [53]|  
|ROWID|UNIQUEIDENTIFIER|  
|signtype|SMALLINT|  
|SMALLINT|SMALLINT|  
|string|ntext|  
| timestamp|datetime2|  
|timestamp con fuso orario locale|datetimeoffset|  
|timestamp con fuso orario|datetimeoffset|  
|UROWID|UNIQUEIDENTIFIER|  
|varchar|ntext|  
|VARCHAR2|ntext|  
|XMLType|Xml|  
  
<sup>\*</sup> Si applica solo al mapping del tipo di valore restituito.  
  
<sup>\*\*</sup> Si applica solo al mapping del tipo di argomento.  
  
### <a name="default-column-type-mapping"></a>Mapping del tipo di colonna predefinito  
La tabella seguente contiene il mapping dei tipi predefinito per le colonne.  
  
|Tipo di dati DB2|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Tipo di dati predefinito|  
|-----------------|-------------------------------------------------------------------------|  
|BFILE|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|blob|varbinary(max)|  
|char|char|  
|carattere variabile [ \* .. \* ]|varchar [ \* ]|  
|Char [ \* .. \* ]|Char [ \* ]|  
|character|char|  
|carattere variabile [ \* .. \* ]|varchar [ \* ]|  
|carattere [ \* .. \* ]|Char [ \* ]|  
|CLOB|ntext|  
|Data|datetime2 [0]|  
|dec|Dec [38] [0]|  
|Dec [ \* .. \* ]|Dec [ \* ] [0]|  
|Dec [ \* .. \* ] [\*..\*]|Dec [ \* ] [ \* ]|  
|decimal|decimale [38] [0]|  
|decimale [ \* .. \* ]|Decimal [ \* ] [0]|  
|decimale [ \* .. \* ] [\*..\*]|Decimal [ \* ] [ \* ]|  
|double precision|float [53]|  
|float|float [53]|  
|float [ \* .. 53]|float [ \* ]|  
|float [54.. \* ]|float [53]|  
|INT|INT|  
|numero intero|INT|  
|long|ntext|  
|long raw|varbinary(max)|  
|Long RAW [ \* .. 8000]|varbinary [ \* ]|  
|Long RAW [8001.. \* ]|varbinary(max)|  
|long varchar|ntext|  
|Long [ \* .. 8000]|varchar [ \* ]|  
|Long [8001.. \* ]|ntext|  
|carattere nazionale|NCHAR|  
|carattere nazionale varying [ \* .. \* ]|nvarchar [ \* ]|  
|National Char [ \* .. \* ]|nchar [ \* ]|  
|carattere nazionale|NCHAR|  
|carattere nazionale variabile [ \* .. \* ]|nvarchar [ \* ]|  
|carattere nazionale [ \* .. \* ]|nchar [ \* ]|  
|NCHAR|NCHAR|  
|nchar [ \* ]|nchar [ \* ]|  
|NCLOB|nvarchar(max)|  
|d'acquisto|float [53]|  
|numero [ \* .. \* ]|numeric [ \* ]|  
|numero [ \* .. \* ] [\*..\*]|numeric [ \* ] [ \* ]|  
|NUMERIC|NUMERIC|  
|numerico [ \* .. \* ]|numeric [ \* ]|  
|numerico [ \* .. \* ] [\*..\*]|numeric [ \* ] [ \* ]|  
|NVARCHAR2 [ \* .. \* ]|nvarchar [ \* ]|  
|non elaborato [ \* .. \* ]|varbinary [ \* ]|  
|real|float [53]|  
|ROWID|UNIQUEIDENTIFIER|  
|SMALLINT|SMALLINT|  
| timestamp|datetime2|  
|timestamp con fuso orario locale|datetimeoffset|  
|timestamp con fuso orario locale [ \* .. \* ]|DateTimeOffset [ \* ]|  
|timestamp con fuso orario|datetimeoffset|  
|timestamp con fuso orario [ \* .. \* ]|DateTimeOffset [ \* ]|  
|timestamp [ \* .. \* ]|datetime2 [ \* ]|  
|UROWID|UNIQUEIDENTIFIER|  
|UROWID [ \* .. \* ]|UNIQUEIDENTIFIER|  
|varchar [ \* .. \* ]|varchar [ \* ]|  
|VARCHAR2 [ \* .. \* ]|varchar [ \* ]|  
|XMLType|Xml|  
  
### <a name="default-local-variable-type-mapping"></a>Mapping del tipo di variabile locale predefinito  
La tabella seguente contiene il mapping dei tipi predefinito per le variabili locali.  
  
|Tipo di dati DB2|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Tipo di dati predefinito|  
|-----------------|-------------------------------------------------------------------------|  
|Bfile|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|binary_interger|INT|  
|BLOB|varbinary(max)|  
|Boolean|bit|  
|Char|char|  
|carattere variabile [ \* .. 8000]|varchar [ \* ]|  
|carattere variabile [8001... \* ]|ntext|  
|Char [ \* .. 8000]|Char [ \* ]|  
|Char [8001.. \* ]|ntext|  
|Carattere|char|  
|carattere variabile [ \* .. 8000]|varchar [ \* ]|  
|variazione di caratteri [8001.. \* ]|ntext|  
|carattere [ \* .. 8000]|Char [ \* ]|  
|carattere [8001.. \* ]|ntext|  
|CLOB|ntext|  
|Data|datetime2 [0]|  
|dec|Dec [38] [0]|  
|Dec [ \* .. \* ]|Dec [ \* ] [0]|  
|Dec [ \* .. \* ] [\*..\*]|Dec [ \* ] [ \* ]|  
|decimal|decimale [38] [0]|  
|decimale [ \* .. \* ]|Decimal [ \* ] [0]|  
|decimale [ \* .. \* ] [\*..\*]|Decimal [ \* ] [ \* ]|  
|double precision|float [53]|  
|Float|float [53]|  
|float [ \* .. 53]|float [ \* ]|  
|float [54.. \* ]|float [53]|  
|Int|INT|  
|Integer|INT|  
|Integer [ \* .. \* ]|numeric [ \* ] [0]|  
|long|ntext|  
|long raw|varbinary(max)|  
|Long RAW [ \* .. 8000]|varbinary [ \* ]|  
|Long RAW [8001.. \* ]|varbinary(max)|  
|carattere nazionale|NCHAR|  
|carattere nazionale variabile [ \* .. 4000]|nvarchar [ \* ]|  
|carattere nazionale variabile [4001.. \* ]|nvarchar(max)|  
|National Char [ \* .. 4000]|nchar [ \* ]|  
|char nazionale [4001.. \* ]|nvarchar(max)|  
|carattere nazionale|NCHAR|  
|carattere nazionale [ \* .. 4000]|nvarchar [ \* ]|  
|carattere nazionale [4001.. \* ]|nvarchar(max)|  
|carattere nazionale variabile [ \* .. 4000]|nvarchar [ \* ]|  
|carattere nazionale variabile [4001.. \* ]|nvarchar(max)|  
|Nchar|NCHAR|  
|nchar [ \* .. 4000]|nchar [ \* ]|  
|nchar [4001.. \* ]|nvarchar(max)|  
|nchar varying [ \* .. 4000]|nvarchar [ \* ]|  
|variabile nchar [4001.. \* ]|nvarchar(max)|  
|NCLOB|nvarchar(max)|  
|Number|float [53]|  
|numero [ \* .. \* ]|numeric [ \* ]|  
|numero [ \* .. \* ] [\*..\*]|numeric [ \* ] [ \* ]|  
|Numeric|numerico [38] [0]|  
|numerico [ \* .. \* ]|numeric [ \* ]|  
|numerico [ \* .. \* ] [\*..\*]|numeric [ \* ] [ \* ]|  
|NVARCHAR2 [ \* .. 4000]|nvarchar [ \* ]|  
|NVARCHAR2 [4001.. \* ]|nvarchar(max)|  
|pls_integer|INT|  
|non elaborato [ \* .. 8000]|varbinary [ \* ]|  
|non elaborato [8001.. \* ]|varbinary(max)|  
|Real|float [53]|  
|Rowid|UNIQUEIDENTIFIER|  
|Signtype|SMALLINT|  
|Smallint|SMALLINT|  
|stringa [ \* .. 8000]|varchar [ \* ]|  
|stringa [8001.. \* ]|ntext|  
| timestamp|datetime2|  
|timestamp con fuso orario locale|datetimeoffset|  
|timestamp con fuso orario|datetimeoffset|  
|timestamp con fuso orario locale [ \* .. \* ]|DateTimeOffset [ \* ]|  
|timestamp con fuso orario [ \* .. \* ]|DateTimeOffset [ \* ]|  
|timestamp [ \* .. \* ]|datetime2 [ \* ]|  
|UROWID|UNIQUEIDENTIFIER|  
|UROWID [ \* .. \* ]|UNIQUEIDENTIFIER|  
|varchar [ \* .. 8000]|varchar [ \* ]|  
|varchar [8001.. \* ]|ntext|  
|VARCHAR2 [ \* .. 8000]|varchar [ \* ]|  
|VARCHAR2 [8001.. \* ]|varcha (max)|  
|XMLType|Xml|  
  
## <a name="see-also"></a>Vedere anche  
[Informazioni di riferimento sull'interfaccia utente &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
