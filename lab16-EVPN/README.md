# EVPN
È il _control plane_ per il protocollo VXLAN. Risolve i problemi delle reti VXLAN statiche:
- MAC flooding: la rete è inondata di pacchetti MAC
- Scoperta di VTEP manuale
- Configurazione dei tunnel manuale

Esistono 5 tipi diversi di messaggio: i tipi 1 e 4 sono usati per il piano di controllo di VM multi-homed, e perciò non verranno approfonditi

## Tipo 2
Sono messaggi scambiati tra `VTEP` per fare advertising di indirizzi ip e MAC: si vuole sincronizzare le tabelle MAC. Ogni volta che un VTEP viene a conoscenza di un nuovo host, si invia l'update. Più nel dettaglio, può essere usato in due situazioni:
- ARP broadcast suppression: quando una VM deve inviare un pacchetto, invia una richiesta ARP in broadcast nel modo classico. Quando questa richiesta raggiunge il VTEP, questo risponde con il MAC address corrispondente. Infatti questa informazione è presente nella sua tabella perché quando un nuovo dispositivo EVPN si connette, automaticamente invia l'informazione tramite un _gratuitous ARP_, cioè un pacchetto ARP autonomo. Inoltre, l'informazione è propagata agli altri VTEP tramite un annuncio di tipo 2 triggerato automaticamente alla ricezione di questa informazione
- VM migration: quando una VM cambia il VTEP dietro cui è connesso, automaticamente, come detto, informerà il nuovo VTEP che a sua volta informerà gli altri. Quando questo aggiornamento raggiunge il VTEP a cui precedentemente era connessa la VM, questo invierà un ARP probe a quell'indirizzo per verificare se è ancora connessa e l'informazione ricevuta è valida. Se la richiesta fallisce, i routes vengono ritirati.

## Tipo 3
Servono a scambiare L2VNI e indirizzi IP dei VTEP e, di conseguenza, permettono il discovery automatico dei nuovi VTEP e la creazione automatica dei tunnel tra di essi.

## Tipo 5
Servono a fare advertisement di prefissi IP di qualsiasi lunghezza e a condividere informazioni relative a L3VNI. Sono quindi usati per il routing tra subnet L2VNI dello stesso tenant.


## Lab
<!--todo-->