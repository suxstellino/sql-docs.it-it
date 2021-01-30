---
description: Connessione tramite origini dati dei file
title: Connessione tramite origini dati file | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connecting to driver [ODBC], file data sources
- SQLDriverConnect function [ODBC], connecting using file data sources
- connecting to data source [ODBC], SQLDriverConnect
- connecting to driver [ODBC], SQLDriverConnect
- connecting to data source [ODBC], file data sources
- file data sources [ODBC]
ms.assetid: 3003f8c2-8be6-41cc-8d9c-612e9bd0f3ae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 285b2ba9d553d43a3b1ca7fdda8c08434201acdc
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077009"
---
# <a name="connecting-using-file-data-sources"></a>Connessione tramite origini dati dei file
Le informazioni di connessione per un'origine dati file vengono archiviate in un file con estensione DSN. Di conseguenza, la stringa di connessione può essere utilizzata ripetutamente da un singolo utente o condivisa tra più utenti se è installato il driver appropriato. Il file contiene un nome di driver (o un altro nome dell'origine dati nel caso di un'origine dati di file non condivisibile) e, facoltativamente, una stringa di connessione che può essere utilizzata da **SQLDriverConnect**. Gestione driver compila la stringa di connessione per la chiamata a **SQLDriverConnect** dalle parole chiave nel file con estensione DSN.  
  
 Un'origine dati file consente a un'applicazione di specificare le opzioni di connessione senza dover compilare una stringa di connessione da usare con **SQLDriverConnect**. L'origine dati del file viene in genere creata specificando la parola chiave **SaveFile** , che fa in modo che Gestione driver salvi la stringa di connessione di output creata da una chiamata a **SQLDriverConnect** nel file con estensione DSN. Tale stringa di connessione può essere utilizzata ripetutamente chiamando **SQLDriverConnect** con la parola chiave **FileDSN** . In questo modo viene semplificato il processo di connessione e viene fornita un'origine permanente della stringa di connessione.  
  
 Le origini dati dei file possono anche essere create chiamando **SQLCreateDataSource** nella dll del programma di installazione. È possibile scrivere le informazioni nel file con estensione DSN chiamando **SQLWriteFileDSN** e leggere dal file con estensione DSN chiamando **SQLReadFileDSN**; entrambe queste funzioni sono disponibili anche nella DLL del programma di installazione. Per informazioni sulla DLL del programma di installazione, vedere [Configuring Data Sources](../../../odbc/reference/install/configuring-data-sources.md).  
  
 Le parole chiave utilizzate per le informazioni di connessione si trovano nella sezione [ODBC] di un file con estensione DSN. Le informazioni minime che un file con estensione DSN può condividere nella sezione [ODBC] è la parola chiave del DRIVER:  
  
```  
DRIVER = SQL Server  
```  
  
 Il file con estensione DSN condivisibile contiene in genere una stringa di connessione, come indicato di seguito:  
  
```  
DRIVER = SQL Server  
UID = Larry  
DATABASE = MyDB  
```  
  
 Quando l'origine dati del file non è condivisibile, il file con estensione DSN contiene solo una parola chiave **DSN** . Quando Gestione driver invia le informazioni in un'origine dati di file non condivisibile, si connette a seconda dell'origine dati indicata dalla parola chiave **DSN** . Un file con estensione DSN non condivisibile conterrebbe la parola chiave seguente:  
  
```  
DSN = MyDataSource  
```  
  
 La stringa di connessione utilizzata per un'origine dati file è l'Unione delle parole chiave specificate nel file con estensione DSN e le parole chiave specificate nella stringa di connessione nella chiamata a **SQLDriverConnect**. Se una delle parole chiave nel file con estensione DSN è in conflitto con le parole chiave nella stringa di connessione, gestione driver decide quale valore di parola chiave deve essere utilizzato. Per ulteriori informazioni, vedere [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
## <a name="see-also"></a>Vedere anche  
 [https://support.microsoft.com/kb/165866](https://mskb.pkisolutions.com/kb/165866)
