# ffs-check_mk
check_mk Skripte für das Monitoring der Freifunk Stuttgart Gateways

* Zur Verwendung einfach dieses Repo nach <code>/etc/check_mk/</code> clonen und die Datei mrpe.conf anpassen. 
* Dann dem Betreuer des Monitorservers bekanntgeben, welches Gateway inst monitoring aufgenommen werden soll

## Dependencies und Konfiguration auf dem Gateway

Auf dem Gateway müssen für debian basierte Systeme die folgenden Abhängigkeiten erfüllt sein:

* check-mk-agent
* jq
* nagios-plugins
* xinetd

Die Datei /etc/xinetd.d/check_mk muss so angepasst werden, dass Sie in etwa folgendermassen aussieht:

      service check_mk
      {
	       type           = UNLISTED
	       port           = 6556
	       socket_type    = stream
	       protocol       = tcp
	       wait           = no
	       user           = root
	       server         = /usr/bin/check_mk_agent

	       # If you use fully redundant monitoring and poll the client
	       # from more then one monitoring servers in parallel you might
	       # want to use the agent cache wrapper:
	       #server         = /usr/bin/check_mk_caching_agent

	       # configure the IP address(es) of your Nagios server here:
	       # only_from      = 127.0.0.1 

	       # Don't be too verbose. Don't log every check. This might be
	       # commented out for debugging. If this option is commented out
	       # the default options will be used for this service.
	       log_on_success =

       	   disable        = no
       }

Wenn man mag, kann man die Zeile mit only_from um die IPs der Monitoring Server ergänzen und das Kommentarzeichen erntfernen und so den Zugriff auf die Monitor-Server zu bechränken.
