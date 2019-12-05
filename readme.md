
# pypcap-monitor
A python project to sniff the internet traffic and stored it into 
Redis database. 
This project is modified based on pypcap-monitor code(https://github.com/VizIoT/pypcap-monitor)
## How To Run
```bash
cd pypcap-monitor # go to the project folder
tmux attach
./make-run.sh &
./kill &
tmux detach
```
1. Use [tmux](https://github.com/tmux/tmux) to create a session
2. Run make-run.sh script to keep the python script running
3. Run kill.sh script to kill the python script every 30 minutes.

## Python File Explanation:
1. [sniff.py](./sniff.py): Use [scapy](https://github.com/secdev/scapy) library to
sniff the data. Insert the sniffed data into a Redis database.
2. [addDevices.py](./addDevices.py): Read the device mac and name information from
a file in the router. Store the device information into the MongoDB

## Config.yml File Explanation:
make-run.sh runs the code with config.yml file
```
db_class_name: '<DB_CLASS_NAME>' # currently supports mongodb(MongodbDatabase) and redis(RedisDatabase)
db_host: '<DB_HOST>'
db_port: '<DB_PORT>'
sniff_config: # this configuration is only needed in sniffing mode
  iface: '<INTERFACE>'
  filter: '<PACKET_FILTER>'
mode: '<MODE>' # mode is one of sniffing, adding_device, delete_aggregated_data, aggregate
time_before: '<TIME_BEFORE>' # time before in seconds, this field is only required in delete_aggregated_data and aggregate mode
```

## Scapy Configuration
Ask Daniel for which iface should be listened to in the router
```python
  # sniff iface en0 of all tcp and udp packets
  sniff(iface='en0', prn=http_header, filter="tcp or udp")
  
  # sniff iface en0 of tcp port 80 and 443 packets
  sniff(iface='en0', prn=http_header, filter="tcp port (80 or 443)")
  
  # sniff iface en1 of tcp port 80 and 443 packets
  sniff(iface='eth1', prn=http_header, filter="tcp port (80 or 443)", store=0)
  
  # sniff iface eth1 of all tcp and udp packets
  sniff(iface='eth1', prn=http_header, filter="tcp or udp")
```
