# POST /cloud/virtualdatacenters/{vdc}/virtualappliances/{vapp}/virtualmachines/{vm}/collectd

## cURL

```bash
curl -X POST http://localhost:9000/api/cloud/virtualdatacenters/541/virtualappliances/322/virtualmachines/369/collectd \
     -H 'Accept:text/json,application/json' \
     -H 'Content-Type:application/json' \
     -d @requestpayload.json \
     -u user:password --verbose
```

**Success status code**: `204`

## Request payload

```json
[
  {
    "datasourceNames": [
      "933cc3bb-ce9b-4529-8854-c2e0b3db8f15",
      "e62ded09-cb81-4f84-b952-0e1670caafb4"
    ],
    "datasourcetypes": [
      "ABSOLUTE",
      "GAUGE"
    ],
    "dsnames": [
      "933cc3bb-ce9b-4529-8854-c2e0b3db8f15",
      "e62ded09-cb81-4f84-b952-0e1670caafb4"
    ],
    "host": "0d5be059-3489-40b2-8ca9-2eb06ac380ee",
    "long_time": 1479997080,
    "plugin_instance": "4877672f-1e7d-4aba-9fea-e27f04edb0ba",
    "pluginInstance": "4877672f-1e7d-4aba-9fea-e27f04edb0ba",
    "dstypes": [
      "ABSOLUTE",
      "GAUGE"
    ],
    "plugin": "454dbbf3-1b8d-48e6-873b-013186aab177",
    "interval": 10,
    "typeInstance": "e105b17d-c6ba-4ba3-a767-79fc7de1abc9",
    "length": 2,
    "long_interval": 10,
    "values": [
      "10373115",
      "12863138"
    ],
    "time": 1479997080,
    "type_instance": "e105b17d-c6ba-4ba3-a767-79fc7de1abc9",
    "type": "5c707508-a1f2-4318-aa35-3f91b74378c3"
  },
  {
    "datasourceNames": [
      "71d83476-7906-47d9-a053-f7cf2080f809"
    ],
    "datasourcetypes": [
      "ABSOLUTE"
    ],
    "dsnames": [
      "71d83476-7906-47d9-a053-f7cf2080f809"
    ],
    "host": "78c620c2-8b48-405c-b2b0-39d22527b9ef",
    "long_time": 1479997080,
    "plugin_instance": "c8d66a06-bf36-4f8d-ae4d-3c1dc9868662",
    "pluginInstance": "c8d66a06-bf36-4f8d-ae4d-3c1dc9868662",
    "dstypes": [
      "ABSOLUTE"
    ],
    "plugin": "e4d3ba5b-e322-4e18-8856-29f3db6240f5",
    "interval": 10,
    "typeInstance": "6d90f553-100a-4137-816b-695663cbc621",
    "length": 1,
    "long_interval": 10,
    "values": [
      "1857444"
    ],
    "time": 1479997080,
    "type_instance": "6d90f553-100a-4137-816b-695663cbc621",
    "type": "e9553cea-9930-40aa-a89d-96d937ea50e0"
  },
  {
    "datasourceNames": [
      "d1d67266-b3e8-489b-bd8d-9cfc2bc22322"
    ],
    "datasourcetypes": [
      "ABSOLUTE"
    ],
    "dsnames": [
      "d1d67266-b3e8-489b-bd8d-9cfc2bc22322"
    ],
    "host": "dafdcd40-1ad9-4fa1-ab89-6821ec3dc7b5",
    "long_time": 1479997080,
    "plugin_instance": "9c63a050-c6f2-461f-8380-a6ec96c476b7",
    "pluginInstance": "9c63a050-c6f2-461f-8380-a6ec96c476b7",
    "dstypes": [
      "ABSOLUTE"
    ],
    "plugin": "946f66fb-c230-4ded-a2ac-4f130520d5c8",
    "interval": 10,
    "typeInstance": "befc79ea-49ba-4bf1-bff5-fe7f04bacb65",
    "length": 1,
    "long_interval": 10,
    "values": [
      "5177812"
    ],
    "time": 1479997080,
    "type_instance": "befc79ea-49ba-4bf1-bff5-fe7f04bacb65",
    "type": "9ad979ed-3cc1-4339-8e76-59f6c1adf073"
  },
  {
    "datasourceNames": [
      "f28d1829-d3de-4f50-b604-33b79422645c"
    ],
    "datasourcetypes": [
      "COUNTER"
    ],
    "dsnames": [
      "f28d1829-d3de-4f50-b604-33b79422645c"
    ],
    "host": "0d835519-bdc6-4b52-8ccd-0970f2ac252b",
    "long_time": 1479997080,
    "plugin_instance": "0b84221f-f282-4662-9f3c-198d9facbb51",
    "pluginInstance": "0b84221f-f282-4662-9f3c-198d9facbb51",
    "dstypes": [
      "COUNTER"
    ],
    "plugin": "bcd3327c-2c00-4651-9ad7-51caccadd6f7",
    "interval": 10,
    "typeInstance": "1882cdaf-d99d-4494-b7a4-527d0e6e3c98",
    "length": 1,
    "long_interval": 10,
    "values": [
      "3023252"
    ],
    "time": 1479997080,
    "type_instance": "1882cdaf-d99d-4494-b7a4-527d0e6e3c98",
    "type": "5f25fa66-8484-4e98-8169-edf2d6cf22f9"
  },
  {
    "datasourceNames": [
      "e786a610-7c90-451a-9955-078e90071969"
    ],
    "datasourcetypes": [
      "DERIVE"
    ],
    "dsnames": [
      "e786a610-7c90-451a-9955-078e90071969"
    ],
    "host": "7af8662b-33a3-4cb0-afed-f68340d43b62",
    "long_time": 1479997080,
    "plugin_instance": "4b3e9685-7e7d-4f6f-8256-6a22f8e3308c",
    "pluginInstance": "4b3e9685-7e7d-4f6f-8256-6a22f8e3308c",
    "dstypes": [
      "DERIVE"
    ],
    "plugin": "1023cd92-7f4b-4462-ad8a-305312aa9e94",
    "interval": 10,
    "typeInstance": "803f1271-7cac-426f-9951-33379c49bba8",
    "length": 1,
    "long_interval": 10,
    "values": [
      "9775473"
    ],
    "time": 1479997080,
    "type_instance": "803f1271-7cac-426f-9951-33379c49bba8",
    "type": "30529502-f812-4386-8d7b-9076ccf72d5b"
  }
]
```

## Response payload

*(none)*