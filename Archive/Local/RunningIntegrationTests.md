# Running Integration Tests on Blackbox
Locally, we run integration tests on the ```blackbox.poly.edu``` server. These tests are intended to test large portions of the Seattle system end-to-end.



## Configuration

 * Seattle blackbox tests live on blackbox.poly.edu in /home/integrationtester/cron_tests/
 * The tests run as the integrationtester user on a cron schedule scheduled by the user cron of the integrationtester user
 * The test logs are written to /home/integrationtester/cron_logs/

----
## centralizedputget Test

### Purpose
Attempt to put a (k,v) into our centralized hash table and then get it back. On error send an email to some folks.

### Deployment
To deploy centralizedputget, run the deploy_centralizedputget.py located in trunk/integrationtests/deployment_scripts/. Run the script with the option -c in order to set up the crontab on the machine as well to run centralizedputget once an hour.

In order to set everything up, create a folder in the same directory that the folder trunk/ is located <folder_1>. Create a second folder <target_folder> where you want to deploy the centralizedputget script. Run preparetest.py on <folder_1>. Then copy over the file deploy_centralizedputget.py and setup_crontab.py from the deploy_scripts/ folder to <folder_1>. Copy over preparetest.py from trunk/ to <folder_1> Then run the deploy_centralizedputget script using the following line:
```
python deploy_centralizedputget.py -c <target_folder> <target_log_folder>
```
<target_log_folder> is the folder where you want the logs from the centralizedputget script to be stored.

### Schedule
This script runs once an hour. The current crontab line on blackbox looks like:
```
0 * * * * /usr/bin/python /home/integrationtester/deploy_test/centralizedputget.py > /home/integrationtester/deploy_test/cron_log.centralizedputget
```

----
## clearinghouse testlookupnodestates Test

### Purpose
Attempts to acquire, renew and release VMs from the Clearinghouse.

### Deployment
To deploy the testlookupnodestates test, create a folder where you want to deploy the test script. Run preparetest.py to that folder. Then copy over the file testlookupnodestates.py to that folder.

### Schedule
This script runs once an hour. The current crontab line on blackbox looks like:
```
0 * * * * cd /home/integrationtester/cron_tests/test_vessel_acquire_release/ && /usr/bin/python /home/integrationtester/cron_tests/test_vessel_acquire_release/test_vessel_acquire_renew_release.py >> /home/integrationtester/cron_logs/cron_log.test_vessel_acquire_renew_release 2>&1
```
----
## downloadandinstallseattle Test

### Purpose
Download, install and check the resulting Seattle installation for correct state (onepercent state).

### Deployment
To deploy downloadandinstallseattle, run the deploy_downloadandinstallseattle.py located in trunk/integrationtests/deployment_scripts/. Run the script with the option -c in order to set up the crontab on the machine as well to run downloadandinstallseattle once a day. 

In order to set everything up, create a folder in the same directory that the folder trunk/ is located <folder_1>. Create a second folder <target_folder> where you want to deploy the downloadandinstallseattle script. Run preparetest.py on <folder_1>. Then copy over the file deploy_downloadandinstallseattle.py and setup_crontab.py from the deploy_scripts/ folder to <folder_1>. Copy over preparetest.py from trunk/ to <folder_1> Then run the deploy_downloadandinstallseattle script using the following line:
```
python deploy_downloadandinstallseattle.py -c <target_folder> <target_log_folder>
```
<target_log_folder> is the folder where you want the logs from the downloadandinstallseattle script to be stored.

### Schedule
This script runs once a day. The current crontab line on blackbox looks like:
```
15 16 * * * /usr/bin/python /home/integrationtester/deploy_test/downloadandinstallseattle.py > /home/integrationtester/deploy_test/cron_log.downloadandinstallseattle
```

----
## geoip_lookup Test

### Purpose
Verify that the GeoIP servers are running and functional.

### Deployment
To deploy this test, run preparetest.py from trunk to an empty directory. Then, copy trunk/integrationtests/common/* and trunk/integrationtests/geoip_lookup/geoip_lookup.py to the above directory.

### Schedule
This script runs every 15 minutes. The current crontab line on blackbox looks like:
```
*/15 * * * * /usr/bin/python /home/integrationtester/cron_tests/geoip_lookup/geoip_lookup.py >> /home/integrationtester/cron_logs/cron_log.geoip_lookup 2>&1
```

----
## ping_machines Test

### Purpose
Verify that our production and testing machines are running and contactable.

### Deployment
To deploy this test, copy trunk/integrationtests/common/* and trunk/integrationtests/ping_machines/ping_machines.py to an empty directory.

### Schedule
This script runs once every hour. The current crontab line on blackbox looks like:
```
25 * * * * /usr/bin/python /home/integrationtester/cron_tests/ping_machines/ping_machines.py >> /home/integrationtester/cron_logs/cron_log.ping_machines 2>&1
```

----
## monitor_disk Test

### Purpose
Checks the current machine to make sure that it is not running out of disk space. This script should be run on each machine.

### Deployment
To deploy this test, run preparetest.py from trunk to an empty directory. Then, copy trunk/integrationtests/ping_machines/ping_machines.py to the above directory.

### Schedule
This script runs once every 3 hours. The current crontab line on blackbox and the Clearinghouse looks like:
```
0 */3 * * * /usr/bin/python /home/integrationtester/cron_tests/monitor_disk/monitor_disk.py >> /home/integrationtester/cron_logs/cron_log.monitor_disk
```

----
## monitor_process Test

### Purpose
Ensures that critical system processes are running.  This test should be run on both seattle.poly.edu and seattleclearinghouse.poly.edu.

### Deployment
Copy over the files in ~/trunk/integrationtests/common/* and ~/trunk/integrationtests/monitor_disk/* to a new directory.

### Running
This script is run once every 15 minutes.  Note the -seattleclearinghouse argument.
```
*/15 * * * * /usr/bin/python /home/integrationtester/cron_tests/monitor_script/monitor_processes.py -seattleclearinghouse >> /home/integrationtester/cron_logs/cron_log.monitor_processes 2>&1
```
----
## test_nat_servers_running

### Purpose

Checks that our nat forwarders are alive.  Sends out an e-mail if they aren't.

### Deployment

To deploy this test, first run [source:seattle/branches/repy_v2/preparetest.py] **(note: repyV2 branch)** to an empty directory. Then, copy trunk/integrationtests/common/* and trunk/integrationtests/nat/test_nat_servers_running.py to the same directory.  To run, simply run python test_nat_servers_running.py from that directory.

### Schedule

This script runs once every hour. The current crontab line on blackbox looks like:

```
# NAT forwarders monitoring test (Hourly)
50 * * * * export GMAIL_USER='seattle.devel@gmail.com' && export GMAIL_PWD='7S1qzQvnY0aQ' && /usr/bin/python /home/integrationtester/cron_tests/nat_tests/test_nat_servers_running.py >> /home/integrationtester/cron_logs/cron_log.nat_servers_running 2>&1
```


----
## custom_installer build_installers Test

### Purpose
Verifies that installers can be successfully built.

### Deployment
To deploy this test, copy trunk/integrationtests/common/* and trunk/integrationtests/custom_installer_tests/test_build_installers.py to an empty directory.

### Schedule
This script runs once every day. The current crontab line on blackbox looks like:
```
2 15 * * * /usr/bin/python /home/integrationtester/cron_tests/custom_installer_tester/test_build_installers.py >> /home/integrationtester/cron_logs/cron_log.test_build_installers 2>&1
```

----
## custom_installer test_invalid_input Test

### Purpose
Tests that the custom installer builder reports failures when given invalid requests.

### Deployment
To deploy this test, copy trunk/integrationtests/common/* and trunk/integrationtests/custom_installer_tests/test_build_installers.py to an empty directory.

### Schedule
This script runs once every day. The current crontab line on blackbox looks like:
```
2 16 * * * /usr/bin/python /home/integrationtester/cron_tests/custom_installer_tester/test_invalid_input.py >> /home/integrationtester/cron_logs/cron_log.test_invalid_input 2>&1
```

----
## timeserver time_servers_running Test

### Purpose
Send out emails if fewer than 8 timeservers are running.

### Deployment
To deploy this test, copy trunk/integrationtests/common/* and trunk/integrationtests/time/* to an empty directory.

### Schedule
This script runs once every day. The current crontab line on blackbox looks like:
```
2 16 * * * /usr/bin/python /home/integrationtester/cron_tests/custom_installer_tester/test_invalid_input.py >> /home/integrationtester/cron_logs/cron_log.test_invalid_input 2>&1
```

----
## timeserver tcp_time Test

### Purpose
Verifies that the time servers are returning correct values.

### Deployment
To deploy this test, copy trunk/integrationtests/common/* and trunk/integrationtests/time/* to an empty directory.  **You can skip this step if you have deployed the time_servers_running test already, as you can run both tests from the same directory.**

### Schedule
This script runs once every hour. The current crontab line on blackbox looks like:
```
40 * * * * /usr/bin/python /home/integrationtester/cron_tests/timeserver_tests/test_time_tcp.py >> /home/integrationtester/cron_logs/cron_log.time_tcp 2>&1
```

----
## lookup_node_states Test

### Purpose
Checks that our nodecounts in each node state are reasonable.  Sends out an e-mail if they aren't.

### Deployment
To deploy this test, first run preparetest.py to an empty directory. Then, copy trunk/integrationtests/common/* and trunk/integrationtests/lookup_node_states/test_lookup_node_states.py to the same directory. Copy the state keys from trunk/seattlegeni/node_state_transitions/statekeys (trunk/seattlegeni/node_state_transitions/beta_statekeys for the beta clearinghouse) to the same directory. 

### Schedule
This script runs once every hour. The current crontab line on blackbox looks like:
```

54 * * * * /usr/bin/python /home/integrationtester/cron_tests/lookup_node_states/test_lookup_node_states.py >> /home/integrationtester/cron_logs/cron_log.lookup_node_states 2>&1

```


----
## zenodotus_alive Test

### Purpose
Checks that DNS records for zenodotus.poly.edu refer to blackbox.

### Deployment
To deploy this test, copy trunk/integrationtests/common/* and trunk/integrationtests/zenodotus/zenodotus_alive.py to a new directory.

### Schedule
This script runs once every 6 hours. The current crontab line on blackbox looks like:
```
# Zenodotus DNS monitoring test (Once every 6 hours)
0 */6 * * * /usr/bin/python /home/integrationtester/cron_tests/zenodotus/zenodotus_alive.py

```


----
## Deprecated Tests
These tests verify functionality that have been deprecated, and are no longer in use.

### opendhtputget Test

#### Purpose
Attempt to put a (k,v) into OpenDHT and then get it back. On error send an email to some folks.

#### Deployment
To deploy opendhtputget, run the deploy_opendhtputget.py located in trunk/integrationtests/deployment_scripts/. Run the script with the option -c in order to set up the crontab on the machine as well to run opendhtputget once an hour. 

In order to set everything up, create a folder in the same directory that the folder trunk/ is located <folder_1>. Create a second folder <target_folder> where you want to deploy the opendhtputget script. Run preparetest.py on <folder_1>. Then copy over the file deploy_opendhtputget.py and setup_crontab.py from the deploy_scripts/ folder to <folder_1>. Copy over preparetest.py from trunk/ to <folder_1> Then run the deploy_centralizedputget script using the following line:
```
python deploy_opendhtputget.py -c <target_folder> <target_log_folder>
```
<target_log_folder> is the folder where you want the logs from the opendhtputget script to be stored.

#### Schedule
This script runs once an hour. The current crontab line on blackbox looks like:
```
30 * * * * /usr/bin/python /home/integrationtester/deploy_test/opendhtputget.py > /home/integrationtester/deploy_test/cron_log.opendhtputget
```