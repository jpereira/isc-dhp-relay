isc-dhp-relay
=============

this is upstream from "ftp://ftp.isc.org/isc/dhcp/4.3.1/dhcp-4.3.1.tar.gz" with support to circuit_id/remote_id option82

1) from the original source

# mkdir -p /opt/isc-dhcp
# cd /opt/isc-dhcp
# wget https://raw.githubusercontent.com/jpereira/isc-dhp-relay/master/isc-dhcp-4.3.1_dhcrelay-custom-circuit_id-remote_id.patch 
# wget ftp://ftp.isc.org/isc/dhcp/4.3.1/dhcp-4.3.1.tar.gz
# tar xzf dhcp-4.3.1.tar.gz
# cd dhcp-4.3.1
# cat ../isc-dhcp-4.3.1_dhcrelay-custom-circuit_id-remote_id.patch | patch -p0
# ./configure --prefix=/usr
# make
# make install

2) version already applied
# git clone https://github.com/jpereira/isc-dhp-relay isc-dhcp-relay.git
# cd isc-dhcp-relay.git/dhcp-4.3.1
# ./configure --prefix=/usr
# make
# make install

3) example of relaying dhcp packets to 192.168.1.34, att: remember that the interface need to have a valid ip
# dhcrelay -h
Usage: dhcrelay [-4] [-d] [-q] [-a <circuit_id> <remote_id>] [-D]
                     [-A <length>] [-c <hops>] [-p <port>]
                     [-pf <pid-file>] [--no-pid]
                     [-m append|replace|forward|discard]
                     [-i interface0 [ ... -i interfaceN]
                     server0 [ ... serverN]

       dhcrelay -6   [-d] [-q] [-I] [-c <hops>] [-p <port>]
                     [-pf <pid-file>] [--no-pid]
                     [-s <subscriber-id>]
                     -l lower0 [ ... -l lowerN]
                     -u upper0 [ ... -u upperN]
       lower (client link): [address%]interface[#index]
       upper (server link): [address%]interface

circuit_id/remote_id interpreted sequences are:

  %%  a single %
  %i  Its name of interfaceN that received the request
  %h  interfaceN hardware address
  %H  Client hardware address
  %I  DHCP relay agent IP Address

# dhcrelay -i eth0 -a "example-circuitid;%i;%h;%I" "example-remoteid;%i;%h;%I" 192.168.1.34

