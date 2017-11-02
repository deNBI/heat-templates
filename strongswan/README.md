# Strongswan VPN endpoint

This template creates a Strongswan based VPN endpoint, allowing users to access project interna network(s) via a IPSEC tunnel.

## Site specific setup

The 'public-network' parameter has a default value of 'external_network'. This network name may need to be adopted for cloud sites.

## Using the template

The templates requires:

  * an existing project internal network to be tunneled
  * a public network to allocate a floating ip from as tunnel endpoint
  * vpn user and password (for vpn authentication, not related to openstack)
  * a flavor (small/ tiny flavors should be ok)
  * an ubuntu based image (should match flavor, tested with ubuntu cloud image)
  * ssh key for ssh based access to vpn endpoint

The templates creates a new virtual machines, associates a new floating IP to the machine, and configures Strongswan within the virtual machine.

The template's output are

  * strongswan client configuration
  * strongswan client secrets

To use the endpoint, the user needs:

  * install strongswan (e.g. `apt-get install strongswan`)
  * copy the output to the `ipsec.conf` and `ipsec.secrets` files (located in `/etc`)
  * restart the ipsec daemon (`ipsec restart`)

`ipsec statusall` should print an established VPN afterwards. Details may vary for other distributions.

## Restrictions / Improvements

  * the setup uses a pre-shared password and username defined by the user. Should this be changed to random strings to enforce *safe[tm]* credentials?
  * the ipsec tunnel setup has not been checked for security considerations
  * remote side is not restricted, maybe add a parameter for remote network (using 0.0.0.0 as default)?