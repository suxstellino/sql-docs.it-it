---
description: Creare un attributo di file (Master Data Services)
title: Creare un attributo di file
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- creating file attributes [Master Data Services]
- attributes [Master Data Services], creating file attributes
ms.assetid: d224886b-2ef1-4658-8b01-2213cc4b8df6
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 25023eb11f9968f9edc3e7159f32252b0a116291
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272682"
---
# <a name="create-a-file-attribute-master-data-services"></a>Creare un attributo di file (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]creare un attributo di file per popolare i valori di attributo con i file.

## <a name="prerequisites"></a>Prerequisiti
 Per eseguire questa procedura:

-   È necessario disporre di autorizzazione per accedere all'area funzionale **Amministrazione sistema** .

-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).

-   È necessario che sia presente un'entità perché ne venga creato l'attributo. Per altre informazioni, vedere [Creare un'entità &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).

## <a name="attribute-information"></a>Informazioni sugli attributi
 Per ogni indice creato, viene aggiunta alla griglia una riga con sette colonne. La tabella seguente descrive le colonne.

|Colonna|Descrizione|
|------------|-----------------|
|Stato|Stato dell'attributo.<br /><br /> Quando si fa clic su Salva, viene visualizzata l' ![icona di aggiornamento dell'immagine di stato](../master-data-services/media/mds-statusicon-updating.png "Icona per lo stato di aggiornamento") , che indica che l'attributo è in fase di aggiornamento.<br /><br /> Se si verificano errori durante la creazione o la modifica di un attributo, viene visualizzata l'immagine ![icona di stato errore](../master-data-services/media/mds-statusicon-error.png "Icona per lo stato di errore") .<br /><br /> In caso contrario, lo stato è OK e viene visualizzata l'immagine ![icona di stato OK](../master-data-services/media/mds-statusicon-ok.png "Icona per lo stato OK") .|
|Nome|Nome dell'attributo.|
|Nome visualizzato|Nome visualizzato dell'attributo.|
|Descrizione|Descrizione dell'attributo.|
|Larghezza in pixel visualizzazione|Larghezza dell'attributo.|
|Tipo e proprietà|Informazioni sul tipo e sul tipo di dati dell'attributo.|
|Abilita rilevamento modifiche|Specifica se l'attributo è abilitato per il rilevamento delle modifiche e visualizza tra parentesi il numero del gruppo.|

 Quando si fa clic su un attributo vengono visualizzate le informazioni seguenti.

-   **Creato da**: nome dell'utente che ha creato l'attributo.

-   **Il**: data e ora di creazione dell'attributo.

-   **Aggiornato da**: nome dell'ultimo utente che ha aggiornato l'attributo.

-   **Il**: data e ora dell'ultimo aggiornamento dell'attributo.

### <a name="to-create-a-file-attribute"></a>Per creare un attributo di file

1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Amministrazione sistema**.

2.  Nella pagina **Gestisci modello** selezionare un modello dalla griglia e quindi fare clic su **entità**.

3.  Nella pagina **Gestisci entità** selezionare la riga dell'entità per la quale si vuole creare un attributo.

4.  Fare clic su **Attributi**.

5.  Nella pagina **Gestisci attributi** eseguire una di queste operazioni, quindi fare clic su **Aggiungi**.

    -   Se l'attributo è per i membri foglia, selezionare **Foglia** nella casella di riepilogo **Tipo di membro** .

    -   Se l'attributo è per i membri consolidati, selezionare **Consolidato** nella casella di riepilogo **Tipo di membro** .

    -   Se l'attributo è per le raccolte, selezionare **Raccolta** nella casella di riepilogo **Tipo di membro** .

6.  Nella casella **Nome** digitare un nome per l'attributo. Per un elenco di parole che non devono essere usate come nomi di attributo, vedere [parole riservate &#40;Master Data Services&#41;](../master-data-services/reserved-words-master-data-services.md).

7.  Facoltativamente, digitare un nome visualizzato e specificare una descrizione per l'attributo nella casella **Descrizione** .

8.  Nella casella **Larghezza in pixel visualizzazione** digitare la larghezza della colonna attributo da visualizzare nella griglia **Esplora** .

9. Nell'elenco **Tipo di attributo** selezionare **File**.

10. Nell'elenco **Estensione file** selezionare un tipo di file che l'utente può caricare o lasciare il valore predefinito (*.\*) per consentire tutti i tipi di file.

11. Facoltativamente, selezionare **Abilita rilevamento modifiche** per tenere traccia delle modifiche a gruppi di attributi. Per altre informazioni, vedere [Aggiungere attributi ad un gruppo rilevamento modifiche &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).

12. Fare clic su **Salva**.

## <a name="see-also"></a>Vedere anche
 [Attributi &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md) [modificare un nome di attributo e un tipo di dati &#40;Master Data Services](../master-data-services/change-an-attribute-name-and-data-type-master-data-services.md)&#41;[creare un Domain-Based attributo](../master-data-services/create-a-domain-based-attribute-master-data-services.md) &#40;Master Data Services&#41;[creare un attributo di testo &#40;](../master-data-services/create-a-text-attribute-master-data-services.md) Master Data Services&#41;


