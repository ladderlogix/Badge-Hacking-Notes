# Analyzing Flash Dump

<note>

This was written with the 2024 badge while the steps remain the same the data/passwords/urls have changed

</note>

I will be using [esp32 image parser](https://github.com/tenable/esp32_image_parser) to analyze, instead of [esp32knife](https://github.com/BlackVS/esp32knife) because I could not esp32knife to recognize my dump or get it to dump my firmware

## Installing esp32 image parser

### Download the repo
```bash
git clone https://github.com/tenable/esp32_image_parser
```
### Install Requirements
```bash
python -m pip install -r requiremnets.txt
```

## Showing the dump partitions

```bash
python esp32_image_parser.py show_partitions badge_flash.bin
```

### Annotated Output
```
└─$ python esp32_image_parser.py show_partitions badge_flash.bin
reading partition table...
entry 0:
  label      : nvs       -> Non-voliatle Storage
  offset     : 0x9000
  length     : 20480
  type       : 1 [DATA]
  sub type   : 2 [WIFI]

entry 1:
  label      : otadata
  offset     : 0xe000
  length     : 8192
  type       : 1 [DATA]
  sub type   : 0 [OTA]

entry 2:
  label      : app0       -> Core 0 Program
  offset     : 0x10000
  length     : 3342336
  type       : 0 [APP]
  sub type   : 16 [ota_0]

entry 3:
  label      : app1       -> Core 1 Program
  offset     : 0x340000
  length     : 3342336
  type       : 0 [APP]
  sub type   : 17 [ota_1]

entry 4:
  label      : spiffs     -> File Systems
  offset     : 0x670000
  length     : 1572864
  type       : 1 [DATA]
  sub type   : 130 [unknown]

entry 5:
  label      : coredump   -> Crash Dumps
  offset     : 0x7f0000
  length     : 65536
  type       : 1 [DATA]
  sub type   : 3 [unknown]

MD5sum:
467eb896e29d1aa938c557f4cdfc4c5b
Done
```

## Pulling Data Out of the Partitions
### Reading Non-Volatile Storage

Esp32 Image Parser can dump nvs data

```bash
python esp32_image_parser.py show_partitions badge_flash.bin
```

#### Truncated Output
```bash
└─$ python esp32_image_parser.py dump_nvs badge.bin -partition nvs
Dumping partition 'nvs' to nvs_out.bin
Page 0
  page state : FULL
  page seq no. : 0
  page version : 2
  crc 32 : 3115986308
  page entry state bitmap (decoded) : 222222222222222222222222202222222222222222222222222222222222222222222222222222222222222222222222222222222222202222222222222233
  Entry 0
  Bitmap State : Written
    Written Entry 0
      NS Index : 0
      Type : U8
      Span : 1
      ChunkIndex : 255
      Key : badge_config
      Data (U8) : 1

  Entry 1
  Bitmap State : Written
    Written Entry 1
      NS Index : 1
          NS : badge_config
      Type : STR
      Span : 3
      ChunkIndex : 255
      Key : ServerPath
      String :
        Size : 38
        Data : https://wwhf2024.s3.amazonaws.com/v2/

  Entry 4
  Bitmap State : Written
    Written Entry 4
      NS Index : 1
          NS : badge_config
      Type : STR
      Span : 2
      ChunkIndex : 255
      Key : FirmwareName
      String :
        Size : 13
        Data : wwhf2024.bin

  Entry 6
  Bitmap State : Written
    Written Entry 6
      NS Index : 1
          NS : badge_config
      Type : I32
      Span : 1
      ChunkIndex : 255
      Key : HardwareType
      Data (I32) : 4

  Entry 7
  Bitmap State : Written
    Written Entry 7
      NS Index : 1
          NS : badge_config
      Type : STR
      Span : 2
      ChunkIndex : 255
      Key : SSIDName
      String :
        Size : 12
        Data : WWHF Badges

  Entry 9
  Bitmap State : Written
    Written Entry 9
      NS Index : 1
          NS : badge_config
      Type : STR
      Span : 2
      ChunkIndex : 255
      Key : SSIDPassword
      String :
        Size : 18
        Data : 77HackTheEsp3too@

  Entry 11
  Bitmap State : Written
    Written Entry 11
      NS Index : 1
          NS : badge_config
      Type : STR
      Span : 3
      ChunkIndex : 255
      Key : MQTTBroker
      String :
        Size : 34
        Data : w6e1deed.ala.us-east-1.emqxsl.com

  Entry 14
  Bitmap State : Written
    Written Entry 14
      NS Index : 1
          NS : badge_config
      Type : STR
      Span : 2
      ChunkIndex : 255
      Key : MQTTBroadcast
      String :
        Size : 17
        Data : broadcast/action

  Entry 16
  Bitmap State : Written
    Written Entry 16
      NS Index : 1
          NS : badge_config
      Type : STR
      Span : 2
      ChunkIndex : 255
      Key : MQTTEvents
      String :
        Size : 14
        Data : events/device

  Entry 18
  Bitmap State : Written
    Written Entry 18
      NS Index : 1
          NS : badge_config
      Type : STR
      Span : 2
      ChunkIndex : 255
      Key : MQTTUsername
      String :
        Size : 7
        Data : badges

  Entry 20
  Bitmap State : Written
    Written Entry 20
      NS Index : 1
          NS : badge_config
      Type : STR
      Span : 2
      ChunkIndex : 255
      Key : MQTTPassword
      String :
        Size : 20
        Data : TPY_net1pvg*ywf.cjk

  Entry 22
  Bitmap State : Written
    Written Entry 22
      NS Index : 1
          NS : badge_config
      Type : I32
      Span : 1
      ChunkIndex : 255
      Key : MQTTPort
      Data (I32) : 8883

  Entry 23
  Bitmap State : Written
    Written Entry 23
      NS Index : 1
          NS : badge_config
      Type : STR
      Span : 2
      ChunkIndex : 255
      Key : DeviceName
      String :
        Size : 14
        Data : HaySupernova2
```

### Extracting App0/1 + Other Partitions
While not the most need is possible to pull out app0 using this program. This could be used to convert to an elf but at this time no program that I know can convert this with out errors.

```bash
└─$ python esp32_image_parser.py dump_partition badge_flash.bin -partition app0
Dumping partition 'app0' to app0_out.bin
```
# Reviewing Creds found in NVS

We see three big systems of interest in NVS

## Wifi Creds

We can pull the SSID and Password
```
SSID: WWHF Badges
Password: 77HackTheEsp3too@
```
There is nothing of interest using that as all transmissions are encrypted, but if you are not connected to the official wifi you can create your own
## AWS Bucket

We see where the bin files are being stored
```
https://wwhf2024.s3.amazonaws.com/v2/wwhf2024.bin
```

and we can guess and see that there is another one at

```
https://wwhf2024.s3.amazonaws.com/v1/wwhf2024.bin
```

## MQTT

```
URL:  w6e1deed.ala.us-east-1.emqxsl.com
Username: badges
Password: TPY_net1pvg*ywf.cjk
MQTT Port: 8883

Subscriptions
	- broadcast/action
	- events/device
	- badges (Do not worry see extra for more info on this one)
```

