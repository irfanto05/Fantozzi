# Intro

Questo semplice script realizzato in Python è stato creato per monitorare il numero di processi caricati e attivi, la Ram occupata e la Ram libera di un sistema operativo Windows su cui è stata abilitata la funzionalità snmp.

# Attivare Agent SNMP su Windows 10
Per prima cosa è necessario abilitare la funzionalità SNMP sul pc che si vuole monitorare, per farlo bisogna seguire i seguenti passi:
- 1 Fare click su "Start"
- 2 Cliccare su "Impostazioni"
- 3 Clicare su "App"
- 4 Selezionare sulla destra "App e Funzionalità"
- 5 Cliccare su "Funzionalità facoltative"
- 6 Cercare nell'elenco Simple Network Management Protocol e attendere l'installazione

Una volta che il servizio sarà abilitato bisognerà configurarlo:

- 1 Fare click su "Start"
- 2 Digitare "Servizi" e cliccarci
- 3 Cercare "Servizio SNMP" nell'elenco
- 4 Fare click destro col mouse
- 5 Selezionare "proprietà"
- 6 Nella sezione "Sicurezza" disattivare la spunta "Invia Trap di autenticazione"
- 6-1 Sempre nella sezione "Sicurezza" cliccare sul tasto "aggiungi" sotto "Community" 
- 6-2 Nella finestra che si aprirà selezionare i diritti di "Lettura/scrittura"
- 6-3 Digitare il nome della community che si vuole attribuire (da usare poi come input al parametro communuty dello script)
- 6-4 Cliccare su "aggiungi"
- 7 Cliccare su "applica"
- 8 Chiudere le proprietà e cliccare nuovamente col tasto destro del mouse su "Servizio SNMP"
- 9 Cliccare su "Riavvia"

A questo punto il sistema che si vuole monitorare sarà configurato per ricevere le richieste da parte di un altro sistema su cui sarà in esecuzione lo script snmp_monitorRamProcesses.py

# Installazione su Ubuntu
Per poter utilizzare il software è necessario installare i seguenti pacchetti su Ubuntu:
- Python 3.9
- easysnmp
- rrdtool

con i seguenti comandi bash:

```bash
sudo apt-get install libsnmp-dev snmp-mibs-downloader gcc python-dev
pip3 install easysnmp
pip3 install rrdtool
```
Lo script utilizza il Mib HOST-RESOURCES-MIB (.1.3.6.1.2.1.25)
Inoltre per evitare errori come:

```bash
easysnmp.exceptions.EASYSNMPUnknownObjectIDError: unknown object id (hrStorageSize)
```
Bisognerà andare a modificare il file in "/etc/snmp/snmp.conf" aprendolo con un editor di testo e modificare la riga "mibs: " commentandola
da:
```bash
# As the snmp packages come without MIB files due to license reasons, loading
# of MIBs is disabled by default. If you added the MIBs you can reenable
# loading them by commenting out the following line.
mibs : 
```
a:
```bash
# As the snmp packages come without MIB files due to license reasons, loading
# of MIBs is disabled by default. If you added the MIBs you can reenable
# loading them by commenting out the following line.
#mibs : 
```

# Utilizzo
Per avviarlo basta digitare il comando bash:
```bash
python3 snmp_monitor_RamProcesses.py [hostname] [community]
```
Dove hostname è l'host target del monitoraggio e community è la community SNMP scelta nella configurazione dell'Agent su Windows 10

# Esempio di utilizzo
```bash
python3 snmp_monitor_RamProcesses.py 192.168.1.17 home

--> Informazioni sistema operativo: Stringa
--> Numero di utenti presenti sul sistema operativo: integer
--> Data del sistema: AAAA-MM-GG HH:MM:SS.mmmmmm
--> Ram totale del sistema: in Gb
--> Spazio totale su disco: in Gb
--> Spazio occupato su disco: in Gb
```
I grafici per Ram occupata, Ram libera e processi attivi sono nella cartella reports(generata automaticamente dallo script nella diractory in cui si trova)

Grafico per la Ram utilizzata:

![alt text](https://github.com/irfanto05/Fantozzi/blob/main/ram_graph.png)

Grafico per i Processi caricati e in esecuzione:

![alt text](https://github.com/irfanto05/Fantozzi/blob/main/process_graph.png)

Grafico per la Ram libera:

![alt text](https://github.com/irfanto05/Fantozzi/blob/main/freeRam_graph.png)

Per terminare l'esecuzione dello script basterà digitare Ctrl+C (SIGINT) sulla bash in cui è in esecuzione lo script

# Sviluppo e testing
Lo script è stato realizzato con Gedit 3.38.0 su sistema operativo Ubuntu 20.10, eseguito su macchina virtuale Oracle Virtualbox 6.1;  ed è stato testato monitorando un agent su cui è installato Windows 10 Pro e attivato il servizio snmp seguendo le istruzioni dell'introduzione.
