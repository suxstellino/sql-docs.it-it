---
description: Esecuzione dell'applicazione di esempio Address Book
title: Esecuzione dell'applicazione di esempio Address Book | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: 3a2644e9-d634-4ae6-a5b7-13fb7b317ec7
author: rothja
ms.author: jroth
ms.openlocfilehash: b4e77339de5ec7ebddbdb1314228abf6eef8c15d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031813"
---
# <a name="running-the-address-book-sample-application"></a>Esecuzione dell'applicazione di esempio Address Book
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Per eseguire l'applicazione Address Book, seguire questa procedura.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
### <a name="to-run-this-application"></a>Per eseguire l'applicazione  
  
1.  Assicurarsi che Microsoft SQL Server sia in esecuzione. Fare clic sul pulsante **Start**, scegliere **programmi**, **Microsoft SQL Server 7,0**, quindi fare clic su **Service Manager**. Se è presente una freccia verde nel cerchio bianco, SQL Server è in esecuzione. In caso contrario (sarà presente un quadrato rosso nel cerchio bianco), fare clic su **Avvia/continua**.  
  
2.  In Microsoft Internet Explorer 4,0 o versione successiva digitare l'indirizzo seguente:  
  
     **https://** *webserver* **/RDS/AddressBook/AddrBook.asp**  
  
     dove *webserver* è il nome del server Web in cui sono installati i componenti server RDS.  
  
3.  È quindi possibile provare diversi scenari nell'applicazione di esempio Address Book, ad esempio la ricerca di un utente in base al nome di posta elettronica, elencando tutte le persone con il titolo "Program Manager" o modificando i record esistenti. Fare clic su **trova** per riempire la griglia di dati con tutti i nomi disponibili.  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto di data binding di Address Book](./address-book-data-binding-object.md)