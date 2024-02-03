+++
author = "Cristian Porzio"
title = "Progetto di Ingegneria del Software"
date = "2024-01-20"
description = "Documentazione del progetto e parere personale"
tags = [
    "blog",
]
categories = [
    "Post", "Coding"
]
series = ["Progetti"]
image = "lamp.jpg"
+++

<!--more-->
## Scopo del Progetto
Ciao a tutti! In questi mesi io ed i miei colleghi abbiamo lavorato molto su un progetto di Ingegneria del Software incentrato sulla logopedia. In breve, abbiamo sviluppato un sito web che permette ai logopedisti di assegnare esercizi di 6 tipologie:
- Cruciverba.
- Riconoscimento parole corrette.
- Lettura del testo.
- Riconoscimento e lettura di oggetti dalle immagini.
- Riconoscimento e scrittura di oggetti dalle immagini.
- Riconoscimento e associazione dell'immagine all'apposito testo.
Ed ovviamente permette ai singoli pazienti di eseguirli e ricevere una correzione valutata dal nostro sistema. 
La cernita di questi esercizi sono stati sviluppati assieme ad un team di studenti e logopedisti che si sono gentilmente offerti di aiutarci.

Non finisce qui:
- I Pazienti possono prenotarsi sulle date disponibili presenti nell'agenda del proprio Logopedista.
- I Pazienti e Logopedisti dispongono di una chat per lo scambio di messaggi.
- I Pazienti e Logopedisti dispongono di una chat speciale per il ricevimento di notifiche del sistema.
- I Pazienti sono invitati dai propri Logopedisti al sistema. Riceveranno via email un PIN da utilizzare in fase di registrazione.
- I Logopedisti per registrarsi al sistema necessitano di una Licenza (acquistata separatamente).
- Pazienti e Logopedisti possono accettare la telemetria del sistema che salverà le loro abitudini di utilizzo per fini scientifici.
- I Pazienti e Logopedisti possono modificare i propri dati personali
- È prevista una procedura di recupero della password nella pagina di Login.
- È prevista una procedura di cambio password nella pagina del proprio profilo.

Per conoscere le tecnologie utilizzate, raccomandiamo la visualizzazione di [GitHub](https://github.com/pastore99/TalkAId/tree/main) e del Manuale d'Installazione


### Documentazione
Trovi tutti i [file del progetto e la documentazione qui](https://github.com/pastore99/TalkAId/tree/main).

Qui l'[analisi di SonarCloud](https://sonarcloud.io/summary/overall?id=panuozzo77_TalkAId).

##### Documento Criteri Rispettati
{{< pdfReader "2023_C16_DCR_ver1.0.pdf" >}}

##### Manuale Utente
{{< pdfReader "2023_C16_MU_ver1.3.pdf" >}}

##### Manuale Di Installazione
{{< pdfReader "2023_C16_MDI_ver1.1.pdf" >}}

##### Requirements Analysis Document
{{< pdfReader "2023_C16_RAD_ver2.6.pdf" >}}

##### System Design Document
{{< pdfReader "2023_C16_SDD_ver1.7.pdf" >}}

##### Object Design Document
{{< pdfReader "2023_C16_ODD_ver1.7.pdf" >}}

##### Test Plan
{{< pdfReader "2023_C16_TP_ver1.5.pdf" >}}

##### Test Case Specification
{{< pdfReader "2023_C16_TCS_ver1.3.pdf" >}}

##### Test Incident Report
{{< pdfReader "2023_C16_TIR_ver1.2.pdf" >}}

##### Test Summary Report
{{< pdfReader "2023_C16_TSR_ver1.1.pdf" >}}

## Riflessioni
È stato uno dei progetti che ha richiesto il maggior tempo tra quelli che ho affrontato in questo corso di laurea. Precisamente, abbiamo iniziato il 20/10/2023 e consegnato il 20/01/2023. Il dover studiare gli argomenti, contemporaneamente applicarli al proprio dominio d'interesse e possedere un buon occhio critico sulla documentazione e sul codice è stato snervante e soprattutto molto denso da buttare giù. Abbiamo dovuto valutare quando e quanto fosse necessario fare le cose fatte bene e quando invece se ne poteva fare a meno, dovendo fare i conti tra: meetings, revisioni su GitHub pre-pull-requests, pair-programming, imprevisti, codice ribelle che smette di funzionare, irreperibilità in alcune fasce orarie dei team member e professori che cambiano il flusso di lavoro in corso d'opera. Abbiamo lavorato tanto e ci siamo sacrificati ancor di più per la buona riuscita di questo progetto.  

Sono molto fiero dell'utilizzo che abbiamo fatto di GitHub, applicando la metodologia del Trunk Based Development abbiamo avuto una gestione meno caotica del codice e molti errori potevano essere risolti prima di introdurre il nuovo codice nel Trunk principale. Anche se tutto non è stato rosa e fiori coff coff...

## Ringraziamenti
Si ringraziano i **Project Manager:** Carmine Pastore e Nicola Laurino ed i **Team Member:** Raffaele Monti, Samuele Sparno, Luigi Salvatore Pio Petrillo, Anna Benedetta Salerno, Michele D'Arienzo per la realizzazione di questo stupendo progetto.


