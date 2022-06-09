<SIP>

# SIP components
- **UAC** : User agent Client
- **UAS** : User agent Server
- **B2BUA** : is a UAS directly connected with a UAC that may replace a SIP proxy or connect to one


- SIP response only for the signaly not for the media
- RTP (real time protocol) : is directly connect between UAC - UAS - B2BUA

<br>

 When you want to connect to :

- **pbx** : private branch excahnge
- **pstn** : public switch telephone network

you need to use a gateway.

<br>

- UAC starts the call 
- UAS anyone answering a call is a UAS (phone or server)

- **IVR** = interactive voice responsive (UAS : server)

<br>

> Server : <br>
*User* : **admin** <br>
*Passwd* : **opensips**

<br>

**Methods** : primary function that a request invoke on a server

RFC defines 6 methods : <br>
- **invite** : session establishment
- **bye** : terminate ad existing session (end the call)
- **cancel** : cancel a pending request (cancel ananswered call)
- **ack** : acknowledge an INVITE (confirm)
- **options** : query the capabilities of the USA (test)
- **register** : register the user in the location table (register phone or other device)

<br>

6 types of response : 
- **provisional response** :
> 1. *100* Trying : stop live retransmission
> 2. *180* Ringing : say that the phone is ringing

- **success** : 
> 1. *200* OK : for invites
> 2. *202* Accepted : for non-invites request

- **redirect** : is used by redirect servers, most of these messages is used on specifics areas

- **client errors** : the code start with 4 (like 400, 401, 402)

- **servers errors** : 
> 1. when you have an *500* error this mean that you have an Internal Server Error (at the logic of your system)
> 2. *503* Service Unavailable : congestion, you forward the request on another server

- **global errors** : you shouldn't do anything to fail over the request, because the problem don't depend on you

<br>

SIP has 5 function (features) : 
1. user location and registration
2. user availability : the server check if the user is available and if the refistration is expired or not
3. session setup : to stablish a session
4. session management : you can complete manage the session
5. discovery of user capabilities : such as codex, streams and many other usage in the SIP negotiation

<br><br>

 ## REGISTRATION PROCESS :

- **registration request** : when a phone wants to inform its location, it send a registration to a Location Database to a SIP register (in the packet there's also the source and destination socket) 
> N.B: if you want to inform the location you **MUST** register

<br>

- **registration authentication sequence** :
1. UAC send the registration request
2. the SIP Proxy refuse it and send an error packet (401 unauthorized) that contains information (which are useful for authorize header)
3. UAC resend the registration request with the authorize header (with all the information to authenticate it)
4. the SIP Proxy send back the 200/OK packet

<br>

- **record expiration** : UAC suggests a expiration time, and the UAS decides to agree expiration time

<br>

- **typical SIP proxy configurations** : usually the proxy has a Minimun expires, a Maximum expires and a Default expires. If the cloud don't send a expires the default value becomes the expiration time. 

<br>

- **registration details** : the address of record (AOR) can have more than one contact. The registration time mustn't be too small, because too many close request can cause problems. So if you miss some registration maybe you need to increase the registration time.
> N.B: UAC and the UAS clock have to be synchronized

<br><br>

 ## SESSION SETUP AND MANAGEMENT :
- the *SIP ladder* and the *SIP trapezoid* show the same thing, but the first shows the simuation and the other shows the topology.

- **Simulation of a call** : <br> *phone1 <--> proxy1 <--> proxy2 <--> phone2* 

>1. phone1 --- invite ---> proxy1
>2. proxy1 --- trying ---> phone1 **
>3. proxy1 --- invite ---> proxy2
>4. proxy2 --- trying ---> proxy1
>5. proxy2 --- invite ---> phone2
>6. phone2 --- ringing ---> proxy2
>7. proxy2 --- ringing ---> proxy1
>8. proxy1 --- ringing ---> phone1
>9. phone2 --- OK ---> proxy2
>10. proxy2 --- OK ---> proxy1
>11. proxy1 --- OK ---> phone1
>12. phone1 --- ACK ---> phone2
>13. phone2 --- BYE ---> phone1
>14. phone1 --- OK ---> phone2

** *before the invite3 the proxy1 send a request at a DNS server to learn the IP address*

<br><br>
