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
Attualmente non é disponibile l'SDK per SO iOS. Sono in corso gli sviluppi della soluzione. 

Licenza
=======
Il codice sorgente è rilasciato sotto licenza BSD (codice SPDX: BSD-3-Clause).