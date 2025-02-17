# Extra

## How the scanners report a scan

You might notice some activity on MQTT when you scan your badge but this not how the server determines if a scan takes place. It uses http post request to /bouties

It can be represented as

```python
import requests

url = "https://hardwarelabs.io/bounties"

payload = "{\r\n    \"mac_address\": \"30:30:F9:7B:90:AC\",\r\n    \"rfid\": \"f4bfd940\"\r\n}"
headers = {}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
```

or

![image_5.png](image_5.png)

where rfid is the lower case UID. We discoverd the format through the serial messages produced by the badge reader.

Badge reader serial:
```
[BADGE] Loading badge config...
[BADGE] Loading bounties...
[BADGE] WWHF Badge v1.27
[BADGE] EC:DA:3B:5E:12:78
[WiFi] Wi-Fi ready
[WiFi] Station mode started
[BOUNTY] Mounted bounty cache filesystem
[WiFi] Station connected to AP
[RFID] Bounty Captured: f4bfd940
```

From here I wrote a script to brute force all the OUIs we saw on the network to try to hunt down the last event 5 scanner (We later found out this doesn't exist)

### Scanner Brute Force

```python
import aiohttp
import asyncio
import json
import itertools
from tqdm import tqdm

url = "https://hardwarelabs.io/bounties"

# OUI of the scanners
ouis = ["34:B7:DA:5E","34:B7:DA:5F","34:B7:DA:5D"]

mac_suffix_range = range(0x00, 0xFF + 1)  # This will range from 00 to FF (0-255 in hex)

headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer W8nqEekeZ2a4LeXVVuQ2yNYwRrsDYT'
}
rfid = "f4bfd940"

# Limit the number of concurrent requests
MAX_CONCURRENT_REQUESTS = 1000

async def send_request(session, mac_address):
    payload = json.dumps({
        "mac_address": mac_address,
        "rfid": rfid
    })
    
    async with session.post(url, headers=headers, data=payload) as response:
       # print(await response.text())
        #print(response.status)
        if response.status == 201:  # Only print if the status is 201
            response_text = await response.text()
            print(f"Testing MAC: {mac_address} -> Response: {response_text}")
            return True  # Indicating success
        return False  # Indicating failure

async def brute_force_mac_addresses():
    total_mac_addresses = len(ouis) * (0xFF + 1) * (0xFF + 1)
    progress = tqdm(total=total_mac_addresses, desc="Brute-forcing MAC addresses", unit="request")

    # Set up the aiohttp session
    async with aiohttp.ClientSession() as session:
        tasks = []
        for oui in ouis:
            for suffix1, suffix2 in itertools.product(mac_suffix_range, repeat=2):
                mac_address = f"{oui}:{suffix1:02X}:{suffix2:02X}"
                print(mac_address)
                
                # Create the task for each request
                task = asyncio.ensure_future(send_request(session, mac_address))
                tasks.append(task)
                
                # Send a batch of requests
                if len(tasks) >= MAX_CONCURRENT_REQUESTS:
                    results = await asyncio.gather(*tasks)
                    tasks = []  # Reset the task list
                    progress.update(MAX_CONCURRENT_REQUESTS)

        # Await any remaining tasks
        if tasks:
            await asyncio.gather(*tasks)
            progress.update(len(tasks))
    
    progress.close()  # Close the progress bar when done

if __name__ == "__main__":
    asyncio.run(brute_force_mac_addresses())
```

### All scans solver

**This code is from @CW0 and his [repo](https://github.com/Cooperw/ctf/tree/master/2024-10-11-wwhf24)**
```python
# claim_bounties.py :: GPT-4o

# usage: python claim_bounties.py 7bdfd941

import requests
import sys
import argparse
import time
import os


'''
export API_BEARER_TOKEN="W8nqEekeZ2a4LeXVVuQ2yNYwRrsDYT"

7bdfd940 shiloh
f4b4d940 logix
7159d940 vort
fcb9d940 alex
a3c9d940 rsulliva
57b4d940 netwarsninja66
'''

def main():
    # Define the list of MAC addresses
    mac_addresses = [
        "EC:DA:3B:5D:D0:7C",
        "EC:DA:3B:5E:0C:24",
        "EC:DA:3B:5E:0C:34",
        "EC:DA:3B:5E:0C:88",
        "EC:DA:3B:5E:11:D8",
        "EC:DA:3B:5E:11:E8",
        "EC:DA:3B:5E:11:F0",
        "EC:DA:3B:5E:11:F4",
        "EC:DA:3B:5E:12:30",
        "EC:DA:3B:5E:12:34",
        "EC:DA:3B:5E:12:78",
        "EC:DA:3B:5E:12:8C",
        "EC:DA:3B:5E:12:90",
        "EC:DA:3B:5E:12:A8",
        "EC:DA:3B:5E:16:D0",
        "EC:DA:3B:5E:16:F0",
        "EC:DA:3B:5E:1F:80",
        "EC:DA:3B:5E:1F:C8",
        "EC:DA:3B:5E:48:30",
        "EC:DA:3B:5E:53:F8",
        "EC:DA:3B:5E:6D:48",
        "EC:DA:3B:5E:6D:54",
        "EC:DA:3B:5E:6D:5C",
        "EC:DA:3B:5E:6D:68",
        "EC:DA:3B:5E:6D:70",
        "EC:DA:3B:5E:6D:80",
        "EC:DA:3B:5F:5B:50",
        "EC:DA:3B:5F:FD:90",
        "EC:DA:3B:5F:FD:94",
        "EC:DA:3B:5F:FD:AC",
        "EC:DA:3B:5F:FD:B4",
        "EC:DA:3B:5F:FD:C0",
        "EC:DA:3B:5F:FD:C4",
        "EC:DA:3B:5E:17:30" # unlisted
    ]

    # Set up command-line argument parsing
    parser = argparse.ArgumentParser(description='Send POST requests to hardwarelabs.io/bounties.')
    parser.add_argument('rfid', type=str, help='RFID value to include in the POST request.')
    args = parser.parse_args()

    rfid = args.rfid

    endpoint = "https://hardwarelabs.io/bounties"

    # Retrieve the Bearer Token from environment variables
    bearer_token = os.getenv('API_BEARER_TOKEN')
    if not bearer_token:
        print("Error: API_BEARER_TOKEN environment variable not set.")
        sys.exit(1)

    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {bearer_token}"
    }

    success_count = 0
    error_count = 0

    for index, mac in enumerate(mac_addresses, start=1):
        payload = {
            "mac_address": mac,
            "rfid": rfid
        }

        try:
            response = requests.post(endpoint, json=payload, headers=headers)
            if response.status_code == 201:
                success_count += 1
                print(f"SUCCESS [{index}/{len(mac_addresses)}]: MAC {mac} - Status Code: {response.status_code}")
            else:
                error_count += 1
                print(f"ERROR   [{index}/{len(mac_addresses)}]: MAC {mac} - Status Code: {response.status_code} - Response: {response.text}")
        except requests.exceptions.RequestException as e:
            error_count += 1
            print(f"EXCEPTION[{index}/{len(mac_addresses)}]: MAC {mac} - {e}")

        # Rate limiting: Wait for 5 seconds before the next request, except after the last request
        if index < len(mac_addresses):
            time.sleep(5)  # Pause for 5 seconds

    print("\n=== Summary ===")
    print(f"Total Requests: {len(mac_addresses)}")
    print(f"Successful (201): {success_count}")
    print(f"Failed: {error_count}")

if __name__ == "__main__":
    main()
```

## Badge Reader Firmware

You can download from [here](https://bucket.ics.red/reader.bin)
[https://bucket.ics.red/reader.bin](https://bucket.ics.red/reader.bin)

## Simulating the Binary

Cutter will attempt to simulate the code

![image_4.png](image_4.png)

## Extra MQTT Topic

Badge registration name is preformed using the mqtt topic `/badges/{BadgeName}`
