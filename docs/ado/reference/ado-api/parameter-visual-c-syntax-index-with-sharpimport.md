---
description: 'Parametro (Visual C++ indice della sintassi con #import)'
title: 'Parametro (Visual C++ indice della sintassi con #import) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- 'Parameter collection [ADO], Visual C++ syntax index with #import'
ms.assetid: 6b43cf70-9695-47b0-9e68-f36898859b6b
author: rothja
ms.author: jroth
ms.openlocfilehash: b3c958930b82a15f9dc9cbfd14be52a82f90c86e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041141"
---
# <a name="parameter-visual-c-syntax-index-with-import"></a>Parametro (Visual C++ indice della sintassi con #import)
## <a name="methods"></a>Metodi  
  
```  
HRESULT AppendChunk( const _variant_t & Val );  
```  
  
## <a name="properties"></a>Proprietà  
  
```  
long GetAttributes( );  
void PutAttributes( long plParmAttribs );  
__declspec(property(get=GetAttributes,put=PutAttributes)) long  
    Attributes;  
  
enum ParameterDirectionEnum GetDirection( );  
void PutDirection( enum ParameterDirectionEnum plParmDirection );  
__declspec(property(get=GetDirection,put=PutDirection)) enum  
    ParameterDirectionEnum Direction;  
  
_bstr_t GetName( );  
void PutName( _bstr_t pbstr );  
__declspec(property(get=GetName,put=PutName)) _bstr_t Name;  
  
unsigned char GetNumericScale( );  
void PutNumericScale( unsigned char pbScale );  
__declspec(property(get=GetNumericScale,put=PutNumericScale)) unsigned  
    char NumericScale;  
  
unsigned char GetPrecision( );  
void PutPrecision( unsigned char pbPrecision );  
__declspec(property(get=GetPrecision,put=PutPrecision)) unsigned char  
    Precision;  
  
long GetSize( );  
void PutSize( long pl );  
__declspec(property(get=GetSize,put=PutSize)) long Size;  
  
enum DataTypeEnum GetType( );  
void PutType( enum DataTypeEnum psDataType );  
__declspec(property(get=GetType,put=PutType)) enum DataTypeEnum Type;  
  
_variant_t GetValue( );  
void PutValue( const _variant_t & pvar );  
__declspec(property(get=GetValue,put=PutValue)) _variant_t Value;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Parameter](./parameter-object.md)