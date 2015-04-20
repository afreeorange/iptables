## Introduction

A simple IPTables-based firewall I used in a past life for my servers.

## Setup

1. Run `setup` first. 
2. Edit `40-firewall-input`. Contains rules for several common services.
3. Edit the correspondingly named `output`, `forward`, & `nat` scripts.
4. Finally, run `firewall`. 

## Adding a group of IPs/blocks

To use a group of IPs or blocks, I

1. Defined them in `10-firewall-definitions`
2. Wrote a small bash loop. 

For example, to open up ports 80 and 443 to a trusted groups, I'd edit `50-firewall-input` to allow IPs/blocks defined in `10-firewall-definitions`:

    echo -e "  - HTTP/S (Trusted subnets only)"
    for IP_TRUSTED in $GROUP_TRUSTED; do
        $IPTABLES -A INPUT -p tcp -s $IP_TRUSTED -m multiport --dport 80,443 -m state --state NEW -j ACCEPT
    done

## Adding Custom Rules

To add custom rules, I'd create a file named

 > **mn**-firewall-*foobar*

where `m` and `n` are integers, and "foobar" is anything. For example, I'd create a file called `55-firewall-input-custom` and then run `firewall` to add the rules.

This system allowed me to keep my custom rules and continue to get the latest versions of the firewall from a repository.

## License

[MIT](https://raw.githubusercontent.com/afreeorange/mit-license/master/LICENSE)
