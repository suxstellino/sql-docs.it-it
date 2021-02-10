---
description: Impostazioni del progetto (mapping dei tipi) (SybaseToSQL)
title: Impostazioni progetto (mapping dei tipi) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2698fb3a-f9e6-4e04-94e0-dad289d7ed0a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9e9086feb8adcb64ba1a24191e91028fc870d488
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069921"
---
# <a name="project-settings-type-mapping-sybasetosql"></a>Impostazioni del progetto (mapping dei tipi) (SybaseToSQL)
La pagina mapping dei tipi della finestra di dialogo **Impostazioni progetto** contiene impostazioni che personalizzano il modo in cui SSMA converte i tipi di dati di Sybase Adaptive Server Enterprise (ASE) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipi di dati.  
  
La pagina mapping dei tipi è disponibile nelle finestre di dialogo **Impostazioni progetto** e **Impostazioni progetto predefinite** .  
  
-   Per specificare le impostazioni di mapping dei tipi per tutti i progetti SSMA futuri, nel menu **strumenti** selezionare **Impostazioni progetto predefinite**, selezionare il tipo di progetto di migrazione per il quale è necessario visualizzare o modificare le impostazioni dall'elenco a discesa **versione destinazione migrazione** e quindi selezionare **mapping dei tipi** nella parte inferiore del riquadro sinistro.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , quindi selezionare mapping dei **tipi** nella parte inferiore del riquadro sinistro.  
  
## <a name="options"></a>Opzioni  
**Tipo origine**  
Tipo di dati ASE mappato.  
  
**Tipo destinazione**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Tipo di dati di destinazione per il tipo di dati ASE specificato.  
  
Vedere la tabella nella sezione seguente per il mapping dei tipi predefiniti di SSMA per Sybase.  
  
**Aggiungere**  
Fare clic su questo pulsante per aggiungere un tipo di dati all'elenco di mapping.  
  
**Modifica**  
Fare clic per modificare il tipo di dati selezionato nell'elenco mapping.  
  
**Rimuovi**  
Fare clic per rimuovere il mapping dei tipi di dati selezionati dall'elenco mapping.  
  
**Ripristina predefiniti**  
Fare clic per reimpostare l'elenco dei mapping dei tipi sui valori predefiniti di SSMA.  
  
## <a name="default-type-mapping"></a>Mapping dei tipi predefiniti  
La tabella seguente contiene il mapping dei tipi predefinito tra l'ambiente del servizio app e i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipi di dati.  
  
|Tipo di dati ASE|Tipo di dati di SQL Server|  
|-----------------|------------------------|  
|**bigint**|**bigint**|  
|**binary**|**binary**|  
|**binario [ \* .. 8000]**|**Binary [ \* ]**|  
|**binario [8001.. \* ]**|**varbinary(max)**|  
|**bit**|**bit**|  
|**char**|**char**|  
|**char varying**|**varchar**|  
|**carattere variabile [ \* .. 8000]**|**varchar [ \* ]**|  
|**carattere variabile [8001... \* ]**|**ntext**|  
|**Char [ \* .. 8000]**|**Char [ \* ]**|  
|**Char [8001.. \* ;]**|**ntext**|  
|**character**|**char**|  
|**character varying**|**varchar**|  
|**carattere variabile [ \* .. 8000]**|**varchar [ \* ]**|  
|**variazione di caratteri [8001.. \* ]**|**ntext**|  
|**carattere [ \* .. 8000]**|**Char [ \* ]**|  
|**carattere [8001.. \* ]**|**ntext**|  
|**date**|**date**|  
|**datetime**|**datetime2 [3]**|  
|**Dec**|**decimal**|  
|**Dec [ \* .. \* ]**|**Decimal [ \* ]**|  
|**Dec [ \* .. \* ] [\*..\*]**|**Decimal [ \* ] [ \* ]**|  
|**decimal**|**decimal**|  
|**decimale [ \* .. \* ]**|**Decimal [ \* ]**|  
|**decimale [ \* .. \* ] [\*..\*]**|**Decimal [ \* ] [ \* ]**|  
|**precisione doppia**|**float [53]**|  
|**float**|**float [53]**|  
|**float [ \* .. 15**|**float [24]**|  
|**float [16.. \* ]**|**float [53]**|  
|**image**|**image**|  
|**int**|**int**|  
|**integer**|**int**|  
|**longsysname**|**nvarchar [255]**|  
|**money**|**money**|  
|**carattere nazionale**|**nchar**|  
|**National Char [ \* .. 4000]**|**nchar [ \* ]**|  
|**carattere nazionale varying**|**nvarchar**|  
|**carattere nazionale variabile [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**carattere nazionale variabile [4001.. \* ]**|**nvarchar(max)**|  
|**char nazionale [4001.. \* ]**|**nvarchar(max)**|  
|**carattere nazionale**|**nchar**|  
|**carattere nazionale [ \* .. 4000]**|**nchar [ \* ]**|  
|**carattere nazionale [4001.. \* ]**|**nvarchar(max)**|  
|**carattere nazionale variabile**|**nvarchar**|  
|**carattere nazionale variabile [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**carattere nazionale variabile [4001.. \* ]**|**nvarchar(max)**|  
|**varchar nazionale**|**nvarchar**|  
|**varchar nazionale [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**varchar nazionale [4001.. \* ]**|**nvarchar(max)**|  
|**nchar**|**nchar**|  
|**variabile nchar**|**nvarchar**|  
|**nchar varying [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**variabile nchar [4001.. \* ]**|**nvarchar(max)**|  
|**nchar [ \* .. 4000]**|**nchar [ \* ]**|  
|**nchar [4001.. \* ]**|**nvarchar(max)**|  
|**numeric**|**numeric**|  
|**numerico [ \* .. \* ]**|**numeric [ \* ]**|  
|**numerico [ \* .. \* ] [\*..\*]**|**numeric [ \* ] [ \* ]**|  
|**nvarchar**|**nvarchar**|  
|**nvarchar [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**nvarchar [4001.. \* ]**|**nvarchar(max)**|  
|**real**|**float [24]**|  
|**smalldatetime**|**smalldatetime**|  
|**smallint**|**smallint**|  
|**smallmoney**|**smallmoney**|  
|**sysname**|**nvarchar [128]**|  
|**sysname [ \* .. \* ]**|**nvarchar [255]**|  
|**text**|**text**|  
|**time**|**tempo [3]**|  
|**timestamp**|**rowversion**|  
|**tinyint**|**tinyint**|  
|**UNICHAR**|**nchar**|  
|**UNICHAR variabile**|**nvarchar**|  
|**UNICHAR variabile [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**UNICHAR variabile [4001.. \* ]**|**nvarchar(max)**|  
|**UNICHAR [ \* .. 4000]**|**nchar [ \* ]**|  
|**UNICHAR [4001.. \* ]**|**nvarchar(max)**|  
|**UNITEXT**|**nvarchar(max)**|  
|**univarchar**|**nvarchar**|  
|**univarchar [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**univarchar [4001.. \* ]**|**nvarchar(max)**|  
|**bigint senza segno**|**numerico [20] [0]**|  
|**int senza segno**|**bigint**|  
|**smallint senza segno**|**int**|  
|**tinyint senza segno**|**tinyint**|  
|**varbinary**|**varbinary**|  
|**varbinary [ \* .. 8000]**|**varbinary [ \* ]**|  
|**varbinary [8001.. \* ]**|**varbinary(max)**|  
|**varchar**|**varchar**|  
|**varchar [ \* .. 8000]**|**varchar [ \* ]**|  
|**varchar [8001.. \* ]**|**ntext**|  
  
