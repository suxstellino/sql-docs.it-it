---
description: sysmergeschemaarticles (Transact-SQL)
title: sysmergeschemaarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergeschemaarticles_TSQL
- sysmergeschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemaarticles system table
ms.assetid: b5085979-2f76-48e1-bf3b-765a84003dd9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 579acde4f82556f176e5923a66db48c1faafe27f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210196"
---
# <a name="sysmergeschemaarticles-transact-sql"></a>sysmergeschemaarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Tiene traccia degli articoli solo schema per la replica di tipo merge. Questa tabella è archiviata nei database di pubblicazione e di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome dell'articolo solo schema nella pubblicazione di tipo merge.|  
|**type**|**tinyint**|Specifica il tipo dell'articolo solo schema. I possibili valori sono i seguenti:<br /><br /> **0x20** = articolo solo schema stored procedure.<br /><br /> **0x40** = Visualizza solo gli articoli relativi allo schema o alla vista indicizzata.|  
|**objid**|**int**|Identificatore dell'oggetto di base dell'articolo. Può corrispondere all'identificatore di oggetto di una procedura, una vista, una vista indicizzata o una funzione definita dall'utente.|  
|**artid**|**uniqueidentifier**|ID dell'articolo.|  
|**description**|**nvarchar(255)**|Descrizione dell'articolo.|  
|**pre_creation_command**|**tinyint**|Azione predefinita da eseguire quando viene creato l'articolo nel database di sottoscrizione:<br /><br /> **0 =** None: se la tabella esiste già nel Sottoscrittore, non viene eseguita alcuna azione.<br /><br /> **1** = Drop: Elimina la tabella prima di ricrearla.<br /><br /> **2** = Delete: rilascia un'eliminazione in base alla clausola WHERE nel filtro di subset.<br /><br /> **3** = troncamento: uguale a **2**, ma elimina pagine anziché righe. La clausola WHERE in questo caso non viene utilizzata.|  
|**pubid**|**uniqueidentifier**|Identificatore univoco della pubblicazione.|  
|**Stato**|**tinyint**|Specifica lo stato dell'articolo solo schema. I possibili valori sono i seguenti:<br /><br /> **1** = non sincronizzato: lo script di elaborazione iniziale per la pubblicazione della tabella viene eseguito alla successiva esecuzione del agente di snapshot.<br /><br /> **2** = attivo: lo script di elaborazione iniziale per la pubblicazione della tabella è stato eseguito.<br /><br /> **5** = New_inactive-da aggiungere.<br /><br /> **6** = New_active-da aggiungere.|  
|**creation_script**|**nvarchar(255)**|Percorso e nome di uno script pre-snapshot facoltativo dello schema dell'articolo utilizzato per la creazione della tabella di destinazione.|  
|**schema_option**|**binario (8)**|Mappa di bit dell'opzione di generazione dello schema per l'articolo solo schema specificato. Può essere il risultato di un'operazione con OR logico bit per bit eseguita su uno o più dei valori seguenti:<br /><br /> **0x00** = Disabilita lo scripting da parte del agente di snapshot e usa il CreationScript fornito.<br /><br /> **0x01** = genera la creazione dell'oggetto (Create Table, crea procedura e così via).<br /><br /> **0x10** = genera un indice cluster corrispondente.<br /><br /> **0x20** = converte i tipi di dati definiti dall'utente in tipi di dati di base.<br /><br /> **0x40** = genera indice o indici non cluster corrispondenti.<br /><br /> **0x80** = include l'integrità referenziale dichiarata nelle chiavi primarie.<br /><br /> **0x100** = replica i trigger utente in un articolo di tabella, se definito.<br /><br /> **0x200** = replica i vincoli FOREIGN KEY. Se la tabella con riferimenti non fa parte di una pubblicazione, tutti i vincoli di chiave esterna su una tabella pubblicata non vengono replicati.<br /><br /> **0x400** = replica i vincoli check.<br /><br /> **0x800** = replica le impostazioni predefinite.<br /><br /> **0x1000** = replica le regole di confronto a livello di colonna.<br /><br /> **0x2000** = replica le proprietà estese associate all'oggetto di origine dell'articolo pubblicato.<br /><br /> **0x4000** = replica le chiavi univoche se definito in un articolo di tabella.<br /><br /> **0x8000** = replica una chiave primaria e le chiavi univoche di un articolo di tabella come vincoli mediante istruzioni ALTER TABLE.<br /><br /> Per ulteriori informazioni sui valori possibili per **schema_option**, vedere [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Nome dell'oggetto di destinazione nel database di sottoscrizione. Viene utilizzato solo per articoli solo schema, quali articoli di stored procedure, viste e funzioni definite dall'utente.|  
|**destination_owner**|**sysname**|Proprietario dell'oggetto nel database di sottoscrizione, se non è **dbo**.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
