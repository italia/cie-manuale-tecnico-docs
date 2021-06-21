.. _testing:

=======
Testing
=======

Al termine delle attivitá di implementazione, il Service Provider deve effettuare dei test in ambiente di pre-produzione volti alla verifica della corretta funzionalitá del servizio e produrre opportune evidenze in formato immagine (screenshot). Tali evidenze devono essere caricate sul `portale di federazione erogatori di sevizi <https://federazione.servizicie.interno.gov.it>`__ al fine di rendere possibile le verifiche tecniche svolte dal personale del Poligrafico incaricato allo svolgimento di tale attività. 


Il set minimo di test che dovrà essere eseguito correttamente comprende quelli volti a provare la corretta gestione almeno dei seguenti “error codes”, come dettagliati nell'apposita sezione relativa alla specifica dei codici d'errore restituiti dal CIE ID SERVER (:ref:`codicierrori`)

   - 1=Success;
   - 21=Timeout durante l'autenticazione
   - 22=Utente sceglie di non proseguire con l'invio degli attributi
   - 25=Processo di autenticazione annullato dall'utente

Nel caso di esito positivo delle verifiche suddette il Service Provider può procedere con la fase successiva di federazione in ambiente di produzione.


