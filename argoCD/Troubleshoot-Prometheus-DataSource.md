# TO CONFIGURE PROMETHEUS IN GRAFANA USE THIS URL "http://host.docker.internal:9090". 
# https://stackoverflow.com/questions/74029504/spring-prometheus-grafana-err-reading-prometheus-post-http-localhost90
If using the same host for Prometheus and Grafana docker containers; In this case, setting up http://host.docker.internal:9090 as the Prometheus datasource will do the trick:

You can use the docker inspect command to find the IP address of the Prometheus container and then replace the localhost word with it.

It works for https://stackoverflow.com/a/74061034/4841138

Also, if you deploy the stack by docker compose and all dockers are in same network, you can do that:

URL: http://prometheus:9090

In above, prometheus is the domain name of the prometheus docker, which can be resolved by all dockers within same network.