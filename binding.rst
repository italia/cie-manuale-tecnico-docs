.. _binding:

======================================
Modalitá di trasmissione dei messaggi
======================================

.. role:: xml(code)
  :language: xml

Il profilo SAML SSO raccomanda l'uso di TLS 1.2; lo scambio dei messaggi tra le entità della federazione può avvenire secondo due modalità:

- **HTTP-POST:**
- **HTTP-Redirect:** 


Le richieste di autenticazione (:xml:`<samlp:AuthnRequest>`) **POSSONO** essere trasmesse in entrambe le modalitá.

.. note::
    * Nel caso di binding **HTTP-POST** **SI DEVE** inserire il campo :xml:`Signature` all'interno della richiesta di autenticazione SAML :xml:`<samlp:AuthnRequest>` cosí come specificato nella sezione :ref:`protocolli` 
    * Nel caso di binding **HTTP-Redirect** il campo :xml:`Signature` **NON DEVE** essere presente nella richiesta di autenticazione SAML :xml:`<samlp:AuthnRequest>`, ma **DEVE** essere inserita nella URL come *query parameter*.

Le risposte inviate dall'Identity Provider (:xml:`<saml2:Response>`), invece, **DEVONO** essere trasmesse tramite binding **HTTP-POST**.

Binding HTTP-POST
=================

In questo tipo di binding, il messaggio HTTP contiene una form HTML all'interno della quale è codificato in formato *Base64* il costrutto SAML firmato in accordo alla specifica XML Digital Signature. Il trasporto del messaggio di richiesta di autenticazione prevede due parametri fondamentali: 

    - **SAMLRequest** o **SAMLResponse**: contengono, rispettivamente, la codifica della :xml:`<AuthnRequest>` e della :xml:`<Response>`
    - **RelayState**: indica la risorsa originariamente richiesta dall'utente e alla quale dovrà essere trasferito il controllo al termine della fase di autenticazione; puó essere un UUID, un url, un percorso di file o un mini-blob con codifica binaria *Base64*

La codifica *Base64* é richiesta solo per i messaggi del protocollo SAML in formato XML *SAMLRequest* e *SAMLResponse* e non per il *RelayState*, che è un token specifico dell'applicazione usato per identificare lo stato, la cui codifica o decodifica é in carico all'applicazione stessa.


Binding HTTP-Redirect
=====================

Quando si usa un binding di tipo Redirect, la firma viene posta nella URL come *query parameter*. Tutti i parametri sono *URL-encoded*. Il messaggio HTTP trasporta i seguenti parametri

    - :xml:`SAMLRequest`: Un costrutto SAML codificato in formato *Base64* e compresso con algoritmo *DEFLATE*. Come da specifica, il messaggio SAML non contiene la firma in formato XML *Digital Signature* esteso (come avviene in generale nel caso di binding HTTP-POST). La specifica indica come modalità alternativa quella di specificare con parametri aggiuntivi l'algoritmo utilizzato per firmare (**SigAlg**) e la stringa con la codifica *Base64* URL-encoded dei byte del messaggio SAML (**Signature**).

    - :xml:`RelayState`: Identifica il servizio originariamente richiesta dall'utente e a cui trasferire il controllo alla fine del processo di autenticazione. 

    - :xml:`SigAlg`: Identifica l'algoritmo usato per la firma prodotta secondo il profilo specificato per SAML utilizzando chiavi RSA almeno a 1024 bit e algoritmo di digest SHA-256 o superiore;
    
    - :xml:`Signature`: Contiene la firma digitale della *query string*, così come prodotta prima di aggiungere questo parametro, utilizzando l'algoritmo indicato al parametro precedente;
    


Esempio HTML di utilizzo 
========================

Di seguito un esempio di form HTML per trasferire in HTTP-POST la richiesta di autenticazione

.. code-block:: html
    :linenos:
    
    <html>
        <head>
            [...]
        </head>
        <body onload="javascript:document.forms[0].submit()">
            <form method="post" action="https://idserver.servizicie.interno.gov.it/idp/profile/SAML2/POST/SSO">
                <input type="hidden" name="SAMLRequest"
                    value="[...]">
                <input type="hidden" name="RelayState" value="...">
                <input type="submit" value="Invia"/>
            </form>
        </body>
    </html>
    
Di seguito un esempio di form HTML per trasferire la risposta in HTTP-POST

.. code-block:: html
    :linenos:
    
    <html>
        <head>
            [...]
        </head>
        <body onload="javascript:document.forms[0].submit()">
            <form method="post" action="https://service_provide_assertion_consumer">
                <input type="hidden" name="SAMLResponse"
                    value="[...]">
                <input type="hidden" name="RelayState" value="...">
                <input type="submit" value="Invia"/>
            </form>
        </body>
    </html>

La form HTML è corredata da uno script che la rende auto-postante (se Javascript è abilitato nel browser dell'utente) all'indirizzo indicato nell'attributo **action**. Quindi, il browser dell'utente elabora la risposta HTTP e invia una richiesta HTTP POST verso il servizio dell'entità destinataria.



