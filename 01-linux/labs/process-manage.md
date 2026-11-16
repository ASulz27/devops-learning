Short lab to test Linux processes

1. Start a dummy process via this command below.

sleep 1000 &

2. Check to see if the dummy process is actually running via this command.

jobs -l 

Result:  51178 Running                 sleep 1000 &

3. Kill the process using the PID and the kill command.

kill 51178


