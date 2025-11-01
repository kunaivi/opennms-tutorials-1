
[Main Menu](../README.md) | [Session 5](../session5/README.md) | [Additional Notes](../session5/AdditionalNotes1.md)

# Additional Notes

## Requested topics from previous sessions

### Question: How can we change the timezone formatting in the web user interface? The default is not very user friendly.
Answer: Set the `org.opennms.ui.datettimeformat` property

Link to Formatting instructions: [DataTimeFormatter](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)
```
opennms@e45dfa7efd9c:~$ cat etc/opennms.properties.d/timeformat.properties
org.opennms.ui.datettimeformat=yyyy-MM-dd HH:mm:ss (z)
```

### Controlling data collections

- [ ] Limit nodes from SNMP data collection based on a surveillance category `Collect-Enabled`

File: `etc/collectd-configuration.xml`
```xml
<package name="example1" remote="false">
  <filter>categoryName == 'Collect-Enabled'</filter>
  <include-range begin="1.1.1.1" end="254.254.254.254"/>
  <include-range begin="::1" end="ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff"/>
  ...
```

- [ ] Assign surveillance category `Collect-Enabled` to `linux-01 and verify the collection packages in the web user interface all three linux nodes
- [ ] Change the data collection frequency and how they relate to collection packages and RRD/JRobin files
- [ ] Assign node `linux-03` to the surveillance category `Collect-10s`
- [ ] Create a new collection set `default-10s` in the datacollection-config.xml, RRDTool and JRobin limitations
- [ ] Add a second SNMP collection package with the collection interval 10 sec instead of 5 min.

File: `etc/collectd-configuration.xml`
```xml
<package name="collect-10s" remote="false">
  <filter>categoryName == 'Collect-10s'</filter>
  <include-range begin="1.1.1.1" end="254.254.254.254"/>
  <include-range begin="::1" end="ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff"/>
  <service name="SNMP" interval="10000" user-defined="false" status="on">
    <parameter key="collection" value="default-10s"/>
    <parameter key="thresholding-enabled" value="true"/>
  </service>
</package>
```

IMPORTANT: To apply the settings Collectd needs a reload when a category was changed in the inventory

- [ ] Reload Collectd configuration with `ssh admin@localhost -p 8101 reload-daemon Collectd`
- [ ] It seems like we have a bug in 32.0.5 with the filter, as soon one filter applies for a node both collection packages are applied

### Thresholding

- [ ] Create a high threshold for CPU usage higher than 50%

![cpu-user-threshold.png](images%2Fcpu-user-threshold.png)

- [ ] Install the `stress-ng` command on `linux-03` which can be used to simulate a high workload to trigger a threshold exceeded

```bash
docker compose exec -u root linux-03 apk add --no-cache stress-ng # Install the required package in the linux-03 container
docker compose exec -u root linux-03 stress-ng -c 1 -l 55 # Simulate ~55% CPU usage
```

### REST API calls for measurements

```
curl -u admin http://localhost:8980/opennms/rest/measurements/node%5B6%5D.nodeSnmp%5B%5D/CpuRawUser?start=-7200000&maxrows=30&aggregation=AVERAGE
```

REST API call examples can be used from `rest-examples` directory using the REST API tool [Bruno](https://www.usebruno.com).