---
description: sysschemaarticles (Transact-SQL)
title: sysschemaarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysschemaarticles_TSQL
- sysschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysschemaarticles system table
ms.assetid: 67a1c039-c283-4a9c-bacc-b9b3973590c3
author: cawrites
ms.author: chadam
ms.openlocfilehash: ecefc7fe927f5013d5647c276cf4dc1e687c0ff0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102704"
---
# <a name="sysschemaarticles-transact-sql"></a>sysschemaarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Tiene traccia degli articoli relativi solo allo schema per pubblicazioni transazionali e snapshot. Questa tabella è archiviata nel database di pubblicazione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|ID dell'articolo.|  
|**creation_script**|**nvarchar(255)**|Percorso e nome di uno script di schema dell'articolo utilizzato per la creazione della tabella di destinazione.|  
|**description**|**nvarchar(255)**|Voce descrittiva per l'articolo.|  
|**dest_object**|**sysname**|Nome dell'oggetto del database di sottoscrizione se l'articolo è relativo solo allo schema, quali articoli di stored procedure, viste e funzioni definite dall'utente.|  
|**nome**|**sysname**|Nome dell'articolo relativo solo allo schema in una pubblicazione.|  
|**objid**|**int**|Identificatore dell'oggetto di base dell'articolo. Può corrispondere all'identificatore di oggetto di una procedura, vista, vista indicizzata o funzione definita dall'utente.|  
|**pubid**|**int**|ID della pubblicazione.|  
|**pre_creation_cmd**|**tinyint**|Specifica l'azione eseguita dal sistema se nel Sottoscrittore viene rilevato un oggetto esistente avente lo stesso nome durante l'applicazione dello snapshot per l'articolo:<br /><br /> **0** = niente.<br /><br /> **1** = Elimina tabella di destinazione.<br /><br /> **2** = Elimina tabella di destinazione.<br /><br /> **3** = troncamento della tabella di destinazione.|  
|**Stato**|**int**|Mappa di bit utilizzata per indicare lo stato dell'articolo.|  
|**type**|**tinyint**|Tipo di articolo relativo solo allo schema:<br /><br /> **32** = stored procedure.<br /><br /> **64** = vista o vista indicizzata. <br /><br /> **96** = funzione di aggregazione.<br /><br /> **128** = funzione.|  
|**schema_option**|**binario (8)**|Maschera di bit dell'opzione di creazione dello schema per l'articolo specificato. Imposta la creazione automatica della stored procedure nel database di destinazione per ogni sintassi CALL/MCALL/XCALL e può corrispondere al risultato dell'applicazione dell'operatore OR logico bit per bit a uno dei valori seguenti:<br /><br /> **0x00** = Disabilita lo scripting dal agente di snapshot e USA *creation_script*.<br /><br /> **0x01** = genera la creazione dell'oggetto (Create Table, create procedure e così via). Corrisponde al valore predefinito per gli articoli di stored procedure.<br /><br /> **0x02** = genera stored procedure personalizzate per l'articolo, se definite.<br /><br /> **0x10** = genera un indice cluster corrispondente.<br /><br /> **0x20** = converte i tipi di dati definiti dall'utente in tipi di dati di base.<br /><br /> **0x40**= genera gli indici non cluster corrispondenti.<br /><br /> **0x80**= include l'integrità referenziale dichiarata nelle chiavi primarie.<br /><br /> **0x73** = genera l'istruzione CREATE TABLE, crea indici cluster e non cluster, converte i tipi di dati definiti dall'utente in tipi di dati di base e genera script di stored procedure personalizzati da applicare nel Sottoscrittore. Corrisponde al valore predefinito per tutti gli articoli, tranne gli articoli di stored procedure.<br /><br /> **0x100**= replica i trigger utente in un articolo di tabella, se definito.<br /><br /> **0x200**= replica i vincoli FOREIGN KEY. Se la tabella con riferimenti non fa parte di una pubblicazione, i vincoli FOREIGN KEY di una tabella pubblicata non vengono replicati.<br /><br /> **0x400**= replica i vincoli check.<br /><br /> **0x800**= replica le impostazioni predefinite.<br /><br /> **0x1000**= replica le regole di confronto a livello di colonna.<br /><br /> **0x2000**= replica le proprietà estese associate all'oggetto di origine dell'articolo pubblicato.<br /><br /> **0x4000**= replica le chiavi univoche se definite in un articolo di tabella.<br /><br /> **0x8000**= replica la chiave primaria e le chiavi univoche di un articolo di tabella come vincoli utilizzando le istruzioni ALTER TABLE.|  
|**dest_owner**|**sysname**|Proprietario della tabella nel database di destinazione.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
