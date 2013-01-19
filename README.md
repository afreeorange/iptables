## Introduction

A simple IPTables-based firewall I used in a past life for my servers.

## Setup

1. Run `setup` first. 
2. Then edit `40-firewall-input`. You'll find rules for many common services in this file.
3. You can edit the correspondingly named output, forward, & nat scripts if you'd like.
4. Finally, run `firewall`. 

## Adding a group of IPs/blocks

To use a group of IPs or blocks, you will need to 

1. Define them in `10-firewall-definitions`
2. Write a small bash loop. 

For example, to open up ports 80 and 443 to a trusted groups, I'd edit `50-firewall-input` to allow IPs/blocks defined in `10-firewall-definitions`:

        echo -e "  - HTTP/S (Trusted subnets only)"
        for IP_TRUSTED in $GROUP_TRUSTED; do
            $IPTABLES -A INPUT -p tcp -s $IP_TRUSTED -m multiport --dport 80,443 -m state --state NEW -j ACCEPT
        done

## Adding Custom Rules

To add custom rules, do not edit the ones you check out from the repo. Your custom rules/definitions file should be named

 > **mn**-firewall-**foobar**

where `m` and `n` are numbers, and "foobar" is anything you want.

For example, if you needed to add a new input rule specific to that server, create a file called `55-firewall-input-custom` and then run `firewall`

This system allows you to keep your custom rules and continue to get the latest versions of the firewall from this repository.

## License

MIT. Copyright 2013 Nikhil Anand `<nikhil@mantralay.org>`
