# ac-pserver

ac-pserver is a Python 3 rewrite of the example C# UDP server, written by Kunos for [Assetto Corsa](http://www.assettocorsa.net/en/). Once Assetto Corsa (not the SDK or acServer) is installed on your machine, the original C# UDP example code is available at Steam/steamapps/common/assettocorsa/sdk/dev/acRemoteServerUDP_Example/acRemoteServerUDP_Example


This project is just meant as a starting point: by default it simply prints on the command line the information sent by the server, and provides send/receive method through a Pserver class. The code should be pretty self explanatory.

## acServer Configuration

acServer.exe and pserver.py will communicate with each other UDP.
Two ports are required. One is the listen port of acServer,
the other is the listen port of pserver.py. Each one sends data to
the listen port of the other.


You will need to choose two arbitrary ports.
Update the values for the following variables in server_cfg.ini:


    ; acServer LISTENS on this port
    UDP_PLUGIN_LOCAL_PORT=11000

    ; acServer SENDS TO this IP address and port
    UDP_PLUGIN_ADDRESS=127.0.0.1:10000


The ports above must be mirrored in these constants in pserver.py:

    # pserver.py SENDS TO this IP address
    UDP_IP = "127.0.0.1"

    # pserver.py LISTENS on this port
    UDP_PORT = 10000

    # pserver.py SENDS TO this port
    UDP_SEND_PORT = 11000



## Running the Code

    python3 pserver.py

In production you would probably want to use a process control system such as [Supervisor](http://supervisord.org/).


## Troubleshooting

If you are not getting data from acServer, first check that
UDP_PLUGIN_LOCAL_PORT and UDP_PLUGIN_ADDRESS are defined only
once in server_cfg.ini. (Presumably it's last write wins)

Next step is run `netstat` and verify that acServer is listening
on the port you expect. acServer will actually be listening on TWO UDP ports.
First the one that we specified above with
    UDP_PLUGIN_LOCAL_PORT=11000
And also the port specified in server_cfg.ini as "UDP_PORT", but
this second one we don't actually care about for the sake of this exercise.

If acServer is still not listening on the port you expect, revert to
the default server_cfg.ini, add values for the above variables,
and get that working first.

You can also use `netstat` to verify that pserver.py is listening on the port you
expect.

