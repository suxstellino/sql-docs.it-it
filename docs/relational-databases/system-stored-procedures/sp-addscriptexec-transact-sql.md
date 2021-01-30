---
description: sp_addscriptexec (Transact-SQL)
title: sp_addscriptexec (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addscriptexec
- sp_addscriptexec_TSQL
helpviewer_keywords:
- sp_addscriptexec
ms.assetid: 1627db41-6a80-45b6-b0b9-c0b7f9a1c886
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6349479d4f3ce3097771516640b0eec258e32f5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207155"
---
# <a name="sp_addscriptexec-transact-sql"></a>sp_addscriptexec (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Invia uno script SQL (file con estensione sql) a tutti i Sottoscrittori di una pubblicazione. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addscriptexec [ @publication = ] publication  
    [ , [ @scriptfile = ] 'scriptfile' ]  
    [ , [ @skiperror = ] 'skiperror' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @scriptfile = ] 'scriptfile'` È il percorso completo del file script SQL. *scriptfile* è di **tipo nvarchar (4000)** e non prevede alcun valore predefinito.  
  
`[ @skiperror = ] 'skiperror'` Indica se il agente di distribuzione o agente di merge deve arrestarsi quando viene rilevato un errore durante l'elaborazione dello script. *SkipError* è di **bit** e il valore predefinito è 0.  
  
 **0** = l'agente si arresterà.  
  
 **1** = l'agente continua lo script e ignora l'errore.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *Server* di pubblicazione non deve essere utilizzato per la pubblicazione da un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_addscriptexec** viene utilizzata per la replica transazionale e la replica di tipo merge.  
  
 **sp_addscriptexec** non viene utilizzato per la replica snapshot.  
  
 Per utilizzare **sp_addscriptexec**, l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account del servizio deve disporre delle autorizzazioni di lettura e scrittura per la posizione dello snapshot e le autorizzazioni di lettura per il percorso in cui sono archiviati gli script.  
  
 L' [utilità sqlcmd](../../tools/sqlcmd-utility.md) viene utilizzata per eseguire lo script nel Sottoscrittore e lo script viene eseguito nel contesto di sicurezza utilizzato dal agente di distribuzione o agente di merge durante la connessione al database di sottoscrizione. Quando l'agente viene eseguito in una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , viene utilizzata l' [utilità osql](../../tools/osql-utility.md) anziché [SQLCMD](../../tools/sqlcmd-utility.md).  
  
 **sp_addscriptexec** è utile per l'applicazione di script ai sottoscrittori e utilizza [SQLCMD](../../tools/sqlcmd-utility.md) per applicare il contenuto dello script al Sottoscrittore. Tuttavia, poiché le configurazioni dei Sottoscrittori sono soggette a variazioni, gli script testati prima dell'invio al server di pubblicazione potrebbero comunque causare errori in un Sottoscrittore. *SkipError* offre la possibilità di agente di distribuzione o agente di merge ignorare gli errori e continuare con. Usare [SQLCMD](../../tools/sqlcmd-utility.md) per testare gli script prima di eseguire **sp_addscriptexec**.  
  
> [!NOTE]  
>  Gli errori ignorati vengono comunque registrati nella cronologia dell'agente a titolo di riferimento.  
  
 L'utilizzo di **sp_addscriptexec** per pubblicare un file di script per le pubblicazioni mediante FTP per il recapito di snapshot è supportato solo per i [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sottoscrittori.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addscriptexec**.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di script durante la sincronizzazione &#40;la programmazione Transact-SQL della replica&#41;](../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md)   
 [Sincronizzare i dati](../../relational-databases/replication/synchronize-data.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
