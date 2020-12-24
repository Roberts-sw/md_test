## STAL_C-protocol

`S` __start__ synchronisatie/berichtstart:  ASCII-code 13 *&lt;CR&gt;*  
`T` __type__ bericht:  *v* (bus vrij) of *w* (wacht op bericht)  
`A` __adres__: *0* is server, *255* is groep  
`L` __lengte__ van inhoud  
`_` *inhoud*, mits `L` ongelijk 0, meestal [inhoudstype](#inhoudstype) + overige inhoud  
`C` __controle__ op geldigheid van bericht  

Berichten volgens dit protocol zijn grofweg één van:  
`V` __vraag__: *w*-bericht aan een adres  
`A` __antwoord__: *v*-bericht van een adres  
`M` __mededeling__: *v*-bericht aan adres *255* is voor iedereen  

Opbouw van de communicatie is enigszins volgens het OSI-model van de organisatie ISO,  
met de Engelse ontwikkelaars-ezelsbrug: *Please do not tell sales people anything*, of  
andersom in het Duits: *Alle Priester saufen Tequila nach der Predigt*.

1. De fysieke verbinding is half-duplex RS-485.
2. interrupt-routines *RS485_ontvang()* voor ontvangst en *RS485_zend()* voor verzending.  
   Deze routines behandelen alleen dataBytes en leveren een resultaatvlag op om aan te geven  
   dat een bericht compleet is.  
2. Netwerkfunctie *RCOMM_run()* regelt tijdgebonden zaken voor communicatietoegang en bij  
   voorbeeld *drive enable*. Bij een compleet ontvangen bericht roept deze *RS485_verwerk()*  
   aan om afhandeling middels functietabellen te splitsen over `V` `A` of `M`, en [inhoudstype](#inhoudstype).  

Voor een STAL_C-bericht is meestal het inhoudstype bepalend voor de verdere inhoud.

<a name="inhoudstype"></a>
### __inhoudstype__  
*0* `L3_NACK` poll-vraag, of afwijzend antwoord  
*1* `L3_INFO` info-aanvraag, of infoblok  
*2* `L3_LEES` aanvraag om geheugendata  
*3* `L3_SCHRIJF` vraag om data op te slaan  
*4* `L3_BUITENTEMP` mededeling van buitentemperatuur  
*5* `L3_DATUMTIJD` mededeling van datum/tijd  
*6* `L3_BLOK` transport van datablokken  
*...*

In het navolgende wordt alleen *6* `L3_BLOK` behandeld.

<a name="type_blok"></a>
#### __6 L3_BLOK__
Een blokbericht aan een apparaat is een vraag om gegevens te leveren of over te nemen.  
Het bericht heeft een minimale inhoudslengte van 5, na type *6* volgt een kopje van 4 Bytes:

1. *0* in eenvoudige gevallen, hier verder niet behandeld
2. *blok*, deze wordt intern eventueel gecorrigeerd voor afdeling
3. *start*, dit is de startindex binnen de variabelengroep *blok*
4. *aantal*, het aantal indices waarover dit bericht handelt, 0 betekent hooguit tot groepseinde,  
   of passend binnen het bericht: 255 = 1 Byte inhoudstype + 4 Bytes kop + 125 indices * 2 Bytes.

Indien de lengte in de blokvraag 5 bedraagt, is dat het teken om gegevens op te vragen, waarbij  
op enkele uitzonderingen na deze in het antwoord verstrekt worden.  
De lengte boven 5 bepaalt het aantal gegevens dat het apparaat geacht wordt over te nemen.  
Sommige gegevens worden als ongeldig beschouwd en niet overgenomen, daarnaast is het  
apparaatafhankelijk of berekende/gemeten waarden worden overgenomen, maar gesteld dat er  
geldige instellingen worden aangeleverd en het bericht in orde is worden deze overgenomen.
