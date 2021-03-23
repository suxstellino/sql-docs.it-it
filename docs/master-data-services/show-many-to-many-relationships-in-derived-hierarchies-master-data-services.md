---
description: Mostrare le relazioni molti-a-molti nelle gerarchie derivate (Master Data Services)
title: Mostrare le relazioni molti-a-molti nelle gerarchie derivate
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 8b2a9c43-40e0-48f7-a6a9-325beb9f27da
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ab2eaf50f73ae59bdde5cb40c75050bb0bc8206f
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833802"
---
# <a name="show-many-to-many-relationships-in-derived-hierarchies-master-data-services"></a>Mostrare le relazioni molti-a-molti nelle gerarchie derivate (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Le gerarchie derivate (DH), che già mostravano le relazioni uno-a-molti, ora possono mostrare anche le relazioni molti-a-molti.  
  
## <a name="many-to-many-m2m-relationships"></a>Relazioni molti-a-molti (M2M)  

Una relazione molti-a-molti (M2M) tra due entità può essere modellata usando una terza entità che stabilisce un mapping tra esse.  
  
![mds_hierarchies_manytomany](../master-data-services/media/mds-hierarchies-manytomany.png "mds_hierarchies_manytomany")  
  
Nell'esempio precedente c'è una relazione M2M fra le entità **Dipendente** e **CorsoFormazione** , fornita dall'entità di mapping **RegistrazioneCorso**. Un dipendente può essere registrato come studente in più corsi e ogni corso può includere più studenti.  
  
È possibile creare una gerarchia derivata che Visualizza, ad esempio, gli studenti per classe oppure invertire la relazione e mostrare le classi raggruppate per studente.  

> [!NOTE]
> [!INCLUDE [sssql16-md](../includes/sssql16-md.md)] è stata introdotta la gerarchia derivata per le relazioni M2M. Questa funzionalità non era disponibile prima di tale versione.
  
Per prima cosa, passare alla pagina di gestione della gerarchia derivata e creare una nuova gerarchia derivata:  
  
 ![mds_hierarchies_add_derived_hierarchy](../master-data-services/media/mds-hierarchies-add-derived-hierarchy.png "mds_hierarchies_add_derived_hierarchy")  
  
 Aggiungere quindi i livelli alla nuova gerarchia derivata, iniziando dal basso verso l'alto. In questo esempio si desidera mostrare gli studenti (dipendenti) raggruppati per corso. L'entità **Dipendente** è perciò il livello foglia della gerarchia e viene aggiunta per prima:  
  
 ![mds_hierarchies_edit_derived_hierarchy_one](../master-data-services/media/mds-hierarchies-edit-derived-hierarchy-one.PNG "mds_hierarchies_edit_derived_hierarchy_one")  
  
 Nella schermata precedente si noti che l'entità **Dipendente** appare sotto **Livelli correnti** al centro come unico livello. L' **Anteprima** della gerarchia derivata a destra mostra semplicemente un elenco di tutti i membri dell'entità **Employee** . La sezione **Livelli disponibili** a sinistra mostra i livelli che possono essere aggiunti sopra il livello corrente (**Dipendente**). Molti di questi sono attributi basati su dominio (DBA) per l'entità **Dipendente** , incluso il DBA **Reparto** .  
  
 A partire da [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)], esiste un nuovo tipo di livello che modella le relazioni M2M, ad esempio: **Corso (mappato mediante RegistrazioneCorso.Studente)**. Il nome del livello è più dettagliato rispetto degli altri, in modo da riflettere le informazioni aggiuntive necessarie per descrivere univocamente la relazione di mapping. Trascinare il livello fino al livello **Dipendente** nella sezione **Livelli correnti** :  
  
 ![mds_hierarchies_edit_derived_hierarchy_two](../master-data-services/media/mds-hierarchies-edit-derived-hierarchy-two.PNG "mds_hierarchies_edit_derived_hierarchy_two")  
  
 A questo punto l'anteprima mostra i dipendenti raggruppati in base ai corsi di formazione per i quali sono registrati. Poiché si tratta di una relazione M2M, ogni membro figlio può avere più elementi padre. Nell'esempio precedente il dipendente **6 {Hillman, Reinout N}** è registrato come studente in due corsi, **1 {Master Data Services 101}** e **4 {Career-Limiting Moves}**.  
  
 Questa relazione di mapping può essere visualizzata anche invertita, raggruppando i corsi per studente:  
  
 ![mds_hierarchies_available_entities_and_hierarchies](../master-data-services/media/mds-hierarchies-available-entities-and-hierarchies.PNG "mds_hierarchies_available_entities_and_hierarchies")  
  
 Ancora una volta è possibile notare che un elemento figlio può apparire sotto più elementi padre: corso di formazione **1 {Master Data Services 101}** appare sia sotto **6 {Hillman, Reinout N}** sia sotto **40 {Ford, Jeffrey L}**.  
  
 I membri dell'entità di mapping **registrazionecorso** non vengono visualizzati in un punto qualsiasi all'interno della gerarchia derivata. Vengono usati semplicemente per definire le relazioni tra i membri padre e figlio nella gerarchia.  
  
 È possibile modificare la relazione M2M cambiando i membri dell'entità di mapping. Procedere in uno dei modi seguenti. La relazione M2M è di sola lettura nella pagina **Esplora gerarchia derivata** .  
  
-   Modificare i membri dell'entità di mapping nella pagina **Esplora entità** mediante il componente aggiuntivo Master Data Services per Excel o mediante la gestione temporanea dei dati.  
  
-   Trascinare i nodi figlio tra gli elementi padre nella pagina **Esplora gerarchia derivata** .  
  
     Questo metodo consente di modificare i membri esistenti quando possibile e aggiungere nuovi membri quando necessario. I membri esistenti non vengono eliminati.  
  
     Ad esempio, con l'entità di mapping RegistrazioneCorso, quando si sposta uno studente al nodo inutilizzato, il valore dell'attributo class (corso) del corrispondente membro dell'entità di mapping viene modificato in null e il membro non viene eliminato. Al contrario, quando si sposta uno studente dal nodo inutilizzato a uno dei corsi, se in quel corso è presente un membro del mapping esistente corrispondente allo studente per il quale il corso è null, tale membro viene modificato mediante la modifica del corso da null al nuovo elemento padre. Se tale membro non è presente, ne viene aggiunto uno.  
  
     Questa procedura evita l'eliminazione dei membri per impedire l'eliminazione indesiderata di altri dati utente, ad esempio se l'entità di mapping contiene altri attributi oltre ai due che definiscono la relazione padre-figlio. Gli utenti devono eseguire le eliminazioni in modo esplicito direttamente sull'entità di mapping.  
  
 Il nuovo livello M2M può trovarsi in qualsiasi punto all'interno di una gerarchia derivata che sia consentito un livello di attributo basato su dominio (DBA). Un livello M2M può trovarsi nella parte superiore, come negli esempi precedenti. Può trovarsi sopra e/o sotto un livello DBA, inclusi i livelli ricorsivi. Può trovarsi sotto un livello estremità di una gerarchia esplicita (deprecata). Più relazioni M2M possono essere concatenate nella stessa gerarchia derivata.  
  
 I livelli M2M possono essere nascosti, proprio come gli altri livelli di gerarchia derivata.  
   
### <a name="m2m-relationship-in-sample-model"></a><a name="M2MSample"></a> Relazione molti-a-molti (M2M) nel modello di esempio  
Per una dimostrazione di una relazione molti-a-molti, visualizzare la gerarchia derivata Region Climate del modello di esempio Customer incluso in [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)].   
  
Come mostra l'immagine seguente, il nome del livello che modella questa relazione è ![mds_Number1](../master-data-services/media/mds-number1.png)**Climate (mappato attraverso RegionClimate.Region)**. ![mds_Number2](../master-data-services/media/mds-number2.png)**Preview** mostra le regioni raggruppate in base ai tipi di clima a cui sono associate. Questa è una relazione molti-a-molti poiché include regioni (membri figlio) associate a più climi (membri padre). Ad esempio, ![mds_Number3](../master-data-services/media/mds-number3.png)**APCR {Asia Pacifico}** è associata a ![mds_Number4](../master-data-services/media/mds-number4.png)**A {Tropical}** e ![mds_Number5](../master-data-services/media/mds-number5.png)**B {Dry}**.  
  
![mds_M2MRelationship_Example_CustomerModel](../master-data-services/media/mds-m2mrelationship-example-customermodel.png)  
  
Per istruzioni sulla distribuzione del modello di esempio Customer e di altri modelli di esempio inclusi in [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)], vedere [Distribuzione di modelli di esempio e dati](~/master-data-services/sql-server-samples-model-deployment-packages-mds.md).   
  
## <a name="one-many-relationship"></a>Relazione uno-a-molti  
 Un membro di una gerarchia derivata può essere padre di molti membri figlio, ma non può avere in genere più di un padre (per le eccezioni, vedere [Sicurezza dei membri](#bkmk_member_security)). Si supponga ad esempio che ci siano due entità, Dipendente e Reparto, in cui ogni dipendente appartiene a un singolo reparto. Questa relazione viene modellata aggiungendo all'entità Dipendente un attributo basato su dominio (DBA) che fa riferimento all'entità Reparto:  
  
 ![mds_hierarchies_onetomany](../master-data-services/media/mds-hierarchies-onetomany.png "mds_hierarchies_onetomany")  
  
 Si tratta di una relazione uno-a-molti in quanto ogni dipendente appartiene a un solo reparto ma ogni reparto può avere più dipendenti. È possibile creare una gerarchia derivata che Visualizza i dipendenti raggruppati per reparto:  
  
 ![mds_hierarchies_dh_screenshot](../master-data-services/media/mds-hierarchies-dh-screenshot.png "mds_hierarchies_dh_screenshot")  
  
##  <a name="member-security"></a><a name="bkmk_member_security"></a> Sicurezza dei membri  
 Una gerarchia che consente la duplicazione dei membri (consente che un membro abbia più di un elemento padre) non può essere usata per assegnare autorizzazioni di sicurezza ai membri. Ad esempio:  
  
-   Una gerarchia derivata ricorsiva (RDH) che non esegue l'ancoraggio delle ricorsioni null (ogni membro al livello ricorsivo appare sia sotto la radice che nel relativo elemento padre ricorsivo).  
  
-   Una gerarchia derivata ricorsiva con un livello al di sopra del livello ricorsivo (ogni membro del livello ricorsivo appare sia sotto l'elemento padre non ricorsivo che nel relativo elemento padre ricorsivo).  
  
-   Una gerarchia derivata con un livello M2M (un elemento figlio può essere mappato a molti elementi padre).  
  
## <a name="collections"></a>Raccolte  
 Le raccolte e le gerarchie esplicite sono deprecate. La stored procedure di conversione (udpConvertCollectionAndConsolidatedMembersToLeaf) converte i membri della raccolta in membri foglia e crea gerarchie derivate molti-a-molti per acquisire informazioni sui membri della raccolta.  
  
## <a name="see-also"></a>Vedere anche  
 [Gerarchie derivate &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
