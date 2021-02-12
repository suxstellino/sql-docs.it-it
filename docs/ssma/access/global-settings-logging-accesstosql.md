---
description: Impostazioni globali (registrazione) (AccessToSQL)
title: Impostazioni globali (registrazione) (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 835b09b5-eb42-47ea-b46e-e115d4d6461f
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d9a4c8ea82558598700f5ae18121335c4a181f6b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078852"
---
# <a name="global-settings-logging-accesstosql"></a>Impostazioni globali (registrazione) (AccessToSQL)
Utilizzare la finestra di dialogo **Impostazioni globali** per specificare le impostazioni di registrazione per SSMA. Queste impostazioni vengono in genere modificate solo quando si utilizza il supporto tecnico.  
  
Per accedere a questa finestra di dialogo, scegliere **Impostazioni globali** dal menu **strumenti** , quindi fare clic sul pulsante **registrazione** nella parte inferiore del riquadro sinistro.  
  
## <a name="options"></a>Opzioni  
**Livello messaggi**  
Le opzioni seguenti sono disponibili nel **livello messaggi**:  
  
|Opzione|Descrizione|  
|----------|---------------|  
|**[tutte le categorie]**|Utilizzato per impostare il livello di registrazione per tutte le opzioni seguenti.|  
|**Agente di raccolta**|Raccoglie i metadati relativi allo schema di origine e li salva nel progetto.|  
|**Converter**|Converte le strutture degli oggetti di database di origine, ad esempio tabelle e stored procedure, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] strutture corrispondenti.|  
|**Migrator dati**|Esegue la migrazione dei dati dal database di origine in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Formattatore**|Componente secondario del convertitore che genera script per lo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schema.|  
|**Interfaccia utente grafica**|Messaggi visualizzati quando si usa lo strumento SSMA.|  
|**Linker**|Risolve gli identificatori SQL e fornisce informazioni ad altri componenti.|  
|**Altri**|Tutti i messaggi che non sono in nessun'altra categoria.|  
|**Parser**|Analizza lo schema di origine.|  
|**Sincronizzazione**|Carica gli oggetti di database di origine in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**TreeConverter**|Converte gli oggetti nei metadati di origine in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadati.|  
  
Per ogni opzione in **livello messaggi** configurare uno dei seguenti livelli di registrazione per SSMA:  
  
|||  
|-|-|  
|**Errore irreversibile**|Consente di scrivere nel log solo messaggi di errore irreversibili.|  
|**Error (Errore) (Error (Errore)e)**|Scrivi messaggi di errore irreversibili e di errore nel log.|  
|**Avviso**|Scrivere messaggi di errore, di avviso e di errore irreversibile nel log.|  
|**Info**|Scrivere i messaggi di errore informativi, di avviso, di errore e di errore irreversibile nel log.|  
|**Eseguire il debug**|Scrivere nel log tutti i messaggi, inclusi i messaggi di debug.|  
  
**Percorso del file di log**  
Il percorso e il nome del file di log SSMA. Per specificare un nome diverso, fare clic sul percorso corrente, quindi fare clic sul pulsante Sfoglia (**...**).  
  
**Dimensioni del file di log**  
Dimensioni massime del file di log in KB. Le dimensioni minime sono pari a 10 KB. Le dimensioni predefinite sono pari a 10240 KB.  
  
**Numero totale di file di log**  
Quando un log si riempie, SSMA Rinomina il file di log e ne avvia uno nuovo. Utilizzando questa impostazione, specificare il numero massimo di file di log da contenere. Il valore minimo è 2.  
  
