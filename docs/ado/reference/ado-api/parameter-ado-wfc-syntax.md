---
description: Parameter (sintassi ADO/WFC)
title: Parametro (sintassi ADO-WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Parameter collection [ADO], ADO/WFC syntax
ms.assetid: d00d1e1e-14b1-41a2-a00f-2a3cb7396f15
author: rothja
ms.author: jroth
ms.openlocfilehash: be74f3405a3b8ade04f2598a125efde759cf4195
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170639"
---
# <a name="parameter-ado---wfc-syntax"></a>Parameter (sintassi ADO/WFC)
## <a name="package-commswfcdata"></a>pacchetto com. ms. wfc. Data  
  
### <a name="constructor"></a>Costruttore  
  
```  
public Parameter()  
public Parameter(String name)  
public Parameter(String name, int type)  
public Parameter(String name, int type, int dir)  
public Parameter(String name, int type, int dir, int size)  
public Parameter(String name, int type, int dir, int size, Object value)  
```  
  
### <a name="methods"></a>Metodi  
  
```  
public void appendChunk(byte[] bytes)  
public void appendChunk(char[] chars)  
public void appendChunk(String chars)  
```  
  
### <a name="properties"></a>Proprietà  
  
```  
public int getAttributes()  
public void setAttributes(int attr)  
public int getDirection()  
public void setDirection(int dir)  
public String getName()  
public void setName(String name)  
public int getNumericScale()  
public void setNumericScale(int scale)  
public int getPrecision()  
public void setPrecision(int prec)  
public int getSize()  
public void setSize(int size)  
public int getType()  
public void setType(int type)  
public com.ms.com.Variant getValue()  
public void setValue(Object v)  
public AdoProperties getProperties()  
```  
  
## <a name="parameter-accessor-methods"></a>Metodi della funzione di accesso Parameter  
 La proprietà [value](./value-property-ado.md) di un oggetto [Parameter](./parameter-object.md) Ottiene o imposta il contenuto di tale oggetto. Il contenuto è rappresentato come VARIANT, un tipo di oggetto a cui è possibile assegnare un valore e uno dei diversi tipi di dati.  
  
 ADO/WFC implementa la proprietà **value** con il metodo **GetValue** , che restituisce un oggetto Variant. e il metodo **SetValue** , che accetta un VARIANT come argomento. Le varianti sono estremamente efficienti in determinati linguaggi, ad esempio Microsoft Visual Basic.  
  
 Oltre alla proprietà **value** , ADO/WFC fornisce metodi per la *funzione di accesso* che utilizzano i tipi di dati Java per ottenere e impostare il contenuto di oggetti **Parameter** . La maggior parte di questi metodi ha nomi del form **Get**_DataType_ o **set**_DataType_.  
  
 Si è verificata un'eccezione significativa: non esiste alcuna proprietà **getNull** ; è invece disponibile una proprietà **IsNull** che restituisce un valore booleano che indica se il campo è null.  
  
```  
public boolean getBoolean()  
public void setBoolean(boolean v)  
public byte getByte()  
public void setByte(byte v)  
public double getDouble()  
public void setDouble(double v)  
public float getFloat()  
public void setFloat(float v)  
public int getInt()  
public void setInt(int v)  
public long getLong()  
public void setLong(long v)  
public short getShort()  
public void setShort(short v)  
public String getString()  
public void setString(String v)  
public boolean isNull()  
public void setNull()  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Parameter](./parameter-object.md)