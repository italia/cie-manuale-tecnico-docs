.. _federazione:

===========
Federazione
===========
In accordo con il protocollo SAML, l'accesso al servizio di autenticazione offerto dal Ministero dell'Interno in qualitá di IdP, é consentito tramite un meccanismo preliminare di federazione che consiste nello scambio di metadata che descrivono in un modo standardizzato gli attori coinvolti, le risorse, il supporto e gli endpoint che espletano effettivamente i servizi federati.


Metadata IdP
============
CieID server è disponibile sia in ambiente di produzione che in ambiente di test/preproduzione. I metadati XML relativi ai due ambienti sono raggiungibili ai seguenti indirizzi:

- `Metadati di PRODUZIONE <https://idserver.servizicie.interno.gov.it/idp/shibboleth?Metadata>`__

- `Metadati di TEST/PRE-PRODUZIONE <https://preproduzione.idserver.servizicie.interno.gov.it/idp/shibboleth?Metadata>`__ 



Metadata SP
===========
Il Service Provider (SP) deve predisporre un file di metadata conforme allo standard SAML v2 e, tramite una apposita sezione presente nel `portale di federazione erogatori di sevizi <https://federazione.servizicie.interno.gov.it>`__, deve effettuare il caricamento del metadata sugli ambienti di pre-produzione e produzione messi a disposizione dal Ministero dell'Interno (cfr. il `Manuale operativo per i fornitori di servizi pubblici e privati <https://docs.italia.it/italia/cie/cie-manuale-operativo-docs>`__ per ulteriori dettagli sul processo di onboarding). 
É possibile, inoltre, aggiornare un metadata già federato utilizzando la medesima procedura di caricamento dei metadata prevista dal portale di federazione sopra citato. É consentito, nell'ambito di una richiesta di federazione, il caricamento di un solo file di metadata per ambiente, e ogni caricamento successivo andrà a sostituire il precedente file di metadata. 

.. note::
    Dal punto di vista tecnico i metadata, oltre a ereditare le specifiche dallo standard SAML v2, condividono gran parte della struttura con quella attualmente prevista dalle Regole Tecniche SPID. Le differenze principali riguardano sostanzialmente le informazioni aggiuntive e di censimento del Service Provider e dell'eventuale partner tecnologico che cura gli aspetti tecnici di onboarding. Per maggiori dettagli consultare il capitolo :ref:`ciespid`

.. role:: xml(code)
    :language: xml

Struttura del metadata 
======================
 Un file metadata é strutturato gerarchicamente in un elemento radice :xml:`<md:EntityDescriptor>` (un server SAML che esegue determinati compiti per conto ad esempio di un SP) e uno o piú elementi ad esso associato. Diversamente da quanto prescritto dallo standard SAML v.2, che consente, tramite l'elemento radice :xml:`md:EntitiesDescriptor>`, di inserire in un unico metadata più :xml:`<md:EntityDescriptor>`, lo schema "Entra con CIE" prevede obbligatoriamente un solo elemento :xml:`<md:EntityDescriptor>` come radice del metadata.

.. code-block:: xml
    :linenos:
    
    <md:EntityDescriptor xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" entityID="https://service-provider.it/sp">
       [...] 
    </md:EntityDescriptor>

.. note::   
    Ogni entità (Service Provider, Identity Provider, o altra), in una federazione SAML, è tecnicamente identificata e distinta dalle altre entità mediante il suo *EntityID*: una stringa che valorizza l'attributo :xml:`entityID` obbligatorio dell'elemento radice di ogni metadata SAML. 
    Il Service Provider è responsabile della scelta di un *EntityID* **unico** per ciascun metadata. Metadata multipli afferenti al medesimo *EntityID* sono rifiutati.
    Si consiglia di utilizzare, come *EntityID* un URL in https, non eccedente 1024 caratteri, afferente al dominio del soggetto del metadata SAML. 

L'elemento :xml:`<EntityDescriptor>` costituisce il contenitore al cui interno si trovano le regole e le direttive valide solo ed esclusivamente per quel singolo soggetto. É strutturato internamente come una sequenza di elementi tra i quali il Service Provider deve necessariamente inserire i seguenti, tutti **obbligatori** (e presenti *una e una sola sola volta*, salvo ove espressamente indicato): 

    - :xml:`<Signature>` riportante il sigillo elettronico apposto sul metadadata stesso. Per ulteriori informazioni si faccia riferimento al capitolo sull':ref:`infrastruttura a chiave pubblica <pki>`.

    - :xml:`<SPSSODescriptor>` contenente le principali informazioni su chiavi crittografiche, URL e binding. Tale elemento descrive il ruolo del Service Provider e gli *endpoint* tecnici che espone verso gli altri soggetti della federazione.

    - :xml:`<Organization>` in cui sono indicati le stringhe identificative dell'organizzazione a cui afferisce il soggetto del metadata.

    - Una o due istanze :xml:`<ContactPerson>` in cui sono indicati ulteriori dati identificativi del soggetto cui il metadata SAML si riferisce e di un eventuale soggetto che, in veste di partner tecnologico, ne cura gli aspetti tecnici di federazione, sviluppo e messa in esercizio. I dati contenuti nelle istanze :xml:`<ContactPerson>` sono regolamentati più avanti.

Si consiglia che i *namespace* XML rilevanti per il metadata SAML (sopratutto quelli utilizzati in più punti del metadata) siano indicati *una tantum* nell'elemento radice dello stesso.

.. code-block:: xml
    :linenos:
    
    <md:EntityDescriptor 
      xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" 
      xmlns:ds="http://www.w3.org/2000/09/xmldsig#" 
      entityID="https://service-provider.it/sp">   
        <ds:Signature> [...] </ds:Signature>
        <md:SPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
            [...]
        </md:SPSSODescriptor>
        <md:Organization> [...] </md:Organization>
        <md:ContactPerson> [...] </md:ContactPerson>
        [...]
    </md:EntityDescriptor>

--------------------
Sigillo sui metadata
--------------------
Per ulteriori informazioni si faccia riferimento al capitolo sull':ref:`infrastruttura a chiave pubblica <pki>`.


--------------------------------------------
Descrittori di ruolo per il Service Provider
--------------------------------------------
Le informazioni tecnicamente più rilevanti sono contenute nell'elemento :xml:`<md:SPSSODescriptor>`:

    - la chiave o le chiavi pubbliche utilizzate dal SP per l'autenticazione durante la fase di scambio dei messaggi previsti dal protocollo SAML (per ulteriori informazioni si faccia riferimento al capitolo sull':ref:`infrastruttura a chiave pubblica <pki>`);
    - gli URL degli *endpoint* dei servizi tecnicamente esposti dal SP verso gli altri soggetti della federazione;
    - elenco delle "categorie di attributi" (*attribute set*) SAML che il SP può richiedere all'Identity Provider (IdP).

.. note::

    In merito a gli attributi richiesti dal SP si precisa che la versione attuale del IdP può accogliere solo richieste realtive alla categoria di attributi *Minimum eIDAS Dataset* (nome, cognome, data di nascita e codice fiscale). 

Gli attributi dell'elemento :xml:`<SPSSODescriptor>` che **devono** essere presenti sono:

    - :xml:`protocolSupportEnumeration`: indica il protocollo SAML supportato che nel caso di Entra con CIE é SAML v2.0 e che deve quindi necessariamente essere valorizzato con la stringa :code:`urn:oasis:names:tc:SAML:2.0:protocol`;  
    - :xml:`AuthnRequestsSigned`: booleano che indica se le richieste di autenticazione sono sigillate elettronicamente o meno; **deve** essere valorizzato con :code:`true`;
    - :xml:`WantAssertionsSigned`: booleano che indica se il SP si aspetta che le asserzioni SAML contenute nella risposta di autenticazione siano sigillate elettronicamente o meno; **deve** essere valorizzato con :code:`true`.

Gli elementi che sono contenuti all'interno dell':xml:`<SPSSODescriptor>` [e la loro cardinalità] sono riportati di seguito:
    
    - :xml:`<KeyDescriptor>` [uno o più]; 
    - :xml:`<SingleLogoutService>` [uno o più];
    - :xml:`<NameIDFormat>` [facoltativo, al massimo uno];
    - :xml:`<AssertionConsumerService>` [uno o più];
    - :xml:`<AttributeConsumingService>` [uno o più];
    - :xml:`<Extensions>` [al massimo uno]: Elemento **facoltativo**, riservato ad estensioni SAML relative a funzionalità aggiuntive del SP.


KeyDescriptor
-------------
Ciascun elemento :xml:`<KeyDescriptor>` contiene una chiave crittografica pubblica utilizzata per le seguenti azioni sui messaggi inviati dal SP:

    - apposizione di sigilli elettronici (attributo :xml:`use` valorizzato con :code:`signing`),
    - cifratura (attributo :xml:`use` valorizzato con :code:`encryption`).

Lo schema *Entra con CIE* prevede che ogni SP dichiari **almeno una** chiave pubblica ":code:`signing`", cioè da utilizzare per apporre sigilli elettronici sulle proprie richieste di autenticazione SAML (*request*).
All'interno di ciascun :xml:`<KeyDescriptor>` è presente un elemento :xml:`<KeyInfo>`, conforme con lo standard `XML Signature Syntax and Processing <https://www.w3.org/TR/xmldsig-core2/>`__ del `W3C <https://www.w3.org>`__. Gli algoritmi crittografici da utilizzare sono descritti nel capitolo relativo all':ref:`infrastruttura a chiave pubblica <pki>`.

SingleLogoutService
-------------------
Per facilitare la compatibilità con i successivi metodi di log-out, deve essere presente *almeno un* elemento :xml:`<SingleLogoutService>`. Ciascun elmento deve contenere i seguenti attributi:
    
    * :xml:`Location`: riporta la URL (in https) all'*endpoint* del servizio per la ricezione delle richieste di *single logout*;
   
    * :xml:`Binding`: descrive il tipo di binding e può assumere uno dei seguenti valori:

        - :xml:`urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect`, (almeno un'istanza **deve** avere questo metodo per lo schema *Entra con CIE*);
        - :xml:`urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST`;
        - :xml:`urn:oasis:names:tc:SAML:2.0:bindings:SOAP`.

Come specificato nella sezione :ref:`logout`, l'IdP server non prevede, attualmente, un meccanismo di *single logout SAML*.

NameIDFormat
------------
L'elemento :xml:`<NameIDFormat>` specifica il formato con cui vengono gestiti i :xml:`<NameID>` nell'ambito del protocollo SAML per identificare il soggetto a cui si riferisce un'asserzione. In particolare, nel caso specifico di Entra con CIE, se presente tale elemento deve essere valorizzato come :xml:`urn:oasis:names:tc:SAML:2.0:nameid-format:transient`, per indicare che le informazioni hanno una validitá transitoria e riferita solo alla specifica sessione di autenticazione.

Assertion Consumer Service
--------------------------
Deve essere presente **almeno una** istanza *Assertion Consumer Service* (**AsCS** - elemento :xml:`<AssertionConsumerService>`) dove verrà mappata la URL per l'invio delle risposte SAML, occorre riportare i seguenti attributi (tutti obbligatori salvo ove espressamente indicato):
    
    - :xml:`Binding`: valorizzato alternativamente con:

        - :code:`urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST`;
        - :code:`urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect`;
    
    - :xml:`Location`: URL in https dell'*endpoint* del servizio per la ricezione delle risposte di autenticazione;
    
    - :xml:`index`: valorizzato con un numero intero non-negativo che identifica *univocamente* il AsCS in fase di richiesta di autenticazione;

    - :xml:`isDefault`: **facoltativo**, è valorizzato con un booleano che indica quale sia il AsCS di default;
    
.. note::
    
    **un solo** AsCS (solitamente quello con :xml:`index` pari a :code:`0`) può avere l'attributo :xml:`isDefault` valorizzato con :code:`true` (tutti gli altri AsCS possono omettere questo attributo, oppure valorizzarlo con :code:`false`); 

Attribute Consuming Service
---------------------------
Deve essere presente **almeno una** istanza di *Attribute Consuming Service* (**AtCS** - :xml:`<AttributeConsumingService>`) che descrive la categoria di attributi (*attribute set*) richiesti dal SP. L'elemento contiene un unico attributo:
        
    - :xml:`index`: valorizzato con un numero intero non-negativo che identifica *univocamente* l'*attribute set*;

All'interno di ciascun AtCS sono presenti i seguenti elementi [indicati con la loro cardinalità]:

    - :xml:`<ServiceName>` [uno], contenente un identificativo della classe di servizi relativo all'*attribute set* o, in alternativa un *UUID v.4* dell'*attribute set* richiedibile dal SP, comprensivo dell'attributo :xml:`xmlns:lang`, valorizzato con una stringa vuota.

    - :xml:`<ServiceDescription>` [zero o più], ciascuno contenente una descrizione testuale dell'*attribute set*.
    
       - Ciascun istanza di questo elemento presenta l'attributo :xml:`xmlns:lang`, valorizzato con il codice ISO 639 della lingua in cui è scritta tale descrizione.

    - :xml:`<RequestedAttribute>` [uno o più], ciascuno contenente i seguenti attributi
    
       - :xml:`Name` (*obbligatorio*) -- il nome tecnico dell'attributo da richiedere (senza spazi);
       - :xml:`NameFormat` (*facoltativo*) -- il formato con cui ci si aspetta venga restituito l'attributo; se presente, è valorizzato con la seguente *alternativa*:

          - :code:`urn:oasis:names:tc:SAML:2.0:attrname-format:basic`,
          - :code:`urn:oasis:names:tc:SAML:2.0:attrname-format:uri`,

I soli *attribute set* utilizzabili come AtCS per lo schema *Entra con CIE* sono quelli che comprendono gli attributi previsti nel *Minimum Dataset eIDAS*:

    - :code:`name` (tipo :xml:`xsd:string`) **nome** della persona fisica; 
    - :code:`familyName` (tipo :xml:`xsd:string`) **cognome** della persona fisica;
    - :code:`dateOfBirth` (tipo :xml:`xsd:string`) **data di nascita** della persona fisica; 
    - :code:`fiscalNumber` (tipo :xml:`xsd:string`) **codice fiscale** della persona fisica.

.. code-block:: xml
    :linenos:
    
    <md:AttributeConsumingService index='0'>
        <md:ServiceName xml:lang=''>urn:uuid:bc212b14-d920-4052-900d-86cc5ab48a3a</md:ServiceName>
        <md:RequestedAttribute Name='name' NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic"/>
        <md:RequestedAttribute Name='familyName' NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic"/>
        <md:RequestedAttribute Name='dateOfBirth' NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic"/>
        <md:RequestedAttribute Name='fiscalNumber' NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic"/>  
    </md:AttributeConsumingService>

---------------------------------------------
Informazioni aggiuntive del Service Provider
---------------------------------------------
L'elemento :xml:`<md:Organization>` indica alcune informazioni prioritarie circa la persona giuridica Service Provider.
La lingua utilizzata per valorizzare queste informazioni (**obbligatoria** almeno la lingua italiana) è indicata in ciascuno degli elementi figli mediante la valorizzazione del codice ISO 639 della lingua nell'attributo :xml:`xmlns:lang`.
Ciascuna lingua è indicata con un'istanza della terna *completa* dei seguenti elementi: 
    
    - :xml:`<OrganizationName>`: Nome completo del SP, così come compare nei pubblici registri, con il corretto uso di maiuscole, minuscole, accenti e altri segni diacritici (p.es. :code:`Istituto Nazionale Previdenza Sociale - INPS`);
    - :xml:`<OrganizationDisplayName>`: Denominazione del SP - eventualmente senza l'esplicitazione di acronimi (p.es. :code:`INPS`). Il valore di questo elemento è utilizzato dall'Identity Provider per mostrare all'utente (nella schermata di autenticazione) il SP a cui stanno per essere inviati gli attributi richiesti.
    - :xml:`<OrganizationURL>`: La URL di una pagina web del sito istituzionale dell'organizzazione (con la lingua dei testi della pagina corrispondente a quanto ripotato nel corrispondente attributo :xml:`xmlns:lang`).

.. code-block:: xml
    :linenos:
    
    <md:Organization>
        <md:OrganizationName xml:lang="it">Istituto Service Provider</md:OrganizationName>
        <md:OrganizationName xml:lang="en">Service Provider Institute</md:OrganizationName>
        <md:OrganizationDisplayName xml:lang="it">ISP</md:OrganizationDisplayName>
        <md:OrganizationDisplayName xml:lang="en">SPI</md:OrganizationDisplayName>
        <md:OrganizationURL xml:lang="it">https://www.isp.it</md:OrganizationURL>
        <md:OrganizationURL xml:lang="en">https://www.isp.it</md:OrganizationURL>
    </md:Organization>

--------------------------------------------
Informazioni di censimento e contatto
--------------------------------------------
Il metadata contiene *una o due* istanze di elementi :xml:`<ContactPerson>`, entrambe dotate di attributo :xml:`contactType`:

   - nel caso di Service Provider autonomi (il cui referente tecnico è cioè "interno" al SP), vi è *una* sola istanza con :xml:`contactType` pari a :code:`administrative`;
   - nel caso di soggetti che si affidano ad un partner tecnologico "esterno" come referente tecnico, vi sono *due* simili istanze: 
    
        - la prima ha il :xml:`contactType` valorizzato come :code:`administrative` (con le informazioni identificative del SP, cui afferisce il proprio *referente amministrativo*);
        - l'altra con il :xml:`contactType` valorizzato come :code:`technical` (e contenente le informazioni identificative del partner tecnologico, cui afferisce il *referente tecnico* del SP).

I sopraelencati elementi :xml:`<ContactPerson>` sono così valorizzati:

    - :xml:`<Extensions>` *obbligatoria*, contenente i seguenti elementi, tutti che utilizzano il *namespace* XML di CIE ( https://www.cartaidentita.interno.gov.it/saml-extensions ):

       - Un'*alternativa obbligatoria* tra i seguenti due elementi "vuoti":

          - :xml:`<Public/>` per le **PP.AA.**,
          - :xml:`<Private/>` per i soggetti **privati**;

       - :xml:`<IPACode>` *obbligatorio* per le **Pubbliche Amministrazioni** (PP.AA.) e i Gestori di Pubblici Servizi, è valorizzato con il **codice IPA** così come risultante dall'`Indice PA <https://www.indicepa.gov.it>`__ (IPA); ad esempio, :code:`ipzsspa` (Istituto Poligrafico e Zecca dello Stato S.p.A.);
       - :xml:`<IPACategory>` valorizzato *facoltativamente* per le **PP.AA.** e gli altri soggetti iscritti ad `IPA <https://www.indicepa.gov.it>`__, è valorizzato con la sua `Categoria IPA <https://www.indicepa.gov.it/public-services/docs-read-service.php?dstype=FS&filename=Categorie_Amministrazioni.pdf>`__; ad esempio, :code:`L6` (Comuni italiani) ovvero :code:`L37` (Gestori di Pubblici Servizi).
       - :xml:`<VATNumber>` *obbligatorio* per soggetti **privati** dotati di partita IVA (e *facoltativo* altrimenti), è valorizzato con il numero di **partita IVA** (o *VAT Number* internazionale), comprensivo del codice ISO 3166-1 α2 del Paese di appartenenza, *senza* spazi; ad esempio, :code:`IT12345678901`.
       - :xml:`<FiscalCode>` *obbligatorio* per i soggetti **privati** (e *facoltativo* altrimenti), è valorizzato con il **codice fiscale** della persona giuridica; ad esempio: :code:`12345678901`.  
       - :xml:`<NACE2Code>` (uno o più) *obbligatorio* per i soggetti **privati** (e *facoltativo* per tutti gli altri, se ne sono dotati), è valorizzato con il `codice ATECO <https://www.istat.it/it/archivio/17888#valori>`__ del soggetto; in caso di soggetti esteri (pubblici e privati), è sempre facoltativo e valorizzato con il `codice NACE (rev. 2) <https://ec.europa.eu/eurostat/ramon/nomenclatures/index.cfm>`__ (dal quale sono declinati i codici ATECO per l'Italia); ad esempio :code:`12.34.56`. In caso si possieda più codici ATECO o NACE, questi possono essere inseriti mediante istanze mutliple delle'elemento (ciascuna contenente un unico codice)
       - :xml:`<Municipality>` *obbligatorio*, è valorizzato con il `codice ISTAT del Comune <https://www.istat.it/storage/codici-unita-amministrative/Elenco-comuni-italiani.xls>`__ (anche detto "codice Belfiore" - tutto in maiuscolo) ove ha la sede legale il soggetto; nel caso di soggetti esteri, *se presente*, è valorizzato con lo *Zip code* della sede legale; ad esempio :code:`H501` (Roma), :code:`W1F 9AS` (Quartiere Soho di Londra).
       - :xml:`<Province>` *facoltativo*, è valorizzato con la sigla automobilistica della Provincia (tutta in maiuscole) dove si trova la sede legale del soggetto; ad esempio :code:`MI`; in caso di soggetti esteri, *se presente*, è valorizzato con :code:`EE`.
       - :xml:`<Country>` *obbligatorio* per soggetti **esteri** (e *facoltativo* altrimenti), è valorizzato con il codice ISO 3166-1 α2 del Paese ove è situata la sede legale del soggetto; ad esempio :code:`IT` (Italia).  
       - Ulteriori estensioni previste dal Sistema Pubblico delle Identità Digitali (*SPID*), anche se ignorate dallo schema *Entra con CIE*. 

    - :xml:`<Company>` *obbligatorio* e valorizzato con il nome completo del soggetto. Nel caso delle istanze relative al Service Provider (cioè nel caso di istanza con attributo :xml:`contactType` valorizzato come :code:`administrative`) tale elemento deve essere valorizzato *esattamente* come l'elemento :xml:`<OrganizationName>` (nell'istanza della lingua del Paese dell'organizzazione) presente nell'antenato indiretto :xml:`<Organization>`;
    - :xml:`<EmailAddress>` *obbligatorio* e valorizzato con l'indirizzo di una casella email istituzionale (preferibilmente *non* PEC) per comunicare istituzionalmente con il Service Provider. L'indirizzo email **deve** coincidere con quello indicato in fase di richiesta di adesione riferito, a seconda della tipologia del soggetto cui si riferisce l'elemento :xml:`<ContactPerson>`, al referente amministrativo o tecnico.
    - :xml:`<TelephoneNumber>` *facoltativo* e valorizzato con il numero di telefono (dotato di prefisso internazionale, *senza* spazi - ad esempio :xml:`+39061234567`) per comunicare con il  soggetto cui si riferisce l'elemento :xml:`<ContactPerson>`. **Non** deve essere un numero telefonico personale.


.. note::
    Nella compilazione degli elementi sopraelencati è necessario assicurarsi che le informazioni riportate siano le medesime inserite in fase di richiesta di adesione. Nel caso di esito negativo a fronte di una verifica, il metadata non sarà considerato valido ai fini della federazione. È importante sottolineare che la modalità di compilazione delle informazioni di censimento appena descritte può essere differente rispetto a quanto previsto per lo schema di identificazione SPID in quanto essa riflette le differenti procedure amministrative previste dagli schemi "Entra con CIE" e SPID in relazione alle rispettive fasi di onboarding. 

---------------
Estensioni SAML
---------------
Gli elementi :xml:`<Extensions>` opzionalmente presenti nei metadata SAML servono a contenere estensioni proprietarie -- dello schema *Entra con CIE* o relative ad altri schemi di identificazione elettronica (quali ad esempio *SPID*).
Le implementazioni tecniche che non "riconoscono" particolari ulteriori estensioni oltre a quelle dello schema *Entra con CIE*, **devono** ignorarle (fintanto che siano rappresentate in una sintassi XML formalmente corretta) senza produrre condizioni di errore. 

-------------------
Esempio di metadata
-------------------
Di seguito si riporta un esempio di metadata per un Service Provider privato che si presenta autonomamente (senza un partner tecnologico "esterno"). Questo esempio include i soli elementi obbligatori previsti dal presente manuale.


.. literalinclude:: metadata_strict_sp_private.xml
   :language: xml
   :linenos:


Di seguito si riporta un esempio di metadata per un Service Provider (nell'esempio pubblico) che si presenta per tramite di un partner tecnologico (nell'esempio privato) che funge da referente tecnico "esterno" al SP. Questo esempio include, oltre agli elementi obbligatori, anche alcuni di quelli opzionali.

.. literalinclude:: metadata_full_sp_pubblico_tp_private.xml
   :language: xml
   :linenos:
