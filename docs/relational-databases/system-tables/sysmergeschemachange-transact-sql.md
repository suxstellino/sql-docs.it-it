---
description: sysmergeschemachange (Transact-SQL)
title: sysmergeschemachange (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergeschemachange_TSQL
- sysmergeschemachange
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemachange system table
ms.assetid: ae9ce16e-6ee9-4c7c-8210-a9bf2c7efdf0
author: cawrites
ms.author: chadam
ms.openlocfilehash: e4fcb1360495a72ca5650abc1c76e7c7dd224f2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209209"
---
# <a name="sysmergeschemachange-transact-sql"></a>sysmergeschemachange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene informazioni sugli articoli pubblicati generati dall'agente snapshot. Questa tabella è archiviata nei database di pubblicazione e di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pubid**|**uniqueidentifier**|ID della pubblicazione.|  
|**artid**|**uniqueidentifier**|ID dell'articolo.|  
|**SchemaVersion**|**int**|Numero dell'ultima modifica dello schema.|  
|**schemaguid**|**uniqueidentifier**|ID univoco dell'ultimo schema.|  
|**schematype**|**int**|Tipo di modifica dello schema:<br /><br /> **-1** = non valido.<br /><br /> **1** = comando SQL.<br /><br /> **2** = script dello schema.<br /><br /> **3** = utilità per la copia bulk nativa (BCP).<br /><br /> **4** = bcp di tipo carattere.<br /><br /> **5** = ultima generazione registrata.<br /><br /> **6** = ultima generazione inviata.<br /><br /> **7** = directory.<br /><br /> **8** = priorità.<br /><br /> **9** = periodo di conservazione.<br /><br /> **10** = script di trigger.<br /><br /> **11** = ALTER TABLE.<br /><br /> **12** = Reinitialize all.<br /><br /> **13** = ALTER TABLE (non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ).<br /><br /> **14** = reinizializza con caricamento.<br /><br /> **15** = script di vincolo e di indice.<br /><br /> **16** = pulizia dei metadati.<br /><br /> **17** = aggiornamento dell'ultima generazione inviata.<br /><br /> **18** = livello di compatibilità con le versioni precedenti.<br /><br /> **19** = convalida le informazioni del Sottoscrittore.<br /><br /> **20** = partizionato correttamente.<br /><br /> **21** = sistema di risoluzione personalizzato.<br /><br /> **22** = ordine di elaborazione degli articoli.<br /><br /> **23** = pubblicato nella pubblicazione transazionale.<br /><br /> **24** = compensazione degli errori.<br /><br /> **25** = cartella snapshot alternativa.<br /><br /> **26** = solo download.<br /><br /> **27** = rilevamento delle eliminazioni.<br /><br /> **40** = script di pre-Creazione Snapshot.<br /><br /> **45** = script di snapshot post-creazione.<br /><br /> **46** = script utente su richiesta.<br /><br /> **50** = inizio intestazione snapshot.<br /><br /> **51** = fine intestazione snapshot.<br /><br /> **52** = trailer dello snapshot.<br /><br /> **53** = indirizzo FTP (file Transfer Protocol).<br /><br /> **54** = porta FTP.<br /><br /> **55** = sottodirectory FTP.<br /><br /> **56** = snapshot compresso.<br /><br /> **57** = accesso FTP.<br /><br /> **58** = password FTP.<br /><br /> **60** = script di pre-creazione di sistema.<br /><br /> **61** = schema stored procedure.<br /><br /> **62** = schema di visualizzazione.<br /><br /> **63** = schema di funzione definito dall'utente.<br /><br /> **64** = indici della vista.<br /><br /> **65** = proprietà estese.<br /><br /> **66** = convalida.<br /><br /> **67** = comando SQL pre-snapshot.<br /><br /> **71** = convalida snapshot dinamici.<br /><br /> **80** = BCP 9,0 nativo per la tabella di sistema.<br /><br /> **81** = BCP 9,0 di tipo carattere tabella di sistema.<br /><br /> **82** = BCP 9,0 nativo per la tabella di sistema (solo globale).<br /><br /> **83** = BCP 9,0 (solo globale).<br /><br /> **84** = BCP 9,0 nativo per la tabella di sistema (Lightweight).<br /><br /> **85** = BCP 9,0 (Lightweight) con caratteri di tabella di sistema.<br /><br /> **128** = BCP dinamico (bit).<br /><br /> **131** = BCP nativo dinamico.<br /><br /> **132** = bcp Dynamic character.<br /><br /> **208** = BCP 9,0 nativo per la tabella di sistema dinamica.<br /><br /> **209** = BCP 9,0 con carattere di tabella di sistema dinamico.<br /><br /> **210** = BCP 9,0 nativo per la tabella di sistema dinamica (solo globale).<br /><br /> **211** = BCP 9,0 (solo globale) con caratteri di tabella di sistema dinamici.<br /><br /> **212** = BCP 9,0 nativo per la tabella di sistema dinamica (Lightweight).<br /><br /> **213** = BCP 9,0 (Lightweight) con caratteri di tabella di sistema dinamici.<br /><br /> **300** = azioni DDL (Data Definition Language).<br /><br /> **1024** = controllo snapshot incrementale.<br /><br /> **1049** = cartella snapshot incrementale.<br /><br /> **1074** = intestazione di inizio dello snapshot incrementale.<br /><br /> **1075** = intestazione finale dello snapshot incrementale.<br /><br /> **1076** = trailer dello snapshot incrementale.<br /><br /> **1077** = indirizzo FTP incrementale.<br /><br /> **1078** = porta FTP incrementale.<br /><br /> **1079** = sottodirectory FTP incrementale.<br /><br /> **1080** = snapshot compresso incrementale.<br /><br /> **1081** = accesso FTP incrementale.<br /><br /> **1082** = password FTP incrementale.|  
|**schematext**|**nvarchar (2000)**|Nome del file di script o comando che include un nome di file.|  
|**schemastatus**|**tinyint**|Specifica se è in sospeso una modifica dello schema per l'articolo. I possibili valori sono i seguenti:<br /><br /> **0** = inattivo.<br /><br /> **1** = attivo.<br /><br /> Quando una modifica dello schema è in sospeso, questo valore viene impostato su **1**.|  
|**schemasubtype**|**int**|Sottotipo di modifica dello schema:<br /><br /> **1** = ADDCOLUMN<br /><br /> **2** = DROPCOLUMN<br /><br /> **3** = ALTERCOLUMN<br /><br /> **4** = ADDPRIMARYKEY<br /><br /> **5** = ADDUNIQUE<br /><br /> **6** = ADDREFERENCE<br /><br /> **7** = DROPCONSTRAINT<br /><br /> **8** = AddDefault<br /><br /> **9** = addcheck<br /><br /> **10** = DISABLETRIGGER<br /><br /> **11** = ENABLETRIGGER<br /><br /> **12** = DISABLETRIGGER<br /><br /> **13** = ENABLETRIGGER<br /><br /> **14** = ENABLECONSTRAINT<br /><br /> **15** = DISABLECONSTRAINT<br /><br /> **16** = ENABLECONSTRAINT<br /><br /> **17** = DISABLECONSTRAINT|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
