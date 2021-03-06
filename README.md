# nubis-terraform-vpn

A terraform module creating a vpn connection, this works for a nubis account
and makes several assumptions about how a VPC is configured

## Usage

```bash
module "vpn" {
    source                  = "github.com/nubisproject/nubis-terraform-vpn"
    enabled                 = true
    region                  = "us-west-2"
    arenas                  = [ "core" ]
    account_name            = "account-name-here"
    technical_contact       = "name@example.com"
    vpc_id                  = "vpc-1234,vpc-234"
    vpn_bgp_asn             = "65000"
    ipsec_target            = "<datacenter public IP here>"
    private_route_table_id  = "rtb-1234"
    public_route_table_id   = "rtb-3456"

    # This is optional if you want to print config
    output_config           = true
}
```

Caveat on `vpn_bgp_asn` and `ipsec_target` you can set multiple BGP asn and
multiple IP sec targets. So when calling this module you can do this:

```bash
vpn_bgp_asn  = "65001,65002"
ipsec_target = "1.2.3.4,5.6.7.8"
```

The first BGP ASN corresponds to the first IP sec target and so on and so forth

## Inputs and Outputs

Inputs:

* `enabled`: Enable this module or not. Default is `1`
* `region`: AWS Region. Default `us-west-2`
* `arenas`: Arena name, nubis specific. Default `core`
* `account_name`: Name of the AWS account
* `technical_contact`: Technical contact. Default `infra-aws@mozilla.com`
* `vpc_id`: Self explanatory VPC ID
* `vpn_bgp_asn`: Gateway's BGP Autonomous System Number(ASN). Default `65000`
* `destination_cidr_block`: Destination cidr for route. Defaults `10.0.0.0/8`
* `ipsec_target`: IP address of the remote end
* `private_route_table_id`: Route table ID for private vpc
* `public_route_table_id`: Route table ID for the public vpc
* `output_config`: Optional option, saves config to file. Default `false`

Outputs:

* `vpn_gateway_id`: Gateway ID nothing special
* `vpn_customer_gateaway_id`: Customer gateway ID
* `vpn_tunnel1_address`: Public IP of vpn first tunnel
* `vpn_tunnel1_bgp_asn`: AS number of first tunnel
* `vpn_tunnel1_preshared_key`: Key for first tunnel
* `vpn_tunnel2_address`: Public IP of vpn second tunnel
* `vpn_tunnel2_bgp_asn`: AS number of second tunnel
* `vpn_tunnel2_preshared_key`: Key for second tunnel

