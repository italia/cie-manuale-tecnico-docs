==================
Assistenza tecnica
==================

.. _`assistenza`:

L'assistenza agli erogatori di servizi deve avvenire mediante uno specifico indirizzo di posta elettronica messo a disposizione dal Ministero dell'Interno. In caso di problemi di natura amministrativa o tecnica è necessario, dunque, inviare una apposita richiesta di supporto all'indirizzo cie.enti@interno.it fornendo tutte le informazioni necessarie per espletare adeguatamente l'attività di supporto, comprese le informazioni di contatto relative al soggetto richiedente e al Service Provider a cui afferisce. 

In caso di disservizio e/o problematiche di sicurezza il Ministero Interno, eventualmente avvalendosi del Poligrafico, contatterà all'indirizzo mail/telefono i referenti comunicati in fase di onboarding.

Troubleshooting 
===============

.. role:: xml(code)
  :language: xml

Prima di procedere con la richiesta di supporto ai referenti tecnici dell'Identity Provider, si consiglia di verificare la correttezza sintattica e semantica dei metadata come specificato nella sezione :ref:`federazione` nonché la corretta implementazione dei :ref:`protocolli` e delle :ref:`binding`. In particolare, al fine di agevolare le attivitá di integrazione e test dei Service Provider (SP), di seguito si riporta un elenco *non* esaustivo di potenziali problemi che possono impedire la corretta autenticazione con una indicazione relativa di troubleshooting: 

.. list-table:: Troubleshooting
  :name: tabella-troubleshooting
  :widths: 25 25
  :header-rows: 1

  * - Schermata IdP (Cie ID Server)                     
    - Troubleshooting SP
  * - *"L'applicazione a cui hai acceduto non è registrata per l'utilizzo con questo servizio."*
    - Verificare i dati di federazione o la richiesta di autenticazione. Tale errore puó essere causato dal fatto che il Service Provider sta inviando una richiesta di autenticazione nella quale il valore presente nel campo :xml:`<Issuer>` non trova corrispondenza con quello valorizzato nell'attributo :xml:`entityID` presente nei metadata
  * - *"Il servizio di accesso non è stato in grado di identificate una modalità compatibile per rispondere all'applicazione richiesta. Ciò generalmente è dovuto ad una configurazione errata dell'applicazione e dovrebbe essere segnalata al proprietario o al team di supporto dell'applicazione."*
    - Verificare che siano rispettati i requisiti relativi ai binding per la corretta trasmissione dei messaggi (:xml:`ProtocolBinding`)
  * - *"Il servizio di accesso e l'applicazione richiesta non hanno una configurazione di sicurezza compatibile e la richiesta non può essere soddisfatta."*
    - Problemi relativi alla configurazione di sicurezza. In tal caso puó essere utile verificare che sia rispettato il requisito minimo relativo al TLS (versione almeno pari a 1.2)
  * - *"La richiesta non può essere soddisfatta poiché il messaggio ricevuto non rispetta i requisiti di sicurezza del servizio di accesso."*
    - Potenziale problema sui sigilli elettronici (chiamati anche, impropriamente, "firme digitali") apposti sul metadata del SP stesso e/o sulle *request*. Occorre verificare:

        1. la presenza di un sigillo elettronico, nell'elemento :xml:`<Signature>` in testa al metadata del SP, afferente al certificato elettronico di cui al punto successivo;
        2. la validità del certificato elettronico presente nell'elemento :xml:`<Signature>` al punto precedente;
        3. la presenza, nella *request*, di un sigillo elettronico (afferente a uno dei certificati elettronici di cui al punto successivo) localizzato, *alternativamente*, nell'elemento :xml:`<Signature>` della *request*, nel caso di binding in HTTP POST, *ovvero* nel parametro :code:`Signature` della *query string* veicolante la *request*, nel caso di binding HTTP Redirect;
        4. la validitá del certificato elettronico afferente al sigillo di cui al punto precedente; tale certificato, all'interno del metadata del SP, si trova tra gli elementi :xml:`KeyDescriptor` con l'attributo :xml:`use` valorizzato con :xml:`signing`;
        5. la coerenza dell'attributo :xml:`Destination` nella *request* con l'attributo :xml:`Location` del tag :xml:`SingleSignOnService` riportato nel metadata dell'IdP in relazione al tipo di binding utilizzato per inviare la *request*.

      Per ulteriori informazioni si faccia riferimento al capitolo sull':ref:`infrastruttura a chiave pubblica <pki>`.
  * - *"Il servizio di accesso non è stato in grado di identificate una modalità compatibile per rispondere all'applicazione richiesta. Ciò generalmente è dovuto ad una configurazione errata dell'applicazione e dovrebbe essere segnalata al proprietario o al team di supporto dell'applicazione."*
    - Ciò può essere dovuto al fatto che non è possibile risolvere l'endpoint di *AssertionConsumerService*. Tale problematica si presenta, ad esempio, quando l'IdP effettua il controllo di base, previsto dal protocollo SAML, sull'AssertionConsumerServiceURL indicato nella richiesta di autenticazione e quello presente all'interno del relativo metadata

Nel caso il Service Provider, a seguito di una corretta autenticazione, non venga reindirizzato correttamente all'URL richiesta, é necessario verificare che gli attributi 

  - :xml:`AttributeConsumingServiceIndex`, 
  - :xml:`AssertionConsumerServiceURL`, 
  
siano correttamente referenziati in modo coerente con quanto riportato all'interno dei metadata (ad esempio, tramite le loro referenze numeriche, assegnate mediante gli attributi :xml:`index`). 

.. note::

  Per meglio effettuare l'attivitá di troubleshooting, si suggerisce l'utilizzo di strumenti di debug, quali ad esempio l'estensione di Google Chrome `"SAML-Tracer" <https://chrome.google.com/webstore/detail/saml-tracer/mpdajninpobndbfcldcmbpnnbhibjmch?hl=>`__, che consentono di intercettare i messaggi SAML scambiati e poter quindi effettuare le verifiche necessarie dei requisiti previsti per la corretta integrazione dello schema di autenticazione Entra con CIE.

