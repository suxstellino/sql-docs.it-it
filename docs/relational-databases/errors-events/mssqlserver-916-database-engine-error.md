---
title: MSSQLSERVER_916 | Microsoft Docs
description: L'accesso non ha le autorizzazioni sufficienti per connettersi al database di SQL Server denominato. Vedere la descrizione dell'errore e delle possibili soluzioni.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 916 (Database Engine error)
ms.assetid: 73eb6581-99fe-49a5-9b42-e239d7ffe27f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b57df3ff247d4c896719197c71d770a4c929a906
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99153679"
---
# <a name="mssqlserver_916"></a>MSSQLSERVER_916
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|916|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|NOTUSER|  
|Testo del messaggio|L'entità server "%.*ls" non è in grado di accedere al database "%.\*ls" nel contesto di sicurezza corrente.|  
  
## <a name="explanation"></a>Spiegazione  
L'accesso non dispone delle autorizzazioni sufficienti per connettersi al database denominato. Gli account di accesso che possono connettersi a questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ma che non dispongono di autorizzazioni specifiche in un database ricevono le autorizzazioni di utente guest. Si tratta di una misura di sicurezza per impedire agli utenti di un database di connettersi ad altri database per cui non dispongono di privilegi. È possibile ricevere questo messaggio di errore quando l'utente guest non dispone delle autorizzazioni CONNECT per il database denominato e la proprietà TRUSTWORTHY non è impostata. È possibile ricevere questo messaggio di errore quando l'utente guest non dispone delle autorizzazioni CONNECT per il database denominato.  
  
Quando l'autorizzazione CONNECT al database msdb viene negata o revocata, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] può ricevere questo errore quando Esplora oggetti tenta di mostrare lo stato di gestione basata su criteri di ciascun database. Esplora oggetti usa le autorizzazioni dell'accesso corrente per cercare nel database msdb queste informazioni, operazione che causa l'errore. Viene inoltre visualizzato il messaggio di errore seguente:  
  
Impossibile recuperare i dati per la richiesta specificata. (Microsoft.SqlServer.Management.Sdk.Sfc)  
  
## <a name="user-action"></a>Azione dell'utente  
  
> [!WARNING]  
> Prima di eludere questa misura di sicurezza, assicurarsi di sapere quali utenti sono autenticati in vari database. I metodi seguenti consentono agli utenti che dispongono delle autorizzazioni per un database di connettersi ad altri database che potrebbero esporre i dati a utenti malintenzionati. Quando i database indipendenti sono abilitati, i passaggi seguenti consentono ai proprietari di un database di concedere l'accesso all'altro database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
È possibile connettersi al database in uno dei modi seguenti:  
  
-   Concedere l'accesso specifico al database denominato. Nell'esempio seguente viene concesso l'accesso `Adventure-Works\Larry` al database `msdb`.  

    ```sql
    USE msdb ;
    
    GO
    
    GRANT CONNECT TO [Adventure-Works\Larry] ;
    ```
  
-   Concedere l'autorizzazione CONNECT al database denominato nel messaggio di errore per l'utente guest. Nell'esempio seguente viene concessa l'autorizzazione `CONNECT` per il database `msdb` per l'utente `guest`.  

    ```sql
    USE msdb ;
    
    GO
    
    GRANT CONNECT TO guest ;
    ```
  
-   Abilitare la proprietà TRUSTWORTHY per il database che ha autenticato l'utente.  

    ```sql
    ALTER DATABASE AdventureWorks SET TRUSTWORTHY ON;
    ```
