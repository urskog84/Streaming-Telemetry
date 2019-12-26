# Some exampel config for cisco 1000v and Stremning telemetry

Goal to send telemetry data from a cisco ios xe device to a telegraf instans and store valuse in a infuxdb.

Tested in GNS3..
I use version - 16.12.01a
Image name - csr1000v-universalk9.16.12.01a-serial.qcow2

## YangModels

To explore all the yang model to this version
git clone https://github.com/YangModels/yang.git
cd in to Folder /vendor/cisco/xe/16121 -> last foler is related to to the version number

## pyang module

use the python module pyang to explore the modules

- pyang -f tree Cisco-IOS-XE-interfaces-oper.yang --tree-path interfaces/interface/statistics
- pyang -f tree Cisco-IOS-XE-process-cpu-opper.yang --tree-path cpu-usage/cpu-utilization/

## rest examples

You can fetch data via rest API. 
To access the Cisco-IOS-XE-interfaces-oper Mode via rest tools like Poatman or curl use the folowing parameters
in the URL add https://<host>/restconf/data/<YangModel>:<Name>

'''
curl --location --request GET 'https://192.168.1.101/restconf/data/Cisco-IOS-XE-interfaces-oper:interfaces' \
--header 'Accept: application/yang-data+json' \
--header 'Content-Type: application/yang-data+json' \
--header 'Authorization: Basic Y2lzY286Y2lzY28='
'''

## Configure the cisco device

folow the config exampel [basconfig-csr1000v.conf](./basconfig-csr1000v.conf) to enable restconf and netconf.

### Setup of telemetry strem 

in the config the sub element "filter xpath" is not using the same name standard as the exmples above
is using the "prefix" of the yang module. the prefix can be find in the *.yang modue file in the first 10 row,

see file --> [ios_telemetry.conf](./ios_telemetry.conf)

## Referens likns

- https://blogs.cisco.com/developer/getting-started-with-model-driven-telemetry
- https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2018/pdf/BRKSPG-2999.pdf
- https://wiki.geant.org/download/attachments/121342260/From%20Finger-Defined%20Networks%20to%20Streaming%20Telemetry%20-.pdf?version=1&modificationDate=1524236282776&api=v2&download=true
- https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/prog/configuration/168/b_168_programmability_cg/RESTCONF.html

## Grafana setup
- https://blog.misaunders.com/2018/homelab-dashboard-with-grafana-influxdb-telegraf/