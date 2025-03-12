


tcpdump

Desde la DB
SHOW PROCESSLIST;


Local Port Forwarding

    ssh -i gcp_remote -L 3306:localhost:3306 diegoposada@34.27.180.215

    ssh -i gcp_remote -N -L 3306:localhost:3306 diegoposada@34.27.180.215

Remote Port Forwarding

    ssh -i gcp_remote -R 3306:localhost:3306 diegoposada@34.27.180.215

    ssh -i gcp_remote -f -N -R 3306:localhost:3306 diegoposada@34.27.180.215  (background execution)



    docker exec -it mysql-server mysql -h 127.0.0.1 -P 3306 -u root -p


**LOCAL PORT FORWARDING**


ssh -i gcp_remote -L 3306:localhost:3306 diegoposada@34.27.180.215


Si esta ocupado se puede cambiar el puerto local.



**REMOTE PORT FORWARDING**


