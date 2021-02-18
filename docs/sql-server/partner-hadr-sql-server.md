---
title: Partner di disponibilità elevata e ripristino di emergenza per SQL Server
description: Elenchi di partner di terze parti con soluzioni per offrire disponibilità elevata e ripristino di emergenza per i servizi SQL Server.
ms.topic: conceptual
ms.custom: seo-dt-2019
ms.date: 09/17/2017
ms.prod: sql
ms.technology: release-landing
ms.author: mikeray
author: MikeRayMSFT
ms.openlocfilehash: 7c6a58d336de824aaad46b9e67d4f491fe96fc37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339359"
---
# <a name="sql-server-high-availability-and-disaster-recovery-partners"></a>Partner di disponibilità elevata e ripristino di emergenza per SQL Server
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
Per garantire la disponibilità elevata e il ripristino di emergenza per i servizi di SQL Server, è disponibile un'ampia gamma di strumenti leader nel settore.  In questo articolo vengono evidenziate le aziende partner Microsoft con soluzioni per la disponibilità elevata e il ripristino di emergenza che supportano Microsoft SQL Server.

## <a name="high-availability-and-disaster-recovery-partners"></a>Partner per la disponibilità elevata e il ripristino di emergenza

| Partner | Descrizione | Collegamenti | 
| --- | --- | --- |
|![Azure][5] |**Azure Site Recovery**<br>Azure Site Recovery replica i carichi di lavoro in esecuzione in macchine virtuali o server fisici in modo che rimangano disponibili in una posizione secondaria se il sito primario non è disponibile. È possibile eseguire la replica e il failover delle macchine virtuali di SQL Server dal data center locale ad Azure o a un altro data center locale oppure da un data center di Azure a un altro data center di Azure.<br><br> Edizioni Enterprise e Standard di SQL Server 2008 R2 - SQL Server 2016|[Sito Web][azure_website]<br>[Marketplace][azure_marketplace]<br>[Foglio dati][azure_datasheet]<br>[Twitter][azure_twitter]<br>[Video][azure_youtube]|
|![DH2i][2] |**DH2i**<br>DxEnterprise è il software di disponibilità intelligente per Windows, Linux e Docker che consente di ottenere il tempo di inattività pianificato e non pianificato più vicino a zero, rende possibili enormi risparmi sui costi, semplifica drasticamente la gestione e supporta il consolidamento sia fisico che logico.<br><br>SQL Server 2005+, Windows Server 2008R2+, Ubuntu 16+, RHEL 7+, CentOS 7+|[Sito Web][dh2i_website]<br>[Foglio dati][dh2i_datasheet]<br>[Twitter][dh2i_twitter]<br>[Video][dh2i_youtube]|
|![Logo HPE][4] |**HPE Serviceguard**<br>HPE Serviceguard per Linux (SGLX) consente di proteggere i carichi di lavoro critici di SQL Server 2017 in Linux&reg; da tempi di inattività pianificati e non pianificati con i relativi errori dell'infrastruttura e delle applicazioni negli ambienti fisici e virtuali su qualsiasi distanza. HPE SGLX A.12.20.00 e versioni successive offre opzioni di monitoraggio e recupero sensibili al contesto per i carichi di lavoro di SQL Server delle istanze del cluster di failover e dei gruppi di disponibilità Always On. Ottimizzare i tempi di attività con HPE SGLX senza compromettere l'integrità e le prestazioni dei dati.<br><br>SQL Server 2017 in Linux - RedHat 7.3, 7.4, SUSE 12 SP2, SP3|[Sito Web][hpe_website]<br>[Foglio dati][hpe]<br>[Download copia di valutazione][hpe_download]<br>[Blog][hpe_download]<br>[Twitter][hpe_twitter]
|![IDERA][3]|**IDERA**<br>SQL Safe Backup è una soluzione di backup e recupero ad alte prestazioni per SQL Server che consente di risparmiare denaro riducendo i tempi di backup e le dimensioni dei file di backup dei database e offre l'accesso immediato in lettura e scrittura ai database all'interno dei file di backup.<br><br>Microsoft SQL Server: 2005 SP1 o versioni successive, 2008, 2008 R2, 2012, 2014, 2016; tutte le versioni |[Sito Web][idera_website]|
|![NEC][7]|**NEC**<br>ExpressCluster è una soluzione di disponibilità elevata e ripristino di emergenza completa e del tutto automatizzata contro tutti gli errori gravi, inclusi gli errori hardware, software, di rete e del sito per SQL Server e le applicazioni associate in esecuzione su computer fisici o macchine virtuali in ambienti locali o cloud.<br><br>Microsoft SQL Server: 2005 o versione successiva, tutte le edizioni |[Sito Web][necec_website]<br>[Foglio dati][necec_datasheet]<br>[Video][necec_youtube]<br>[Scaricare][necec_download]|
|![Portworx][6] |**Portworx**<br>Portworx è la soluzione per i contenitori con stato in esecuzione nell'ambiente di produzione. Con Portworx gli utenti possono gestire qualsiasi database o servizio con stato in qualsiasi infrastruttura usando un'utilità di pianificazione del contenitore, ad esempio Kubernetes, Mesosphere DC/OS e Docker Swarm. Portworx risolve i cinque problemi più comuni affrontati dai team DevOps durante l'esecuzione dei database nei contenitori e altri servizi con stato nell'ambiente di produzione: persistenza, disponibilità elevata, automazione dei dati, supporto per più archivi dati e infrastruttura, sicurezza.<br><br>SQL Server 2017 in Docker |[Sito Web][portworx_website]<br>[Documentazione][portworx_docs]<br>[Video][portworx_youtube]|
|![SIOS][8] |**SIOS**<br>La tecnologia SIOS offre soluzioni economiche per la disponibilità elevata e il ripristino di emergenza per SQL Server in Windows o Linux. Il clustering SIOS SANless rende superflua una SAN di archiviazione condivisa, offrendo così la massima flessibilità per proteggere le applicazioni più importanti in configurazioni fisiche, virtuali, cloud e cloud ibride, in ambienti con uno o più siti.<br><br>Aggiungere SIOS DataKeeper all'ambiente per il clustering di failover di Windows Server per creare una risorsa volume SANless che sostituisce l'archiviazione condivisa tradizionale, rendendo più semplice eseguire WSFC in Azure.<br><br>SIOS Protection Suite è una soluzione di clustering altamente flessibile che protegge le applicazioni Linux cruciali, come SQL Server, SAP, HANA, Oracle e molte altre.|[Sito Web][sios_website]<br>[Foglio dati][sios_datasheet]<br>[Twitter][sios_twitter]<br>[Marketplace][sios_marketplace]<br>[Video][sios_youtube]|
|![Veeam][1] |**Veeam**<br>Veeam Backup & Replication è una soluzione di backup e per la disponibilità potente, economica e facile da usare. Offre un recupero rapido, affidabile e flessibile delle applicazioni virtualizzate e dei dati, combinando il backup e la replica delle macchine virtuali in un'unica soluzione software. Veeam Backup & Replication offre un eccellente supporto per gli ambienti virtuali VMware vSphere e Microsoft Hyper-V.<br><br>SQL Server 2005 SP4 - SQL Server 2016 in Windows |[Sito Web][veeam_website]<br>[Foglio dati][veeam_datasheet]<br>[Twitter][veeam_twitter]<br>[Video][veeam_youtube]|

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni su ulteriori partner, vedere [monitoraggio][mon_partners], [partner di gestione][management_partners] e [partner di sviluppo][dev_partners].

<!--Image references-->
[1]: ./media/partner-hadr-sql-server/Veeam-green-logo.png
[2]: ./media/partner-hadr-sql-server/dh2i-logo.png
[3]: ./media/partner-hadr-sql-server/idera-logo.png
[4]: ./media/partner-hadr-sql-server/hpe.png
[5]: ./media/partner-hadr-sql-server/azure-logo.png
[6]: ./media/partner-hadr-sql-server/portworx-logo.png
[7]: ./media/partner-hadr-sql-server/nec-logo.png
[8]: ./media/partner-hadr-sql-server/sios-logo.png

<!--Article links-->
[mon_partners]: ./partner-monitor-sql-server.md
[management_partners]: ./partner-management-sql-server.md
[dev_partners]: ./partner-dev-sql-server.md

<!--Website links -->
[veeam_website]:https://www.veeam.com/
[dh2i_website]:https://dh2i.com
[idera_website]:https://www.idera.com/productssolutions/sqlserver
[hpe_website]: https://www.hpe.com/us/en/product-catalog/detail/pip.376220.html
[azure_website]: /azure/site-recovery/site-recovery-sql
[necec_website]: https://www.necam.com/ExpressCluster/
[portworx_website]: https://portworx.com/
[sios_website]: https://us.sios.com/

<!--Get Started Links-->

<!--Datasheet Links-->
[veeam_datasheet]:https://www.veeam.com/veeam_backup_9_5_datasheet_en_ds.pdf
[dh2i_datasheet]:https://dh2i.com/wp-content/uploads/DxE-Win-QuickFacts.pdf
[hpe]:https://www.hpe.com/h20195/v2/default.aspx?cc=us&lc=en&oid=376220
[necec_datasheet]: https://www.necam.com/docs/?id=0d9ef7a7-f935-4909-b6bb-20a47b3
[azure_datasheet]: /azure/site-recovery/vmware-physical-azure-support-matrix
[sios_datasheet]: https://us.sios.com/solutions/high-availability-cluster-software-cloud/

<!--Marketplace Links -->
[azure_marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps?search=site%20recovery&page=1
[sios_marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8
<!--Press links-->
<!--[veeam_press]:-->

<!--YouTube links-->
[veeam_youtube]:https://www.youtube.com/user/YouVeeam
[dh2i_youtube]:https://www.youtube.com/user/dh2icompany 
[idera_youtube]:https://www.idera.com/resourcecentral/videos/sql-safe-overview
[azure_youtube]: https://mva.microsoft.com/en-US/training-courses/is-your-lack-of-a-disaster-recovery-site-keeping-you-up-at-night-8680?l=oF7YrFH1_7504984382
[necec_youtube]: https://www.youtube.com/watch?v=9La3Cw1Q1Jk
[portworx_youtube]: https://www.youtube.com/channel/UCSexpvQ9esSRgiS_Q9_3mLQ
[sios_youtube]: https://www.youtube.com/watch?v=U3M44gJNWQE

<!--Twitter links-->
[veeam_twitter]:https://twitter.com/veeam
[dh2i_twitter]:https://twitter.com/dh2i
[hpe_twitter]:https://twitter.com/hpe
[azure_twitter]:https://twitter.com/hashtag/azuresiterecovery
[sios_twitter]:https://www.twitter.com/SIOSTech

<!--Docs links>-->
[portworx_docs]: https://docs.portworx.com/

<!--Download links-->
[hpe_download]: https://h20392.www2.hpe.com/portal/swdepot/displayProductInfo.do?productNumber=SGLX-DEMO
[necec_download]: https://www.necam.com/ExpressCluster/30daytrial/
<!--Blog links-->
[hpe_blog]: https://community.hpe.com/t5/Servers-The-Right-Compute/SQL-Server-for-Linux-Is-Here-and-A-New-Chapter-for-Mission/ba-p/6977571#.WiHWW0xFwUE