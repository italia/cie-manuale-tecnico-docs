.. _pki:

=====================================================
Crittografia e infrastruttura a chiave pubblica (PKI)
=====================================================

.. role:: xml(code)
    :language: xml

----------------------
Sigilli di federazione
----------------------
Tutti gli *enti federati*, cioè i soggetti che entrano nella federazione CIE - ad esempio Service Provider (SP) e Identity Provider (Id) - utilizzano *chiavi crittografiche private* sotto il loro esclusivo controllo per:

    - creare un sigillo elettronico sul metadata propro di ciascun ente federato;
    - creare sigilli elettronici sulle richieste di autenticazione (*request*) inviate dagli SP all'IdP;
    - creare sigilli elettronici sulle asserzioni (*assertion*) e sulle risposte di autenticazione (*response*) restituite dall'IdP agli SP;
    - zcifrare i messaggi scambiati con altri enti federati.

I sigilli elettronici apposti in modalità *enveloped* sulle evidenze SAML (cioè all'interno dello stesso documento XML che rappresenta il metadata, la *request* o la *response*), denominati *sigilli di federazione*, sono contenuti nell'elemento :xml:`<Signature>` in testa all'evidenza.
I certificati elettronici per:

    - la creazione di sigilli elettronici sulle *request*, le *assertion* e le *response*;
    - la cifratura dei messaggi scambiati tra enti federati;

sono contenuti, all'interno di elementi :xml:`<KeyDescriptor>` con attributo :xml:`<use>` valorizzato, rispettivamente, come :code:`<signing>` o :code:`<encryption>`.
I certificati elettronici afferenti a chiavi crittografiche utilizzate dagli enti federati per altri scopi sono contenute in ulteriori estensioni SAML dei metadata.
Tutti i sigilli di federazione **devono** essere acclusi per intero mediante elementi :xml:`<KeyInfo>` contenenti il *payload* del certificato in un elemento :xml:`<X509Certificate>`. Tali elementi rispettano lo standard `XML Signature Syntax and Processing <https://www.w3.org/TR/xmldsig-core2/>`__ del `W3C <https://www.w3.org>`__, nella versione riportata nelle specifiche SAML di riferimento per lo schema *CieID*.

----------------------------------------
Struttura dei certificati di federazione
----------------------------------------
I certificati di federazione sono conformi con quanto previsto dalla normativa *RFC 5280* e rispettano anche la normativa comunitaria ETSI in materia di sigilli elettronici avanzati.

I Service Provider possono, per il momento, adottare anche certificati generati in modalità *self-signed*. Tali certificati, però, perdono la denominazione di sigilli di federezione.

In tutti i certificati di *sigillo* elettronico (non trattandosi di certificati di *firma* elettronica), è **vietato** l'uso delle seguenti estensioni X.509 / X.520:

   - :code:`name` (OID `2.5.4.41 <http://oid-info.com/get/2.5.4.41>`__),
   - :code:`surname` (OID `2.5.4.42 <http://oid-info.com/get/2.5.4.42>`__),
   - :code:`initials` (OID `2.5.4.43 <http://oid-info.com/get/2.5.4.43>`__),
   - :code:`generationQualifier` (OID `2.5.4.44 <http://oid-info.com/get/2.5.4.44>`__),
   - :code:`familyInformation` (OID `2.5.4.64 <http://oid-info.com/get/2.5.4.64>`__),
   - :code:`pseudonym` (OID `2.5.4.65 <http://oid-info.com/get/2.5.4.65>`__).

Ulteriori estensioni possono essere presenti nei certificati, purché non vadano in contrasto con le attuali specifiche tecniche e con le norme e standard qui richiamati.

-----------------------
Algoritmi crittografici
-----------------------
Per la creazione di sigilli elettronici è utilizzato l'algoritmo crittografico RSA con lunghezza delle chiavi di almeno 1024 bit e algoritmo di *hash* SHA-256 o superiore (cioè con lunghezza dell'impronta crittografica pari almeno a 256 bit). 

Per gli scopi di cifratura dei messaggi è adottato l'algoritmo crittografico AES con lunghezza delle chiavi di almeno 256 bit.

