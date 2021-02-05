---
description: Ripristinare i word breaker usati dalla ricerca alla versione precedente (ricerca di SQL Server)
title: Ripristinare i word breaker usati dalla ricerca alla versione precedente
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 29b4488e-4c6a-4bf0-a64d-19e2fdafa7ae
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: f99e5892f697b06bcafcfc35f9672d8ee474bc40
ms.sourcegitcommit: 10ae200635b9e8554e6bc6f658125e1a80d4d5ae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2021
ms.locfileid: "99589297"
---
# <a name="revert-word-breakers-used-by-search-to-previous-version-sql-server-search"></a>Ripristinare i word breaker usati dalla ricerca alla versione precedente (ricerca di SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] viene installata e abilitata una versione dei word breaker e degli stemmer per tutte le lingue supportate dalla ricerca full-text, a eccezione del coreano. In questo articolo viene descritto come passare da questa versione dei componenti alla versione precedente o come tornare alla nuova versione dalla versione precedente.  
  
 In questo articolo non vengono prese in considerazione le lingue seguenti:  
  
- **Inglese**. Per ripristinare i componenti per la lingua inglese, vedere [Modifica del word breaker utilizzato per le lingue Inglese (Stati Uniti) e Inglese (Regno Unito)](../../relational-databases/search/change-the-word-breaker-used-for-us-english-and-uk-english.md).  
  
- **Danese, polacco e turco**. I word breaker di terze parti per il danese, il polacco e il turco inclusi con le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono stati sostituiti con i componenti [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
- **Ceco e greco**. Sono disponibili word breaker nuovi per il ceco e il greco. Nelle versioni precedenti della ricerca full-text di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è incluso il supporto per queste due lingue.  
  
- **Coreano**. Il word breaker e lo stemmer per il coreano non sono aggiornati in questa versione.  
  
 Per informazioni generali su word breaker e stemmer, vedere [Configurazione e gestione di word breaker e stemmer per la ricerca](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
##  <a name="overview-of-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="overview"></a> Panoramica del ripristino di word breaker e stemmer  
 Le istruzioni relative al ripristino di word breaker e stemmer variano a seconda della lingua. Nella tabella seguente vengono riepilogati i tre set di azioni che potrebbe essere necessario eseguire per ripristinare la versione precedente dei componenti.  
  
|File corrente|File precedente|Numero di lingue interessate|Azione per i file|Azione per le voci del Registro di sistema|  
|------------------|-------------------|----------------------------------|----------------------|---------------------------------|  
|NaturalLanguage6.dll|NaturalLanguage6.dll|34|Ottenere e installare una versione precedente di NaturalLanguage6.dll, sovrascrivendo la versione corrente del file.|Non è necessaria alcuna azione.<br /><br /> I valori e le chiavi del Registro di sistema non sono cambiate per questa versione.|  
|(Altro nome file)|NaturalLanguage6.dll|5|Ottenere e installare una versione precedente di NaturalLanguage6.dll, sovrascrivendo la versione corrente del file.|Modificare un set di voci del Registro di sistema per specificare la versione precedente dei componenti.|  
|(Altro nome file)|(Altro nome file)|6|Non è necessaria alcuna azione.<br /><br /> [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] vengono copiate entrambe le versioni precedente e corrente dei componenti nella cartella Binn.|Modificare un set di voci del Registro di sistema per specificare la versione precedente dei componenti.|  
  
> [!WARNING]  
>  Se si sostituisce la versione corrente del file NaturalLanguage6.dll con una versione diversa, viene modificato il comportamento di tutte le lingue che utilizzano questo file.  
  
 I file descritti in questo articolo sono file DLL installati nella cartella `MSSQL\Binn` per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il percorso completo è in genere il seguente:  
  
 `C:\Program Files\Microsoft SQL Server\<instance>\MSSQL\Binn`  
  
##  <a name="languages-for-which-the-file-name-of-both-the-current-and-previous-word-breaker-is-naturallanguage6dll"></a><a name="nl6nl6"></a> Lingue per le quali il nome file del word breaker corrente e precedente è NaturalLanguage6.dll  
 Per le lingue elencate nella tabella seguente, il nome file del word breaker corrente e precedente è NaturalLanguage6.dll. Per ripristinare questi componenti, è necessario sovrascrivere NaturalLanguage6.dll con una versione diversa dello stesso file. Non è necessario modificare le voci del Registro di sistema perché per questa versione non sono cambiate.  
  
> [!WARNING]  
>  Se si sostituisce la versione corrente del file NaturalLanguage6.dll con una versione diversa, viene modificato il comportamento di tutte le lingue che utilizzano questo file.  
  
 **Elenco delle lingue interessate**  
  
|Linguaggio|Abbreviazione<br />utilizzata nel<br />Registro di sistema|LCID|  
|--------------|---------------------------------------|----------|  
|Bengalese|`ben`|1093|  
|Bulgaro|`bgr`|1026|  
|Catalano|`cat`|1027|  
|Spagnolo|`esn`|3082|  
|Francese|`fra`|1036|  
|Gujarati|`guj`|1095|  
|Ebraico|`heb`|1037|  
|Hindi|`hin`|1081|  
|Croato|`hrv`|1050|  
|Indonesiano|`ind`|1057|  
|Islandese|`isl`|1039|  
|Italiano|`ita`|1040|  
|Kannada|`kan`|1099|  
|Lituano|`lth`|1063|  
|Lettone|`lvi`|1062|  
|Malayalam|`mal`|1100|  
|Marathi|`mar`|1102|  
|Malese|`msl`|1086|  
|Neutralità|`Neutral`|0000|  
|Norvegese Bokmål|`nor`|1044|  
|Punjabi|`pan`|1094|  
|Portoghese (Brasile)|`ptb`|1046|  
|Portoghese|`ptg`|2070|  
|Romeno|`rom`|1048|  
|Slovacco|`sky`|1051|  
|Sloveno|`slv`|1060|  
|Serbo - Alfabeto cirillico|`srb`|3098|  
|Serbo - Alfabeto latino|`srl`|2074|  
|Svedese|`sve`|1053|  
|Tamil|`tam`|1097|  
|Telugu|`tel`|1098|  
|Ucraino|`ukr`|1058|  
|Urdu|`urd`|1056|  
|Vietnamita|`vit`|1066|  
  
 La tabella precedente è ordinata alfabeticamente in base alla colonna Abbreviazione.  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="nl6nl6revert"></a> Per ripristinare i componenti precedenti  
  
1.  Spostarsi sulla cartella Binn descritta in precedenza.  
  
2.  Eseguire il backup della versione [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] di NaturalLanguage6.dll in un altro percorso.  
  
3.  Copiare la versione precedente di NaturalLanguage6.dll dalla cartella Binn di un'istanza di [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] o [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] nella cartella Binn dell'istanza di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    > [!WARNING]  
    >  Questa modifica interessa tutte le lingue che utilizzano NaturalLanguage6.dll sia nella versione corrente che in quella precedente.  
  
4.  Riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

###  <a name="to-restore-the-current-components"></a><a name="nl6nl6restore"></a> Per ripristinare i componenti correnti  
  
1.  Spostarsi sul percorso in cui si è eseguito il backup della versione [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] di NaturalLanguage6.dll.  
  
2.  Copiare la versione corrente di NaturalLanguage6.dll dal percorso di backup nella cartella Binn dell'istanza di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    > [!WARNING]  
    >  Questa modifica interessa tutte le lingue che utilizzano NaturalLanguage6.dll sia nella versione corrente che in quella precedente.  
  
3.  Riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="languages-for-which-the-file-name-of-the-previous-word-breaker-only-is-naturallanguage6dll"></a><a name="newnl6"></a> Lingue per le quali il nome file del word breaker precedente è solo NaturalLanguage6.dll  
 Per le lingue elencate nella tabella seguente, il nome file del word breaker precedente è diverso da quello della nuova versione. Il nome file precedente è NaturalLanguage6.dll. Per ripristinare la versione precedente, è necessario sovrascrivere la versione corrente di NaturalLanguage6.dll con una versione precedente dello stesso file. È inoltre necessario modificare un set di voci del Registro di sistema per specificare la versione precedente o corrente dei componenti.  
  
> [!WARNING]  
>  Se si sostituisce la versione corrente del file NaturalLanguage6.dll con una versione diversa, viene modificato il comportamento di tutte le lingue che utilizzano questo file.  
  
 **Elenco delle lingue interessate**  
  
|Linguaggio|Abbreviazione<br />utilizzata nel<br />Registro di sistema|LCID|  
|--------------|---------------------------------------|----------|  
|Arabo|ara|1025|  
|Tedesco|deu|1031|  
|Giapponese|jpn|1041|  
|Olandese|nld|1043|  
|Russo|rus|1049|  
  
 La tabella precedente è ordinata alfabeticamente in base alla colonna Abbreviazione.  
  
 Utilizzare le istruzioni seguenti con l'elenco di valori nella sezione [Nomi file e valori del Registro di sistema per il ripristino di word breaker e stemmer](#newnl6values).  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="newnl6revert"></a> Per ripristinare i componenti precedenti  
  
1.  Spostarsi sulla cartella Binn descritta in precedenza.  
  
2.  Non rimuovere i file per la versione corrente dei componenti dalla cartella Binn.  
  
3.  Eseguire il backup della versione [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] di NaturalLanguage6.dll in un altro percorso.  
  
4.  Copiare la versione precedente di NaturalLanguage6.dll dalla cartella contenitori di un'istanza di [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] o [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] nella cartella contenitori della nuova [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] istanza di.  
  
    > [!WARNING]  
    >  Questa modifica interessa tutte le lingue che utilizzano NaturalLanguage6.dll sia nella versione corrente che in quella precedente.  
  
5.  Nel Registro di sistema passare al nodo seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
6.  Utilizzare i passaggi seguenti per aggiungere nuove chiavi per i ClassID COM per le interfacce del word breaker e dello stemmer precedenti per la lingua selezionata:  
  
    1.  Aggiungere una nuova chiave con il valore della tabella per il word breaker precedente.  
  
    2.  Aggiornare i dati (predefiniti) del valore della chiave al nome file del word breaker precedente riportato nella tabella.  
  
    3.  Se la lingua selezionata utilizza uno stemmer, aggiungere una nuova chiave con il valore riportato nella tabella per lo stemmer precedente.  
  
    4.  Se la lingua selezionata utilizza uno stemmer, aggiornare i dati (predefiniti) del valore di quella chiave al nome file dello stemmer precedente riportato nella tabella.  
  
7.  Nel Registro di sistema passare al nodo seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<language_key>* rappresenta l'abbreviazione per la lingua utilizzata nel Registro di sistema, ad esempio "fra" per il francese e "esn" per lo spagnolo.  
  
8.  Aggiornare il valore della chiave **WBreakerClass** al valore riportato nella tabella per il word breaker corrente.  
  
9. Se la lingua selezionata utilizza uno stemmer, aggiornare il valore della chiave **StemmerClass** al valore riportato nella tabella per lo stemmer corrente.  
  
10. Riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="to-restore-the-current-components"></a><a name="newnl6restore"></a> Per ripristinare i componenti correnti  
  
1.  Spostarsi sul percorso in cui si è eseguito il backup della versione [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] di NaturalLanguage6.dll.  
  
2.  Copiare la versione corrente di NaturalLanguage6.dll dal percorso di backup nella cartella Binn dell'istanza di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    > [!WARNING]  
    >  Questa modifica interessa tutte le lingue che utilizzano NaturalLanguage6.dll sia nella versione corrente che in quella precedente.  
  
3.  Nel Registro di sistema passare al nodo seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
4.  Se le chiavi seguenti non esistono, utilizzare i passaggi seguenti per aggiungere nuovi chiavi per i ClassID COM per le interfacce del word breaker e dello stemmer correnti per la lingua selezionata:  
  
    1.  Aggiungere una nuova chiave con il valore riportato nella tabella per il word breaker corrente.  
  
    2.  Aggiornare i dati (predefiniti) del valore della chiave al nome file del word breaker corrente riportato nella tabella.  
  
    3.  Se la lingua selezionata utilizza uno stemmer, aggiungere una nuova chiave con il valore riportato nella tabella per lo stemmer corrente.  
  
    4.  Se la lingua selezionata utilizza uno stemmer, aggiornare i dati (predefiniti) del valore di quella chiave al nome file dello stemmer corrente riportato nella tabella.  
  
5.  Nel Registro di sistema passare al nodo seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<language_key>* rappresenta l'abbreviazione per la lingua utilizzata nel Registro di sistema, ad esempio "fra" per il francese e "esn" per lo spagnolo.  
  
6.  Aggiornare il valore della chiave **WBreakerClass** al valore riportato nella tabella per il word breaker precedente.  
  
7.  Se la lingua selezionata utilizza uno stemmer, aggiornare il valore della chiave **StemmerClass** al valore riportato nella tabella per lo stemmer precedente.  
  
8.  Riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="file-names-and-registry-values-for-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="newnl6values"></a> Nomi file e valori del Registro di sistema per il ripristino di word breaker e stemmer  
 Utilizzare l'elenco seguente di nomi file e di voci del Registro di sistema con le istruzioni riportate nella sezione precedente. Utilizzare i valori precedenti per ripristinare la versione precedente o utilizzare i valori correnti per ripristinare la versione corrente dei componenti.  
  
 Gli elementi seguenti sono elencati alfabeticamente in base all'abbreviazione utilizzata per ogni lingua.  
  
 **Arabo (ara), LCID 1025**  
  
|Componente|Word breaker|Stemmer|  
|---------------|------------------|-------------|  
|CLSID precedente|7EFD3C7E-9E4B-4a93-9503-DECD74C0AC6D|483B0283-25DB-4c92-9C15-A65925CB95CE|  
|Nome file precedente|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID corrente|04b37e30-c9a9-4a7d-8f20-792fc87ddf71|nessuno|  
|Nome file corrente|MSWB7.dll|nessuno|  
  
 **Tedesco (deu), LCID 1031**  
  
|Componente|Word breaker|Stemmer|  
|---------------|------------------|-------------|  
|CLSID precedente|45EACA36-DBE9-4e4a-A26D-5C201902346D|65170AE4-0AD2-4fa5-B3BA-7CD73E2DA825|  
|Nome file precedente|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID corrente|dfa00c33-bf19-482e-a791-3c785b0149b4|8a474d89-6e2f-419c-8dd5-9b50edc8c787|  
|Nome file corrente|MSWB7.dll|MSWB7.dll|  
  
 **Giapponese (jpn), LCID 1041**  
  
|Componente|Word breaker|Stemmer|  
|---------------|------------------|-------------|  
|CLSID precedente|E1E8F15E-8BEC-45df-83BF-50FF84D0CAB5|3D5DF14F-649F-4cbc-853D-F18FEDE9CF5D|  
|Nome file precedente|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID corrente|04096682-6ece-4e9e-90c1-52d81f0422ed|nessuno|  
|Nome file corrente|MsWb70011.dll|nessuno|  
  
 **Olandese (nld), LCID 1043**  
  
|Componente|Word breaker|Stemmer|  
|---------------|------------------|-------------|  
|CLSID precedente|2C9F6BEB-C5B0-42b6-A5EE-84C24DC0D8EF|F7A465EE-13FB-409a-B878-195B420433AF|  
|Nome file precedente|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID corrente|69483c30-a9af-4552-8f84-a0796ad5285b|CF923CB5-1187-43ab-B053-3E44BED65FFA|  
|Nome file corrente|MSWB7.dll|MSWB7.dll|  
  
 **Russo (rus), LCID 1049**  
  
|Componente|Word breaker|Stemmer|  
|---------------|------------------|-------------|  
|CLSID precedente|2CB6CDA4-1C14-4392-A8EC-81EEF1F2E079|E06A0DDD-E81A-4e93-8A8D-F386C3A1B670|  
|Nome file precedente|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID corrente|aaa3d3bd-6de7-4317-91a0-d25e7d3babc3|d42c8b70-adeb-4b81-a52f-c09f24f77dfa|  
|Nome file corrente|MSWB7.dll|MSWB7.dll|  
  
##  <a name="languages-for-which-neither-the-previous-nor-the-current-file-name-is-naturallanguage6dll"></a><a name="newnew"></a> Lingue per le quali né il nome file precedente del word breaker né quello corrente è NaturalLanguage6.dll  
 Per le lingue elencate nella tabella seguente, i nomi file dei word breaker e degli stemmer precedenti sono diversi da quelli delle nuove versioni. Né il nome file precedente né quello corrente è NaturalLanguage6.dll. Non è necessario sostituire alcun file perché durante l'installazione di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] vengono copiate entrambe le versioni precedente e corrente dei componenti nella cartella Binn. È tuttavia necessario modificare un set di voci del Registro di sistema per specificare la versione precedente o corrente dei componenti.  
  
 **Elenco delle lingue interessate**  
  
|Linguaggio|Abbreviazione<br />utilizzata nel<br />Registro di sistema|LCID|  
|--------------|---------------------------------------|----------|  
|Cinese semplificato|chs|2052|  
|Cinese tradizionale|cht|1028|  
|Thai|tha|1054|  
|Cinese tradizionale|zh-hk|3076|  
|Cinese tradizionale|zh-mo|5124|  
|Cinese semplificato|zh-sg|4100|  
  
 La tabella precedente è ordinata alfabeticamente in base alla colonna Abbreviazione.  
  
 Utilizzare le istruzioni seguenti con l'elenco di valori nella sezione [Nomi file e valori del Registro di sistema per il ripristino di word breaker e stemmer](#newnewvalues).  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="newnewrevert"></a> Per ripristinare i componenti precedenti  
  
1.  Non rimuovere i file per la versione corrente dei componenti dalla cartella Binn.  
  
2.  Nel Registro di sistema passare al nodo seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
3.  Utilizzare i passaggi seguenti per aggiungere nuove chiavi per i ClassID COM per le interfacce del word breaker e dello stemmer precedenti per la lingua selezionata:  
  
    1.  Aggiungere una nuova chiave con il valore della tabella per il word breaker precedente.  
  
    2.  Aggiornare i dati (predefiniti) del valore della chiave al nome file del word breaker precedente riportato nella tabella.  
  
    3.  Se la lingua selezionata utilizza uno stemmer, aggiungere una nuova chiave con il valore riportato nella tabella per lo stemmer precedente.  
  
    4.  Se la lingua selezionata utilizza uno stemmer, aggiornare i dati (predefiniti) del valore di quella chiave al nome file dello stemmer precedente riportato nella tabella.  
  
4.  Nel Registro di sistema passare al nodo seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<language_key>* rappresenta l'abbreviazione per la lingua utilizzata nel Registro di sistema, ad esempio "fra" per il francese e "esn" per lo spagnolo.  
  
5.  Aggiornare il valore della chiave **WBreakerClass** al valore riportato nella tabella per il word breaker corrente.  
  
6.  Se la lingua selezionata utilizza uno stemmer, aggiornare il valore della chiave **StemmerClass** al valore riportato nella tabella per lo stemmer corrente.  
  
7.  Riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="to-restore-the-previous-components"></a><a name="newnewrestore"></a> Per ripristinare i componenti precedenti  
  
1.  Non rimuovere i file per la versione precedente dei componenti dalla cartella Binn.  
  
2.  Nel Registro di sistema passare al nodo seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
3.  Se le chiavi seguenti non esistono, utilizzare i passaggi seguenti per aggiungere nuovi chiavi per i ClassID COM per le interfacce del word breaker e dello stemmer correnti per la lingua selezionata:  
  
    1.  Aggiungere una nuova chiave con il valore riportato nella tabella per il word breaker corrente.  
  
    2.  Aggiornare i dati (predefiniti) del valore della chiave al nome file del word breaker corrente riportato nella tabella.  
  
    3.  Se la lingua selezionata utilizza uno stemmer, aggiungere una nuova chiave con il valore riportato nella tabella per lo stemmer corrente.  
  
    4.  Se la lingua selezionata utilizza uno stemmer, aggiornare i dati (predefiniti) del valore di quella chiave al nome file dello stemmer corrente riportato nella tabella.  
  
4.  Nel Registro di sistema passare al nodo seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<language_key>* rappresenta l'abbreviazione per la lingua utilizzata nel Registro di sistema, ad esempio "fra" per il francese e "esn" per lo spagnolo.  
  
5.  Aggiornare il valore della chiave **WBreakerClass** al valore riportato nella tabella per il word breaker precedente.  
  
6.  Se la lingua selezionata utilizza uno stemmer, aggiornare il valore della chiave **StemmerClass** al valore riportato nella tabella per lo stemmer precedente.  
  
7.  Riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="file-names-and-registry-values-for-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="newnewvalues"></a> Nomi file e valori del Registro di sistema per il ripristino di word breaker e stemmer  
 Utilizzare l'elenco seguente di nomi file e di voci del Registro di sistema con le istruzioni riportate nella sezione precedente. Utilizzare i valori precedenti per ripristinare la versione precedente o utilizzare i valori correnti per ripristinare la versione corrente dei componenti.  
  
 Gli elementi seguenti sono elencati alfabeticamente in base all'abbreviazione utilizzata per ogni lingua.  
  
 **Cinese semplificato (chs), LCID 2052**  
  
|Componente|Word breaker|  
|---------------|------------------|  
|CLSID precedente|12CE94A0-DEFB-11D2-B31D-00600893A857|  
|Nome file precedente|chsbrkr.dll|  
|CLSID corrente|E0831C90-BAB0-4ca5-B9BD-EA254B538DAC|  
|Nome file corrente|MsWb70804.dll|  
  
 **Cinese tradizionale (cht), LCID 1028**  
  
|Componente|Word breaker|  
|---------------|------------------|  
|CLSID precedente|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Nome file precedente|chtbrkr.dll|  
|CLSID corrente|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Nome file corrente|MsWb70404.dll|  
  
 **Thai (tha), LCID 1054**  
  
|Componente|Word breaker|Stemmer|  
|---------------|------------------|-------------|  
|CLSID precedente|CCA22CF4-59FE-11D1-BBFF-00C04FB97FDA|CEDC01C7-59FE-11D1-BBFF-00C04FB97FDA|  
|Nome file precedente|Thawbrkr.dll|Thawbrkr.dll|  
|CLSID corrente|F70C0935-6E9F-4ef1-9F06-7876536DB900|nessuno|  
|Nome file corrente|MsWb7001e.dll|nessuno|  
  
 **Cinese tradizionale (zh-hk), LCID 3076**  
  
|Componente|Word breaker|  
|---------------|------------------|  
|CLSID precedente|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Nome file precedente|chtbrkr.dll|  
|CLSID corrente|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Nome file corrente|MsWb70404.dll|  
  
 **Cinese tradizionale (zh-mo), LCID 5124**  
  
|Componente|Word breaker|  
|---------------|------------------|  
|CLSID precedente|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Nome file precedente|chtbrkr.dll|  
|CLSID corrente|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Nome file corrente|MsWb70404.dll|  
  
 **Cinese semplificato (zh-sg), LCID 4100**  
  
|Componente|Word breaker|  
|---------------|------------------|  
|CLSID precedente|12CE94A0-DEFB-11D2-B31D-00600893A857|  
|Nome file precedente|chsbrkr.dll|  
|CLSID corrente|E0831C90-BAB0-4ca5-B9BD-EA254B538DAC|  
|Nome file corrente|MsWb70804.dll|  
  
## <a name="see-also"></a>Vedere anche  
 [Modifica del word breaker utilizzato per le lingue Inglese (Stati Uniti) e Inglese (Regno Unito)](../../relational-databases/search/change-the-word-breaker-used-for-us-english-and-uk-english.md)   
 [Differenze di comportamento nella ricerca full-text](./full-text-search.md)
