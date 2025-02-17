# 2024 Deadwood Flags

## Flag 1
There is two ways to solve this either by finding the string in the dump or physical analyzing the badge
### Physical
1. Begin by powering your badge and then recording the LED sequences flashing on the UFO. (I have a recoding [here](https://youtube.com/shorts/edSQGYkBatY?feature=share))
2. Transcribe the 8 LEDS to binary strings
``` 
01010011 
01001001 
01000111 
01111011 
01100011 
00110100 
01101110 
01110100 
01011111 
01110011 
01110100 
00110000 
01110000 
01011111 
01110100 
01101000 
00110011 
01011111 
01110011 
00110001 
01100111 
01101110 
00110100 
01101100 
01111101 
```
4. Construct a [CyberChef recipe: FromBinary_8](https://gchq.github.io/CyberChef/#recipe=From_Binary('Space',8)&input=MDEwMTAwMTEKMDEwMDEwMDEKMDEwMDAxMTEKMDExMTEwMTEKMDExMDAwMTEKMDAxMTAxMDAKMDExMDExMTAKMDExMTAxMDAKMDEwMTExMTEKMDExMTAwMTEKMDExMTAxMDAKMDAxMTAwMDAKMDExMTAwMDAKMDEwMTExMTEKMDExMTAxMDAKMDExMDEwMDAKMDAxMTAwMTEKMDEwMTExMTEKMDExMTAwMTEKMDAxMTAwMDEKMDExMDAxMTEKMDExMDExMTAKMDAxMTAxMDAKMDExMDExMDAKMDExMTExMDE)

### Firmware Strings
I threw in the bin file found from the S3 bucket into a disassembler.

<tabs>
    <tab title="IDA Pro">
        <p>This is probably the best tool to analyze the binary but with the high cost it may be unavaible to most. I searched for `{` in strings to hunt down the flag.</p>
        <img src="idastrings.png" alt="Strings view of IDA"/>
    </tab>
    <tab title="Ghidra">
        <img src="GhidraStrings.png" alt="Strings view of IDA"/>
        <p>You will want to set your language to <shortcut>Xtensa Little Endian</shortcut></p>
        <img src="GhidraFlag1.png" alt=""></img>
        <p>It is still hard to pull out the string on this but if you scroll down you will see you string with a message</p>
    </tab>
    <tab title="Binary Ninja">
        <p>This is not the best for decomplation but strings are on point</p>
        <img src="BinNinjaString1.png" alt="Bin strings"/>
    </tab>
    <tab title="Strings">
        To show all strings
        <code-block>string firmware.bin</code-block>
        You can filter down on URL
        <code-block>strings wwhf2024.bin | grep "\.com" -A 4</code-block>
        Or even use bulk extractor to get more info out of the binary
    </tab>
</tabs>


## Flag 2

This is the serial message that will appear after the ascii art. It will take a while for it to pop up

### Serial Output {collapsible="true"}
```
[BADGE] Loading badge config...
[BADGE] Loading bounties...
  - Vendors: 26
  - Staff: 2
  - Tracks: 4
  - Badge: 4
  - SpecialEvents: 4
[BADGE] WWHF Badge v1.26
[BADGE] 24:58:7C:F4:8E:30
[WiFi] Wi-Fi ready
[WiFi] Station mode started
[WiFi] Sta[WiFi] Station disconnected from AP
[WiFi] Station disconnected from AP
[WiFi] Station disconnected from AP
[WiFi] Station disconnected from AP
[WiFi] Station disconnected from AP
[WiFi] Station disconnected from AP
[WiFi] Station disconnected from AP
[WiFi] Station disconnected from AP
[WiFi] Station disconnected from AP
[WiFi] Station disconnected from AP
[WiFi] Station connected to AP
[WiFi] Station got IP address
[WiFi] Registering device with API server
[BADGE] Writing badge config...
[BADGE] Boounties: Staff 2 | Event 4 | Track 4 | Vendor 26 | Badge 4
[BADGE] Writing bounty tracker...
[WiFi] Registration success
[MQTT] Connecting to MQTT Broker as WrangleCelestial3...
[MQTT] Connected to MQTT broker
[MQTT] Subscribed to broadcast messages
[MQTT] Published registration messages
[BADGE] Added Voltage to index 28: 3.90 volts (59%) [Avg: 3.86 volts (52%)]
[BADGE] Added Voltage to index 29: 3.90 volts (61%) [Avg: 3.86 volts (52%)]
[BADGE] Added Voltage to index 30: 3.90 volts (59%) [Avg: 3.86 volts (52%)]
                           _╓▄▓▓▓╬╩^``▒_,░µ_ _                                  
                         ,▓▓╬╢╩`╓Ü@D╚╩``╙╙╩Ü=_-╔___                             
                       ╓▓▓█╬╬Ü#R`Ü `         '|»░`░_                            
                      ║▓█▓╬▒░░Ü ````           `╙\²                            
                      ▓█▓█▓╬Ü`                   '                              
                     ▓█▓███╬Ü                                                   
                    ▐█▓▓██╬╩H                                                   
                    ║█▓██▓╬_                                                    
                    '▓▓▓██▓▒╓_   '    ___                                       
                     █▓▓██╩▀▀▓▄▓_   _______                                     
                     ▀║▓██▓▓Ö╠╬█▌,,`╠╠╩▓DHφ_  __,,,╔▄                           
                   '▀%╣▓█╣╠╬▒╠█▓  \_ ╠==      `      j                         
                    ' ║██▓╬▒╣███       ``    ,                                  
                     ║▓▓╬╣╫╫▓██▒    `ⁿq∩                                        
                     ╠▓█╣╬║▓███▓_   _  '_                                       
                     ║▓▓█▓▓██████▒_                                             
                     ╠▓█████▌╬▒╬Ü▒__╓_,_                                        
                    |║▓█▓██╬███╬▀╙^`````╚▒                                      
                     [╣█▓█╬╣▓█▒▒∩▒__                                            
                     ╙╠▓▓╬╬╣██╣▒_╙`                     .╓╖__                   
                    ⁿ^╠║╬╠╬╩║╝Ñ╬▒__                      ╣▓▓███▓▄╖,_            
                     |▒╩╣╬]╠ÜÜ╠╬╬R@╓╦∩                ,▓█████▓█▓▓▓▓▓██▓▄,       
                     `╔╬╬╣▒╩▒▒∩Ü╠Ü║▒¼               _╣▓████▓█▓▓███▓▓▓▓▓▓▓██▄,   
                     ,▄╣╬╬Ñ╬▒╬╬╩ÜÜ╩╩                ╔▓▓█▓▓▓▓▓▓▓█▓█▓█▓██▓▓▓▓▓▓██▄
                _╓▄██▓╬╬╠▒^ÜÅ╚╙▒[ÜÄ               ,╔▓█▓█▓▓▓▓▓▓█▓▓▓███▓█▓▓▓▓▓▓▓▓▓
             ,▄███████╬╬╬░Ü╙²-   ╠_  ,          ▄▄▓▓█▓██▓▓▓█▓▓▓▓▓▓██▓▓██▓▓█▓▓▓▓▓
           ▄████████████▓▒^▒UÜ _jÜ`=_        µ▓▓█▓█████▓█▓▓█▓▓▓▓██▓██▓█▓█▓▓▓▓▓█▓
         ╔█████████████████▓▓@▄▒╔╠╗▄,_╓-__╒,╦╬█████████████▓▓▓▓▓█▓▓▓▓█▓▓▓▓▓▓███▓
        ╔██████████████████████████████▓██▓▓████████▓████▓▓▓█▓██▓█▓▓██▓█▓▓█▓█▓██

        I2luY2x1ZGUgPHN0ZGlvLmg+CiNpbmNsdWRlIDxzdHJpbmcuaD4KI2luY2x1ZGUgPHN0ZGxpYi5oPgoKdm9pZCByKGNoYXIgKnMsIGludCBzaGlmdCkgewogICAgZm9yIChpbnQgaSA9IDA7IHNbaV07IGkrKykgewogICAgICAgIGlmIChzW2ldID49ICdBJyAmJiBzW2ldIDw9ICdaJykgc1tpXSA9ICgoc1tpXSAtICdBJyArIHNoaWZ0KSAlIDI2KSArICdBJzsKICAgICAgICBlbHNlIGlmIChzW2ldID49ICdhJyAmJiBzW2ldIDw9ICd6Jykgc1tpXSA9ICgoc1tpXSAtICdhJyArIHNoaWZ0KSAlIDI2KSArICdhJzsKICAgIH0KfQoKaW50IG1haW4oaW50IGFyZ2MsIGNoYXIgKmFyZ3ZbXSkgewogICAgaWYgKGFyZ2MgIT0gMikgcmV0dXJuIDE7CiAgICBpbnQgc2hpZnQgPSBhdG9pKGFyZ3ZbMV0pOwogICAgY2hhciBmW10gPSAiRlZUe3JMcl90MF8zaTNlbGp1M2UzfSI7CiAgICByKGYsIHNoaWZ0KTsKICAgIHByaW50ZigiJXNcbiIsIGYpOwogICAgcmV0dXJuIDA7Cn0K
[BADGE] Added Voltage to index 31: 3.93 volts (65%) [Avg: 3.86 volts (53%)]
```

### Extraction of string

Extracting the base64 leaves us with

```
I2luY2x1ZGUgPHN0ZGlvLmg+CiNpbmNsdWRlIDxzdHJpbmcuaD4KI2luY2x1ZGUgPHN0ZGxpYi5oPgoKdm9pZCByKGNoYXIgKnMsIGludCBzaGlmdCkgewogICAgZm9yIChpbnQgaSA9IDA7IHNbaV07IGkrKykgewogICAgICAgIGlmIChzW2ldID49ICdBJyAmJiBzW2ldIDw9ICdaJykgc1tpXSA9ICgoc1tpXSAtICdBJyArIHNoaWZ0KSAlIDI2KSArICdBJzsKICAgICAgICBlbHNlIGlmIChzW2ldID49ICdhJyAmJiBzW2ldIDw9ICd6Jykgc1tpXSA9ICgoc1tpXSAtICdhJyArIHNoaWZ0KSAlIDI2KSArICdhJzsKICAgIH0KfQoKaW50IG1haW4oaW50IGFyZ2MsIGNoYXIgKmFyZ3ZbXSkgewogICAgaWYgKGFyZ2MgIT0gMikgcmV0dXJuIDE7CiAgICBpbnQgc2hpZnQgPSBhdG9pKGFyZ3ZbMV0pOwogICAgY2hhciBmW10gPSAiRlZUe3JMcl90MF8zaTNlbGp1M2UzfSI7CiAgICByKGYsIHNoaWZ0KTsKICAgIHByaW50ZigiJXNcbiIsIGYpOwogICAgcmV0dXJuIDA7Cn0K
```

which in from base64 [cyberchef](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=STJsdVkyeDFaR1VnUEhOMFpHbHZMbWcrQ2lOcGJtTnNkV1JsSUR4emRISnBibWN1YUQ0S0kybHVZMngxWkdVZ1BITjBaR3hwWWk1b1Bnb0tkbTlwWkNCeUtHTm9ZWElnS25Nc0lHbHVkQ0J6YUdsbWRDa2dld29nSUNBZ1ptOXlJQ2hwYm5RZ2FTQTlJREE3SUhOYmFWMDdJR2tyS3lrZ2V3b2dJQ0FnSUNBZ0lHbG1JQ2h6VzJsZElENDlJQ2RCSnlBbUppQnpXMmxkSUR3OUlDZGFKeWtnYzF0cFhTQTlJQ2dvYzF0cFhTQXRJQ2RCSnlBcklITm9hV1owS1NBbElESTJLU0FySUNkQkp6c0tJQ0FnSUNBZ0lDQmxiSE5sSUdsbUlDaHpXMmxkSUQ0OUlDZGhKeUFtSmlCelcybGRJRHc5SUNkNkp5a2djMXRwWFNBOUlDZ29jMXRwWFNBdElDZGhKeUFySUhOb2FXWjBLU0FsSURJMktTQXJJQ2RoSnpzS0lDQWdJSDBLZlFvS2FXNTBJRzFoYVc0b2FXNTBJR0Z5WjJNc0lHTm9ZWElnS21GeVozWmJYU2tnZXdvZ0lDQWdhV1lnS0dGeVoyTWdJVDBnTWlrZ2NtVjBkWEp1SURFN0NpQWdJQ0JwYm5RZ2MyaHBablFnUFNCaGRHOXBLR0Z5WjNaYk1WMHBPd29nSUNBZ1kyaGhjaUJtVzEwZ1BTQWlSbFpVZTNKTWNsOTBNRjh6YVRObGJHcDFNMlV6ZlNJN0NpQWdJQ0J5S0dZc0lITm9hV1owS1RzS0lDQWdJSEJ5YVc1MFppZ2lKWE5jYmlJc0lHWXBPd29nSUNBZ2NtVjBkWEp1SURBN0NuMEsNCg&ieol=CRLF) give us

```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

void r(char *s, int shift) {
    for (int i = 0; s[i]; i++) {
        if (s[i] >= 'A' && s[i] <= 'Z') s[i] = ((s[i] - 'A' + shift) % 26) + 'A';
        else if (s[i] >= 'a' && s[i] <= 'z') s[i] = ((s[i] - 'a' + shift) % 26) + 'a';
    }
}

int main(int argc, char *argv[]) {
    if (argc != 2) return 1;
    int shift = atoi(argv[1]);
    char f[] = "FVT{rLr_t0_3i3elju3e3}";
    r(f, shift);
    printf("%s\n", f);
    return 0;
}
```

This appears to be a simple ceaser cipher. Using [cyberchef](https://gchq.github.io/CyberChef/#recipe=ROT13_Brute_Force(true,true,false,100,0,true,'')&input=RlZUe3JMcl90MF8zaTNlbGp1M2UzfQ&ieol=CRLF) rot13 brute force we can see an option that makes sense

```
Amount =  1: GWU{sMs_u0_3j3fmkv3f3}
Amount =  2: HXV{tNt_v0_3k3gnlw3g3}
Amount =  3: IYW{uOu_w0_3l3homx3h3}
Amount =  4: JZX{vPv_x0_3m3ipny3i3}
Amount =  5: KAY{wQw_y0_3n3jqoz3j3}
Amount =  6: LBZ{xRx_z0_3o3krpa3k3}
Amount =  7: MCA{ySy_a0_3p3lsqb3l3}
Amount =  8: NDB{zTz_b0_3q3mtrc3m3}
Amount =  9: OEC{aUa_c0_3r3nusd3n3}
Amount = 10: PFD{bVb_d0_3s3ovte3o3}
Amount = 11: QGE{cWc_e0_3t3pwuf3p3}
Amount = 12: RHF{dXd_f0_3u3qxvg3q3}
Amount = 13: SIG{eYe_g0_3v3rywh3r3}
Amount = 14: TJH{fZf_h0_3w3szxi3s3}
Amount = 15: UKI{gAg_i0_3x3tayj3t3}
Amount = 16: VLJ{hBh_j0_3y3ubzk3u3}
Amount = 17: WMK{iCi_k0_3z3vcal3v3}
Amount = 18: XNL{jDj_l0_3a3wdbm3w3}
Amount = 19: YOM{kEk_m0_3b3xecn3x3}
Amount = 20: ZPN{lFl_n0_3c3yfdo3y3}
Amount = 21: AQO{mGm_o0_3d3zgep3z3}
Amount = 22: BRP{nHn_p0_3e3ahfq3a3}
Amount = 23: CSQ{oIo_q0_3f3bigr3b3}
Amount = 24: DTR{pJp_r0_3g3cjhs3c3}
Amount = 25: EUS{qKq_s0_3h3dkit3d3}
```

```
SIG{eYe_g0_3v3rywh3r3}
```

### Alternate Method
We can also find this large block of text through strings. Then follow similar methods to above.

```
strings wwhf2024.bin | awk 'length($0) > 200' | base64 -d
```

## Flag 3

The LED in the button right will sometimes burst in a weird pattern. Setting up a youtube live stream to catch this we can create a [video](https://youtube.com/shorts/KFdoEGLke7U?feature=share).

Decoding leaves us with
```
.... .- -.-. -.- - .... . ... .. --. -. .- .-.. ...
```

Putting this into english gives us

```
hackthesignals
```

## Flag 4
This one is very tricky but is the decryption of the mystery signal. If you monitered MQTT long enough you might have saw a mystery signal event pop up. In the data field is an encrypted blob that contains the flag.

```mqtt
{
  "broadcast_event": 12,
  "data": "WfFIAmqjR7RjIJ8fsPdkeUKIUxqrkeZSk9G2+rUKtVv5H1XOmrEWpZjyRUtZVL2AXp4yRFwLkc4C9MgnBwarOA==",
  "date": "10/09/24",
  "time": "05:51PM UTC",
  "message": "Mystery Signal"
}
```

We know that the badge can decrypt the message so we need some way to send it a message so the decryption key can be in memory. We made the badge get extra points by sending it a scan message. ([See extra on this was done](Extra.md#how-the-scanners-report-a-scan))

When we are ready to download memory we do
```bash
python -m esptool dump_mem 0x40380000 393216 out.bin 
```
We choose this memory section based on esp32 [docs](https://dl.espressif.com/public/esp32s3-mm.pdf). (S3 mem sections from @precurse)

To extract all aes keys that are the size we are looking for

```bash
strings out.bin | awk 'length($0) == 16'
```

Using the key to decrypt we get the message. [CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)AES_Decrypt(%7B'option':'Hex','string':'36336135666435393638386530346137'%7D,%7B'option':'Hex','string':''%7D,'ECB','Raw','Raw',%7B'option':'Hex','string':''%7D,%7B'option':'Hex','string':''%7D)From_Hex('Auto')&input=V2ZGSUFtcWpSN1JqSUo4ZnNQZGtlVUtJVXhxcmtlWlNrOUcyK3JVS3RWdjVIMVhPbXJFV3BaanlSVXRaVkwyQVhwNHlSRndMa2M0QzlNZ25Cd2FyT0E9PQ)