# create-elb

Create Load Balancer

## Role Variables

* `elb`: Dictionary containing the information of the Elastic Load Balancer
  * `cross_az_load_balancing` : Distribute load across all configured Availability Zones (Choices: yes, no)[Default: no]
  * `idle_timeout`            : ELB connections from clients and to servers are timed out after this amount of time [Default: (null)]
  * `name`                    : The name of the ELB
  * `region`                  : The AWS region to use. If not specified then the value of the AWS_REGION or EC2_REGION environment variable, if any, is used. See http://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region [Default: (null)]
  * `zones`                   : List of availability zones to enable on this ELB
  * `subnets`                 : A list of VPC subnets to use when creating ELB. Zones should be empty if using this.
  * `scheme`                  : The scheme to use when creating the ELB. For a private VPC-visible ELB use 'internal'. [Default: internet-facing]
  * `security_group_ids`      : A list of security groups to apply to the elb [Default: None]
  * `health_check`            : An associative array of health check configuration settings [Default: None]

    * `ping_protocol`         : http # options are http, https, ssl, tcp
    * `ping_port`             : Ping port
    * `ping_path`             : Ping path 
    * `response_timeout`      : Time to wait when receiving a response from the health check (2 sec - 60 sec)
    * `interval`              : Amount of time between health checks (5 secs - 300 secs)
    * `unhealthy_threshold`   : Number of consecutive healt check failures before declaring an EC2 instance unhealthy
    * `healthy_threshold`     : Number of consecutive healt check successes before declaring an EC2 instance healthy

  * `listeners`               : List of ports/protocols for this ELB to listen on [Default: (null)]

    For example for http:
    * `- protocol`: http # options are http, https, ssl, tcp
      * `load_balancer_port`: 80
      * `instance_port`: 80
      * `proxy_protocol`: True

    For https: 
    * `- protocol`: https
      * `load_balancer_port`: 443
      * `instance_protocol`: http # optional, defaults to value of protocol setting
      * `instance_port`: 80
      * `# ssl` certificate required for https or ssl
      * `ssl_certificate_id`: "arn:aws:iam::123456789012:server-certificate/company/servercerts/ProdServerCert"

## Example playbook

```yaml
- hosts: localhost
  connection: local
  gather_facts: no
  roles:
    - create-elb
```

## License

GPLv2

## Author Information
jamatute (jamatute@paradigma)
