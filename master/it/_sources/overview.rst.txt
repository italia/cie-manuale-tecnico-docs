==============================
Soluzione eID basata sulla CIE
==============================

Lo schema di autenticazione “\ *Entra con CIE*\ ” segue il modello
dell'identità federata e dunque prevede l'introduzione di un Identity Provider (IdP), al quale i
fornitori di servizi online, Service Provider (SP), richiedono, previa
federazione, la verifica dell'identitá dell'utente.

In particolare, lo schema prevede l'istituzione di un IdP unico, il Ministero
dell'Interno, che in qualità di ente responsabile dell'emissione della
CIE, ne cura anche gli aspetti legati all'impiego del documento e delle
credenziali di livello basso e significativo ad esso connesso, come
strumento di identità digitale. Di seguito viene mostrato uno schema
logico della soluzione eID basata sulla CIE.

.. figure:: ../../media/image1.png
    :alt: Schema di autenticazione entra con CIE.
    :width: 15 cm
    :name: schema-cie
    :align: center



L'accesso mediante la CIE ai servizi erogati in rete dalle PP.AA. è reso
possibile attraverso il CieID Server, sito presso il Ministero
dell'Interno. Tale componente
server, che si configura tanto come un server SAML 2.0 che come un
OpenID Provider (OP), è realizzato e manutenuto dal Poligrafico e Zecca
dello Stato S.p.A. (PZS) che riveste il ruolo di partner tecnologico del
Ministero dell'Interno. Il CieID Server svolge le seguenti funzioni:

-  Accetta richieste di autenticazione SAML o OIDC a servizi digitali
   erogati da enti federati ed inviate attraverso il protocollo HTTPS;

-  Effettua l'identificazione informatica dell'utente mediante
   l'esecuzione della fase di challenge secondo il livello di sicurezza
   richiesto dal SP ovvero scelto dall'utente;

-  Nel caso di accesso mediante livello di sicurezza “alto”, verifica la
   validità del certificato a bordo della CIE cooperando con la CA
   Autenticazione;

-  Invia gli attributi qualificati all'erogatore
   di servizio previo consenso esplicito da parte dell'utente;

-  Invia all'erogatore di servizio una asserzione di autenticazione firmata dal Ministero dell'Interno; tale
   asserzione costituisce prova di avvenuto riconoscimento dell'utente da
   parte di CieID Server e del Ministero stesso.

.. note::
   L'interazione con l'utente da parte della componente CieID Server, puó avvenire secondo diverse modalitá:

   -  **Modalitá «desktop»:** l'utente si autentica mediante un browser installato sul proprio computer. Nel caso di accesso di livello 3, utilizza la CIE mediante un lettore RF collegato al computer

   -  **Modalitá «smartphone»**: l'utente si autentica al servizio tramite un browser installato su un dispositivo mobile (smartphone o tablet) dotato dell'app CieID. Nel caso di accesso di livello 3, il dispositivo mobile deve essere necessariamente dotato di interfaccia NFC. In tale circostanza la fase di autenticazione si completa avvicinando la CIE al proprio dispositivo;

   -  **Modalitá «desktop + smartphone»**: l'utente si autentica al servizio tramite un browser installato sul proprio computer e, nel caso di accesso di livello 2 o 3 completa l'autenticazione mediante l'app CieID eventualmente avvicinando la CIE al proprio dispositivo mobile dotato di interfaccia NFC.

Lo schema Entra con CIE si realizza dunque mediante due macro-fasi
distinte:

1. richiesta del servizio esposto dal portale/app del Service Provider
   che avviene all'interno del browser dell'utente nel dominio del SP;

2. autenticazione dell'utente effettuata direttamente dall'Identity
   Provider secondo i passi riportati in precedenza.


