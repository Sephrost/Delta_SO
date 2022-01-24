# Introduzione 
 In questo capitolo tratteremo ,le memorie secondarie nella loro struttura funzionalità e connettività e il perché sono state introdotte, ma intanto capiamo cosa sono. 

Nella categoria delle memorie secondarie appartengono tutti quei dispositivi di memorizzazioni a cui la cpu non può non accedervi direttamente,capaci di immagazzinare dati in modo persistente anche in seguito all’arresto del dispositivo

la cui struttura e caratteristiche risulta essere molto diversa rispetto alle memorie principali, adesso analizzeremo una serie di memorie secondarie piú o meno desuete in modo da comprendere  la struttura e funzionalitá

# Dischi Magnetici 

 dischi magnetici sono il supporto fondamentale di memoria secondaria dei

moderni sistemi computazionale ,sono formati da dei piatti dalla forma rotonda come quella dei cd con un dimensione che oscilla tra i 1,8 e i 3,5 pollici, la loro superficie è rivestita di una materiale magnetico che permette di memorizzare le informazioni . 

Ogni strato è sospesa su ogni strato magnetico ed esse sono attaccate ad al braccio del disco che permette di muoverle in blocco. 

 Ogni superficie è suddivisa per tracce circolari ma esse sono ulteriormente suddivise in settori,ogni insieme di tracce corrispondenti ad una porzione del braccio costituisce un cilindro,in ogni unità a disco possono esservi migliaia di cilindri, ed ogni traccia può contenere centinaia di settori.

la capacità di memoria di un disco viene misura di gigabyte. 

Ogni disco per funzionare necessità di un motore che gli permette di ruotare ad alta velocità espressa in giri al minuto(RPM),in base alla velocità di rotazione varierà la velocità di trasferimento dei dati,cioè la velocità con cui i dati fluiscono dall’unità a disco al calcolatore,altro fattore su cui influisce la velocità di rotazione è il tempo di posizionamento,che consiste di due componenti cioè il tempo necessario al braccio del disco in corrispondenza del cilindro detto anche TEMPO DI RICERCA,ma anche il LATENZA  DI ROTAZIONE cioè il tempo necessario al settore per posizionarsi sotto la testina 

Un disco può essere removibile,ciò  permette che alcuni dischi possono essere montati in base alle necessità,e in questa categoria appartengono periferiche come 

CD,Blu-ray,e u dispositivi di memoria flash (essi sono strutturati con una memoria a sto solido).

Un unità a disco è collegata a un calcolatore tramite un serie di fili detti anche Bus che possono essere di diverse categorie : I/O (input/output),  ATA(ADVANCED TECHNOLOGY ATTACHMENT),SATA (SERIAL ATA),USB(universal serial bus ),FC(fiber channel).

Il processo di trasferimento dei dati viene effettuato attraverso una serie di processi dedicati detti anche controllori ed esistono di vario tipo come , i controllori di macchina che vengono posti sull’estremità del bus più vicino al calcolatore,invece i disk controller sono implementati direttamente nell’unità a disco,che attraverso delle apposite porte mappate al suo interno permettono operazioni di i/o,generalmente ogni disco ha una propria memoria cache incorporata,che permette il trasferimento dei dati con la superficie del disco,ma invece il trasferimento dei dati dalla cache e l’adattatore avviene alla velocità proprio dei dispositivi elettronici.

# Dischi a stato solido
 dischi a stato solido detto anche ssd,sono memoria non volatile che viene utilizzata come disco rigido,ci sono molte varianti di questa tecnologia come la DRAM dotate di una batteria per permettere di mantenere lo stato in caso di caduta di tensione alla tecnologia flash come i chip SLC(single level cell)e MLC(multi level cell).

Le caratteristiche delle ssd sono le stesse dei dischi magnetici ma possono essere decisamente più affidabili perchè non sono composti da parti in movimento ,ed hanno una velocità superiore con consumi nettamente superiori,ma hanno un lato negativo il costo per megabyte decisamente più caro e capacità di memorizzazione inferiore e con una vita a lungo termine più corta rispetto ai dischi rigidi. 

Un ottimo utilizzo degli ssd è negli storage array cioè nella  gestione di meta dati del file system  che richiedono prestazioni elevate,ma vengono utilizzati anche per migliorare le prestazioni di lettura e scrittura di computer con qualche anno sulle spalle rendendoli molto piû utilizzabili nonostante gli anni.


# Nastri Magnetici

I nastri magnetici sono stati il primo esempio di memorie secondarie sul mercato,essi nonostante un grande capacità di immagazzinare dati in modo permanente e con un costo abbastanza abbordabile, peccano soprattutto in un tempo d’accesso molto lungo e quindi non è stata una soluzione duratura negli anni,ma comunque venivano usati spesso per creare backup di dati e per trasferire informazioni tra diversi sistemi elaborativi. 

I nastri vengono avvolti in bobine e scorre su una testina di lettura e scrittura ,il posizionamento nel settore desiderato può richiedere svariato tempo,però la lettura dei dati avviene ad una tempistica paragonabile a quella delle unità a  disco.

I nastri moderni hanno capacità di svariati terabyte, e le unità  a nastro e i driver appositi vengono classificate in base alla loro larghezza ,generalmente dai 4 ai 19 millimetri.


# Strutture dei dischi 

 moderni dischi magnetici sono indirizzati come grandi array monodimensionali di blocchi logici ,dove ogni blocco è la minima unità di trasferimento.

la dimensione oscilla tra i 512 byte ,sebbene alcuni dischi possono essere formattati a basso livello con lo scopo di ottenere una diversa dimensione dei blocchi logici.

l’array monodimensionale è mappato in modo sequenziale  sui settori del disco, il settore 0 è il primo settore della prima traccia sul cilindro piû esteso ,in corrispondenza seguono tutte le altre tracce che lo compongono,sarebbe possibile sfruttare questa corrispondenza per trasformare il numero di un blocco logico in un indirizzo fisico di vecchio tipo consistente,in un numero di cilindro ,ma ciò non è possibile poi sicuramente saranno presente dei settori difettosi(essi vengono sostituiti da altri settori dal disco stesso ) e perché  il numero di settori non è costante. 

Nei supporti che utilizzano la velocità lineare costante la densità di bit per traccia è uniforme,piû è lontana dal centro del disco,tanto maggiore è la lunghezza della traccia tanto minore sarà il numero di settori che essa può contenere. 

Spostandosi da aree esterne verso aree interne il numero dei settori per traccia diminuisce,le tracce nell’area esterna contengono in genere circa il 40% in piû di settori rispetto all’area interna,la velocità di rotazione aumenta sempre di quando la testina si sposta da aree più esterne verso quelle più interne per mantenere la costanti le quantità di dati che scorrono sotto le testine ,questo metodo viene utilizzato anche per i Cd-Rom e Dvd.

In alternativa la velocità di rotazione dei dischi rimane costante, e la densità di bit decresce dalle tracce interne a quelle esterne ,questo metodo è noto come velocità angolare costante .

Il numero di settori per traccia cresce con l’evolversi della tecnologia dei dischi e l’area più esterna di un disco di solito contiene diverse centinaia di settori per traccia.


# Connessione dei dischi 

 i calcolatori accedono alle memorie secondarie in due modi:per i sistemi di piccole dimensioni si utilizza le porte di I/O,oppure tramite un host remoto  in un  file system 

distribuito(memoria secondaria connessa a rete)

Memoria secondaria connessa alla macchina 

la macchina accede alla memoria secondaria tramite porte locali di I/O disponibili in diverse tecnologie che adesso vedremo. 

Le porte di I/O piû datate  sono quelle di tipo ATA o IDE,questa architettura permette di avere non più di due unità per ciascun bus di  I/O ,invece un protocollo piû moderno ed efficace prevede un cablaggio più semplice e il SATA

Se vi è un gran numero di memorie secondarie connessa alla macchina,tra questi unità a disco,le unità RAID ecc, i comandi di I/O che avviano trasferimenti di datI a un dispositivo a un dispositivo di memoria connessa alla macchina sono letture e scritture di blocchi logici di dati diretta a unità di memorizzazione specifiche .

# Memoria secondaria connessa a rete

la connessione di memorie secondarie alla rete è un sistema di memoria specializzato al quale si accede in modo remoto tramite una rete di trasmissione di dati.

I client accedono alla memoria connessa alla rete tramite un'interfaccia RFP,come l’NFS per i sistemi unix o CIFS per i sistemi Windows.                            
Le chiamate di procedura remota (RPC) sono realizzate per mezzo di protocolli come il TCP o UDP sopra una rete IP.

l’unità di memoria è normalmente come una batteria RAID con programmi di controllo che implementano l’interfaccia per le RPC.

Collegare una memoria secondaria alla rete permette di condividere i propri dati con altri utenti connessi alla stessa rete si in modalità cloud che LAN ,però questi sistema per molti versi risulta essere inefficace rispetto al diretto collegamento della memoria alla macchina .

# Storage-area network

Uno svantaggio nel collegare una memoria secondaria alla rete e il problema della

latenza sulla rete .ciò rallenta notevolmente le operazioni di I/O ed è abbastanza grave per i sistemi client-server di grandi dimensioni poiché il rallentamento verrebbe rilevato da tutti gli utenti connessi,ma un'alternativa è quella di utilizzare una Storage-area network (SAN) cioè una rete privata che impiega protocolli specifici per la memorizzazione.

la potenza di una SAN sta nella flessibilità,permettendo di collegare ad essa più macchine e molti storage array.

La SAN attraverso uno switch nega o concede l’accesso alla macchina alla memoria secondaria,permettendo se necessaria di allocare ulteriore memoria se gli utenti ne hanno necessità,ma gli switch SAN hanno spesso porte in maggior numero ciò causa un costo abbastanza rilevante che fa decidere se utilizzare o meno questa soluzione

# Scheduling del disco

  

 Uno delle caratteristiche che rende un sistema operativo efficiente e quella della gestione delle risorse hardware limitandone gli sprechi,questa gestione vale non vale solo per la cpu e per la memoria principale ma vale anche per le memorie secondarie,ogni volta che l’utente corrá effettuare operazioni di I/O un processo effettuare delle system call,in cui sono contenute le diverse informazioni come: 

*  se l’operazione è di input o di output 

* indirizzo nel disco per il trasferimento

* ’indirizzo di memoria per il trasferimento

* il numero di settori da trasferire 

Se queste richieste possono essere soddisfatte l’operazione andrà a buon fine, in caso contrario le richieste saranno inserite in una coda ready relativa quella ’unità ,che perô nel caso di un sistema multiprogrammazione la coda ready può risultare parecchio lunga per risolvere questo problema il sistema operativa utilizza uno degli algoritmi di scheduling che adesso tratteremo.

La decisione su che algoritmo bisogna scegliere varia in base alle necessità e su che ambiente vogliamo lavorare e come vogliamo lavorare ma anche sul quantitativo di richieste che eventualmente dovremmo gestire e con quali tempistiche,insomma la scelta dell'algoritmo migliore si adatterà in base alle nostre necessità



# Scheduling in ordine di arrivo -FCFS

 La struttura di questo algoritmo di scheduling è molto semplice,il primo che arriva sarà il primo ad essere servito,però come efficienza non è delle migliori facciamo un esempio per comprendere il tutto.

Supponiamo di avere la seguente coda ready con valori del tipo 

98,183,37,122,14,124,65,67  ma la nostra testina è posizionata sul blocco numero 53 la testina dovrà prima controllare tutti i settori e poi effettuare l’operazione di scheduling che risulta essere molto lunga,e quindi risulta essere nettamente inefficace,graficamente accade ciò.

![](https://lh5.googleusercontent.com/Z1BkQWu0FEFq8eo3knlDlp2cB0yYHK2Dw6I9WH-QHGJlg46efI1LefRN6yzlis2vyC_1RE7PQxf_KTtyLUMlq7NURtUlaIKgWzcSqiyE2A8tX8fg5t-vh7wrJ9qOXW-uxggJACqy)

# Scheduling SSTF

L'algoritmo SSTF (shortest seek time first) utilizza anche lui un concetto molto semplice quello di servire la richiesta con il tempo di ricerca  minore,però come ben si penserà il problema principale sarà la starvation poiché supponiamo di avere una coda ready con i valori che abbiamo elencato precedentemente cioè 98,183,37,122,14,124,65,67

bene la richiesta che verrà soddisfatta prima sarà quella con la tempo pari a 14 poi 37 e cosî via, ma attenzione la coda ready non è un struttura statica ma dinamica quindi sicuramente verrà aggiornata con delle nuove richieste con tempistiche diverse,quindi le richieste con un tempo di ricerca superiore non verranno mai soddisfatte e sosteranno in coda ready per un tempo indefinito,ecco graficamente cosa accade. 

![](https://lh3.googleusercontent.com/wZsiqerB2x55Jpyo1hV1iUH27XncPOFeYeecjXTMqrH-yoZYIq_g3LVZ2eNZ0pOs-BDLDRGstljrchM9gKS3nAj25UaAkcyymmSq3Pfrfn5TIH6sHwAy0OXPix60Kcl9Axs32Ue-)  
  

# Scheduling -Scan e C-Scan

l'algoritmo di scan si sposta da un estremo all’altro quindi le richieste che verranno soddisfatte saranno in ordine di sequenza,in questo modo le richieste verranno soddisfatte tutte in modo uniforme anche se vi saranno delle richieste che dovranno attendere più di quanto necessario ecco graficamente cosa accade.

Invece l’algoritmo di C-Scan, non è altro che una rivisitazione della versione base con la differenza che la testina dopo aver soddisfatto le richieste di un primo estremo durante la scansione opposta non accetterà le richieste dall’estrema già analizzato

![](https://lh4.googleusercontent.com/aJ-_8CPjItV_IFMgNn2lw_06Tl2hjl3q0MOvJgG1IrBYmj1wXzgE4NWHMKFXtuYuocW_B95CNguVVfj8bPpPp6VpOV82G68e9bBHyqmnQrJu3Lm5cBsCA4f-9UZguyPp-gN_ZLe5) 

![](https://lh5.googleusercontent.com/YRJSE6zRZ_PSPKoo2pdF4PGgWDCw7J2nBXQlVqLBtRmA5-aRhCVGv2LW_avmspHATVB0W1avJlKWNowmhMdelsSgx2mb9atNjwYE2fLFnTxeKc7UmemnqJdAg68kQc0qlG8mZqOM)

 # Scheduling Look e C-Look

 Sono essenzialmente delle versioni definitive degli algoritmi di Scan e C-Scan completando la “Scannerizzazione” delle richieste lungo un estremo controllando se vi sono delle nuove prima di spostarsi lungo un altro estremo(invece Scan e C-Scan se non trovano altre richieste cambiano estremo).
 
 Formattazione del disco 

Il sistema operativo deve anche occuparsi della gestione delle unità a disco,nello specifico della gestione del sistema d’avvio ed eventuali blocchi difettosi. 

Un'unità a disco appena uscita dalla fabbrica è letteralmente una serie di dischi magnetici  ,quindi necessità di una configurazione dei settori,questo processo si chiama formattazione fisica/basso livello,che permette di “riempire” il disco con una speciale struttura dati per ogni settore ,tipicamente una coda e un'intestazione e  in cui insieme ad un'area dei dati  contengono  informazioni usate dal controllore del disco, esempio l’error-correcting code.

Quando il correttore scrive dei dati di una classica operazione di I/O,aggiorno il vettore del ECC(error-correcting code) secondo il contenuto di dati di quel settore. 

Quando il controllore legge dati da quel settore,calcola anche l’ECC e lo confronta con il valore originale,se ne risulta una differenza l’area dei dati di quel settore non è integra,e il settore potrebbe essere difettoso.

L’ECC è un codice per la correzione degli errori tutto in modo automatico ,solo anche dei bit alterati permettono al controllore di trovare i bit in questione ed effettuare il ricalcolo del loro valore originale. Il controllore può riportare se gli errori sono recuperabili o soft error in base alla gravità. Il processo di formattazione del disco viene nella maggioranza dei casi eseguito dal produttore di esso come processo costruttivo,testando se effettivamente l’unità funziona. Prima che utilizzare il disco come contenitore di File  il sistema operativo si occupa di registrare le proprie strutture dati nel disco,cosa che fa in due processi ,nel primo processo si divide il disco in uno o più gruppi di cilindri,dette partizione ,che può essere trattata come se fosse un'unità a disco assestante,ogni partizione può contenere una copia del codice di esecuzione del sistema operativo .

Il punto successivo è la formattazione logica, cioè la creazione del file system,in cui sono descritte le informazione sullo spazio libero ,spazio allocato,e una directory che inizialmente sarà vuota,però per una maggiore efficienza i file system accorpa i blocchi in gruppi,detti Cluster.

Alcuni sistemi operativi permettono ad alcuni programmi di impiegare una porzione del disco come se fosse un grande array sequenziale non contenente nessun file system. Questo array è detto anche di basso livello(raw disk) e il suo input si chiama I/O di basso livello. È possibile rendere certe applicazioni più efficienti permettendogli di implementare servizi di memorizzazione specializzati che usano una partizione di basso livello ,che in alcune applicazioni funziona meglio quando usufruisce degli ordinari servizi del file system.

# Blocco d’avviamento

Per far si che un calcolatore possa funzionare,esso deve contenere un programma iniziale detto bootstrap,che inizializza il sistema in tutti i suoi aspetti,dai registri della cpu al contenuto della memoria centrale. 

 Per far ciò il programma di avviamento  trova il kernel del sistema operativo nei dischi che poi viene caricato in memoria. Generalmente questo programma viene contenuto in una ROM ,perché esse ha un indirizzo di memoria fisso a cui la cpu fare riferimento ogni volta che la macchina viene riavviata o arrestata,ma anche perché non può essere contaminata da virus visto che è una memoria di sola lettura.

Però per sostituire il programma d’avviamento è necessario sostituire i circuiti integrati della ROM,ma per risolvere questo problema i costruttori memorizzano nello ROM un piccolo programma d'avviamento modificabile in futuro detto BOOTLOADER che ha il compito di caricare il programma di bootstrap dal disco .

Il programma di avviamento è registrato in una porzione di disco detto “Blocco di avviamento".

Il codice contenuto nella ROM istruisce il controllore dell’unità affinchè trasferisca i dati dei blocchi d’avviamento nella memoria ,e dopo aver eseguito il codice viene caricato in l’intero sistema operativo.

  

# Blocchi difettosi 

 Le unità a disco sono spesso soggette a malfunzionamenti questo è dato dall’usura delle parti mobili che può causare dei rallentamenti o la rottura e quindi la sostituzione dell’unità  smarrendo tutti i dati in esso contenuti.

Molti dischi già dalla fabbrica contengono dei settori difettosi ed essi sono trattati in modo differente.

Vi sono vari modi per scansionare i settori difettosi  anche una semplice formattazione per rilevare la loro presenza nel disco per poi essere marcato come inutilizzabile.

Altro modo per scansionare o recuperare dei blocchi danneggiati,è quello che il controllore  mantiene  una lista di blocchi aggiornando per tutto il periodo di attività questa lista con i blocchi difettosi,sostituendoli con dei blocchi di riserva nascosti,questa strategia è nota come accantonamento di settori.

Altra alternativa è la traslazione dei settori,che sposta di quanto necessario tutte le unità nascondendo le parti danneggiate .

# Gestione dell’area d’avvicendamento (swapping)

La gestione dell’area di avvicendamento è un compito di basso livello del sistema operativo, in cui la memoria virtuale usa lo spazio dei dischi come estensione della memoria centrale ,ma tutto cioè riduce le prestazioni del sistema operativo,quindi si cerca di utilizzare, l'algoritmo migliore.

 è sempre meglio attribuire un’area di swapping con una dimensione maggiore poiché se si esaurisce la memoria il sistema operativo sarà costretto a terminare il processo .

  

 # Collocazione dell’area d’avvicendamento

 Le possibili collocazioni per un'area d’avvicendamento sono due: all’interno del normale file system, o in una grande partizione del disco a sé stante. 

Nel caso in cui l’area d’avvicendamento è un grande file si può si possono usare le ordinarie funzioni del file system per crearla,assegnando uno spazio di allocazione con il rispettivo nome da noi dichiarato.

Questo approccio è il più semplice ma anche il meno efficace perché richiede troppo tempo nell'attraversare la struttura della directory,causando l’aumento dei tempi di swapping causando seek multipli durante la scrittura o lettura.

Le prestazioni possono essere migliorate implementando una memoria secondaria come cache per le informazioni relative ai blocchi ,ma il costo di attraversamento delle strutture dati del file system permane. 

In alternativa  i può creare una nuova partizione del disco non formattata (raw partition) in cui non è presente nessuna struttura relativa al file system,ma si usa uno speciale gestore dell’area d’avvicendamento per gestire i blocchi che adotta un algoritmo per la riduzione del tempo d’accesso rispetto allo spazio impiegato sul disco ,dato si accede più frequentemente all’area d’avvicendamento rispetto al file system,però c’è il rischio che la frammentazione possa aumentare poichè i file d’avvicendamento hanno una vita media più breve rispetto ai file ordinari. 

  
  

# STRUTTURE RAID

La struttura di tipo RAID permette di gestire più dischi collegati alla stessa macchina,permettendo anche di migliorare le prestazioni in lettura e scrittura ed affidabilità di essi.

Per migliorare l’affidabilità è possibile introdurre una certa ridondanza,è quello di duplicare ogni disco,questo metodo è detto mirroring,così quando uno dei due dischi si danneggia e possibile leggere l'altro ancora funzionante.

La struttura RAID è suddivisa in livella,che poi in base all’esigenza dell’utente.

### Raid 0(striping): 

è il livello più semplice di Raid, e viene utilizzato anche come storage domestico; i dati immessi nello storage Raid 0 sono memorizzati e organizzati con la tecnica Striping, che prevede la suddivisione dei file in blocchi della stessa dimensione, gestiti sulla memoria dei dischi che compongono il sistema; questo livello di Raid non prevede ridondanza,però questo tipo di RAID è poco sicuro sulla sicurezza dei dati poichè se si danneggia un solo disco causa la perdita dei dati .

### RAID 1(Mirroring): 

-questo livello di Raid conta sull' apporto di due unità disco; la tecnica Mirroring     prevede che uno dei due dischi sia la copia speculare dell' altro; le unità disco di un Raid 1 Mirroring per questo sono sempre 2; le prestazioni non sono al top visto che é come contare su un disco singolo,

  

####  RAID 2 (Bit level striping):: 

 questo livello di Raid è del tutto in disuso perché ha per lo più una funzione di controllo, già possibile con i dischi singoli; l' unità di Striping é il singolo bit che viene controllato ad ogni scrittura, e corretto in caso di errore

  

#### RAID 3(Byte level striping with dedicated parity disk):

 questo livello di Raid utilizza la tecnica dello striping e il controllo della parità per la sicurezza e l' affidabilità dei dati iscritti nel Raid stesso; in questo livello di Raid i dati sono scritti in byte non in blocchi o strisce; i dati vengono suddivisi sui vari dischi del Raid 3  e letti contemporaneamente da più dischi per l' uso della tecnica striping.

  

#### Raid 4 (Block level striping with dedicated parity disk): 

questo livello di Raid possiamo associarlo al Raid 3, con la differenza che i dati vengono suddivisi in blocchi, non in byte nelle operazioni di lettura e scrittura non si utilizza tutto l' intero sistema Raid, ma esclusivamente il disco in cui risiede ed è stato salvato il blocco di dati che si cerca;

  

#### RAID 5 (Distributed parity):

 anche questo livello di Raid é paragonabile al funzionamento e alla gestione dati del Raid 3; nel livello di Raid 5 però non esiste un disco dedicato ai bit di parità, ma questi vengono salvati indifferentemente su tutti i dischi del sistema Raid; in termini di ridondanza quindi il calcolo e la scrittura dei file in parità nel livello di Raid 5 sono distribuiti sugli stessi dischi su cui vengono scritti e salvati i dati;

  
  

 # PROBLEMATICHE DEL RAID

 i sistemi con struttura tipo RAID non assicurano sempre la disponibilità dei dati al sistema operativo, questo può essere causato da un puntatore a un file errato,compromettendo i dati. 

Il RAID protegge da errori generati dai supporti fisici ma no da quelli hardware e software una possibile soluzione a questa problematica i file system SOLARIS ZFS applica una checksum interna memorizzata insieme al puntatore al file  ad ogni blocco da mantenere l’integrità dei dati.

Un altro problema della struttura RAID è la mancanza di flessibilità nella modifica della dimensione del file system nel caso in cui più dischi sono collegati tra di loro.

Il sistema ZFS combina la gestione dei file system e quella dei volumi in una unità,e le partizioni di dischi sono riuniti in gruppi di memorizzazione (Pools of storage) attraverso insiemi RAID,la memoria nei sistemi ZFS è basata su malloc e free per allocare e rilasciare memoria nei file system quando viene utilizzata dai blocchi 

  

# REALIZZAZIONE DI UNA MEMORIA STABILE

 In molte applicazioni è necessario disporre di una memoria in grado di mantenere costantemente i dati al proprio interno.

Per realizzare questo tipo di memorizzazione è necessario ricorrere con dei meccanismi di guasto indipendenti ed è necessario coordinare l’aggiornamento delle informazioni in modo tale che un eventuale malfunzionamento non lasci copie in uno stato danneggiato 

Un’operazione di scrittura su disco può avere i seguenti esiti: 

 * Operazione riuscita: i dati sono scritti correttamente sul disco

 * Insuccesso parziale : si è verificato un guasto durante il trasferimento ,e alcuni dei sono       stati aggiornati correttamente .

* Insuccesso totale : il malfunzionamento è avvenuto prima dell’avvio del processo di scrittura nel disco però i dati sono rimasti inalterati