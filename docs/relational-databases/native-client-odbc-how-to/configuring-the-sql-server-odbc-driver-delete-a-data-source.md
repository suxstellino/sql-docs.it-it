---
title: Eliminare un'origine dati (ODBC) | Microsoft Docs
description: Informazioni su come eliminare un'origine dati utilizzando l'amministratore ODBC, a livello di codice o utilizzando un file, prima di utilizzare le applicazioni ODBC con SQL Server 2005 o versione successiva.
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- data sources [ODBC]
ms.assetid: 910e3e16-7b91-49d8-80bb-b4243926afaa
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1dff34d1e0ce2554cd13fb5e261ad35ced8f378c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752561"
---
# <a name="configuring-the-sql-server-odbc-driver---delete-a-data-source"></a>Configurazione del driver ODBC di SQL Server - Eliminare un'origine dati
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Prima di utilizzare le applicazioni ODBC con [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versioni successive, è necessario essere in grado di aggiornare la versione delle stored procedure del catalogo nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nonché aggiungere, eliminare e testare le origini dati.  
  
  È possibile eliminare un'origine dati tramite l'amministratore ODBC, a livello di codice (tramite [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md)) oppure eliminando un file (se il nome dell'origine dati è un file).  
  
### <a name="to-delete-a-data-source-by-using-odbc-administrator"></a>Per eliminare un'origine dati tramite Amministratore ODBC.  
  
1.  Nel **Pannello di controllo** aprire **strumenti di amministrazione**, quindi fare doppio clic su **origini dati ODBC (64 bit)** o **origini dati ODBC (32-bit)**. In alternativa, è possibile eseguire odbcad32.exe dal prompt dei comandi.  
  
2.  Fare clic sulla scheda DSN **utente**, **DSN di sistema** o **DSN su file** .  
  
3.  Consente di selezionare l'origine dati da eliminare.  
  
4.  Fare clic su **Rimuovi**, quindi confermare l'eliminazione.  

## <a name="example"></a>Esempio  
 Per eliminare un'origine dati a livello di codice, chiamare [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md) usando ODBC_REMOVE_DSN o ODBC_REMOVE_SYS_DSN come secondo parametro.  
  
 Nell'esempio seguente viene illustrato come è possibile eliminare un'origine dati a livello di programmazione.  
  
```  
// remove_odbc_data_source.cpp  
// compile with: ODBCCP32.lib user32.lib  
#include <iostream>  
#include \<windows.h>  
#include \<odbcinst.h>  
  
int main() {   
   LPCSTR provider = "SQL Server";   // Windows SQL Server Driver  
   LPCSTR provider = "SQL Server";   // Windows SQL Server driver  
   LPCSTR provider2 = "SQL Server Native Client 11.0";   // SQL Server 2012 Native Client driver  
   LPCSTR dsnname = "DSN=data2";  
   BOOL retval = SQLConfigDataSource(NULL, ODBC_REMOVE_DSN, provider, dsnname);  
   std::cout << retval;   // 1 if successful  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiungere un'origine dati &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/configuring-the-sql-server-odbc-driver-add-a-data-source.md)  
  
  
