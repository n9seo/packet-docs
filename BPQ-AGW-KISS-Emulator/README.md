# AGW BPQ Emulator


Create a basic linbpq or winbpq install.  This config example below gives an example
of the config you could use for an AGW or provide a TNC interface for apps that want
to see a TNC on a (virtual)serial port. You may need to edit this file for Windows 
use case as I did not test it on windows.  My direwolf modems are on remote Raspberry
PIs on the network here so they have addresses in the port definitions.  If you
use soundmodem locally you can remove the lines or change to 127.0.0.1 for the IP.

In the config below AGWPORT=8100 would be port 8100 in the [easyterm](../easyterm/README.md) app.

Here is one with a VHF modem and HF modem commented out.

bpq32.cfg
```text
SIMPLE
NODEALIAS=#SEOKB
NODECALL=N9SEO-9
LOCATOR=NONE
PACLEN=236
AGWPORT=8100
AGWMASK=0x30
AGWSESSIONS=5
AGWLOOPMON=1
AGWLOOPTX=1
BUFFERS=999
IDINTERVAL=0
LINCHAT

TNCPORT
 COMPORT=/home/n9seo/bpq/comport
 TYPE=KANT
 APPLNUM=32
 APPLFLAGS=14
 CONOK=1
 ECHO=1
 AUTOLF=1
ENDPORT


;**********  Port 1 Direwolf **********
;PORT
;   PORTNUM=2                                     ; Port number
;   ID=NET105 DAY NET40 NIGHT                     ; PORTS command text
;   TYPE=EXTERNAL
;   DRIVER=BPQtoAGW
;   IOADDR=1F40
;   NOKEEPALIVES=0
;   QUALITY=0
;   MINQUAL=0
;   CHANNEL=A    
;   MAXFRAME=1                                    ; Max outstanding frames
;   FRACK=6000                                ; Level 2 timeout (ms)
;   RESPTIME=1000                               ; Level 2 delayed ACK (ms)
;   RETRIES=15                                    ; Level 2 max retries
;   PACLEN=43                                     ; Max packet length (bytes)
;   TXDELAY=300                                   ; Transmit keyup delay (ms)
;   SLOTTIME=120                                  ; CMSA interval timer (ms)
;   TXTAIL=100
;   PERSIST=63                                    ; Persistence (256/(# transmissions-1)
;   DIGIPORT=0
;   DIGIFLAG=1                                    ; Allow Digipeat on this port
   ;UNPROTO=BEACON
   ;BCALL=N9SEO
;   CONFIG
;   192.168.76.201 8000
;ENDPORT

;**********  Port 1 Direwolf **********
PORT
   PORTNUM=3                                     ; Port number
   ID=UZ7HO HF MODEM 2                     ; PORTS command text
   TYPE=EXTERNAL
   DRIVER=UZ7HO                                    ; RS232 connection
   NOKEEPALIVES=1
   QUALITY=0
   MINQUAL=0
   CHANNEL=B    
   ;UNPROTO=BEACON ;BTEXT broadcast addrs format: DEST[,digi1[,digi2]] 
   ;BCALL=N9SEO-9
   ;PCALL=N9SEO
   MAXFRAME=1                                    ; Max outstanding frames
   FRACK=6000                                ; Level 2 timeout (ms)
   RESPTIME=1000                               ; Level 2 delayed ACK (ms)
   RETRIES=15                                    ; Level 2 max retries
   PACLEN=43                                     ; Max packet length (bytes)
;  DWAIT=0                                       
   TXDELAY=300                                   ; Transmit keyup delay (ms)
   SLOTTIME=120                                  ; CMSA interval timer (ms)
   TXTAIL=100
   PERSIST=63                                    ; Persistence (256/(# transmissions-1)
   DIGIPORT=0
   DIGIFLAG=1                                    ; Allow Digipeat on this port
   XDIGI=SEO,5
   CONFIG
   ADDR 192.168.76.201 8000
   UPDATEMAP
   RIGCONTROL
   HAMLIB 192.168.76.201:4532 
   ****
ENDPORT
PORT
   PORTNUM=4                                     ; Port number
   ID=145.73MHz 1200baud                       ; PORTS command text
   TYPE=EXTERNAL                                    ; RS232 connection
   DRIVER=BPQtoAGW
   IOADDR=2328
   CHANNEL=A                                     ; TNC channel
   MAXFRAME=2                                    ; Max outstanding frames
   FRACK=5000                                ; Level 2 timeout (ms)
   RESPTIME=40                               ; Level 2 delayed ACK (ms)
   RETRIES=15                                    ; Level 2 max retries
   PACLEN=128                      ; Max packetlength                           
   TXDELAY=700                                   ; Transmit keyup delay (ms)
   SLOTTIME=100                                  ; CMSA interval timer (ms)
   TXTAIL=30
   PERSIST=63                                   ; Persistence (256/(# transmissions-1)
   DIGIFLAG=1                                   ; Allow Digipeat on this porti
   WL2KREPORT PUBLIC, cms.winlink.org, 8085, N9SEO-10, EM10ie, 00-23, 145730000, PKT1200, 25, 24, 2, 0
   CONFIG
   192.168.76.200 9000 
ENDPORT

; below sets up easy term for a direct terminal to call N9SEO for keyboard2keyboard.
APPLICATION 1,EASY,,N9SEO
```