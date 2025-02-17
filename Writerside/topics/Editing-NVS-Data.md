# Editing NVS Data

```bash
git clone https://github.com/ladderlogix/esp_nvs_editor
```

Get NVS.bin from ESP32_image_parser when dumping the partition

```bash
python main.py json nvs.bin nvs.json
```

Open JSON strings are hex encoded (Will change in future PR)

```bash
python main.py bin nvs.json nvs.patched
```

flash to chip (run from esp-idf 5.3, under components\partition_table)

```bash
python .\parttool.py --port COM5 write_partition --partition-name=nvs --input "D:\NVSReplacing\nvs.patched.bin"
```
