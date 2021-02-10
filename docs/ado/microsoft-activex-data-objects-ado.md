---
description: Microsoft ActiveX Data Objects (ADO)
title: Microsoft ActiveX Data Objects (ADO) | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, about
ms.assetid: 2fa6237b-44b8-4b6c-9952-5acd80a54e20
author: rothja
ms.author: jroth
ms.openlocfilehash: 511b556971496633abd6d347ab416b75a8f9f27c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036041"
---
# <a name="microsoft-activex-data-objects-ado"></a>Microsoft ActiveX Data Objects (ADO)

ActiveX Data Objects è un modello di programmazione, il che significa che non dipende da alcun motore back-end specificato. Attualmente, tuttavia, l'unico motore che supporta il modello ADO è OLE-DB. Sono disponibili molti provider OLE DB nativi e un provider OLE DB per ODBC. ADO viene utilizzato nei programmi C++ e Visual Basic per connettersi ai SQL Server e ad altri database. Naturalmente, funziona anche per connettersi al database SQL di Azure nel cloud.

In ogni sezione di questo articolo viene descritto un componente di ADO.

> [!NOTE]
> ADO.NET è diverso da ADO. ADO.NET, e molti altri driver di connessione SQL e le relative lingue, vengono illustrati a partire da [SQL Server driver](../connect/sql-connection-libraries.md).

  
## <a name="ado"></a>ADO  
 Microsoft ActiveX Data Objects (ADO) consente alle applicazioni client di accedere e manipolare i dati da un'ampia gamma di origini tramite un provider di OLE DB. I vantaggi principali sono la facilità d'uso, la velocità elevata, il sovraccarico di memoria ridotto e un footprint di disco ridotto. ADO supporta funzionalità chiave per la creazione di applicazioni client/server e basate sul Web.  
  
## <a name="ado-md"></a>ADO MD  
 Microsoft ActiveX Data Objects (Multidimensional) (ADO MD) consente di accedere facilmente ai dati multidimensionali da linguaggi quali Microsoft Visual Basic e Microsoft Visual C++. ADO MD estende Microsoft ActiveX Data Objects (ADO) per includere gli oggetti specifici di dati multidimensionali, ad esempio gli oggetti CubeDef e Cellt. Con ADO MD è possibile esplorare uno schema multidimensionale, eseguire una query su un cubo e recuperare i risultati.  
  
 Come ADO, ADO MD USA un provider di OLE DB sottostante per ottenere l'accesso ai dati. Per lavorare con ADO MD, il provider deve essere un provider di dati multidimensionale (MDP) come definito dalla specifica OLE DB per OLAP. MDPs presentano i dati nelle viste multidimensionali anziché i provider di dati tabulari (TDP) che presentano i dati nelle viste tabulari. Per informazioni più dettagliate sulla sintassi e sui comportamenti specifici supportati dal provider, vedere la documentazione relativa al provider di OLE DB OLAP.  
  
## <a name="rds"></a>Servizi desktop remoto  
 Remote Data Service (RDS) è una funzionalità di ADO, che consente di spostare i dati da un server a un'applicazione client o a una pagina Web, di modificare i dati nel client e di restituire gli aggiornamenti al server in un unico round trip.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a  [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="adox"></a>ADOX  
 Microsoft ActiveX Data Objects Extensions for Data Definition Language and Security (ADOX) è un'estensione degli oggetti ADO e del modello di programmazione. ADOX include oggetti per la creazione e la modifica dello schema, nonché per la sicurezza. Poiché si tratta di un approccio basato su oggetti alla manipolazione dello schema, è possibile scrivere codice che funzionerà su varie origini dati indipendentemente dalle differenze nelle relative sintassi native.  
  
 ADOX è una libreria complementare agli oggetti ADO di base. Espone oggetti aggiuntivi per la creazione, la modifica e l'eliminazione di oggetti dello schema, ad esempio tabelle e procedure. Include inoltre oggetti di sicurezza per gestire utenti e gruppi e per concedere e revocare le autorizzazioni per gli oggetti.  
  
## <a name="documentation"></a>Documentazione  
 [Problemi di progettazione della sicurezza ADO](./guide/ado-security-design-issues.md)  
  
 [Manuale del programmatore ADO](./guide/ado-programmer-s-guide.md)  
  
 Introduzione all'utilizzo di ADO, RDS, ADO MD e ADOX.  
  
 [Informazioni di riferimento per programmatori ADO](./reference/ado-programmer-s-reference.md)  
  
 In questa sezione della documentazione di ADO sono contenuti argomenti per ogni oggetto ADO, RDS, ADO MD e ADOX, raccolta, proprietà, proprietà dinamica, metodo, evento ed enumerazione.  
  
 [Glossario di ADO](./ado-glossary.md)  
  
## <a name="support"></a>Supporto  
 Per assistenza gratuita sui problemi ADO, provare a pubblicare il newsgroup ADO public. Questo newsgroup è monitorato dai professionisti del servizio supporto tecnico clienti Microsoft che coprono ADO e da altri sviluppatori ADO esperti.  
  
 Per ulteriori informazioni sulle opzioni di supporto, vedere il sito Web Microsoft Guida e supporto tecnico.