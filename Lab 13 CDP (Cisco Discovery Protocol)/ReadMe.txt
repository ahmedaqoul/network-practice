Lab 13 CDP (Cisco Discovery Protocol)

1. Use CDP to identify which interfaces are used to connect the routers and switches.  
2. Determine which side of the serial connection between R1 and R2 is DCE, and which is DTE. Set a clock rate of 64 KB/s on the DCE side.  
3. What are the default CDP send and hold timers? Confirm this with a show command on one of the devices.  
4. Disable CDP globally on R1, and attempt to view CDP neighbors.  
5. Enable CDP globally on R1, and immediately view CDP neighbors again. Do SW1 and R2 appear instantly?  
6. Disable CDP on the switch interfaces connected to PCs.







R1 :


Router>
Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#int se0/1/0
R1(config-if)#ip address 10.0.0.1 255.255.255.0
R1(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/1/0, changed state to down
R1(config-if)#exit
R1(config)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up

	
R1(config)#exit
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#sh ip int br
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        unassigned      YES unset  administratively down down 
FastEthernet0/1        unassigned      YES unset  administratively down down 
Serial0/1/0            10.0.0.1        YES manual up                    up 
Serial0/1/1            unassigned      YES unset  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down
R1#
R1#
R1# 
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int f0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#no sh

R1(config-if)#
%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up

R1(config-if)#exit






R2 :



Router>
Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R2
R2(config)#int se0/0/0
R2(config-if)#ip address 10.0.0.2 255.255.255.0
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

R2(config-if)#exit
R2(config)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

R2(config)#
R2(config)#exit
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#sh ip int br
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        unassigned      YES unset  administratively down down 
FastEthernet0/1        unassigned      YES unset  administratively down down 
Serial0/0/0            10.0.0.2        YES manual up                    up 
Serial0/0/1            unassigned      YES unset  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down
R2#
R2#
R2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#int f0/0
R2(config-if)#ip address 192.168.2.1 255.255.255.0
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up

R2(config-if)#exit







1. Use CDP to identify which interfaces are used to connect the routers and switches.


R2>
R1>en
R1#sh cdp neigh
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
R2           Ser 0/1/0        166            R       C1841       Ser 0/0/0
Switch       Fas 0/0          166            S       2950        Fas 0/1



R2>
R2>en
R2#sh cdp neigh
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
R1           Ser 0/0/0        140            R       C1841       Ser 0/1/0
Switch       Fas 0/0          140            S       2950        Fas 0/1



SW2#sh cdp neigh
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
R2           Fas 0/1          177            R       C1841       Fas 0/0




2. Determine which side of the serial connection between R1 and R2 is DCE, and which is DTE. Set a clock rate of 64 KB/s on the DCE side. 



R1>
R1>en
R1#sh controllers se0/1/0
Interface Serial0/1/0
Hardware is PowerQUICC MPC860
DCE V.35, clock rate 2000000
idb at 0x81081AC4, driver data structure at 0x81084AC0
SCC Registers:
General [GSMR]=0x2:0x00000000, Protocol-specific [PSMR]=0x8
Events [SCCE]=0x0000, Mask [SCCM]=0x0000, Status [SCCS]=0x00
Transmit on Demand [TODR]=0x0, Data Sync [DSR]=0x7E7E
Interrupt Registers:
Config [CICR]=0x00367F80, Pending [CIPR]=0x0000C000
Mask   [CIMR]=0x00200000, In-srv  [CISR]=0x00000000
Command register [CR]=0x580
Port A [PADIR]=0x1030, [PAPAR]=0xFFFF
       [PAODR]=0x0010, [PADAT]=0xCBFF
Port B [PBDIR]=0x09C0F, [PBPAR]=0x0800E
       [PBODR]=0x00000, [PBDAT]=0x3FFFD
Port C [PCDIR]=0x00C, [PCPAR]=0x200
       [PCSO]=0xC20,  [PCDAT]=0xDF2, [PCINT]=0x00F
Receive Ring
        rmd(68012830): status 9000 length 60C address 3B6DAC4
        rmd(68012838): status B000 length 60C address 3B6D444
Transmit Ring
        tmd(680128B0): status 0 length 0 address 0
        tmd(680128B8): status 0 length 0 address 0
        tmd(680128C0): status 0 length 0 address 0
        tmd(680128C8): status 0 length 0 address 0
        tmd(680128D0): status 0 length 0 address 0
        tmd(680128D8): status 0 length 0 address 0
        tmd(680128E0): status 0 length 0 address 0
        tmd(680128E8): status 0 length 0 address 0
        tmd(680128F0): status 0 length 0 address 0
        tmd(680128F8): status 0 length 0 address 0
        tmd(68012900): status 0 length 0 address 0
        tmd(68012908): status 0 length 0 address 0
        tmd(68012910): status 0 length 0 address 0
        tmd(68012918): status 0 length 0 address 0
        tmd(68012920): status 0 length 0 address 0
        tmd(68012928): status 2000 length 0 address 0

tx_limited=1(2)
 
SCC GENERAL PARAMETER RAM (at 0x68013C00)
Rx BD Base [RBASE]=0x2830, Fn Code [RFCR]=0x18
Tx BD Base [TBASE]=0x28B0, Fn Code [TFCR]=0x18
Max Rx Buff Len [MRBLR]=1548
Rx State [RSTATE]=0x0, BD Ptr [RBPTR]=0x2830
Tx State [TSTATE]=0x4000, BD Ptr [TBPTR]=0x28B0
 
SCC HDLC PARAMETER RAM (at 0x68013C38)
CRC Preset [C_PRES]=0xFFFF, Mask [C_MASK]=0xF0B8
Errors: CRC [CRCEC]=0, Aborts [ABTSC]=0, Discards [DISFC]=0
Nonmatch Addr Cntr [NMARC]=0
Retry Count [RETRC]=0
Max Frame Length [MFLR]=1608
Rx Int Threshold [RFTHR]=0, Frame Cnt [RFCNT]=0
User-defined Address 0000/0000/0000/0000
User-defined Address Mask 0x0000


buffer size 1524

PowerQUICC SCC specific errors:
0 input aborts on receiving flag sequence
0 throttles, 0 enables
0 overruns
0 transmitter underruns
0 transmitter CTS losts
0 aborted short frames

R1#
R1#
R1#int se0/1/0
       ^
% Invalid input detected at '^' marker.
	
R1#
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int se0/1/0
R1(config-if)#clock rate 64000




3. What are the default CDP send and hold timers? Confirm this with a show command on one of the devices. 


R1#
R1#sh cdp interface
Vlan1 is administratively down, line protocol is down
  Sending CDP packets every 60 seconds
  Holdtime is 180 seconds
FastEthernet0/0 is up, line protocol is up
  Sending CDP packets every 60 seconds
  Holdtime is 180 seconds
FastEthernet0/1 is administratively down, line protocol is down
  Sending CDP packets every 60 seconds
  Holdtime is 180 seconds
Serial0/1/0 is up, line protocol is up
  Sending CDP packets every 60 seconds
  Holdtime is 180 seconds
Serial0/1/1 is administratively down, line protocol is down
  Sending CDP packets every 60 seconds
  Holdtime is 180 seconds




4. Disable CDP globally on R1, and attempt to view CDP neighbors. 
/ 
5. Enable CDP globally on R1, and immediately view CDP neighbors 


R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#
R1(config)#cdp run
R1(config)#no cdp run
R1(config)#do sh cdp neigh
% CDP is not enabled
R1(config)#cdp run
R1(config)#do sh cdp neigh
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
Switch       Fas 0/0          176            S       2950        Fas 0/1
R2           Ser 0/1/0        176            R       C1841       Ser 0/0/0


6. Disable CDP on the switch interfaces connected to PCs.

Switch#
Switch#sh cdp neigh
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
R1           Fas 0/1          125            R       C1841       Fas 0/0
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#int range fa0/1 - 3
Switch(config-if-range)#cdp enable
Switch(config-if-range)#no cdp enable
