==============
SDK App Mobile 
==============
I Service Provider che intendono erogare i propri servizi tramite App mobile, hanno la possibilitá di integrare lo schema di autenticazione "Entra con CIE".

- *Flusso con reindirizzamento*: l'App del Service Provider, all'atto della richiesta di autenticazione dell'utente, reindirizza la richiesta all'App CieID che prende in carico la comunicazione e l'autenticazione con la CIE. 

- *Flusso integrato*: il processo di autenticazione viene effettuato direttamente all'interno dell'App del Service Provider, il quale integra la comunicazione con la CIE mediante una libreria software.  

.. figure:: ./media/img_MSC_CIE_App2App.png
   :alt: schema flusso con reindirizzamento
   :width: 15 cm
   :name: flusso-reindirizzamento

   Flusso con reindirizzamento

.. figure:: ./media/img_MSC_CIE_SDK.png
   :alt: schema flusso integrato
   :width: 15 cm
   :name: flusso-integrato

   Flusso integrato

SDK Android
===========
La versione del SDK per SO Android ,**CieID-android-sdk**, é disponibile al link https://github.com/italia/cieid-android-sdk. É costituita da una libreria software, realizzata in codice nativo Android *Kotlin*, e integra un app di esempio che descrive le diverse modalitá di integrazione dello schema "Entra con CIE":

Requisiti di integrazione
-------------------------
L'utilizzo dell'SDK presuppone che il Service Provider sia correttamente federato con l'Identity Provider e che abbia implementato i protocolli SAML v2 previsti dallo schema di autenticazione "Entra con CIE" (cfr. le sezioni relative a :ref:`federazione`, :ref:`protocolli` e :ref:`binding`).

Inoltre é necessario che i seguenti requisiti siano soddisfatti:

    - versione Android 6.0 (API level 23) o successive;

    - utilizzo di un dispositivo mobile dotato di tecnologia NFC;

    - disponibilitá di una connessione ad internet.


Configurazione 
--------------

L'IdP mette a disposizione due ambienti: uno di **preproduzione**, per gli sviluppi applicativi, e l'altro di **produzione**, per la messa in esercizio. Per tale ragione é necessaria una fase iniziale di configurazione, che dipende dal tipo di flusso integrato.

Entrambi i flussi vengono avviati tramite l'utilizzo di una *Webview*, é necessario caricare la URL del Service Provider che integra il pulsante "Entra con CIE" come mostrato nell'esempio:

.. code-block:: java
    :linenos:

    //inserire url service provider
    webView.loadUrl("URL del Service Provider")

Flusso con reindirizzamento
+++++++++++++++++++++++++++

Nel caso di *flusso con reindirizzamento*, per far proseguire correttamente il flusso, é necessario selezionare l'applicazione "CieID" a cui indirizzare le richieste di autenticazione. Ció puó essere fatto modificando i commenti dalle righe di interesse, come mostrato di seguito.

.. code-block:: java
    :linenos:

    val appPackageName = "it.ipzs.cieid"
    //COLLAUDO
    //val appPackageName = "it.ipzs.cieid.collaudo"


Flusso integrato
++++++++++++++++

Per quanto riguarda il *flusso integrato*, invece, la fase di autenticazione viene gestita dalla libreria software. In questo caso é necessario integrare il modulo "CieIDSdk":

.. code-block:: 
    :linenos:

    implementation project(path: ':cieidsdk')

L'SDK utilizza *Gradle* con strumento di build automatico. Per configurare correttamente il flusso, é necessario selezionare l'ambiente server dell'Identity Provider a cui indirizzare le richieste di autenticazione. Ció puó essere fatto modificando il file *build.gradle* modificando i commenti dalle righe di interesse, come mostrato di seguito:

.. code-block:: java
    :linenos:

    //AMBIENTI:
    //Ambiente di produzione
    //buildConfigField "String", "BASE_URL_IDP", "\"https://idserver.servizicie.interno.gov.it/idp/\""

    //Ambiente di collaudo
    buildConfigField "String", "BASE_URL_IDP", "\"https://preproduzione.idserver.servizicie.interno.gov.it/idp/\""


Modalitá di integrazione
------------------------

L'SDK fornisce un app di esempio, con 2 activity, una per flusso, per facilitare al Service Provider l'integrazione all'interno della propria App. La gestione degli errori è demandata all'app integrante.

Integrazione del flusso con reindirizzamento
++++++++++++++++++++++++++++++++++++++++++++

Per integrare nativamente le funzionalità dell'SDK é necessario, per prima cosa, intercettare la URL contenente il valore "/OpenApp" ed avviare l'App CieID integrando il codice seguente: 

.. code-block:: java
    :linenos:

    val intent = Intent()
    try {
        intent.setClassName(appPackageName, className)
        //settare la url caricata dalla webview su /OpenApp
        intent.data = Uri.parse(url)
        intent.action = Intent.ACTION_VIEW
        startActivityForResult(intent, 0)

    } catch (a : ActivityNotFoundException) {
        startActivity(
            Intent(
                Intent.ACTION_VIEW,
                Uri.parse("https://play.google.com/store/apps/details?id=$appPackageName")
            )
        )
    }
    return true

Una volta avviata correttamente l'App CieID, avviene l'autenticazione tramite la CIE, e al termine viene restituita una nuova URL da ricarica nella WebView precedente, come mostrato nell'esempio seguente:

.. code-block:: java
    :linenos:

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)

        val url = data?.getStringExtra(URL)
        webView.loadUrl(url)
    }


Integrazione del flusso integrato
+++++++++++++++++++++++++++++++++

Per integrare le funzionalità dell'SDK si utilizzano i seguenti metodi:

.. code-block:: java
    :linenos:

    //Configurazione iniziale
    CieIDSdk.start(activity, callback)
    //Avvio utilizzo NFC
    CieIDSdk.startNFCListening(activity)
    //Abilitare o disabilitare i log, da disattivare in produzione
    CieIDSdk.enableLog = true
    //Bisogna settare la url caricata dalla pagina web dell' SP dalla webview su /OpenApp
    CieIDSdk.setUrl(url.toString())
    //inserire il pin della CIE
    CieIDSdk.pin = input.text.toString()
    //Avviare NFC
    startNFC()

É necessario, inoltre, realizzare le interfacce di Callback implementando i seguenti metodi:

.. code-block:: java
    :linenos:

    override fun onEvent(event: Event) {
    //evento 
    }
    override fun onError(e: Throwable) {
    //caso di errore
    }
    override fun onSuccess(url: String) {
    //caso di successo con url della pagina da caricare
    }


SDK iOS
=======

CieID-iOS-sdk è un SDK per smartphone iOS sviluppato in Swift che include le funzionalità di autenticazione di "Entra con CIE". Utilizzando questo kit, gli sviluppatori di applicazioni terze iOS possono integrare nella propria app l'autenticazione mediante la cartà d'identità elettronica.

Requisiti tecnici
-----------------

CieID-iOS-sdk richiede versione iOS 13.0 o successive, inoltre è necessario uno smartphone iOS con tecnologia NFC (iPhone 7 o successivo, non è compatibile con iPhone SE di prima generazione - mod 2016).

Requisiti di integrazione
-------------------------

CieID-iOS-sdk necessita che il fornitore del servizio digitale sia un Service Provider federato e che integri la tecnologia abilitante al flusso di autenticazione "Entra con CIE" (cfr. le sezioni relative a :ref:`federazione`, :ref:`protocolli` e :ref:`binding`).

Come si usa
-----------

Il kit integra per il momento il solo flusso di autenticazione con reindirizzamento di seguito descritto. L'integrazione richiede pochi semplici passaggi:

 - Importazione del kit all'interno del progetto
 - Configurazione dell'URL Scheme
 - Configurazione dell'URL di un Service Provider valido all'interno del file Info.plist
 - Configurazione dello smart button Entra con CIE all'interno dello storyboard
 - Inizializzazione e presentazione della webView di autenticazione
 - Gestione dei delegati


Flusso con reindirizzamento
---------------------------

Il flusso di autenticazione con reindirizzamento permette ad un Service Provider accreditato di integrare l'autenticazione Entra con CIE nella propria app iOS, demandando le operazioni di autenticazione all'app CieID. Questo flusso di autenticazione richiede che l'utente abbia l'app CieID installata sul proprio smartphone **in versione 1.2.1 o successiva.**

Flusso interno
--------------

Non disponibile in questa versione

Importazione
------------

Trascinare la folder **CieIDsdk** all'interno del progetto xCode

Configurazione URL Scheme
-------------------------

Nel flusso di autenticazione con reindirizzamento l'applicazione CieID avrà bisogno aprire l'app chiamante per potergli notificare l'avvenuta autenticazione. A tal fine è necessario configurare un URL Scheme nel progetto Xcode come segue:

Selezionare il progetto **Target**, aprire il pannello **Info** ed aprire poi il pannello **URL Types**. Compilare i campi **Identifier** e **URL Scheme** inserendo il **Bundle Identifier** dell'app, impostare poi su **none** il campo **Role**.

Il parametro appena inserito nel campo **URL Scheme** dovrà essere riportato nel file **Info.plist**, aggiungendo un parametro chiamato **SP_URL_SCHEME** di tipo **String**, come mostrato nell'esempio:

.. code-block:: xml
    :linenos:

    <key>SP_URL_SCHEME</key>
    <string>Inserisci qui il parametro URL Scheme</string>



A seguito dell'apertura dell'app la webView dovrà ricevere un nuovo URL e proseguire la navigazione. Di seguito si riporta il metodo **openUrlContext** da importare nello **SceneDelegate** che implementa tale logica:

.. code-block:: swift
    :linenos:

    func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {

	    guard let url = URLContexts.first?.url else {

		    return

	    }

        var urlString : String = String(url.absoluteString)
        if let httpsRange = urlString.range(of: "https://"){

            //Rimozione del prefisso dell'URL SCHEME
            let startPos = urlString.distance(from: urlString.startIndex, to: httpsRange.lowerBound)
            urlString = String(urlString.dropFirst(startPos))

            //Passaggio dell'URL alla WebView
            let response : [String:String] = ["payload": urlString]
            let NOTIFICATION_NAME : String = "RETURN_FROM_CIEID"

            NotificationCenter.default.post(name: 	Notification.Name(NOTIFICATION_NAME), object: nil, userInfo: response)

	    }

    }




Configurazione Service Provider URL
-----------------------------------

Entrambi i flussi vengono avviati tramite l'utilizzo di una WebView, per questo motivo è necessario caricare la URL dell'ambiente di produzione della pagina web del Service Provider che integra il pulsante "Entra con CIE" all'interno del file **Info.plist**, aggiungendo un parametro chiamato **SP_URL** di tipo **String**, come mostrato nell'esempio:

.. code-block:: xml
    :linenos:

    <key>SP_URL</key>
    <string>Inserisci qui l'URL dell'ambiente di produzione del Service Provider</string>
    

Importazione del pulsante Entra con CIE
---------------------------------------

Aggiungere  nello storyboard di progetto un oggetto di tipo **UIButton** ed inserire nella voce **Class** del menù **Identity inspector** la classe che lo gestisce: **CieIDButton**. L'oggetto grafico verrà automaticamente renderizzato con il pulsante ufficiale Entra con CIE.

Eseguire l'autenticazione
-------------------------

Di seguito un esempio di gestione dell'evento **TouchUpInside** per eseguire il codice necessario per inizializzare e presentare la WebView di autenticazione.

.. code-block:: swift
    :linenos:

    @IBAction func  startAuthentication(_ sender: UIButton){
    
	    let cieIDAuthenticator = CieIDWKWebViewController()
	    cieIDAuthenticator.modalPresentationStyle = .fullScreen
	    cieIDAuthenticator.delegate = self
	    present(cieIDAuthenticator, animated: true, completion: nil)

	}


La classe chiamante dovrà essere conforme al protocollo **CieIdDelegate** come mostrato nell'esempio.

.. code-block:: swift
    :linenos:

    class  ExampleViewController: UIViewController, CieIdDelegate {
    ...
    }


L'utente potrà navigare nella webView mostrata che lo indirizzerà sull'app CieID dove potrà eseguire l'autenticazione con la Carta di Identità Elettronica, al termine verrà nuovamente reindirizzato sull'app chiamante in cui potrà dare il consenso alla condivisione delle informazioni personali e portare al termine l'autenticazione.

Al termine dell'autenticazione verrà chiamato il delegato **CieIDAuthenticationClosedWithSuccess**. La chiamata di questo delegato avviene nella classe **CieIDWKWebViewController**. Potrebbe rendersi necessario posticipare la chiamata di questo delegato in base alla logica di autenticazione del Service Provider.

Gestione eventi
---------------

Il protocollo impone la gestione dei seguenti eventi mediante delegati

.. code-block:: swift
    :linenos:

	func  CieIDAuthenticationClosedWithSuccess() {

		print("Authentication closed with SUCCESS")

	}

.. code-block:: swift
    :linenos:

    func  CieIDAuthenticationCanceled() {
    
	    print("L'utente ha annullato l'operazione")
	    
	}

.. code-block:: swift
    :linenos:

	func  CieIDAuthenticationClosedWithError(errorMessage: String) {

		print("ERROR MESSAGE: \(errorMessage)")

	}

 
Licenza
=======
Il codice sorgente è rilasciato sotto licenza BSD (codice SPDX: BSD-3-Clause).