---
description: Field (sintassi ADO/WFC)
title: Field (sintassi ADO-WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Field collection [ADO], ADO/WFC syntax
ms.assetid: 7e01cb24-2338-4f92-ad46-8d97248e1a4d
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d998deb2d09e77e54ba9c733e371cc60a439761
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034101"
---
# <a name="field-ado---wfc-syntax"></a>Field (sintassi ADO/WFC)
## <a name="package-commswfcdata"></a>pacchetto com. ms. wfc. Data  
  
### <a name="methods"></a>Metodi  
  
```  
public void appendChunk(byte[] bytes)  
public void appendChunk(char[] chars)  
public void appendChunk(String chars)  
public byte[] getByteChunk(int len)  
public char[] getCharChunk(int len)  
public String getStringChunk(int len)  
```  
  
### <a name="properties"></a>Proprietà  
  
```  
public int getActualSize()  
public int getAttributes()  
public void setAttributes(int pl)  
public com.ms.com.IUnknown getDataFormat()  
public void setDataFormat(com.ms.com.IUnknown format)  
```  
  
 Per ulteriori informazioni, vedere la documentazione relativa all'interfaccia com. ms. wfc. Data. IDataFormat.  
  
```  
public int getDefinedSize()  
public void setDefinedSize(int pl)  
public String getName()  
public int getNumericScale()  
public void setNumericScale(byte pbNumericScale)  
public Variant getOriginalValue()  
public int getPrecision()  
public void setPrecision(byte pbPrecision)  
public int getType()  
public void setType(int pDataType)  
public Variant getUnderlyingValue()  
public Variant getValue()  
public void setValue(Variant value)  
public AdoProperties getProperties()  
```  
  
### <a name="field-accessor-methods"></a>Metodi della funzione di accesso Field  
 La proprietà [value](../../../ado/reference/ado-api/value-property-ado.md) di un oggetto [Field](../../../ado/reference/ado-api/field-object.md) Ottiene o imposta il contenuto di tale oggetto. Il contenuto è rappresentato come VARIANT, un tipo di oggetto a cui è possibile assegnare un valore e uno dei diversi tipi di dati.  
  
 ADO/WFC implementa la proprietà **value** con il metodo **GetValue** , che restituisce un oggetto Variant. e il metodo **SetValue** , che accetta un VARIANT come argomento. Le varianti sono estremamente efficienti in determinati linguaggi, ad esempio Microsoft Visual Basic.  
  
 Oltre alla proprietà **value** , ADO/WFC fornisce metodi per la *funzione di accesso* che utilizzano i tipi di dati Java per ottenere e impostare il contenuto degli oggetti **campo** . La maggior parte di questi metodi ha nomi del form **Get**_DataType_ o **set**_DataType_.  
  
 Esistono due eccezioni degne di Nota: uno dei metodi **GetObject** restituisce un oggetto assegnato a una classe specificata. Non esiste alcuna proprietà **getNull** ; è invece disponibile una proprietà **IsNull** che restituisce un valore booleano che indica se il campo è null.  
  
```  
public native boolean getBoolean();  
public void setBoolean(boolean v)  
public native byte getByte();  
public void setByte(byte v)  
public native byte[] getBytes();  
public void setBytes(byte[] v)  
public native double getDouble();  
public void setDouble(double v)  
public native float getFloat();  
public void setFloat(float v)  
public native int getInt();  
public void setInt(int v)  
public native long getLong();  
public void setLong(long v)  
public native short getShort();  
public void setShort(short v)  
public native String getString();  
public void setString(String v)  
public native boolean isNull();  
public void setNull()  
public Object getObject()  
public Object getObject(Class c)  
public void setObject(Object value)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Field](../../../ado/reference/ado-api/field-object.md)
