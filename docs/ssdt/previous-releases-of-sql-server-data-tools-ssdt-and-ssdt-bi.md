---
title: Versioni precedenti di SQL Server Data Tools (SSDT)
description: Scoprire quali versioni di SSDT e SSDT-BI funzionano con le versioni di Visual Studio. Scoprire come installare versioni diverse di SSDT e SSDT-BI.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 5d32e301-0f44-4916-b0db-76e8322c0ab7
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 06/17/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 3e2a3e607b561ede827652f61032b90fc7efa336
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353993"
---
# <a name="previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi"></a>Versioni precedenti di SQL Server Data Tools (SSDT e SSDT-BI)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Data Tools (SSDT) fornisce modelli di progetto e aree di progettazione per la creazione di tipi di contenuto di SQL Server, quali database relazionali, modelli di Analysis Services, report di Reporting Services e pacchetti di Integration Services.

Poiché SSDT è compatibile con le versioni precedenti, è sempre possibile usare [la versione più recente di SSDT](download-sql-server-data-tools-ssdt.md) per la progettazione e la distribuzione di database, modelli, report e pacchetti eseguiti in versioni precedenti di SQL Server.

In passato, la shell di Visual Studio usata per la creazione di tipi di contenuto di SQL Server è stata rilasciata con nomi diversi, ad esempio **SQL Server Data Tools**, **SQL Server Data Tools - Business Intelligence** e **Business Intelligence Development Studio**. Le versioni precedenti erano fornite con specifici set di modelli di progetto. Per ottenere tutti i modelli di progetto insieme in un unico SSDT è necessario usare [la versione più recente](download-sql-server-data-tools-ssdt.md). In caso contrario, sarà probabilmente necessario installare più versioni precedenti per ottenere tutti i modelli usati in SQL Server. Per ogni versione di Visual Studio viene installata una sola shell. L'installazione di un secondo SSDT aggiunge soltanto i modelli mancanti.

## <a name="previous-ssdt-releases"></a>Versioni precedenti di SSDT

Scaricare le versioni precedenti di SSDT selezionando il collegamento per il download nella sezione correlata.

| Versione SSDT | Versione di Visual Studio |
|--------------|-----------------------|
| [15.8](#ssdt-for-visual-studio-vs-2017) | 2017 |
| [17.4](#ssdt-for-visual-studio-vs-2015) | 2015 |
| [16.5](#ssdt-for-visual-studio-vs-2013) | 2013 |
| [11.1.50727.1](#ssdt-for-visual-studio-vs-2012) | 2012 |

### <a name="ssdt-for-visual-studio-vs-2017"></a>SSDT per Visual Studio (VS) 2017

**[Scaricare SSDT per Visual Studio 2017 (15.8)](https://go.microsoft.com/fwlink/?linkid=2124319)**

Questa versione di **SSDT per Visual Studio 2017** può essere installata nelle lingue seguenti:

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40a)

### <a name="ssdt-for-visual-studio-vs-2015"></a>SSDT per Visual Studio (VS) 2015

Per installare questa versione di SSDT, è necessario scaricare un'immagine ISO. Il file ISO è un file autonomo che contiene tutti i componenti richiesti da SSDT e può essere scaricato mediante un gestore download ripristinabile, utile nei casi di larghezza di banda della rete limitata o poco affidabile. Una volta scaricata, l'immagine ISO può essere montata come unità.

Passaggi per l'installazione:

1. **[Scaricare SSDT per Visual Studio 2015 (17.4)](https://go.microsoft.com/fwlink/?linkid=2132817)** .

2. Aprire l'immagine ISO.

3. Eseguire il file *SSDTSetup.exe*.

    ![Immagine ISO](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Questa versione di **SSDT per Visual Studio 2015** può essere installata nelle lingue seguenti:

| Linguaggio | Hash SHA256 |
|----------|-------------|
| [Cinese semplificato](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [Cinese tradizionale](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [Francese](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [Tedesco](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [Italiano](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [Giapponese](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [Coreano](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [Russo](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2013"></a>SSDT per Visual Studio (VS) 2013

Per installare questa versione di SSDT, è necessario scaricare un'immagine ISO. Il file ISO è un file autonomo che contiene tutti i componenti richiesti da SSDT e può essere scaricato mediante un gestore download ripristinabile, utile nei casi di larghezza di banda della rete limitata o poco affidabile. Una volta scaricata, l'immagine ISO può essere montata come unità.

Passaggi per l'installazione:

1. **[Scaricare SSDT per Visual Studio 2013 (16.5)](https://go.microsoft.com/fwlink/?linkid=832312)**

2. Aprire l'immagine ISO.

3. Eseguire il file *SSDTSetup.exe*.

    ![Immagine ISO](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Questa versione di **SSDT per Visual Studio 2013** può essere installata nelle lingue seguenti:

| Linguaggio | Hash SHA256 |
|----------|-------------|
| [Cinese semplificato](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [Cinese tradizionale](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [Francese](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [Tedesco](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [Italiano](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [Giapponese](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [Coreano](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [Russo](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [Spagnolo](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2012"></a>SSDT per Visual Studio (VS) 2012

Per installare questa versione di SSDT, è necessario scaricare un'immagine ISO. Il file ISO è un file autonomo che contiene tutti i componenti richiesti da SSDT e può essere scaricato mediante un gestore download ripristinabile, utile nei casi di larghezza di banda della rete limitata o poco affidabile. Una volta scaricata, l'immagine ISO può essere montata come unità.

Passaggi per l'installazione:

1. **[Scaricare SSDT per Visual Studio 2012 (11.1.50727.1)](https://go.microsoft.com/fwlink/?linkid=518814)**

2. Aprire l'immagine ISO.

3. Eseguire il file *SSDTSetup.exe*.

    ![Immagine ISO](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Questa versione di **SSDT per Visual Studio 2012** può essere installata nelle lingue seguenti:

| Linguaggio | Hash SHA256 |
|----------|-------------|
| [Cinese semplificato](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x804) | 78F20325648CFF49D9C58A26186E0AC199D104B3CF634E5663B4B262BEC69C07 |
| [Cinese tradizionale](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x404) | A2722380AF2EE1E8BB52B4FA54A61BAB411E5E5FD5B050108F81ED23DC87366D |
| [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x409) | 748EF78D3F9CC6FE360432C378EA690DE51AEB2C747E87D43E08448D26F93A91 |
| [Francese](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40c) | 40E6504169BF618EDC7EB5B62D1215DC96726D6EEC3CA8EC3EEB49044E4B6FB7 |
| [Tedesco](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x407) | C45C974E6B8F9611BA2AC1EE90C5C507992BCE5693BF46F6C7C138591ED6A123 |
| [Italiano](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x410) | C1B20CDB41C7B1B5DE76A71F9A091A6FDF5186B12F6AA269DA6338B3CB2D91A6 |
| [Giapponese](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x411) | BF56958B904C1C5F28084BD0B16928B6CBFEC83EB3F0C913EC364FA335241943 |
| [Coreano](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x412) | 9023B0656785C357A67E39EB76A2806B923C2BD17342D7226A8ADA384A9F4E28 |
| [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x416) | ACAD9FE03729E289ECE821D92C56CFB1D7FCCEA8423ABF1E164BF3C67ABEFEFB |
| [Russo](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x419) | E2191D787BA833DF4A85B064C5373DC44099E76214FBF9505728702D4D6B83F0 |
| [Spagnolo](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40a) | 6D81FB572A7003C54C29D2ACF076D2CED4A1CA80F329BFF9D41A806920D64EEE |

> [!Note]
> SSDT supporta le due versioni più recenti di Visual Studio. Con il rilascio di Visual Studio 2019, le versioni di SSDT per Visual Studio 2015 e versioni precedenti non vengono più aggiornate. SSDT per Visual Studio 2010 non è più disponibile. Per altre informazioni, vedere la sezione relativa alle *domande frequenti* di [questo post di blog del team SSDT](/archive/blogs/ssdt/sql-server-data-tools-17-0-rc-and-ssdt-in-vs2017).

## <a name="sql-bi-analysis-services-reporting-services-integration-services"></a>SQL BI: Analysis Services, Reporting Services, servizi di integrazione

I modelli di Business Intelligence vengono usati per creare modelli SSAS, report SSRS e pacchetti SSIS. Le finestre di progettazione di BI sono legate a versioni specifiche di SQL Server. Per usare le nuove caratteristiche di Business Intelligence, installare una finestra di progettazione di BI con versione più recente.

## <a name="bi-designers"></a>Finestre di progettazione di BI

[Download di SSDT-BI per Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313) (SQL Server 2014, SQL Server 2012, SQL Server 2008 e 2008 R2)

[Download di SSDT-BI per Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843) (SQL Server 2014, SQL Server 2012, SQL Server 2008 e 2008 R2

Business Intelligence Development Studio (BIDS) viene installato tramite il programma di installazione di SQL Server. Non è disponibile alcun download Web (SQL Server 2008 e 2008 R2).

Per SQL Server 2012 o 2014, è possibile usare **SSDT-BI per Visual Studio 2012** o **SSDT-BI fo Visual Studio 2013**. L'unica differenza tra i due programmi è la versione di Visual Studio.

## <a name="next-steps"></a>Passaggi successivi

- [Scaricare la versione più recente di SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md)
- [Note sulla versione per SQL Server Data Tools (SSDT)](release-notes-ssdt.md)
- [Scaricare SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [Scaricare Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [Strumenti e utilità di SQL](../tools/overview-sql-tools.md)