---
description: DataTypeEnum
title: DataTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DataTypeEnum
helpviewer_keywords:
- DataTypeEnum enumeration [ADO]
ms.assetid: 2c57eca6-9336-4b06-ba10-9fef5926b1d0
author: rothja
ms.author: jroth
ms.openlocfilehash: 700f7aa4053a1e99bcae5cdabe01a5fc6c440258
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171318"
---
# <a name="datatypeenum"></a>DataTypeEnum
Specifica il tipo di dati di un [campo](../../../ado/reference/ado-api/field-object.md), un [parametro](../../../ado/reference/ado-api/parameter-object.md)o una [proprietà](../../../ado/reference/ado-api/property-object-ado.md). L'indicatore del tipo di OLE DB corrispondente viene visualizzato tra parentesi nella colonna Descrizione della tabella seguente.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**AdArray**|0x2000|Valore del flag, sempre combinato con un'altra costante del tipo di dati che indica una matrice dell'altro tipo di dati. Non si applica a ADOX.|  
|**adBigInt**|20|Indica un intero con segno a otto byte (DBTYPE_I8).|  
|**adBinary**|128|Indica un valore binario (DBTYPE_BYTES).|  
|**adBoolean**|11|Indica un valore **booleano** (DBTYPE_BOOL).|  
|**adBSTR**|8|Indica una stringa di caratteri con terminazione null (Unicode) (DBTYPE_BSTR).|  
|**adChapter**|136|Indica un valore di capitolo a quattro byte che identifica le righe in un set di righe figlio (DBTYPE_HCHAPTER).|  
|**adChar**|129|Indica un valore stringa (DBTYPE_STR).|  
|**adCurrency**|6|Indica un valore di valuta (DBTYPE_CY). Currency è un numero a virgola fissa con quattro cifre a destra della virgola decimale. Viene archiviato in un intero con segno a otto byte ridimensionato di 10.000.|  
|**adDate**|7|Indica un valore di data (DBTYPE_DATE). Una data viene archiviata come Double, la parte intera di cui è il numero di giorni a partire dal 30 dicembre 1899 e la parte frazionaria della quale è la frazione di un giorno.|  
|**adDBDate**|133|Indica un valore di data (AAAAMMGG) (DBTYPE_DBDATE).|  
|**adDBTime**|134|Indica un valore di ora (hhmmss) (DBTYPE_DBTIME).|  
|**adDBTimeStamp**|135|Indica un indicatore di data e ora (ad aaaammgghhmmss più una frazione in miliardesimi) (DBTYPE_DBTIMESTAMP).|  
|**adDecimal**|14|Indica un valore numerico esatto con una precisione e una scala fisse (DBTYPE_DECIMAL).|  
|**adDouble**|5|Indica un valore a virgola mobile e precisione doppia (DBTYPE_R8).|  
|**adEmpty**|0|Non specifica alcun valore (DBTYPE_EMPTY).|  
|**adError**|10|Indica un codice di errore a 32 bit (DBTYPE_ERROR).|  
|**adFileTime**|64|Indica un valore a 64 bit che rappresenta il numero di intervalli di 100-nanosecondi a partire dal 1 ° gennaio 1601 (DBTYPE_FILETIME).|  
|**adGUID**|72|Indica un identificatore univoco globale (GUID) (DBTYPE_GUID).|  
|**adIDispatch**|9|Indica un puntatore a un'interfaccia **IDispatch** in un oggetto COM (DBTYPE_IDISPATCH).<br /><br /> **Nota** Questo tipo di dati non è attualmente supportato da ADO. L'utilizzo può causare risultati imprevedibili.|  
|**adInteger**|3|Indica un intero con segno a quattro byte (DBTYPE_I4).|  
|**adIUnknown**|13|Indica un puntatore a un'interfaccia **IUnknown** su un oggetto COM (DBTYPE_IUNKNOWN).<br /><br /> **Nota** Questo tipo di dati non è attualmente supportato da ADO. L'utilizzo può causare risultati imprevedibili.|  
|**adLongVarBinary**|205|Indica un valore binario lungo.|  
|**adLongVarChar**|201|Indica un valore stringa lungo.|  
|**adLongVarWChar**|203|Indica un valore stringa Unicode Long con terminazione null.|  
|**adNumeric**|131|Indica un valore numerico esatto con una precisione e una scala fisse (DBTYPE_NUMERIC).|  
|**adPropVariant**|138|Indica un PROPVARIANT di automazione (DBTYPE_PROP_VARIANT).|  
|**adSingle**|4|Indica un valore a virgola mobile a precisione singola (DBTYPE_R4).|  
|**adSmallInt**|2|Indica un intero con segno a 2 byte (DBTYPE_I2).|  
|**adTinyInt**|16|Indica un intero con segno a 1 byte (DBTYPE_I1).|  
|**adUnsignedBigInt**|21|Indica una Unsigned Integer a otto byte (DBTYPE_UI8).|  
|**adUnsignedInt**|19|Indica una Unsigned Integer a quattro byte (DBTYPE_UI4).|  
|**adUnsignedSmallInt**|18|Indica una Unsigned Integer a due byte (DBTYPE_UI2).|  
|**adUnsignedTinyInt**|17|Indica una Unsigned Integer a un byte (DBTYPE_UI1).|  
|**adUserDefined**|132|Indica una variabile definita dall'utente (DBTYPE_UDT).|  
|**adVarBinary**|204|Indica un valore binario.|  
|**adVarChar**|200|Indica un valore stringa.|  
|**adVariant**|12|Indica una **variante** di automazione (DBTYPE_VARIANT).<br /><br /> **Nota** Questo tipo di dati non è attualmente supportato da ADO. L'utilizzo può causare risultati imprevedibili.|  
|**adVarNumeric**|139|Indica un valore numerico.|  
|**adVarWChar**|202|Indica una stringa di caratteri Unicode con terminazione null.|  
|**adWChar**|130|Indica una stringa di caratteri Unicode con terminazione null (DBTYPE_WSTR).|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. DataType. ARRAY|  
|AdoEnums. DataType. BIGINT|  
|AdoEnums. DataType. BINARY|  
|AdoEnums. DataType. BOOLEAN|  
|AdoEnums. DataType. BSTR|  
|AdoEnums. DataType. CHAPTER|  
|AdoEnums. DataType. CHAR|  
|AdoEnums. DataType. CURRENCY|  
|AdoEnums. DataType. DATE|  
|AdoEnums. DataType. DBDATE|  
|AdoEnums. DataType. DBTIME|  
|AdoEnums. DataType. DBTIMESTAMP|  
|AdoEnums. DataType. DECIMAL|  
|AdoEnums. DataType. DOUBLE|  
|AdoEnums. DataType. EMPTY|  
|AdoEnums. DataType. ERROR|  
|AdoEnums. DataType. FILETIME|  
|AdoEnums. DataType. GUID|  
|AdoEnums. DataType. IDISPATCH|  
|AdoEnums. DataType. INTEGER|  
|AdoEnums. DataType. IUNKNOWN|  
|AdoEnums. DataType. LONGVARBINARY|  
|AdoEnums. DataType. LONGVARCHAR|  
|AdoEnums. DataType. LONGVARWCHAR|  
|AdoEnums. DataType. NUMERIC|  
|AdoEnums. DataType. PROPVARIANT|  
|AdoEnums. DataType. SINGLE|  
|AdoEnums. DataType. SMALLINT|  
|AdoEnums. DataType. TINYINT|  
|AdoEnums. DataType. UNSIGNEDBIGINT|  
|AdoEnums. DataType. UNSIGNEDINT|  
|AdoEnums. DataType. UNSIGNEDSMALLINT|  
|AdoEnums. DataType. E UNSIGNEDTINYINT|  
|AdoEnums. DataType. USERDEFINED|  
|AdoEnums. DataType. VARBINARY|  
|AdoEnums. DataType. VARCHAR|  
|AdoEnums. DataType. VARIANT|  
|AdoEnums. DataType. VARNUMERIC|  
|AdoEnums. DataType. VARWCHAR|  
|AdoEnums. DataType. WCHAR|  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Metodo Append (ADO)](../../../ado/reference/ado-api/append-method-ado.md)  
        [Metodo CreateParameter (ADO)](../../../ado/reference/ado-api/createparameter-method-ado.md)  
    :::column-end:::
    :::column:::
        [Metodo CreateRecordset (Servizi Desktop remoto)](../../../ado/reference/rds-api/createrecordset-method-rds.md)  
        [Proprietà Type (ADO)](../../../ado/reference/ado-api/type-property-ado.md)  
    :::column-end:::
:::row-end:::
