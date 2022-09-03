

# Instruction Manual / Command Description for the Tracker / DSP TNC Firmware Version 1.7

© SCS GmbH & Co. KG January, 2018

## 1. Introduction

The DSPTNC is worldwide the first "Terminal Node Controller" using a Digital Signal Processor (DSP) as protocol and as modem processor.

The close relationship between the hardware level and the higher protocol levels opens the possibility for unique applications, e.g. the implementation of high sophisticated multi-detectors (300 Bd AFSK), efficient implementation of new modes which process complete data packets (Robust-PR), as well as a generally very high flexibility.

The DSPTNC is equally usable as universal TNC for chat and mail operation, as well as a standalone APRS (1) position tracker.

### 1.1 Particular Features of the Hardware:

    * 100 MIPS DSP as CPU.
    * Optically decoupled USB-Port, virtual baud rates 38400/115200 Bd.
    * Generically very good HF-blocking (no self-made QRM (hum)).
    Metal housing.
    TCXO for highest stability of all signals.
    Low current drain 15-90 mA (13.8 V), typically 50 mA, mode dependent.
    Mini-DIN-connector, compatible to the usual transceiver's "packet connector".
    Quad-DIP-switch for basic configuration.
    NMEA-In, NMEA-Out.
    Relay-Switch-Output : Direct connection of a power relay to switch the transceiver's supply power.

### 1.2 Highlights of the Operating System ("BIOS" and "Firmware"):

    Backwards compatible to "The Firmware" of the NORD><LINK (2) e. V. The AX.25-Kernel has in most parts been adopted from "The Firmware" (TNC2), with the friendly authorization of the NORD><LINK e. V.
    Operating modes (modulation, baudrate) and transmit levels are adjustable by software commands, 300, 1200, 9600, 19200 Bd standard-modes & Robust-PR are implemented.
    Frame-collector to increase the efficiency of the AX.25-protocol.
    DAMA-Slave-Protocol implemented - DAMA especially on shortwave very useful.
    Dynamic Round-Trip-Timer.
    DED-Hostmode (also XHOST by DG3DBI), compatible to all common Hostmode programs, e. g. Paxon, WPP, and so on.
    KISS/SMACK (compatible to all common KISS programs, e. g. UI-View).
    Firmware upgrade via USB connection.
    Permanent storing of all parameters possible.
    High reliability through the separation of the BIOS and application ("Firmware").

(1) APRS is a registered trademark of APRS Engineering LLC, USA.
(2) NORD><LINK e. V. http://www.nordlink.org

### 2. Basic Configurations by the DIP-Switches:

    Tracking-Mode, BIOS (Firmware-Update), setting of the virtual baudrate (USB)

    The 4 DIP-switches at the front side of the DSPTNC are numbered consecutively from left to right from 1 to 4. (1=Tracking, 2=Baudrate, 3=BIOS, 4=Option). Standard position (upper position) means "disabled" ("OFF"), lower position means "enabled" ("ON").

* DIP-Switch 1: Tracking

    `OFF`:	Normal TNC-Mode, DSPTNC operates as universal TNC.

    `ON`:  	DSPTNC operates in energy saving Tracking-Mode for position tracking.

In Tracking-Mode the DSPTNC consumes less than 20 mA at 13.8 V operating voltage. Therefore, this mode is suitable for long term position tracking combined with limited battery capacity, especially as the power of the transceiver can also be switched. In result, the average power consumption when using e.g. half hour beacon intervals is very small.

As soon as the Tracking-DIP-switch is activated and the (few) necessary conditions for APRS beacon operation are fulfilled ("Mycall" set (see I-command), position is known (GPS or FIX), %A set to 1 or 2 (see %A-command)), the TRK-LED (second from left) should flash red regularly several times per second. With this the TRK-LED indicates, that the TNC is now operating correctly in "Tracking-Mode". Note: During this waiting period (TRK-LED flashing red) the DSPTNC cannot process any commands from the PC interface, nor receive signals from the transceiver port.

If the TRK-LED is permanently showing red, the "Tracking-Mode" could not be activated because of one (or more) important condition(s) for the transmission of APRS-datagrams is not yet fulfilled. Permanently lighting of the TRK-LED means "Tracking-Mode Error". In this case the "Tracking"-DIP-switch must be set to "OFF again to be able to change and to correct the DSPTNC-configuration.

If the DSPTNC is in Tracking-Mode, it switches on the transceiver (via the relay output) 10 seconds before the next APRS beacon is scheduled and checks if the HF/VHF/UHF channel is free (DCD). During this phase the TRK/STA-LED flashes alternating red/green. If the channel is free after the expiry of the 10 seconds minimum waiting time, the APRS datagram will be transmitted.

Afterwards the transceiver will be switched off again and the waiting condition is resumed (red flashing TRK-LED).

If the channel is occupied, the DSPTNC waits for additional 20 seconds maximum for the channel to become free. If this does not happen in this time then the DSPTNC goes back into the waiting condition without having transmitted the APRS-datagram.

DIP-Switch 2: Baudrate

The baudrate-DIP-switch is read in just once at power on of the DSPTNC. It defines the speed (baudrate) of the virtual COM-port (USB).
OFF:	38400 Bd.
ON: 	115200 Bd.

DIP-Switch 3: BIOS (Possibility to update the operating system)

The BIOS-DIP-switch is read in just once at power on of the DSPTNC and defines if the DSPTNC starts the application software ("Firmware").
OFF:	Application software ("Firmware", Packet-Radio) will be started, if installed. The PWR/NMEA-LED shines green as soon as the BIOS has started the firmware.
ON: 	DSPTNC remains in BIOS mode, it does not start the "Firmware". Now via the BIOS an update of the whole operating system can be performed (BIOS & Firmware). The PWR/NMEA-LED shines red in this case.

Being in BIOS mode, the DSPTNC can be updated with the help of an update tool running on the PC (e.g. TRConfig or Tracker_Update.exe), which means that a new operating system can be loaded into the DSPTNC. Valid update-files have the extension .TRK, e.g. FW1_1.TRK.

While the update is in progress, the LEDs light consecutively green (cyclic). This process usually only replaces the application software ("Firmware"), the BIOS itself remains unchanged.

In special cases it can be necessary the renew the BIOS as well. If the BIOS-DIP-switch is set to OFF again while the DSPTNC is still in BIOS mode, and the update process is started afterwards, then the update process replaces both, BIOS and firmware!

A BIOS-update, however, carries a certain amount of danger that the BIOS will be destroyed in the case of loss of power (or other problems) during the update procedure. Because of this, updating the BIOS should only be done in rare cases! If the DSPTNC no longer contains a correct BIOS, it can only be reactivated by a service technician.

DIP-Switch 4: Option

The Option-DIP-switch is read in just once at power on of the DSPTNC and defines, if the DSPTNC uses the default or the user-defined configuration.
OFF:	The configuration parameters stored by the user with the %ZS command will be loaded at power-up.
ON: 	The default configuration parameters (of factory) of the DSPTNC are loaded at power-up. (This configuration can then be stored with the %ZS command, which deletes any previously stored user defined configuration.)

Audio Input Level Control with the STA-LED

With the device switched on, the DIP switch 4 ("Option") will become effective as audio input level control switch. When set to ON, the STA-LED will have a new function:
STA-LED	
No light:	Input level too low!
Lit green:	Input level in a good range.
Lit red:	Input level too high!

With the input level slightly too low, the demodulators usually work well enough, but with reduced dynamic range. Too high levels, however, have to be avoided in any case, as distorting limiting effects may occur.

3. Configuration by Software Commands

The Configuration Program TRConfig

The easiest way to do the configuration of parameters being adjustable by software commands is to use the PC-program TRConfig, which has been created especially for this purpose. TRconfig translates the desired properties of the DSPTNC, given as easy symbols in a graphical user interface, into commands and sends those to the DSPTNC, and also permits the permanent storing of the complete configuration into the non-volatile memory of the DSPTNC.

The basic configuration requires the own callsign ("Mycall"), the desired Packet-Radio operation mode as well as the transmit output level(s) to modulate the transmitter. For APRS it can be necessary to enter the position (if no GPS receiver is connected), the beacon timer interval as well as the APRS symbol (your own symbol that appears on an APRS-map) and the desired APRS mode ("FIX", which is a manually entered fixed position, or "GPS") as basic configuration.

The available Commands of the DSPTNC Operating System

The DSPTNC comes with two different modes to communicate or to exchange data on the USB connection to the computer: Terminalmode and Hostmode. After power-on the TNC is always in Terminalmode and can be accessed with an easy terminal program like Windows HyperTerm. For data transparent multi-channel operation it is recommended to use the so-called hostmode, which can be activated by command and is supported by many PC programs designed for Packet-Radio operation.

Command interpreter as well as hostmode adhere closely to "The Firmware" by NORD><LINK e.V. and therefore are backwards compatible to "The Firmware" (TNC2).

The DSPTNC makes use of 11 virtual channels for Packet-Radio, which are one monitor-channel (0) and 10 channels for Packet-Radio-connects, which means that 10 parallel Packet-Radio connects can be established the same time.

In terminal mode every command must be preceded with the ESC-character (ASCII 27) to open the command interpreter. Otherwise the characters being entered are sent to the currently selected virtual channel (see command S).

In the case that the virtual channel is not connected, this characters usually are ignored, which means discarded. In channel 0 however, they are interpreted as unproto information and (after a CR as termination) transmitted as a part of an unproto packet.

Apart from a few exceptions, nearly all commands are available in terminal mode as well as in hostmode. In hostmode the operation of the DSPTNC is mainly given by the PC program used.

3.1. To "The Firmware" (NORD><LINK) compatible Commands

Concerning the standard commands the DSPTNC behaves compatible to a TNC2 with "TF2.7b" as operating system. The following commands are available:

A (Add-Linefeed)

Default: 1

Possible parameters: 0, 1

Automatic insertion of a Linefeed-character <LF> after a CARRIAGE RETURN <CR;> to the terminal. 1 = yes, 0 = no.

B

Default: none

Possible parameters: none

Counts the number of the round trips of the main software loop per second. This number allows conclusions about the internal processing speed of the TNC firmware. This command was left remaining just for compatibility reasons but has no relevance to for the user.

C (Connect)

Default: none

Possible parameters: Target call (if required, followed by up to 8 digipeater callsigns)
Example:	C DL6MAA DB0UAL (DL6MAA shall be connected via DB0UAL.)

The C-command is used to establish a connection to a distant station. It is not necessary to enter a 'v' or 'via' between the target call and the digipeater calls. A C-command invoked on channel 0 defines the path for unproto (UI) packets.

D (Disconnect)

Default: none

Possible parameters: none

An existing connection will be disconnected. If after entering the "D" command there is still information being send or confirmed, the "Disconnect" will be carried out when the last information packet is received and confirmed. If the D-Command is repeated, this process can be cancelled.

If the D-command is invoked while a connect or a disconnect is in progress, the TNC immediately resumes the disconnected condition and automatically sends a DISC-frame to prevent unnecessary transmissions for the case that the own TNC did not copy the answers of the distant station.

E (Echo)

Default: 1

Possible parameters: 0, 1

Enables/disables the echoing of entered characters (data or commands) to the terminal. 1 = yes, 0 = no.

To avoid problems with terminal programs with echo enabled, non printable characters are replaced with a ".". Just BELL and TAB are transparently echoed to the terminal.

F (Frack)

Default: 500

Possible parameters: 1...1500

"Frack" is the maximum waiting time between the transmission of a packet and the confirmation by the distant station. (Afterwards a request back will be sent). With "The Firmware" Frack is implemented as a dynamic "Round Trip Timer" which adapts to the actual channel activities.

The value set with the F-command serves as the start value for the automatism and also determines the length of the gap between SABM packets (link establishment).

The time can be entered directly in seconds. With entries < 16 the value will be multiplied with 100 and then divided by 2. Entries > 15 are directly interpreted as 10 msec steps. ("L2-Round Trip Time").

G

Default: none

Possible parameters: 0, 1

Request of the virtual TNC-channels in hostmode. In terminal mode this command is not valid and an error message is issued.

Effect of the parameter:
Channel 0:	G (without parameter):	Linkstatus/Monitorheader/Monitorinfo
	G0:	Monitorheader/Monitorinfo
	G1:	Linkstatus
Channel > 0:	G (without parameter):	Linkstates/Infoframedata
	G0:	Infoframedata
	G1:	Linkstates

I (Mycall)

Default: DSPTNC

Possible parameters: CALLSIGN (own callsign, maximum 6 characters plus SSID.)

Entry of the own callsign ("Mycall"). For each virtual TNC-channel an own callsign can be entered. After a "Disconnect" the callsign of channel 0 is adopted.

ATTENTION: The DSPTNC only transmits after a callsign has been entered, but not with the default callsign "DSPTNC" as "Mycall"!

JHOST (Hostmode)

Default: 0

Possible parameters: 0, 1

Switches between terminalmode and hostmode. The hostmode is WA8DED-compatible and is supported by many PC-application programm (e.g. terminal programs).

K (Timestamp)

Default: 0

Possible parameters: 0, 1, 2 or Time and Date

Activation of the "Timestamp"-function and setting of the implemented 24-h clock with calendar. (A "Timestamp" is an additional timestamp at linkstatus outputs as well as monitor outputs).

Possible parameters:
K (without parameter):	shows Timestamp-parameters and date/time
K 0	Timestamp disable
K 1	Timestamp enabled at status outputs
K 2	Timestamp enabled at status and monitor outputs
Examples:	K 20.02.88	Sets date, European version
	K 02/20/88	Sets date, American version
	K 17:36:00	Sets clock

L (Linkstatus)

Default: none

Possible parameters: 0...10

The L command displays the linkstatus of one (channel number as parameter) or all (no parameter) virtual channel(s). Displayed are routing information (call and list of digipeaters), quantity of received frames, quantity of not yet sent frames, quantity of unacknowledged frames and the retry counter respectively. The respective channel in use is marked with a '+' character.

M (Monitor)

Default: N

Possible parameters: N, U, I, S, C, +, - (multiple parameters simultaneously possible)

Activation and configuration of the monitor mode. The parameters define which frames shall be displayed.

Possible parameters:
N	Monitor disabled
I	Information (I-Packets)
U	Unproto transmissions (UI-Packets)
S	Control-Packets
C	Monitor even whilst connected
+	< List of up to 8 calls >: only packets from this sender
-	< List of up to 8 calls >: no packets from this sender

The combined usage of '+' and '-' parameters is not supported. They have to be entered as last parameter directly before the calls. The entry of '+' or '-' without call(s) deletes the actual list.

Interpretation of SSIDs is not supported!

N (Retry-Counter)

Default: 10

Possible parameters: 0...127

Configures the Retry-Counter. The entered value defines how many times a packet transmission shall be repeated in the case of error. (0 = unlimited).

For every channel a separate value can be entered. After RESET or disconnect the value being defined for channel 0 will be restored. Do NEVER set this value to 0 in the case of unattended operation!

O (MaxFrame)

Default: 2

Possible parameters: 1...7

Maximum allowed number of unacknowledged and unanswered information packets (I-Frames). For every channel a separate value can be entered. After RESET or disconnect the value defined for channel 0 will be restored.

P (Persistence)

Default: 32

Possible parameters: 0...255

Persistence-configuration. Without parameter the current setting is displayed. With DAMA-operation the Persistence value is ignored!

R (Digipeating)

Default: 1

Possible parameters: 0, 1

Enables / disables the digipeat-function. 1 = on, 0 = off.

S (Switch channel)

Default: 0

Possible parameters: 0...10

Selects between the virtual channels (0 = Monitor channel).

T (TX-Delay)

Default: 25

Possible parameters: 0...500

Delay between keying of the transmitter and start of the data transmission itself. ("TX-Delay").

The values are entered in 10 ms steps. Please evaluate and set an as low as possible value by experiment.

U (Connect-Text, "C-Text")

Default: 0

Possible parameters: 0, 1 [optional CText], 2 [optional CText]

With the U-command one has the possibility to automatically transmit a small amount of text to the distant station when a connect is received ("C-Text").

The defined text also remains stored when the U parameter is set back to 0.

With 'U2' the TNC can be forced to disconnect a current connection at receipt of the string "//Q" (in Terminal mode only!). For that, the string "//Q" must be at the beginning of a single packet. This function is disabled in hostmode.
Examples:	U 1 Terminal offline	Entering and activating the C-text "Terminal Offline".
	U 1	Activates already existing C-Text.
	U 2 Terminal offline	Entering and activating the C-text "Terminal Offline" incl. //Q Function.
	U 0	Disabling C-Text.
	U	Displaying C-Text.

V (Version)

Default: none

Possible parameters: none

Displays a string which contains information about the currently installed software version.

W (Slottime)

Default: 10

Possible parameters: 0...127

Defines the "Slottime" in milliseconds. Without parameter the current setting is displayed. The W-value is ignored at DAMA operation. The TNC always goes to transmit immediately.

X (PTT)

Default: 1

Possible parameters: 0, 1

Controls the PTT line of the TNC. If necessary the keying of the radio can be suppressed, e.g. when a channel shall be observed and it shall be prevented that the TNC replies to an incoming connect with a "busy" packet.

1 = normal PTT-operation, 0 = PTT always deactivated.

Y (Number of channels)

Default: 10

Possible parameters: 0...10

Defines the maximum number of simultaneously allowed connections until the next connecting station is rejected with a "busy". Without argument the number of currently connected channels is displayed in the form "maximum number of channels (used channels)"

General limitation: The number of used channels can only be determined correctly when on all channels the same Mycall is used, as defined for the monitor channel S0.

Example-displayed: "10 (0)".

Z (Flow-Control)

Default: 3

Possible parameters: 0...3

Enables/disables the flow-control and the XON/XOFF-handshake to the terminal. With flow-control enabled the TNC does not send characters to the terminal while data or commands are entered. With flow-control disabled characters are immediately sent to the terminal, independent of whether a line of text or a command is currently entered.

If the XON/XOFF-handshake is enabled, the dataflow from the TNC to the terminal can be stopped with Ctrl-S and started again with Ctrl-Q.

Possible parameters:
0	Flow-control off, XON/XOFF off
1	Flow-control on, XON/XOFF off
2	Flow-control off, XON/XOFF on
3	Flow-control on, XON/XOFF on

@B

Default: none

Possible parameters: none

Displays the quantity of free TNC buffers.

@D (Duplex)

Default: 0

Possible parameters: 0,1

Switches on/off the full duplex operation. 0 = off, 1 = on.

@F (Flags)

Default: 0

Possible parameters: 0,1

Send flags in pauses. 0 = no, 1 = yes.

@I (Ipoll)

Default: 60

Possible parameters: 1...256

Defines or displays the value for the maximum length of the IPOLL-frame. (Not valid with DAMA!).

@K (KISS/SMACK)

Default: none

Possible parameters: none

Enables the KISS/SMACK-mode. With the decimal byte-sequence 192, 255, 192, 13 the TNC can be returned to normal operation without RESET (power off/on).

@N (NMEA/GPS baud rate)

Default: 2 (4800 / 9600 Bd autobaud)

Value range:
0:	fix 4800 Bd
1:	fix 9600 Bd
2:	4800/9600 Bd autobaud

Depending on the chosen value, the baudrate of the NMEA input port will be set to a fixed value or detected automatically. Meanwhile GPS mice frequently use 9600 Bd instead of the old 4800 Bd standard.

On setting "autobaud", the baudrate of the NMEA output port initially is always set to 4800 Bd but changed to the detected value as soon as valid NMEA data has been found. On "autobaud", the baudrate search mode will be restarted after 60 seconds if no valid NMEA data has been detected within that timeout period.

The @N parameter can also be stored permanently in the flash ROM using the %ZS command, i.e. the default baudrate setting after power cycling can be defined by the user.

@R (Alias Digipeater Callsign)

Default: SCSTNC

Value range: any callsign, optionally with SSID

If the R parameter is set to 1, packets will be digipeated by the DSPTNC, if the callsign defined with @R is the next in the digipeater path. The H-bit will be set in this case.

@T2 (Timer 2)

Default: 150

Possible parameters: 0...100000

"AX.25-Timer 2": Interval before the acknowledgement of a received packet (in 10 ms steps, 150 means 1.5 seconds)

@T3 (Timer 3)

Default: 18000

Possible parameters: 0...100000

"AX.25-Timer 3": Time the TNC waits for a "sign of life" from the distant station when connected. After T3 is elapsed, the distant station is requested if it is still ready to receive.

@U (UI-Poll)

Default: 0

Possible parameters: 0, 1

Enables or disables UIPOLL (1 = UI+, 0 = UI).

@V

Default: 0

Possible parameters: 0, 1

Enables or disables the call check (1 = yes, 0 = no).

3.2 DSPTNC-specific Commands

(Extension of the "Standard-TNC-Command Set")

APRS-Commands/Configuration for "Stand Alone"-Position Tracking

"Stand alone" means position tracking that works without a PC being connected. The actual position data is sent in regular intervals in APRS format. The position can be entered either manually ("Fix-operation" "Mode %A2") or received from a connected GPS receiver ("GPS-operation" "Mode %A1").

For setting up the APRS-function, several commands are available, always beginning with %A.

APRS-datagrams are always sent with the Packet-Radio modulation mode previously set with the %B-command.

Important:

The APRS-beacon uses as sender callsign the Mycall (see I-command) of the virtual channel 0 or the optional/special APRS-Mycall (%AM) if it was defined. As long as no callsign is defined, the APRS-beacon cannot be activated.

Additionally the DSPTNC must be supplied with a valid position via NMEA-port (GPS-information) in mode %A1, or with a fixed position in mode %A2, entered with the help of the %AO-command - otherwise the Tracker cannot transmit a position beacon.

The following functions are available:

%A (APRS-main switch)

Default: 0

Possible parameters: 0, 1, 2

This "APRS-main switch" serves to setup the APRS-mode:
0:	OFF, APRS-beacon is switched off.
1:	GPS, APRS-beacon transmits GPS data, if available.

The Tracker looks for RMC, GGA and GLL sentences (NMEA) on the GPS-Port. For plain position reports, one of these sentences is sufficient.
2:	FIX, APRS-beacon transmits the fixed position data (settable with %AO), if available.

In GPS-mode the beacon only transmits when the position information is not older than the timeout interval defined with the %AV command (default 20 minutes). If the GPS receiver fails, the beacon stops the transmission after expiry of the timeout interval. Also refer to the %AI-command.

%AA (APRS Altitude)

Default: 0

Possible parameters: 0...3

Configures the Tracker for transmission of the "altitude"-information in APRS packets.

The parameter is defined as follows:
0:	Altitude information disabled.
1:	If altitude information is available from the GPS receiver, it will be inserted in the UNCOMPRESSED format at the beginning of the comment field (/A=xxxxxx).
2:	If altitude information is available from the GPS receiver, it will be transmitted in the COMPRESSED (base 91) as well as in the UNCOMPRESSED format. Attention: In the compressed format the "course/speed" information in the respective packets is omitted.
3:	Similar to 1, but even in the compressed mode, the altitude information is only added uncompressed (at the beginning of the comment field). This way the "course/speed" information is preserved in the compressed position field.

%AB or %AT (APRS Timer)

Default: 900

Possible parameters: 0, 10...7200

Defines the beacon interval in seconds. With the default setting of 900 the beacon transmits every 15 minutes, if position data is available and the "global Mycall" on virtual channel 0 and/or the optional/special APRS-Mycall (%AM) is set.

Parameter 0 activates the speed dependent automatic operation: The interval calculates with the equation: Interval [sec] = 1800/GPS-speed [knots]. The interval is limited from 180 knots to its minimum of 10 seconds. At speeds lower than 1 knots the interval is limited to a maximum of 1800 seconds.

This automatic can only work properly when the speed information is contained in the received GPS data, which means that the receiver provides RMC data sets. If no speed information is available but automatic operation is selected, then the interval is set to 900 seconds. At "FIX"-position operation (%A2, see above) and automatic timer, the firmware sets the interval independent from the speed data of an eventually connected GPS receiver to 1800 seconds.

Speed dependent automatic tracking mode (set DIP-switch 1) is also possible, but with the limitation that the Tracker does not read GPS data during the sleep-phase and does with this not dynamically adapt the actual interval. It calculates the next interval out of the speed information available short term before the next sleep phase.

%AC (APRS Comment)

Default: NONE

Possible parameters: - or maximum 80 characters of comment text.

Defines the comment text that is added to every APRS-datagram. For example, a brief description of the system can be included here: "{DSPTNC} 20 W, Dipole". The comments maximum length is 80 characters. Longer comments will be rejected with an error message. A minus character (-) as first comment character sets the comment to "NONE", respectively deletes the comment.

APRS comments should be as short as possible, since longer APRS-datagrams lead to a (unnecessary) high channel occupation.

%AD (APRS digipeating)

Default: 0

Possible parameters: 0...3

Configures APRS-digipeating.

The parameter is defined as follows:
0:	No APRS-digipeating.
1:	RELAY and WIDE1-1 as digipeating alias.
2:	WIDE and WIDEn-N as digipeating aliases.
3:	RELAY, WIDE, WIDEn-N, and GATE as well as ECHO as digipeating alias.

RELAY, ECHO and WIDE as digipeating aliases will be replaced at digipeating with the global Mycall (I-command at channel 0), i.e. call sign substitution is applied. If a WIDEn-N alias is digipeated, the N parameter will be decremented automatically. In the case of WIDEn-N and GATE no call sign substitution takes place.

The DSPTNC supports the so-called "New Paradigm" for digipeating. If %AD is set to 1, the DSPTNC acts as "fill-in digipeater". The digipeater alias WIDE1-1 is changed to WIDE1 through digipeating.

%AE (APRS status Every)

Default: 0

Possible parameters: 0...20

The %AE-parameter defines the frequency of the interspersed status reports (see %AR-parameter). With a value of 0 (default), the Tracker never sends the status report. With a value of e.g. 3, after every third APRS-position packet a separate status-report packet will be transmitted, exactly 5 seconds after the beginning of the position packet.

Background: The separately transmitted status report can be used alternatively to a long APRS-comment (%AC-parameter). If somebody e.g. sends the description of his equipment not as APRS-comment but as status report, the APRS-position packet itself remains very short. This increases the probability that the important position information (which is only contained in the APRS-position packet and not in the status report) will properly be received and decoded. Less important and not varying information can this way easily (and with lower frequency) be interspersed as a status report.

%AF (APRS Frequency Beacon)

Default: 0

Possible parameters: 0, and 60...30000

The parameter is defined as follows:
0:	Frequency list beacon is switched off.
60...30000:	The frequency list is transmitted every 60-30000 seconds, as long as at least one entry is available in the list. Immediately after setting a valid %AF parameter value, the beacon list is transmitted for the first time.

Every packet generated by the frequency list beacon starts with ">dF (Hz):" followed by 4 call signs and the corresponding (measured) frequency offset. Attention: The frequency list beacon is also transmitted in KISS mode. If the beacon is not desired in KISS mode, the %AF command has to be set to "0" before switching to KISS mode.

As the frequency list has a maximum size of 32 call signs, a frequency list beacon consists of up to 8 data packets in a row.

%AG (APRS Gateway Modem Type)

Default: NONE

Possible parameters: same as for %B command, additionally NONE and HF-DUAL (H only is sufficient as parameter)

Allows cross-mode-digipeating of APRS packets, i.e. packets that have to be digipeated can be transmitted in a mode different from the receiving mode.

If the %AG command is set to a value different from NONE, the DSPTNC retransmits all packets that have to be digipeated due to a valid APRS-Alias (see %AD parameter) using the modulation defined with the %AG command.

(Normally, if cross-mode-digipeating is required, this modulation will be different from the modulation defined with the %B command. However, for test purposes, both modulations may also be identical.) After transmitting a digipeated packet, the DSPTNC automatically switches back to the (normal) modulation defined with the %B command. Receiving of packets is always performed in the modulation defined with the %B command, independent of the %AG setting.

If the %AG parameter is set to a value different from NONE and the %AD parameter is set to 3 at the same time, the DSPTNC also digipeats packets without any digipeater in the path, but only if the destination call begins with the letter "A", for example "APRS". In the digipeated packet the digipeater WIDE3-3 then appears in the path. This feature can be used for routing APRS packets from short wave (where they are often transmitted without digipeater in the path to keep them as short as possible) to VHF.

These packets will appear there with WIDE3-3 in the path an thus have a good chance to reach an I-Gate to be forwarded into the Internet.

After switching to the new modulation, the transmission of the packet is delayed by at least 0.5 seconds. In this time, the DSPTNC checks if there is any valid signal in the respective modulation on the channel (DCD) and, if necessary, further delays the own transmission.

If the %AG parameter is set to HF DUAL ("H" is sufficient as argument, e.g. %AGH), the DSPTNC sends every packet which should be transmitted because of APRS digipeating, TWICE, and that directly one after the other. The first packet is always sent using "AFSK 300 Bd" modulation, using a center frequency of 2000 Hz. There follows an R300 packet. The modem waits at least two seconds between the packets, and checks in this time to see if RPR activity is present on the channel (DCD) in order to minimize collisions.

For the quasi-parallel transmission of own APRS beacons, the %AU-parameter must be set to 1 (see above). Then the setting %AGH causes transmission of the own APRS beacons in HF DUAL mode; Every transmission takes place twice, first in 300 Bd FSK and shortly after in RPR (R300).

Limitation of the DCD-function:

In case of split operation (different RX and TX frequencies), the audio signal for the DCD comes from the "wrong" source, same if a separate receiver and transmitter is used. Remark: In case of a separate RX and TX it would basically be possible to switch the RX-source with the relay output of the DSPTNC. However, this function has not yet been implemented.

%AH (Tracking, HF mode toggle)

Default: 0

Possible parameters: 0...2

Sets the modulation in tracking-mode (DIP-switch 1 set to ON) with APRS-beacon transmissions to alternate between 300 Bd (A)FSK and 300 Bd RPR, independent of the setting of the modulation which has been defined with the %B-parameter! (But the %B-parameter must be set to a value smaller than 1200, otherwise the %AH-parameter "1" will be ignored. That will prevent the station from accidentally transmitting the 300-Bd-waveform via a VHF-digipeater.

%AH2 disables this baudrate restriction but otherwise is identical to %AH1.)

The demodulator will also be set to the correct modulation before the transmission begins. This ensures the correct operation of the DCD-function with the %AH1 setting.

Important: With 300 Bd (A)FSK and with %AH1, generally the audio center frequency is set to 2000 Hz, independent of the %F-parameter. This achieves compatibility with the convention, that with FSK/RPR-channel pairs, the audio center frequency of the FSK-signal must always be 500 Hz above the center frequency of the RPR-signal.

Also refer to: http://www1.scs-ptc.com/rpr.html

The VFO-frequency of the transceiver in %AH1-operation must be set in a way, that for FSK as well as for RPR the channel is hit exactly. Using the standard frequencies on 10 and 14 MHz, this is the case when the display of the transceiver shows 10147.3 kHz (USB) or 14103.3 kHz (LSB).

Background: Up to now (June 2008) significantly more FSK-Gateways than RPR-Gateways exist on short wave channels. If, besides the robustness of the RPR-mode, also the existing FSK-infrastructure shall be used, it is recommended to transmit position data in alternating FSK/RPR-mode.

%AI (APRS tImestamp)

Default: 1

Possible parameters: 0, 1

Enables (1) or disables (0) the APRS-"Timestamp"-function. With "Timestamp" enabled the DSPTNC adds time information (UTC-time and date) to every APRS-transmission received via the NMEA-port (GPS). Of course this is only possible if the data received from the GPS receiver also contains the time information at all. If the GPS time information can be transmitted in the APRS datagrams, then the 20 minutes of GPS-timeout is not applicable any more. If the GPS-receiver fails, APRS datagrams are also transmitted after the expiry of the 20 minutes, but with the old time information, of course.

(Important is that the position and the time at the position belong together.)

%AK (APRS Robust Packet Radio SSID)

Default: 16

Value range: 0...16

If the parameter is set to a value within the range 0...15, the Tracker utilizes that value as SSID for all Unproto RPR packets (e.g. APRS beacons, also in Toggle mode). Thus, for example, it is possible to discriminate between AFSK or RPR packets during DUAL mode operation (DUAL mode beacons).

%AL (APRS Autospeed Rate)

Default: 1

Value range: 0, 2

Allows setting the beacon rate at automatic, speed dependent beacon transmission ("autospeed"). With a value of 1, "autospeed" works as usual (%AT or %AB = 0). With a value of 0 the beacon rate will be halved, with a value of 2 it will be doubled. With the speed going below one knot, the beacon rate will always be reduced to 2 transmissions per hour, independently of %AL setting.

%AM (APRS Mycall)

Default: NONE

Possible parameters: Max. 6-characters callsign with SSID

Defines the (optional) APRS-Mycall. If it is set to NONE, then the global Mycall is used as APRS-Mycall. The APRS-Mycall can be set invalid using the argument "NONE".

Using separate APRS-Mycalls can be useful, if e.g. several different APRS-systems are operated with different SSIDs or if a digipeater shall transfer APRS-data as well as normal Packet-Radio (with different SSIDs), etc.

%AN (APRS Nmea-out)

Default: 0

Possible parameters: 0...3

Physical format: 4800 Bd, 8N1, TTL.

The following modes can be set with the possible parameters:
0:	NMEA-Out is disabled.
1:	APRS-data received on the HF-port (source call, latitude, longitude) are presented as NMEA-sentence ($GPWPL) at the GPS-port of the DSPTNC/Tracker. This kind of data can be used by appropriate GPS-receivers as waypoint information and can be displayed on the screen of the receiver, matched into a map.
2:	APRS-data received on the HF-port are presented in the usual monitor format at the GPS-port.This is helpful e.g. to establish an APRS-gateway (with RX-function only) using a PC or a controller without USB.
3:	Loopback mode. The Tracker/DSPTNC presents the data received at the GPS-port ("NMEA-in") unchanged at the NMEA-out port.
4:	Combines the functions of %AN1 and %AN3. Data is presented at the NMEA-out port also when the DSPTNC is running in KISS-Mode or in Hostmode.

%AO (APRS pOsition)

Default: NONE

Possible parameters: XXXX.XXS/N YYYYY.YYW/E

Enables the entry of a position for the "FIX"-operation (Mode %A2, see above). The position must be entered matching exactly the required "Latitude Longitude"-format, which is degrees including leading zeros, directly followed by minutes with 2 decimal digits and finally the direction. All deviating formats are rejected with an error message.
Example:	%AO 4810.30N 01030.25W <Enter>

A "FIX"-position can only be replaced by a new "FIX"-position, but not completely deleted.

%AP (APRS Path)

Default: APRS via WIDE1-1 WIDE2-1

Possible parameters: APRS-target callsign and maximum 8 digipeater callsigns.

Defines the AX.25 transmit path including target callsign and maximum 8 digipeater callsigns, also with their respective SSID if necessary.
Example:	%AP CQ via RELAY <Enter>
	%AP APRS RELAY WIDE GATE <Enter>

Between the target callsign and (optionally) the digitpeater list, a "v" or "via" can be inserted to increase readability.

A description of the function of current APRS-digipeater callsigns is beyond the scope of this user manual. Appropriate information can be found in relevant literature e.g. Internet. If there is no exact information of available digipeaters available, it is recommended to set "RELAY" as the first digipeater.

%AQ (APRS Digipeater Substitution)

Default: 1

Value range: 0-1

If set to 1, "WIDEn" will be replaced by the global mycall on channel 0 in the case of WIDEn-N-Digipeating and SSID (N) decrementing to 0. The own callsign appears in the digipeater path, i.e. the path gets traceable.

%AR (APRS status report)

Default: NONE

Possible parameters: Up to 80 characters long string (text)

The command %AR serves to define an up to 80 characters long "APRS status report". In the status report, usually additional information besides the APRS-position beacon is transferred.

With the argument "-" the status-report-text can be deleted or deactivated, respectively set to "NONE". The frequency of the status-report transmission is set with the %AE-parameter (see there). Status-reports are generally sent by the DSPTNC/Tracker as separate AX.25-packets.

The (AX.25-) path is identical to that of the APRS-position packets.

%AS (APRS Short, data compression)

Default: 1

Possible parameters: 0, 1

Activates (1) or deactivates (0) the compression of the position data in the APRS-datagram.

The compressed format actually only has advantages: Shorter datagrams, higher accuracy, and speed and direction can be included in the transfer.

However, because some APRS-programs cannot correctly interpret the compressed format, the DSPTNC firmware allows the compression to be switched off. Uncompressed position data can be directly monitored as usual in "Latitude Longitude-format", as they are sent in clear text format.

%AU (APRS Unproto Cross Mode)

Default: 0

Value range: 0...2

If %AU is set to 2, the Tracker uses the particular modulation type which has been defined using the %AG-command (APRS Gateway modem type) for ALL "Unproto" packets (i.e. UI packets and also for "own" APRS beacons, etc.).

Hereby the possibility is opened to change the APRS transmission activity in general to the particular modulation art used for cross mode digipeating.

With %AU 1, unproto packets are only sent with the %AG modulation, if the first character of the target callsign is an "A", just like APRS, APXYZ, etc. In practice, it is therefore possible to send only APRS packets in %AG modulation (e. g. HF-DUAL), whereas other unproto packets will be sent in the "normal" modulation.

In practice, this is normally sensible, as in cross mode digipeating, using a transceiver in split operation, only the PTT "decides" on which "band" the TRX presently finds itself (for transmit operation). On this band, transmissions should thus normally all be of the same modulation art.

For example: One uses the Tracker for cross mode digipeating from VHF to HF (1200 Bd FSK to 300 Bd Robust Packet), e.g. for the position of a mountain tour (with VHF tracker) to also be transmitted on HF. The SCS Tracker works (for example) at the base camp at the foot of the mountain as a cross mode digipeater. Obviously then, the position of the base camp should also be transmitted in RPR on HF and not on VHF with 1200 Bd.

%AV (APRS Valid)

Default: 1200 [sec]

Possible parameters: 10...3600 [sec]

Defines how long the (old) GPS-data is valid for APRS in the %A1-mode ("GPS-mode").

%AY (APRS sYmbol)

Default: 15 [Dot]

Possible parameters: 1-94, a1-a94

Sets the graphic APRS symbol that an APRS-receiving station should display: e.g. a symbolic car in mobile service (Symbol 30). The symbol numbers follow exactly the table in the APRS-protocol version 1.0. The complete protocol information is available on the Internet. Symbols from the alternative table ("alternate table") can be selected by prefixing an "a" before the symbol number. E.g. "%AY a13" for "House (HF)".

If no symbol number is given as an argument, the "% AY" command displays the actual parameter settings with the current symbol and additionally a short description in square brackets, e.g. "A13 [House (HF)]".

Here is a selection of current symbols with their numbers:
6:	HF Gateway
7:	Small Aircraft
13:	House QTH (VHF)
a13:	House (HF)
15:	Dot
27:	Campground
28:	Motorcycle
30:	Car
47:	Balloon
50:	Recreational Vehicle
53:	Bus
56:	Helicopter
57:	Yacht (sail boat)
65:	Ambulance
66:	Bicycle
70:	Fire Truck
74:	Jeep
75:	Truck
83:	Ship (power boat)
86:	Van

%AX (APRS beacon on tracking mode only)

Default: 0

Possible parameters: 0, 1

Enables (0) or disables (1) the APRS-beacon during normal TNC-operation.

If %AX is set to 1, APRS-datagrams are only transmitted with "Tracking"-DIP-switch set to "ON". This has the advantage that one can easily switch between APRS mode and normal TNC mode with just the DIP-switch: If the DSPTNC is utilized for email exchange or chatting, one usually does not like to transmit cyclic position data the same time as well. (For position data usually separate frequencies are used which are not suitable for normal data exchange).

With %AX set to 0 the DSPTNC can transmit APRS-datagrams also during normal TNC usage ("Tracking"-DIP-switch "OFF").

%AZ (NMEA Position Information)

Default: -

Value range: -

Allows to view the current GPS position received from the attached GPS device.

Configuration of the Packet-Radio Mode (Modulation type or "baudrate" at the transceiver port)

The %B-command is used to set the Packet-Radio operating mode. For Packet-Radio generally the AX.25-protocol is utilized.

%B

Default: 1200

Possible parameters: 300, R300, R600, 1200, 9600, 19200

The DSPTNC supports the following Packet-Radio operating modes:
Parameter	Mode
300	300 Bd AFSK (old HF-Packet-Radio standard)
TX center frequency default at 1700 Hz (1600 Hz "Space", 1800 Hz "Mark"). RX center frequency: 1700 Hz +/-400 Hz, automatically tuning.
 
R300	200/600 Bd Robust-Packet-Radio (RPR) automatic selection of speed.
TX center frequency fix at 1500 Hz. RX center frequency: 1500 Hz +/-240 Hz automatically tuning. UI-packets (APRS, Unproto) are transmitted with 200 Bd RPR.
 
R600	Like R300, but UI-packets (APRS, Unproto) are transmitted with 600 Bd RPR.
 
1200	1200 Bd AFSK.
 
9600	9600 Bd Direct-FSK (G3RUH).
 
19200	19200 Bd Direct-FSK (G3RUH).

Highlights of the Modem-Implementations in the DSPTNC:

Generally very reliable and "flicker free" DCD achieved with special DCD algorithms on the signal processor.

New developed multi-detector for 300 Bd AFSK. The signal processor automatically processes a frequency range of +/-400 Hz looking for 300 Bd transmissions and receives all detected signals in parallel.

Exact tuning by the user is not necessary any more as a perfect tuning on the receiver side is always achieved automatically.

If manual tuning of the RX frequency is desired, the DCD-LED can be used for that purpose. It lights green if the actually received packet has a frequency error of less than +/-40 Hz, otherwise it lights red.

Special transmit and receive filters for 1200 Bd AFSK, to avoid adjacent channel interference and degradations by AC hum.

Adaptive DC-removal by the signal processor at 9600 and 19200 Bd Direct-FSK.

Full RPR-implementation with automatic speed switching also in KISS mode with the help of virtual link observation. (To correctly detect the data transfer speed also in KISS environment, the DSPTNC maintains virtual link blocks, which means that it analyzes the AX.25 data exchange.)

%E (Emphasis)

Default: 0

Value range: 0-6

Attenuates the lower transmit tone during transmissions on 1200 Bd at %E * 3 d. As a result, the transmit signal is "emphasized", i.e. higher frequency spectral components are amplified compared to lower frequency spectral components.

%N (TX tail)

Default: 0

Value range: 0-100

Extends the PTT time for %N * 10 milliseconds at the end of a transmission.

In quite rare cases a prolonged PTT signal is required (e.g. using FSK transmitters with inherent delay) in order to completely transmit the tail of a packet.

Support of Multi-TNC systems

%C (Extern Multi-Modem-DCD)

Default: 0

Value range: 0-1

If C% is set to 1, the external DCD control is activated. The two terminals "Data in" and "Data out" on the NMEA port will have new functions:

The "NMEA Out" function is then no longer possible via "Data out", but this terminal will always be "high" (+5 V), as long as the PTT is enabled and the TNC is transmitting. "Data in" is used as external DCD, "active high".

As soon as a "high" state is recognized by the DSPTNC, it activates the modem DCD (DCD LED is red, internal packet radio module detects DCD-state).

This allows to share a single transceiver by two DSPTNC; the access control is simply possible by "crossing" the "Data in" and "Data out" lines of the parallel connected DSPTNCs. (Do not forget the ground line!).

By the external DCD control, the simultaneous transmission of both TNCs, connected to the same transceiver, is prohibited. By using an "OR" circuit via diodes, an unlimited quantity of TNCs can be parallelized.

Setting the transmission tones at 300 Bd FSK

%F

Default: 1700 [Hz]

Possible parameters: 1000...3000 [Hz]

Defines the audio center frequency at 300 Bd (A)FSK ("old HF-Packet"). The value of 1700 Hz has established as a standard within the last years.

Setting the transmission center frequency (audio) at RPR

%L (RPR Center Frequency)

Default: 1500

Value range: 300...2400

Allows free definition of the RPR audio center frequency. This value should only be changed if necessary (e.g. for example when using a narrow-band IF filter).

It must be clear that this command will change the actual RF center frequency of the transmitted signal, also when the setting of the radio's VFO is held constant.

%L is generally used as RPR center frequency, even on DUAL modes ("HF Dual" and "Toggle Mode"). %F is not used in any case on DUAL modes but the AFSK transmit center frequency is always defined as %L+500 Hz.

Additional DSPTNC specific functions

%I (Modem Identification String)

Default: 16-character serial number of the DSPTNC

Value range: any string with maximum 16 characters

Allows to define of a modem identification string with 16 characters at maximum, for usage in multi-Tracker-systems. Default is the 16 character serial number of the DSPTNC.

%K (Automatic GPS Time Setting)

Default: 1

Value range: 0,1

With %K set to 1, GPS time and date will be taken over into the K parameter (which means setting the internal clock of the DSPTNC), as soon as data is available. GPS time is always UTC. This automatic feature leads to an always correct UTC timestamp shown together with monitor recordings.

%M (HF packet monitor)

Presents a list of the 32 last received callsigns in 300 Bd FSK mode, including the measured RX frequency offset, also refer to %T parameter.

%R (Reset)

The %R command restarts the firmware (Packet Radio) and resets the parameters to the values that has previously been stored in the non-volatile memory (see %ZS and %ZL).

Systemtest-Functions

%SY

Default: none

Possible parameters: none

With the help of the %SY-command (only in terminal mode!) the system test menu can be activated. ATTENTION: Packet-Radio-connections in progress are interrupted! The system-test-menu initiates a system reset at termination!

The system-test-menu gives out the following message at start-up:



   SYS-TEST and UTILITY Menu
   =========================

   (A)UDIO (C)ALIB (D)IP (L)ED (N)MEA (P)TT (R)ELAY (S)ERNUM (Q)uit

The system test functions are activated by entering the associated command character (in brackets).

(A)UDIO

"Audio-Loop-Test". Requires a short circuit between Audio-IN and Audio-OU at the transceiver-connector. "AUDIO OK" means that all analog components of the DSPTNC are operating properly. "ERROR" indicates a problem.

(C)ALIB

(C) initiates a test transmission of alternating 0 and 1 bits (independent of %B) on 9600 Bd modulation (like on %B 9600) using the %XF value as amplitude presetting.

Using the (U) and (D) keys (Up and Down) the signal amplitude of the test transmission can be incremented or decremented at 10 mV steps within the possible range (30...3000 mV p/p):

Using the (M) key (Mode) allows to cyclically scroll through the modes 9600, 19200 and 1200 Bd. On 1200 Bd the %XA value is used as amplitude presetting.

The (Q) key allows to terminate the calibration routine.

The calibration routine can, for example, be utilized to figure out the optimum deviation value of 9k6 transmitters.

(D)IP

Shows the actual setting of the DIP-switch. Example: DIP-SWITCH: 0000.

(L)ED

Invokes a LED-test. All LEDs lit interchanging red and green several times.

(N)MEA

Performs a test of the NMEA-interface. Requires a short circuit between NMEA-IN and NMEA-OUT. "NMEA OK" means that the NMEA-port is operating properly, "ERROR" indicates a problem.

(P)TT

Starts the PTT-toggle-test. The PTT line is toggled with every incoming <CR>. The PTT-toggle-test can be terminated with the entry of a "Q".

(R)ELAY

Starts the relay-toggle-test. The relay output is toggled with every incoming <CR> (on/off). The relay-toggle test can be terminated with the entry of a "Q".

(S)ERNUM

Displays the electronic serial number. Example: SERNUMBER: 0100000AE2ADDD48

(Q)UIT

Terminates the system-test-menu and initiates a system-RESET.

Automatic TX-frequency correction at 300 Bd (A)FSK

%T (TX frequency tracking)

Default: 1

Possible parameters: 0, 1

Enables (1) or disables (0) the automatic frequency correction at 300 Bd FSK when transmitting.

Background: The very large tracking range of the FSK-demodulator (300 Bd FSK) provides the possibility of easy and reliable receiving, but when transmitting, it can lead to not properly hitting the receiving frequency of the distant station. (As the DSPTNC does not have a tuning display, a quick and exact adjustment of the frequency at the transceiver is difficult.)

With activated automatic frequency tracking, the DSPTNC now checks a list of the 32 last received callsigns in 300 Bd FSK mode (old "HF-Packet").

This list also contains the measured frequency offsets of these stations.

If the DSPTNC now sends a packet to one of these callsigns from the list, the actual frequency offset will be considered, which means that the receiving frequency of the distant station will exactly be hit without the need to adjust the transceiver respectively.

The maximum remaining frequency offset is +/-12.5 Hz.

Setting the Transmit Level

The DSPTNC manages three separate (operating mode dependent) transmit level parameters:
XA:	For 300/1200 Bd AFSK.
XF:	For 9600 and 19200 Bd Direct-FSK.
XR:	For Robust-PR.

The valid parameter range for all of them is 30-3000 (mV, Peak-to-Peak).

%X

Default: XA = 300, XF = 600, XR = 200

Possible parameters: 30...3000

Sets all three values XA, XF, XR the same time (to the same value).

%XA

Default: 300

Possible parameters: 30...3000

Sets the XA-value (for 300/1200 Bd AFSK).

%XF

Default: 600

Possible parameters: 30...3000

Sets the XF-value (for 9600/19200 Bd Direct-FSK).

%XR

Default: 200

Possible parameters: 30...3000

Set the XR-value (for Robust-PR).

Permanent Storage of all Parameters

%ZS

The %ZS command stores all current settings permanently into the non-volatile memory of the DSPTNC. At power on, these values are always reloaded and used from now on by the firmware of the DSPTNC.

%ZL

The %ZL command resets the configuration parameters to the values that has previously been stored in the non-volatile memory of the DSPTNC (see %ZS).

This has the same effect on the parameters as power cycling with "option"-DIP-switch set to "OFF".

4. The Meaning of the LEDs

PWR/NMEA

Permanently red, when the DSPTNC is in BIOS mode.

Permanently green, when the DSPTNC is running the Packet-Radio-Firmware, but blinks momentarily red when data is read from the NMEA-port but no valid position information has been found yet.

The LED blinks green as soon as NMEA-Data with valid position information has been found and is not older than 5 seconds. "Blinking green" thus is the standard state of this LED during normal operation with a GPS device connected and valid data currently being available.

STA/TRK

In normal operation green, as long as received data in the DSPTNC is waiting to be fetched by the terminal connected.

In Tracking-mode permanently red if a "Tracking Mode Error" exists, otherwise blinks momentarily red if in normal Tracking-Mode operation and in waiting condition. During the short DCD test phase prior to the transmission of the Tracking-APRS-datagram the LED blinks alternately red/green.

In KISS-Mode red, when commands are received from the terminal. Green, when data is received from the terminal. Flashes shortly green when a data packet is sent from the DSPTNC to the terminal computer.

DCD/RDCD

Generally Digital Carrier Detection (valid receive signal is present).

At 1200 Bd AFSK and 9600/19200 Direct-FSK: green.

At 300 Bd Robust-PR: red.

At 600 Bd Robust-PR: green.

At 300 Bd AFSK: green, if the received signal has a frequency deviation of less than +/-40 Hz, red if the received signal has a frequency deviation of more than +/-40 (max. +/-400 Hz).

PTT/RPTT

Generally "Push To Talk"-function ("transmitter activated").

At AFSK and Direct-FSK as well as at 600 Bd Robust-PR green.

At 300 Bd Robust-PR red.

CON/KISS

In normal operation permanently green, if at least one "connect" exists.

In KISS-Mode permanently red.

The CON LED now intermittently shows (only in normal operation!) how many AX.25 connections are simultaneously active. For every active connection, the LED switches from color green to red for 0.5 seconds and then back to green. The number of those "red flashes" corresponds to the number of AX.25 connections. Between these "blinking connection reports" (between end of last red flash and start of first red flash of next "report") there always is a pause of 20 seconds.

5. Background-Information to "Robust-PR"-Mode (RPR)

Up to now Packet-Radio over shortwave has been basically a non-starter, it has even been heavily criticized because of the low effective throughput and repeats. AX.25 is for shortwave not an ideal protocol, but with automatic Frack-setting and a small MAXFrame values the protocol should, however, function much better on a shortwave channel than has previously been the case generally.

One cannot of course expect an asynchronous protocol to reach the same efficiency as a tight synchronous ARQ-protocol (e.g. PACTOR), but for some applications a multi-user service, with very uncritical transmit/receive switching, as well as almost zero power holding up a connection when no data passing, brings a real advantage that outweighs the lower data throughput.

What finally are the reasons that up to now HF-PR works so poorly, and apart from "forwarding" is hardly ever used? One finds a simple answer: The current modulation type for HF-PR namely uncoded 300 Bd FSK is really unsuitable for normal HF channels. The symbols are much too short even with moderate "Multi-Path effect" ("delay spread") to work.

Additionally, because no sort of error correction code is used, even short troughs or "static" will destroy a many seconds long Packet. Just one missing bit leads to a repeat of the whole packet.

To help cure this problem, SCS has developed a new class of robust modulations types especially for Packet-Radio. As a special feature for all the variants of this "Robust PR", a completely new synchronizations algorithm with tracking properties that were not possible before has been realized. Frequency deviations of +/-250 Hz are immediately recognized and without any loss of sensitivity compensated, and this with signals that are buried deep in the noise.

Because of this it's possible to remove a tuning display. One can say with good conscience that this is "Plug and Play" for shortwave.

The current firmware of the DSPTNC provides a narrow band (500 Hz) version of the "Robust PR". A wide band variant (2 kHz) with similar characteristics and 4 times the speed is generally possible.

The currently available "Robust PR" modulation types have the following properties:

Bandwidth:	500 Hz @ -30 dB
Modulation:	Pulse-Shaped OFDM (BPSK, QPSK), similar to PACTOR-III
Average Throughput:	200 or 600 Bit/sec
Crestfactor (PAPR):	3.0 or 4.2 dB
Delay-Spread:	Up to +/-8 8 msec is tolerated
Coding:	High performance convolutional code, "full-frame interleaved", rate/2 or rate3/4

As the "Robust-PR"-demodulator automatically recognizes which modulation is currently received, e.g. an APRS system can successively grow or be adapted to new requirements: If an APRS channel is used by only a few users or repeaters and the average distances are large, then longer but more robust packets can be used. With higher occupied channels with averagely lower distances, the faster and shorter packets can be used.

End of document
