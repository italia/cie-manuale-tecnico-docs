.. _ciespid:

===========================
Interoperabilità con SPID
===========================

.. role:: xml(code)
  :language: xml

Lo schema di identificazione *Entra con CIE* pur avendo molte analogie con lo schema di identificazione SPID, differisce da quest'ultimo su alcuni aspetti che riguardano:

    - la predisposizione dei metadata 
    - i protocolli di comunicazione SAML.

È importante sottolineare che, per quanto riguarda in paticolare i metadata, le differenze di natura tecnica riflettono i diversi iter amministrativi previsti da AgID nei casi di Soggetti Aggregatori e Gestori di Pubblico Servizio, che nel caso di *Entra con CIE* sono gestiti all'interno del medesimo processo amministrativo di onboarding. In altre parole, la figura dell'Aggregatore (di servizi pubblici o privati) e quella di Gestore di Pubblico Servizio non sono previste nello schema *Entra con CIE*. 

---------
Metadata 
---------

In riferimento ai metadata, diversamente da quanto previsto per SPID, lo schema *Entra con CIE* prevede un unico modello di metadata indipendentemente dal ruolo che il soggetto svolge nell'ambito dello schema SPID. In particolare i due elementi nei quali si hanno maggiori impatti sono:

    - l'elemento :xml:`<AttributeConsumingService>` che contiene il set di attributi richiesti in fase di autenticazione prevede, attualmente, solo e soltanto gli attributi relativi al *Minimum Dataset eIDAS* o suoi sottoinsiemi;
    - l'elemento :xml:`<ServiceName>` può contenere un *UUID v.4* dell'*attribute set* richiedibile dal SP, comprensivo dell'attributo :xml:`xmlns:lang`, valorizzato con una stringa vuota;
    - l'elemento :xml:`<md:Organization>` che contiene i dati del Service Provider in veste di persona giuridica;
    - gli elementi :xml:`<ContactPerson>` che dovranno contenere le informazioni di censimento e contatto del Service Provider e dell'eventuale partner tecnologico (cfr. :ref:`federazione`).

I dati identificativi del Service Provider e del partner tecnologico devono coincidere con quelli inseriti in fase di richiesta di adesione. Per maggiori dettagli consultare il capitolo :ref:`federazione`. 


----------------------------
Protocolli di comunicazione
----------------------------

I protocolli di comunicazione previsti da entrambi gli schemi (*Entra con CIE* e SPID) sono basati sullo standard SAML versione 2.0 e, dunque, ereditano da esso le principali specifiche tecniche. Tuttavia nella modalità specifica secondo la quale gli schemi di identificazione sono declinati è possibile individuare alcune lievi differenze che possono avere un impatto sull'implementazione da parte del Service Provider. 

Nella costruzione della richiesta di autenticazione :xml:`<AuthnRequest>` è necessario valorizzare l'attributo :xml:`Destination` coerentemente con l'attributo :xml:`Location` presente nel tag :xml:`SingleSignOnService` riportato nel metadata dell'IdP e relativo al particolare binding utilizzato in fase di richiesta (cfr. :ref:`protocolli` e :ref:`binding` per ulteriori dettagli). 

Per quanto riguarda il parsing e la verifica dei messaggi di *response*, il Service Provider deve tenere conto che, diversamente da quanto previsto da SPID,

    - l'attributo :xml:`Format` dell'elemento :xml:`<samlp:Issuer>` non è presente;
    - l'elemento :xml:`<saml:AuthnContextClassRef>` è valorizzato sempre con il valore :code:`https://www.spid.gov.it/SpidL3**`;
    - gli attributi inviati in risposta alla richiesta di autenticazione corrispondono sempre al *Minimum Dataset eIDAS* e non prevedono, nella versione attuale, l'invio di ulteriori attributi quali ad esempio lo *spidCode*. 