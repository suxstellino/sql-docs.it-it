---
description: Impostazioni del progetto (sincronizzazione) (SybaseToSQL)
title: Impostazioni progetto (sincronizzazione) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2cd6bc01-b8e5-4312-83a4-eac66dc1d460
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a5db262979e7c7e267491e34b54302c0a5b36999
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070516"
---
# <a name="project-settings-synchronization-sybasetosql"></a>Impostazioni del progetto (sincronizzazione) (SybaseToSQL)
La pagina sincronizzazione della finestra di dialogo **Impostazioni progetto** contiene impostazioni che consentono di personalizzare il modo in cui SSMA carica gli oggetti di database, ad esempio tabelle e stored procedure, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
È possibile accedere a due diverse pagine di sincronizzazione che contengono le stesse impostazioni:  
  
-   Se si desidera specificare le impostazioni per tutti i progetti SSMA futuri, nel menu **strumenti** selezionare **Impostazioni progetto predefinite**, selezionare il tipo di progetto di migrazione per il quale è necessario visualizzare o modificare le impostazioni dall'elenco a discesa **versione destinazione migrazione** e quindi selezionare **sincronizzazione** nella parte inferiore del riquadro sinistro.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , quindi selezionare **sincronizzazione** nella parte inferiore del riquadro sinistro.  
  
## <a name="options"></a>Opzioni  
**Tentativi**  
Specifica il numero di tentativi che SSMA deve eseguire quando carica gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Gli oggetti che non vengono caricati nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tentativo corrente verranno ritentati fino a quando SSMA non raggiunge il numero massimo di tentativi nel processo di sincronizzazione corrente.  
  
## <a name="synchronization-for-sql-server"></a>Sincronizzazione per SQL Server  
**Aggiorna oggetto locale sulla modifica di un oggetto locale e remoto**  
Specifica se SSMA deve sostituire i metadati degli oggetti locali con i metadati degli oggetti remoti se gli oggetti locali e remoti cambiano.  
Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
Se si seleziona **Scrivi nel database**, SSMA aggiornerà gli oggetti nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.   
Il set di opzioni predefinito è **Write nel database.**  
  
**Aggiorna oggetto locale sulla modifica di un oggetto locale**  
Specifica se SSMA deve sostituire i metadati degli oggetti locali con i metadati degli oggetti remoti se l'oggetto remoto viene modificato.  
Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
Se si seleziona **Scrivi nel database**, SSMA aggiornerà l'oggetto nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.   
Il set di opzioni predefinito è **Write nel database**.  
  
**Aggiorna oggetto locale per la modifica dell'oggetto remoto**  
Specifica se SSMA deve sostituire i metadati degli oggetti locali con i metadati degli oggetti remoti se l'oggetto remoto viene modificato.  
Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
Se si seleziona **Scrivi nel database**, SSMA aggiornerà l'oggetto nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.   
Il set di opzioni predefinito è **Refresh from database**.  
  
**Aggiorna quando mancano i metadati degli oggetti locali**  
Specifica se SSMA deve creare i metadati locali se un oggetto esiste nel database di destinazione, ma non in locale.  
Se si seleziona **Aggiorna da database**, SSMA seleziona l'opzione Aggiorna da database quando viene soddisfatta la condizione.  
Se si seleziona **Scrivi nel database**, SSMA eliminerà l'oggetto dal database quando viene soddisfatta la condizione.  
Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.   
Il set di opzioni predefinito è **Refresh from database**.  
  
