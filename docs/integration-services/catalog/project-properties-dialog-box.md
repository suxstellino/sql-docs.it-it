---
description: Finestra di dialogo Proprietà progetto
title: Finestra di dialogo Proprietà progetto | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.isprojectprop.general.f1
- sql13.ssis.ssms.isprojectprop.permissions.f1
ms.assetid: d5cf52f5-1fe2-438a-98a3-fe117360acf8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a1be2d0dc581e67db28be7f5c1b96efc6850e609
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349161"
---
# <a name="project-properties-dialog-box"></a>Finestra di dialogo Proprietà progetto

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Un progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è un'unità di distribuzione. In ogni progetto possono essere inclusi pacchetti, parametri e riferimenti all'ambiente. Un progetto è un oggetto a protezione diretta in cui è possibile definire autorizzazioni per le entità di database. Quando un progetto viene ridistribuito, la versione precedente può essere archiviata nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 I parametri del progetto e quelli del pacchetto possono essere utilizzati per assegnare valori alle proprietà dei pacchetti durante la fase di esecuzione. In alcuni parametri devono essere presenti valori prima che il pacchetto possa essere eseguito. Per i valori di parametri che fanno riferimento alle variabili di ambiente è necessario che il progetto disponga del riferimento all'ambiente corrispondente prima dell'esecuzione.  
  
 **Per saperne di più**  
  
-   [Aprire la finestra di dialogo Proprietà progetto](#open_dialog)  
  
-   [Impostare le opzioni nella pagina Generale](#general)  
  
-   [Impostare le opzioni nella pagina Autorizzazioni](#permissions)  
  
##  <a name="open-the-project-properties-dialog-box"></a><a name="open_dialog"></a> Aprire la finestra di dialogo Proprietà progetto  
  
1.  In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]connettersi al server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Verrà eseguita la connessione all'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] in cui è ospitato il database SSISDB.  
  
2.  In Esplora oggetti espandere l'albero per visualizzare il nodo dei **cataloghi di Integration Services** .  
  
3.  Espandere il nodo **SSISDB** .  
  
4.  Espandere la cartella contenente il progetto per cui si desidera impostare le proprietà.  
  
5.  Fare clic con il pulsante destro del mouse sul progetto e quindi scegliere **Proprietà**.  
  
##  <a name="set-the-options-on-the-general-page"></a><a name="general"></a> Impostare le opzioni nella pagina Generale  
 Utilizzare la pagina Generale per visualizzare le proprietà del progetto.  
  
 **Nome**  
 Viene elencato il nome del progetto.  
  
 **Identificatore**  
 Viene elencato l'ID del progetto.  
  
 **Descrizione**  
 Viene visualizzata la descrizione facoltativa del progetto.  
  
 **Versione progetto**  
 Viene elencata la versione del progetto.  
  
 **Data di distribuzione**  
 Vengono elencate la data e l'ora di distribuzione o ridistribuzione del progetto.  
  
##  <a name="set-the-options-on-the-permissions-page"></a><a name="permissions"></a> Impostare le opzioni nella pagina Autorizzazioni  
 Utilizzare la pagina **Autorizzazioni** per visualizzare e impostare le autorizzazioni esplicite per il progetto.  
  
 Esplora  
 Fare clic su **Sfoglia** per selezionare gli utenti e i ruoli per cui si vuole impostare le autorizzazioni, usando la finestra di dialogo **Sfoglia tutte le entità** .  
  
 **Nome**  
 Viene elencato il nome dell'utente o del ruolo.  
  
 **Tipo**  
 Viene elencato il tipo di utente o ruolo.  
  
 **Autorizzazione**  
 Vengono elencare le autorizzazioni.  
  
 **Utente che concede le autorizzazioni**  
 Viene elencato l'utente o il ruolo tramite cui viene concessa l'autorizzazione.  
  
 **Concedi**  
 Se l'opzione **Concedi** è selezionata, l'autorizzazione viene concessa per l'utente o il ruolo selezionato.  
  
 **Nega**  
 Se l'opzione **Nega** è selezionata, l'autorizzazione viene negata all'utente o al ruolo selezionato.  
  
  
