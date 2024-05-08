```
root@ubuntu:/home/eve# cat /etc/bird/bird.conf
# This is a minimal configuration file, which allows the bird daemon to start
# but will not cause anything else to happen.
#
# Please refer to the documentation in the bird-doc package or BIRD User's
# Guide on http://bird.network.cz/ for more information on configuring BIRD and
# adding routing protocols.

# Change this into your BIRD router ID. It's a world-wide unique identification
# of your router, usually one of router's IPv4 addresses.
router id 195.208.24.1;

# The Device protocol is not a real routing protocol. It doesn't generate any
# routes and it only serves as a module for getting information about network
# interfaces from the kernel.
protocol device {
        scan time 10;
}

protocol direct {
        interface "eth0";
}

# The Kernel protocol is not a real routing protocol. Instead of communicating
# with other routers in the network, it performs synchronization of BIRD's
# routing tables with the OS kernel.
protocol kernel {
        persist off;
        scan time 20;
        learn;
        metric 64;      # Use explicit kernel route metric to avoid collisions
                        # with non-BIRD routes in the kernel routing table
        import all;
        export all;     # Actually insert routes into the kernel routing table
}

define ixp_as = 8985;
define ixp_prefixes = [ 195.208.24.0/21+ ];

template bgp RS_CLIENT {
        local as ixp_as;
        rs client;
}

function catch_martians_and_ixp()
prefix set martians;
prefix set ixp_prefixes;
{
  martians = [
  0.0.0.0/8+,
  10.0.0.0/8+,
  100.64.0.0/10+,
  127.0.0.0/8+,
  169.254.0.0/16+,
  172.16.0.0/12+,
  192.0.0.0/24+,
  192.0.2.0/24+,
  192.168.0.0/16+,
  198.18.0.0/15+,
  198.51.100.0/24+,
  203.0.113.0/24+,
  224.0.0.0/4+,
  240.0.0.0/4+ ];

  ixp_prefixes = [ 195.208.24.0/21+ ];

  if net ~ martians || net ~ ixp_prefixes then return true;

  return false;
}

function bgp_ixp_policy(int peer_as)
{
  if (ixp_as, ixp_as) ~ bgp_community then return true;
  if (ixp_as, peer_as) ~ bgp_community then return true;

  return false;
}

filter reject_martians_and_ixp
{
  if catch_martians_and_ixp() then reject;
  if ( net ~ [0.0.0.0/0{25,32} ] ) then {
    reject;
  }
  accept;
}

protocol bgp as_15774 from RS_CLIENT {
        neighbor 195.208.24.2 as 15774;
        export where bgp_ixp_policy(15774);
        import filter reject_martians_and_ixp;
}

protocol bgp as_31133 from RS_CLIENT {
        neighbor 195.208.24.3 as 31133;
        export where bgp_ixp_policy(31133);
        import filter reject_martians_and_ixp;
}

protocol bgp as_26810 from RS_CLIENT {
        neighbor 195.208.24.4 as 26810;
        export where bgp_ixp_policy(26810);
        import filter reject_martians_and_ixp;
}

protocol bgp as_12389 from RS_CLIENT {
        neighbor 195.208.24.5 as 12389;
        export where bgp_ixp_policy(12389);
        import filter reject_martians_and_ixp;
}

protocol bgp as_43733 from RS_CLIENT {
        neighbor 195.208.24.6 as 43733;
        export where bgp_ixp_policy(43733);
        import filter reject_martians_and_ixp;
}


```

[Назад](readme.md)