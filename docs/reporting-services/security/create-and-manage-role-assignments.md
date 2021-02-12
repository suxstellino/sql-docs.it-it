---
description: Creare e gestire assegnazioni di ruolo
title: Creare e gestire assegnazioni di ruolo | Microsoft Docs
ms.date: 05/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- removing role assignments
- roles [Reporting Services], assignments
- security [Reporting Services], role assignments
- modifying role assignments
- deleting role assignments
ms.assetid: 086d0987-b43c-4834-8372-e08fb4b432f8
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e1be919afaea2693329ae6fe957fb6fc5ff3357f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015201"
---
# <a name="create-and-manage-role-assignments"></a>Creare e gestire assegnazioni di ruolo

Un'*assegnazione di ruolo* è costituita dai criteri di sicurezza che determinano le autorizzazioni di un utente o un gruppo. Le autorizzazioni stabiliscono se l'utente o il gruppo può accedere o modificare un elemento specifico del server di report o eseguire un'attività. Un'assegnazione di ruolo è costituita da un unico nome di un account utente o di gruppo e da una o più definizioni di ruolo.

L'ambito delle assegnazioni di ruolo è definito a *livello di elemento* oppure a *livello di sistema*.

- Un'assegnazione di ruolo a livello di elemento viene creata per un elemento o un ramo specifico della gerarchia di cartelle nel server di report. Per creare un'assegnazione di ruolo per una cartella o un elemento specifico, è necessario accedere a tale elemento o cartella.

- Le assegnazioni di ruolo a livello di sistema consentono a utenti selezionati di eseguire attività che influiscono sull'intero sito del server di report. Queste attività includono:
  - Creazione di pianificazioni condivise
  - Gestione di processi
  - Elaborazione di report
  - Impostazione di proprietà

La sicurezza a livello di sistema non definisce l'accesso agli elementi della gerarchia di cartelle del server di report.

## <a name="creating-an-item-level-role-assignment"></a>Creazione di un'assegnazione di ruolo a livello di elemento

Da qui è possibile creare un'assegnazione di ruolo separata per ogni account utente o di gruppo che deve accedere al server di report. Se l'account fa parte di un dominio diverso da quello in cui si trova il server di report, è necessario specificare il nome di dominio. Dopo aver specificato un account, scegliere una o più definizioni di ruolo. Le definizioni di ruolo si sommano tra loro, ovvero l'assegnazione per un determinato utente o gruppo supporta la combinazione di tutte le attività delle varie definizioni.

Per consentire un accesso esteso, scegliere un elemento in posizione elevata nella gerarchia di cartelle, ad esempio la cartella radice Home. In seguito si potranno creare assegnazioni di ruolo per bloccare aree specifiche della gerarchia di cartelle.

Per creare un'assegnazione di ruolo, è necessario appartenere al gruppo locale Administrators nel server di report. È possibile delegare tale compito assegnando altri utenti al ruolo **Gestione contenuto** .

Per altre informazioni o per creare o gestire le assegnazioni di ruolo, vedere [Concedere l'accesso utente a un server di report](../../reporting-services/security/grant-user-access-to-a-report-server.md)
  
## <a name="creating-a-system-level-role-assignment"></a>Creazione di un'assegnazione di ruolo a livello di sistema

Le assegnazioni di ruolo a livello di sistema e di elemento sono associate. È necessario creare un'assegnazione di ruolo a livello di sistema per ogni utente o gruppo che ha un'assegnazione di ruolo a livello di elemento.

Le assegnazioni di ruolo a livello di sistema includono un'ampia gamma di autorizzazioni, ma non le autorizzazioni che fanno parte di un'assegnazione di ruolo a livello di elemento.

A differenza delle autorizzazioni di sistema in un computer, i ruoli a livello di sistema nei server di report non definiscono autorizzazioni globali che includono tutte le attività possibili, ma specificano solo un set di attività il cui ambito è il sito del server di report. Le assegnazioni di ruolo a livello di sistema determinano se gli utenti possono visualizzare le proprietà dell'applicazione, ad esempio l'immagine o il titolo della home page, visualizzare o gestire le pianificazioni condivise o usare Generatore report.

Per altre informazioni o per creare o gestire un'assegnazione di ruolo a livello di sistema, vedere [Concedere l'accesso utente a un server di report](../../reporting-services/security/grant-user-access-to-a-report-server.md) e [Ruoli predefiniti](../../reporting-services/security/role-definitions-predefined-roles.md).  

## <a name="modifying-a-role-assignment"></a>Modifica di assegnazioni di ruolo

Un'assegnazione di ruolo può essere modificata in qualsiasi momento. Le modifiche hanno effetto quando l'assegnazione viene salvata, ma non vengono applicate durante una sessione utente. Se si modifica un'assegnazione di ruolo per negare l'accesso mentre il report è aperto da un utente, l'utente può comunque continuare a usarlo per la sessione attiva.

Se si aggiunge un account utente a un gruppo che fa già parte di un'assegnazione di ruolo, l'account utente non potrà accedere immediatamente agli elementi dopo la modifica. Questo ritardo è dovuto alla memorizzazione nella cache dei token di autenticazione da parte di Internet Information Services (IIS). È possibile attendere che i token vengano aggiornati (in genere l'attesa è di 15 minuti) oppure reimpostare IIS per aggiornare immediatamente la cache.

È possibile modificare una sola assegnazione di ruolo alla volta. Non è possibile eseguire un'operazione di ricerca e sostituzione globale per modificare i nomi delle definizioni di ruolo o le impostazioni delle assegnazioni di ruolo né per trovare tutte le assegnazioni di ruolo che includono un utente o un gruppo specifico.

## <a name="deleting-a-role-assignment"></a>Eliminazione di assegnazioni di ruolo

È possibile eliminare un'assegnazione di ruolo selezionando la casella di controllo accanto all'assegnazione che si vuole eliminare e quindi facendo clic su **Elimina**. È inoltre possibile eliminare assegnazioni di ruolo facendo clic su **Ripristina sicurezza padre**. Quando si seleziona questo pulsante, le assegnazioni di ruolo esistenti per l'elemento vengono eliminate e sostituite con le assegnazioni ereditate dall'elemento padre.

## <a name="see-also"></a>Vedere anche

[Concedere l'accesso utente a un server di report](../../reporting-services/security/grant-user-access-to-a-report-server.md)  
[Assegnazioni di ruolo](../../reporting-services/security/role-assignments.md)  
[Definizioni di ruolo](../../reporting-services/security/role-definitions.md)  
[Predefined Roles](../../reporting-services/security/role-definitions-predefined-roles.md)  
[Concessione di autorizzazioni in un server di report in modalità nativa](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)
