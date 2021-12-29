# Using Duo multi factor authentication for users

We at SUDO don't reocmmend removing passwords for client certificates, those passwords still provide protection in situations where client certificate is compromised.

Duo is an advanced multi factor authentication which providers another layer of protection and verifies user identity.

## Usage

In order to enable Duo multi factor authentication the following steps are required.

* Choose a more secure [cipher](https://community.openvpn.net/openvpn/wiki/SWEET32) to use because since [OpenVPN 2.3.13](https://community.openvpn.net/openvpn/wiki/ChangesInOpenvpn23#OpenVPN2.3.13) the default openvpn cipher BF-CBC will cause a renegotiated connection every 64 MB of data

* Generate server configuration with `-3` and `-C $CIPHER` options

        docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_genconfig -u udp://vpn.example.com -3 -C $CIPHER -e "_DUO_IKEY=<IKEY>" -e "DUO_SKEY=<SKEY>" -e "DUO_HOST=<HOST>"

* Generate your client certificate, Make sure the Duo username matches the user below, or add the user below as an alias to the Duo user.

        docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn easyrsa build-client-full <user>

