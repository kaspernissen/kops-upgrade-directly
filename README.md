# Update directly from Kops 1.5.1 and Kubernetes 1.5.2 to Kops 1.6.2 and Kubernetes 1.6.2

## Setup Kops

Link: https://github.com/kubernetes/kops/releases/tag/1.5.1

## Setup initial cluster

```
kops create cluster \
  --name $KOPS_NAME \
  --state $KOPS_STATE_STORE \
  --node-count 3 \
  --zones eu-west-1a,eu-west-1b,eu-west-1c \
  --master-zones eu-west-1a,eu-west-1b,eu-west-1c \
  --dns-zone=${PRIVATE_HOSTED_ZONE_ID} \
  --dns private \
  --node-size t2.medium \
  --master-size t2.small \
  --topology private \
  --networking weave \
  --image kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09 \
  --kubernetes-version=1.5.2 \
  --bastion
  ```

Output:
```
$ kops create cluster \
  --name $KOPS_NAME \
  --state $KOPS_STATE_STORE \
  --node-count 3 \
  --zones eu-west-1a,eu-west-1b,eu-west-1c \
  --master-zones eu-west-1a,eu-west-1b,eu-west-1c \
  --dns-zone=${PRIVATE_HOSTED_ZONE_ID} \
  --dns private \
  --node-size t2.medium \
  --master-size t2.small \
  --topology private \
  --networking weave \
  --image kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09 \
  --kubernetes-version=1.5.2 \
  --bastion
I0629 13:45:51.876326   13858 create_cluster.go:475] Inferred --cloud=aws from zone "eu-west-1a"
I0629 13:45:51.876450   13858 create_cluster.go:610] Using SSH public key: /Users/kaspernissen/.ssh/id_rsa.pub
I0629 13:45:52.602767   13858 subnets.go:183] Assigned CIDR 172.20.32.0/19 to subnet eu-west-1a
I0629 13:45:52.602812   13858 subnets.go:183] Assigned CIDR 172.20.64.0/19 to subnet eu-west-1b
I0629 13:45:52.602828   13858 subnets.go:183] Assigned CIDR 172.20.96.0/19 to subnet eu-west-1c
I0629 13:45:52.602846   13858 subnets.go:197] Assigned CIDR 172.20.0.0/22 to subnet utility-eu-west-1a
I0629 13:45:52.602864   13858 subnets.go:197] Assigned CIDR 172.20.4.0/22 to subnet utility-eu-west-1b
I0629 13:45:52.602887   13858 subnets.go:197] Assigned CIDR 172.20.8.0/22 to subnet utility-eu-west-1c
Previewing changes that will be made:


*********************************************************************************

A new kubernetes version is available: 1.5.7
Upgrading is recommended (try kops upgrade cluster)

More information: https://github.com/kubernetes/kops/blob/master/permalinks/upgrade_k8s.md#1.5.7

*********************************************************************************

I0629 13:45:54.661776   13858 dns.go:89] Private DNS: skipping DNS validation
I0629 13:45:54.668831   13858 executor.go:91] Tasks: 0 done / 114 total; 34 can run
I0629 13:45:55.165623   13858 executor.go:91] Tasks: 34 done / 114 total; 27 can run
I0629 13:45:55.865404   13858 executor.go:91] Tasks: 61 done / 114 total; 36 can run
I0629 13:45:56.268847   13858 executor.go:91] Tasks: 97 done / 114 total; 10 can run
I0629 13:45:56.551077   13858 executor.go:91] Tasks: 107 done / 114 total; 7 can run
I0629 13:45:56.703199   13858 executor.go:91] Tasks: 114 done / 114 total; 0 can run
Will create resources:
  AutoscalingGroup/bastions.k8s.test.phennex.com
  	MinSize             	1
  	MaxSize             	1
  	Subnets             	[name:utility-eu-west-1a.k8s.test.phennex.com, name:utility-eu-west-1b.k8s.test.phennex.com, name:utility-eu-west-1c.k8s.test.phennex.com]
  	Tags                	{KubernetesCluster: k8s.test.phennex.com, k8s.io/role/bastion: 1, Name: bastions.k8s.test.phennex.com}
  	LaunchConfiguration 	name:bastions.k8s.test.phennex.com

  AutoscalingGroup/master-eu-west-1a.masters.k8s.test.phennex.com
  	MinSize             	1
  	MaxSize             	1
  	Subnets             	[name:eu-west-1a.k8s.test.phennex.com]
  	Tags                	{k8s.io/role/master: 1, Name: master-eu-west-1a.masters.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com}
  	LaunchConfiguration 	name:master-eu-west-1a.masters.k8s.test.phennex.com

  AutoscalingGroup/master-eu-west-1b.masters.k8s.test.phennex.com
  	MinSize             	1
  	MaxSize             	1
  	Subnets             	[name:eu-west-1b.k8s.test.phennex.com]
  	Tags                	{k8s.io/role/master: 1, Name: master-eu-west-1b.masters.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com}
  	LaunchConfiguration 	name:master-eu-west-1b.masters.k8s.test.phennex.com

  AutoscalingGroup/master-eu-west-1c.masters.k8s.test.phennex.com
  	MinSize             	1
  	MaxSize             	1
  	Subnets             	[name:eu-west-1c.k8s.test.phennex.com]
  	Tags                	{k8s.io/role/master: 1, Name: master-eu-west-1c.masters.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com}
  	LaunchConfiguration 	name:master-eu-west-1c.masters.k8s.test.phennex.com

  AutoscalingGroup/nodes.k8s.test.phennex.com
  	MinSize             	3
  	MaxSize             	3
  	Subnets             	[name:eu-west-1a.k8s.test.phennex.com, name:eu-west-1b.k8s.test.phennex.com, name:eu-west-1c.k8s.test.phennex.com]
  	Tags                	{k8s.io/role/node: 1, Name: nodes.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com}
  	LaunchConfiguration 	name:nodes.k8s.test.phennex.com

  DHCPOptions/k8s.test.phennex.com
  	DomainName          	eu-west-1.compute.internal
  	DomainNameServers   	AmazonProvidedDNS

  DNSName/api.k8s.test.phennex.com
  	Zone                	name:ZF3HEQO28R8JR id:ZF3HEQO28R8JR
  	ResourceType        	A
  	TargetLoadBalancer  	name:api.k8s.test.phennex.com id:api-k8s

  DNSName/bastion.k8s.test.phennex.com
  	Zone                	name:ZF3HEQO28R8JR id:ZF3HEQO28R8JR
  	ResourceType        	A
  	TargetLoadBalancer  	name:bastion.k8s.test.phennex.com id:bastion-k8s

  EBSVolume/a.etcd-events.k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1a
  	VolumeType          	gp2
  	SizeGB              	20
  	Encrypted           	false
  	Tags                	{KubernetesCluster: k8s.test.phennex.com, k8s.io/etcd/events: a/a,b,c, k8s.io/role/master: 1, Name: a.etcd-events.k8s.test.phennex.com}

  EBSVolume/a.etcd-main.k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1a
  	VolumeType          	gp2
  	SizeGB              	20
  	Encrypted           	false
  	Tags                	{Name: a.etcd-main.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com, k8s.io/etcd/main: a/a,b,c, k8s.io/role/master: 1}

  EBSVolume/b.etcd-events.k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1b
  	VolumeType          	gp2
  	SizeGB              	20
  	Encrypted           	false
  	Tags                	{k8s.io/role/master: 1, Name: b.etcd-events.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com, k8s.io/etcd/events: b/a,b,c}

  EBSVolume/b.etcd-main.k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1b
  	VolumeType          	gp2
  	SizeGB              	20
  	Encrypted           	false
  	Tags                	{k8s.io/etcd/main: b/a,b,c, k8s.io/role/master: 1, Name: b.etcd-main.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com}

  EBSVolume/c.etcd-events.k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1c
  	VolumeType          	gp2
  	SizeGB              	20
  	Encrypted           	false
  	Tags                	{k8s.io/etcd/events: c/a,b,c, k8s.io/role/master: 1, Name: c.etcd-events.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com}

  EBSVolume/c.etcd-main.k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1c
  	VolumeType          	gp2
  	SizeGB              	20
  	Encrypted           	false
  	Tags                	{k8s.io/role/master: 1, Name: c.etcd-main.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com, k8s.io/etcd/main: c/a,b,c}

  ElasticIP/eu-west-1a.k8s.test.phennex.com
  	AssociatedNatGatewayRouteTable	name:private-eu-west-1a.k8s.test.phennex.com

  ElasticIP/eu-west-1b.k8s.test.phennex.com
  	AssociatedNatGatewayRouteTable	name:private-eu-west-1b.k8s.test.phennex.com

  ElasticIP/eu-west-1c.k8s.test.phennex.com
  	AssociatedNatGatewayRouteTable	name:private-eu-west-1c.k8s.test.phennex.com

  IAMInstanceProfile/bastions.k8s.test.phennex.com

  IAMInstanceProfile/masters.k8s.test.phennex.com

  IAMInstanceProfile/nodes.k8s.test.phennex.com

  IAMInstanceProfileRole/bastions.k8s.test.phennex.com
  	InstanceProfile     	name:bastions.k8s.test.phennex.com id:bastions.k8s.test.phennex.com
  	Role                	name:bastions.k8s.test.phennex.com

  IAMInstanceProfileRole/masters.k8s.test.phennex.com
  	InstanceProfile     	name:masters.k8s.test.phennex.com id:masters.k8s.test.phennex.com
  	Role                	name:masters.k8s.test.phennex.com

  IAMInstanceProfileRole/nodes.k8s.test.phennex.com
  	InstanceProfile     	name:nodes.k8s.test.phennex.com id:nodes.k8s.test.phennex.com
  	Role                	name:nodes.k8s.test.phennex.com

  IAMRole/bastions.k8s.test.phennex.com

  IAMRole/masters.k8s.test.phennex.com

  IAMRole/nodes.k8s.test.phennex.com

  IAMRolePolicy/additional.bastions.k8s.test.phennex.com
  	Role                	name:bastions.k8s.test.phennex.com

  IAMRolePolicy/additional.masters.k8s.test.phennex.com
  	Role                	name:masters.k8s.test.phennex.com

  IAMRolePolicy/additional.nodes.k8s.test.phennex.com
  	Role                	name:nodes.k8s.test.phennex.com

  IAMRolePolicy/bastions.k8s.test.phennex.com
  	Role                	name:bastions.k8s.test.phennex.com

  IAMRolePolicy/masters.k8s.test.phennex.com
  	Role                	name:masters.k8s.test.phennex.com

  IAMRolePolicy/nodes.k8s.test.phennex.com
  	Role                	name:nodes.k8s.test.phennex.com

  InternetGateway/k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com
  	Shared              	false

  Keypair/kubecfg
  	Subject             	cn=kubecfg
  	Type                	client

  Keypair/kubelet
  	Subject             	cn=kubelet
  	Type                	client

  Keypair/master
  	Subject             	cn=kubernetes-master
  	Type                	server
  	AlternateNames      	[100.64.0.1, api.internal.k8s.test.phennex.com, api.k8s.test.phennex.com, kubernetes, kubernetes.default, kubernetes.default.svc, kubernetes.default.svc.cluster.local]

  LaunchConfiguration/bastions.k8s.test.phennex.com
  	ImageID             	kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-05-02
  	InstanceType        	t2.micro
  	SSHKey              	name:kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f id:kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f
  	SecurityGroups      	[name:bastion.k8s.test.phennex.com]
  	AssociatePublicIP   	true
  	IAMInstanceProfile  	name:bastions.k8s.test.phennex.com id:bastions.k8s.test.phennex.com
  	RootVolumeSize      	20
  	RootVolumeType      	gp2
  	SpotPrice

  LaunchConfiguration/master-eu-west-1a.masters.k8s.test.phennex.com
  	ImageID             	kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09
  	InstanceType        	t2.small
  	SSHKey              	name:kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f id:kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f
  	SecurityGroups      	[name:masters.k8s.test.phennex.com]
  	AssociatePublicIP   	false
  	IAMInstanceProfile  	name:masters.k8s.test.phennex.com id:masters.k8s.test.phennex.com
  	RootVolumeSize      	20
  	RootVolumeType      	gp2
  	SpotPrice

  LaunchConfiguration/master-eu-west-1b.masters.k8s.test.phennex.com
  	ImageID             	kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09
  	InstanceType        	t2.small
  	SSHKey              	name:kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f id:kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f
  	SecurityGroups      	[name:masters.k8s.test.phennex.com]
  	AssociatePublicIP   	false
  	IAMInstanceProfile  	name:masters.k8s.test.phennex.com id:masters.k8s.test.phennex.com
  	RootVolumeSize      	20
  	RootVolumeType      	gp2
  	SpotPrice

  LaunchConfiguration/master-eu-west-1c.masters.k8s.test.phennex.com
  	ImageID             	kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09
  	InstanceType        	t2.small
  	SSHKey              	name:kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f id:kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f
  	SecurityGroups      	[name:masters.k8s.test.phennex.com]
  	AssociatePublicIP   	false
  	IAMInstanceProfile  	name:masters.k8s.test.phennex.com id:masters.k8s.test.phennex.com
  	RootVolumeSize      	20
  	RootVolumeType      	gp2
  	SpotPrice

  LaunchConfiguration/nodes.k8s.test.phennex.com
  	ImageID             	kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09
  	InstanceType        	t2.medium
  	SSHKey              	name:kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f id:kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f
  	SecurityGroups      	[name:nodes.k8s.test.phennex.com]
  	AssociatePublicIP   	false
  	IAMInstanceProfile  	name:nodes.k8s.test.phennex.com id:nodes.k8s.test.phennex.com
  	RootVolumeSize      	20
  	RootVolumeType      	gp2
  	SpotPrice

  LoadBalancer/api.k8s.test.phennex.com
  	ID                  	api-k8s
  	Subnets             	[name:utility-eu-west-1a.k8s.test.phennex.com, name:utility-eu-west-1b.k8s.test.phennex.com, name:utility-eu-west-1c.k8s.test.phennex.com]
  	SecurityGroups      	[name:api-elb.k8s.test.phennex.com]
  	Listeners           	{443: {"InstancePort":443}}
  	HealthCheck         	{"Target":"TCP:443","HealthyThreshold":2,"UnhealthyThreshold":2,"Interval":10,"Timeout":5}

  LoadBalancer/bastion.k8s.test.phennex.com
  	ID                  	bastion-k8s
  	Subnets             	[name:utility-eu-west-1c.k8s.test.phennex.com, name:utility-eu-west-1a.k8s.test.phennex.com, name:utility-eu-west-1b.k8s.test.phennex.com]
  	SecurityGroups      	[name:bastion-elb.k8s.test.phennex.com]
  	Listeners           	{22: {"InstancePort":22}}
  	HealthCheck         	{"Target":"TCP:22","HealthyThreshold":2,"UnhealthyThreshold":2,"Interval":10,"Timeout":5}
  	ConnectionSettings  	{"IdleTimeout":300}

  LoadBalancerAttachment/api-master-eu-west-1a
  	LoadBalancer        	name:api.k8s.test.phennex.com id:api-k8s
  	AutoscalingGroup    	name:master-eu-west-1a.masters.k8s.test.phennex.com id:master-eu-west-1a.masters.k8s.test.phennex.com

  LoadBalancerAttachment/api-master-eu-west-1b
  	LoadBalancer        	name:api.k8s.test.phennex.com id:api-k8s
  	AutoscalingGroup    	name:master-eu-west-1b.masters.k8s.test.phennex.com id:master-eu-west-1b.masters.k8s.test.phennex.com

  LoadBalancerAttachment/api-master-eu-west-1c
  	LoadBalancer        	name:api.k8s.test.phennex.com id:api-k8s
  	AutoscalingGroup    	name:master-eu-west-1c.masters.k8s.test.phennex.com id:master-eu-west-1c.masters.k8s.test.phennex.com

  LoadBalancerAttachment/bastion-elb-attachment
  	LoadBalancer        	name:bastion.k8s.test.phennex.com id:bastion-k8s
  	AutoscalingGroup    	name:bastions.k8s.test.phennex.com id:bastions.k8s.test.phennex.com

  ManagedFile/k8s.test.phennex.com-addons-bootstrap
  	Location            	addons/bootstrap-channel.yaml

  ManagedFile/k8s.test.phennex.com-addons-core.addons.k8s.io
  	Location            	addons/core.addons.k8s.io/v1.4.0.yaml

  ManagedFile/k8s.test.phennex.com-addons-dns-controller.addons.k8s.io
  	Location            	addons/dns-controller.addons.k8s.io/v1.5.1.yaml

  ManagedFile/k8s.test.phennex.com-addons-kube-dns.addons.k8s.io
  	Location            	addons/kube-dns.addons.k8s.io/v1.5.1.yaml

  ManagedFile/k8s.test.phennex.com-addons-limit-range.addons.k8s.io
  	Location            	addons/limit-range.addons.k8s.io/v1.5.0.yaml

  ManagedFile/k8s.test.phennex.com-addons-networking.weave
  	Location            	addons/networking.weave/v1.8.2.yaml

  ManagedFile/k8s.test.phennex.com-addons-storage-aws.addons.k8s.io
  	Location            	addons/storage-aws.addons.k8s.io/v1.5.0.yaml

  NatGateway/eu-west-1a.k8s.test.phennex.com
  	ElasticIP           	name:eu-west-1a.k8s.test.phennex.com
  	Subnet              	name:utility-eu-west-1a.k8s.test.phennex.com
  	AssociatedRouteTable	name:private-eu-west-1a.k8s.test.phennex.com

  NatGateway/eu-west-1b.k8s.test.phennex.com
  	ElasticIP           	name:eu-west-1b.k8s.test.phennex.com
  	Subnet              	name:utility-eu-west-1b.k8s.test.phennex.com
  	AssociatedRouteTable	name:private-eu-west-1b.k8s.test.phennex.com

  NatGateway/eu-west-1c.k8s.test.phennex.com
  	ElasticIP           	name:eu-west-1c.k8s.test.phennex.com
  	Subnet              	name:utility-eu-west-1c.k8s.test.phennex.com
  	AssociatedRouteTable	name:private-eu-west-1c.k8s.test.phennex.com

  Route/0.0.0.0/0
  	RouteTable          	name:k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	InternetGateway     	name:k8s.test.phennex.com

  Route/private-eu-west-1a-0.0.0.0/0
  	RouteTable          	name:private-eu-west-1a.k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	NatGateway          	name:eu-west-1a.k8s.test.phennex.com

  Route/private-eu-west-1b-0.0.0.0/0
  	RouteTable          	name:private-eu-west-1b.k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	NatGateway          	name:eu-west-1b.k8s.test.phennex.com

  Route/private-eu-west-1c-0.0.0.0/0
  	RouteTable          	name:private-eu-west-1c.k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	NatGateway          	name:eu-west-1c.k8s.test.phennex.com

  RouteTable/k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com

  RouteTable/private-eu-west-1a.k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com

  RouteTable/private-eu-west-1b.k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com

  RouteTable/private-eu-west-1c.k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com

  RouteTableAssociation/private-eu-west-1a.k8s.test.phennex.com
  	RouteTable          	name:private-eu-west-1a.k8s.test.phennex.com
  	Subnet              	name:eu-west-1a.k8s.test.phennex.com

  RouteTableAssociation/private-eu-west-1b.k8s.test.phennex.com
  	RouteTable          	name:private-eu-west-1b.k8s.test.phennex.com
  	Subnet              	name:eu-west-1b.k8s.test.phennex.com

  RouteTableAssociation/private-eu-west-1c.k8s.test.phennex.com
  	RouteTable          	name:private-eu-west-1c.k8s.test.phennex.com
  	Subnet              	name:eu-west-1c.k8s.test.phennex.com

  RouteTableAssociation/utility-eu-west-1a.k8s.test.phennex.com
  	RouteTable          	name:k8s.test.phennex.com
  	Subnet              	name:utility-eu-west-1a.k8s.test.phennex.com

  RouteTableAssociation/utility-eu-west-1b.k8s.test.phennex.com
  	RouteTable          	name:k8s.test.phennex.com
  	Subnet              	name:utility-eu-west-1b.k8s.test.phennex.com

  RouteTableAssociation/utility-eu-west-1c.k8s.test.phennex.com
  	RouteTable          	name:k8s.test.phennex.com
  	Subnet              	name:utility-eu-west-1c.k8s.test.phennex.com

  SSHKey/kubernetes.k8s.test.phennex.com-f5:54:56:d7:30:5d:4b:b7:40:e4:47:ca:9e:34:63:4f
  	KeyFingerprint      	34:42:f7:60:ed:3c:5b:97:4f:7d:f0:3d:7a:e0:c3:ca

  Secret/admin

  Secret/kube

  Secret/kube-proxy

  Secret/kubelet

  Secret/system-controller_manager

  Secret/system-dns

  Secret/system-logging

  Secret/system-monitoring

  Secret/system-scheduler

  SecurityGroup/api-elb.k8s.test.phennex.com
  	Description         	Security group for api ELB
  	VPC                 	name:k8s.test.phennex.com
  	RemoveExtraRules    	[port=443]

  SecurityGroup/bastion-elb.k8s.test.phennex.com
  	Description         	Security group for bastion ELB
  	VPC                 	name:k8s.test.phennex.com
  	RemoveExtraRules    	[port=22]

  SecurityGroup/bastion.k8s.test.phennex.com
  	Description         	Security group for bastion
  	VPC                 	name:k8s.test.phennex.com
  	RemoveExtraRules    	[port=22]

  SecurityGroup/masters.k8s.test.phennex.com
  	Description         	Security group for masters
  	VPC                 	name:k8s.test.phennex.com
  	RemoveExtraRules    	[port=22, port=443, port=4001, port=4789, port=179]

  SecurityGroup/nodes.k8s.test.phennex.com
  	Description         	Security group for nodes
  	VPC                 	name:k8s.test.phennex.com
  	RemoveExtraRules    	[port=22]

  SecurityGroupRule/all-master-to-master
  	SecurityGroup       	name:masters.k8s.test.phennex.com
  	SourceGroup         	name:masters.k8s.test.phennex.com

  SecurityGroupRule/all-master-to-node
  	SecurityGroup       	name:nodes.k8s.test.phennex.com
  	SourceGroup         	name:masters.k8s.test.phennex.com

  SecurityGroupRule/all-node-to-node
  	SecurityGroup       	name:nodes.k8s.test.phennex.com
  	SourceGroup         	name:nodes.k8s.test.phennex.com

  SecurityGroupRule/api-elb-egress
  	SecurityGroup       	name:api-elb.k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	Egress              	true

  SecurityGroupRule/bastion-egress
  	SecurityGroup       	name:bastion.k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	Egress              	true

  SecurityGroupRule/bastion-elb-egress
  	SecurityGroup       	name:bastion-elb.k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	Egress              	true

  SecurityGroupRule/bastion-to-master-ssh
  	SecurityGroup       	name:masters.k8s.test.phennex.com
  	Protocol            	tcp
  	FromPort            	22
  	ToPort              	22
  	SourceGroup         	name:bastion.k8s.test.phennex.com

  SecurityGroupRule/bastion-to-node-ssh
  	SecurityGroup       	name:nodes.k8s.test.phennex.com
  	Protocol            	tcp
  	FromPort            	22
  	ToPort              	22
  	SourceGroup         	name:bastion.k8s.test.phennex.com

  SecurityGroupRule/https-api-elb-0.0.0.0/0
  	SecurityGroup       	name:api-elb.k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	Protocol            	tcp
  	FromPort            	443
  	ToPort              	443

  SecurityGroupRule/https-elb-to-master
  	SecurityGroup       	name:masters.k8s.test.phennex.com
  	Protocol            	tcp
  	FromPort            	443
  	ToPort              	443
  	SourceGroup         	name:api-elb.k8s.test.phennex.com

  SecurityGroupRule/master-egress
  	SecurityGroup       	name:masters.k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	Egress              	true

  SecurityGroupRule/node-egress
  	SecurityGroup       	name:nodes.k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	Egress              	true

  SecurityGroupRule/node-to-master-tcp-4194
  	SecurityGroup       	name:masters.k8s.test.phennex.com
  	Protocol            	tcp
  	FromPort            	4194
  	ToPort              	4194
  	SourceGroup         	name:nodes.k8s.test.phennex.com

  SecurityGroupRule/node-to-master-tcp-443
  	SecurityGroup       	name:masters.k8s.test.phennex.com
  	Protocol            	tcp
  	FromPort            	443
  	ToPort              	443
  	SourceGroup         	name:nodes.k8s.test.phennex.com

  SecurityGroupRule/node-to-master-tcp-6783
  	SecurityGroup       	name:masters.k8s.test.phennex.com
  	Protocol            	tcp
  	FromPort            	6783
  	ToPort              	6783
  	SourceGroup         	name:nodes.k8s.test.phennex.com

  SecurityGroupRule/node-to-master-udp-6783
  	SecurityGroup       	name:masters.k8s.test.phennex.com
  	Protocol            	udp
  	FromPort            	6783
  	ToPort              	6783
  	SourceGroup         	name:nodes.k8s.test.phennex.com

  SecurityGroupRule/node-to-master-udp-6784
  	SecurityGroup       	name:masters.k8s.test.phennex.com
  	Protocol            	udp
  	FromPort            	6784
  	ToPort              	6784
  	SourceGroup         	name:nodes.k8s.test.phennex.com

  SecurityGroupRule/ssh-elb-to-bastion
  	SecurityGroup       	name:bastion.k8s.test.phennex.com
  	Protocol            	tcp
  	FromPort            	22
  	ToPort              	22
  	SourceGroup         	name:bastion-elb.k8s.test.phennex.com

  SecurityGroupRule/ssh-external-to-bastion-elb-0.0.0.0/0
  	SecurityGroup       	name:bastion-elb.k8s.test.phennex.com
  	CIDR                	0.0.0.0/0
  	Protocol            	tcp
  	FromPort            	22
  	ToPort              	22

  Subnet/eu-west-1a.k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1a
  	CIDR                	172.20.32.0/19
  	Shared              	false

  Subnet/eu-west-1b.k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1b
  	CIDR                	172.20.64.0/19
  	Shared              	false

  Subnet/eu-west-1c.k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1c
  	CIDR                	172.20.96.0/19
  	Shared              	false

  Subnet/utility-eu-west-1a.k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1a
  	CIDR                	172.20.0.0/22
  	Shared              	false

  Subnet/utility-eu-west-1b.k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1b
  	CIDR                	172.20.4.0/22
  	Shared              	false

  Subnet/utility-eu-west-1c.k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com
  	AvailabilityZone    	eu-west-1c
  	CIDR                	172.20.8.0/22
  	Shared              	false

  VPC/k8s.test.phennex.com
  	CIDR                	172.20.0.0/16
  	EnableDNSHostnames  	true
  	EnableDNSSupport    	true
  	Shared              	false

  VPCDHCPOptionsAssociation/k8s.test.phennex.com
  	VPC                 	name:k8s.test.phennex.com
  	DHCPOptions         	name:k8s.test.phennex.com

Will modify resources:
  DNSZone/ZF3HEQO28R8JR
  	PrivateVPC          	 <nil> -> name:k8s.test.phennex.com

Must specify --yes to apply changes

Cluster configuration has been created.

Suggestions:
 * list clusters with: kops get cluster
 * edit this cluster with: kops edit cluster k8s.test.phennex.com
 * edit your node instance group: kops edit ig --name=k8s.test.phennex.com nodes
 * edit your master instance group: kops edit ig --name=k8s.test.phennex.com master-eu-west-1a

Finally configure your cluster with: kops update cluster k8s.test.phennex.com --yes
```

That looks fine, let's just spin up the cluster.

```
$ kops update cluster k8s.test.phennex.com --yes

*********************************************************************************

A new kubernetes version is available: 1.5.7
Upgrading is recommended (try kops upgrade cluster)

More information: https://github.com/kubernetes/kops/blob/master/permalinks/upgrade_k8s.md#1.5.7

*********************************************************************************

I0629 13:47:24.508389   14273 dns.go:89] Private DNS: skipping DNS validation
I0629 13:47:25.441824   14273 executor.go:91] Tasks: 0 done / 114 total; 34 can run
I0629 13:47:25.829892   14273 vfs_castore.go:422] Issuing new certificate: "kubelet"
I0629 13:47:25.901313   14273 vfs_castore.go:422] Issuing new certificate: "master"
I0629 13:47:26.258009   14273 vfs_castore.go:422] Issuing new certificate: "kubecfg"
I0629 13:47:27.214400   14273 executor.go:91] Tasks: 34 done / 114 total; 27 can run
I0629 13:47:28.828844   14273 executor.go:91] Tasks: 61 done / 114 total; 36 can run
I0629 13:47:29.315822   14273 launchconfiguration.go:302] waiting for IAM instance profile "bastions.k8s.test.phennex.com" to be ready
I0629 13:47:29.827592   14273 launchconfiguration.go:302] waiting for IAM instance profile "masters.k8s.test.phennex.com" to be ready
I0629 13:47:29.991750   14273 launchconfiguration.go:302] waiting for IAM instance profile "nodes.k8s.test.phennex.com" to be ready
I0629 13:47:30.104134   14273 launchconfiguration.go:302] waiting for IAM instance profile "masters.k8s.test.phennex.com" to be ready
I0629 13:47:30.105073   14273 launchconfiguration.go:302] waiting for IAM instance profile "masters.k8s.test.phennex.com" to be ready
I0629 13:47:40.556973   14273 executor.go:91] Tasks: 97 done / 114 total; 10 can run
I0629 13:47:41.256064   14273 executor.go:91] Tasks: 107 done / 114 total; 7 can run
I0629 13:47:41.316584   14273 natgateway.go:263] Waiting for NAT Gateway "nat-06ac47879e5be10cf" to be available (this often takes about 5 minutes)
I0629 13:47:41.322745   14273 natgateway.go:263] Waiting for NAT Gateway "nat-0a2208c8a892f5b13" to be available (this often takes about 5 minutes)
I0629 13:47:41.375999   14273 natgateway.go:263] Waiting for NAT Gateway "nat-0f64445f8481803f5" to be available (this often takes about 5 minutes)
I0629 13:49:28.194760   14273 executor.go:91] Tasks: 114 done / 114 total; 0 can run
I0629 13:49:28.194842   14273 dns.go:140] Pre-creating DNS records
I0629 13:49:29.872259   14273 update_cluster.go:204] Exporting kubecfg for cluster
Wrote config for k8s.test.phennex.com to "/Volumes/prod/kubernetes/config_prod"
Kops has set your kubectl context to k8s.test.phennex.com

Cluster is starting.  It should be ready in a few minutes.

Suggestions:
 * list nodes: kubectl get nodes --show-labels
 * ssh to the bastion: ssh -i ~/.ssh/id_rsa admin@bastion.k8s.test.phennex.com
 * read about installing addons: https://github.com/kubernetes/kops/blob/master/docs/addons.md
 ```

 Everything comes up nicely, yeah!

 ```
 $ kubectl get nodes
NAME                                           STATUS         AGE       VERSION
ip-172-20-116-20.eu-west-1.compute.internal    Ready,master   5m        v1.5.2
ip-172-20-123-210.eu-west-1.compute.internal   Ready          4m        v1.5.2
ip-172-20-38-130.eu-west-1.compute.internal    Ready          4m        v1.5.2
ip-172-20-51-232.eu-west-1.compute.internal    Ready,master   6m        v1.5.2
ip-172-20-73-196.eu-west-1.compute.internal    Ready,master   6m        v1.5.2
ip-172-20-83-179.eu-west-1.compute.internal    Ready          4m        v1.5.2
```

... and kube-system as well!

```
$ kubectl get pods -n kube-system -owide
NAME                                                                  READY     STATUS    RESTARTS   AGE       IP               NODE
dns-controller-849840987-8wm54                                        1/1       Running   0          7m        172.20.73.196    ip-172-20-73-196.eu-west-1.compute.internal
etcd-server-events-ip-172-20-116-20.eu-west-1.compute.internal        1/1       Running   0          6m        172.20.116.20    ip-172-20-116-20.eu-west-1.compute.internal
etcd-server-events-ip-172-20-51-232.eu-west-1.compute.internal        1/1       Running   0          7m        172.20.51.232    ip-172-20-51-232.eu-west-1.compute.internal
etcd-server-events-ip-172-20-73-196.eu-west-1.compute.internal        1/1       Running   0          7m        172.20.73.196    ip-172-20-73-196.eu-west-1.compute.internal
etcd-server-ip-172-20-116-20.eu-west-1.compute.internal               1/1       Running   0          6m        172.20.116.20    ip-172-20-116-20.eu-west-1.compute.internal
etcd-server-ip-172-20-51-232.eu-west-1.compute.internal               1/1       Running   0          7m        172.20.51.232    ip-172-20-51-232.eu-west-1.compute.internal
etcd-server-ip-172-20-73-196.eu-west-1.compute.internal               1/1       Running   0          7m        172.20.73.196    ip-172-20-73-196.eu-west-1.compute.internal
kube-apiserver-ip-172-20-116-20.eu-west-1.compute.internal            1/1       Running   0          7m        172.20.116.20    ip-172-20-116-20.eu-west-1.compute.internal
kube-apiserver-ip-172-20-51-232.eu-west-1.compute.internal            1/1       Running   1          7m        172.20.51.232    ip-172-20-51-232.eu-west-1.compute.internal
kube-apiserver-ip-172-20-73-196.eu-west-1.compute.internal            1/1       Running   0          7m        172.20.73.196    ip-172-20-73-196.eu-west-1.compute.internal
kube-controller-manager-ip-172-20-116-20.eu-west-1.compute.internal   1/1       Running   0          7m        172.20.116.20    ip-172-20-116-20.eu-west-1.compute.internal
kube-controller-manager-ip-172-20-51-232.eu-west-1.compute.internal   1/1       Running   0          7m        172.20.51.232    ip-172-20-51-232.eu-west-1.compute.internal
kube-controller-manager-ip-172-20-73-196.eu-west-1.compute.internal   1/1       Running   1          7m        172.20.73.196    ip-172-20-73-196.eu-west-1.compute.internal
kube-dns-782804071-29ghv                                              4/4       Running   0          6m        10.34.0.1        ip-172-20-123-210.eu-west-1.compute.internal
kube-dns-782804071-vjxnp                                              4/4       Running   0          7m        10.37.0.1        ip-172-20-83-179.eu-west-1.compute.internal
kube-dns-autoscaler-2813114833-w64br                                  1/1       Running   0          7m        10.46.0.1        ip-172-20-38-130.eu-west-1.compute.internal
kube-proxy-ip-172-20-116-20.eu-west-1.compute.internal                1/1       Running   0          6m        172.20.116.20    ip-172-20-116-20.eu-west-1.compute.internal
kube-proxy-ip-172-20-123-210.eu-west-1.compute.internal               1/1       Running   0          5m        172.20.123.210   ip-172-20-123-210.eu-west-1.compute.internal
kube-proxy-ip-172-20-38-130.eu-west-1.compute.internal                1/1       Running   0          6m        172.20.38.130    ip-172-20-38-130.eu-west-1.compute.internal
kube-proxy-ip-172-20-51-232.eu-west-1.compute.internal                1/1       Running   0          8m        172.20.51.232    ip-172-20-51-232.eu-west-1.compute.internal
kube-proxy-ip-172-20-73-196.eu-west-1.compute.internal                1/1       Running   0          8m        172.20.73.196    ip-172-20-73-196.eu-west-1.compute.internal
kube-proxy-ip-172-20-83-179.eu-west-1.compute.internal                1/1       Running   0          5m        172.20.83.179    ip-172-20-83-179.eu-west-1.compute.internal
kube-scheduler-ip-172-20-116-20.eu-west-1.compute.internal            1/1       Running   0          6m        172.20.116.20    ip-172-20-116-20.eu-west-1.compute.internal
kube-scheduler-ip-172-20-51-232.eu-west-1.compute.internal            1/1       Running   1          8m        172.20.51.232    ip-172-20-51-232.eu-west-1.compute.internal
kube-scheduler-ip-172-20-73-196.eu-west-1.compute.internal            1/1       Running   0          8m        172.20.73.196    ip-172-20-73-196.eu-west-1.compute.internal
weave-net-1rm0f                                                       2/2       Running   0          7m        172.20.116.20    ip-172-20-116-20.eu-west-1.compute.internal
weave-net-b93gg                                                       2/2       Running   1          7m        172.20.38.130    ip-172-20-38-130.eu-west-1.compute.internal
weave-net-cjfgd                                                       2/2       Running   0          6m        172.20.123.210   ip-172-20-123-210.eu-west-1.compute.internal
weave-net-fr2pb                                                       2/2       Running   0          7m        172.20.51.232    ip-172-20-51-232.eu-west-1.compute.internal
weave-net-w3tp7                                                       2/2       Running   1          6m        172.20.83.179    ip-172-20-83-179.eu-west-1.compute.internal
weave-net-w6ll8                                                       2/2       Running   0          7m        172.20.73.196    ip-172-20-73-196.eu-west-1.compute.internal
```

# Upgrading directly to Kops 1.6.2 and Kubernetes 1.6.2

## Installing Kops 1.6.2

Link: https://github.com/kubernetes/kops/releases/tag/1.6.2

Verify
```
$ kops version
Version 1.6.2 (git-98ae12a)
```

## Before upgrading

Before upgrading the cluster, lets check the know issues:

```
$ kubectl -n kube-system get configmap kube-dns
Error from server (NotFound): configmaps "kube-dns" not found
```

The kube-dns config-map is not present. Let's create it before upgrading.

```
$ kubectl create configmap -n kube-system kube-dns
configmap "kube-dns" created
```

... and lastly verify that has been created

```
$ kubectl -n kube-system get configmap kube-dns
NAME       DATA      AGE
kube-dns   0         29s
```

Awesome!

## Let's upgrade the cluster

```
$ kops upgrade cluster
Using cluster from kubectl context: k8s.test.phennex.com

ITEM				PROPERTY		OLD							NEW
Cluster				KubernetesVersion	1.5.2							1.6.2
InstanceGroup/bastions		Image			kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-05-02	kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02
InstanceGroup/master-eu-west-1a	Image			kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09	kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02
InstanceGroup/master-eu-west-1b	Image			kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09	kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02
InstanceGroup/master-eu-west-1c	Image			kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09	kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02
InstanceGroup/nodes		Image			kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09	kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02

Must specify --yes to perform upgrade
```
... and applying this to the configuration results in:
```
$ kops upgrade cluster --yes
Using cluster from kubectl context: k8s.test.phennex.com

ITEM				PROPERTY		OLD							NEW
Cluster				KubernetesVersion	1.5.2							1.6.2
InstanceGroup/bastions		Image			kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-05-02	kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02
InstanceGroup/master-eu-west-1a	Image			kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09	kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02
InstanceGroup/master-eu-west-1b	Image			kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09	kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02
InstanceGroup/master-eu-west-1c	Image			kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09	kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02
InstanceGroup/nodes		Image			kope.io/k8s-1.5-debian-jessie-amd64-hvm-ebs-2017-01-09	kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02

Updates applied to configuration.
You can now apply these changes, using `kops update cluster k8s.test.phennex.com`
```

## Update the cluster

Now we need to update the cluster, let's first verify the changes:

```
$ kops update cluster k8s.test.phennex.com
I0629 14:07:07.774215   16172 dns.go:91] Private DNS: skipping DNS validation
I0629 14:07:07.780826   16172 executor.go:91] Tasks: 0 done / 119 total; 41 can run
I0629 14:07:08.378317   16172 executor.go:91] Tasks: 41 done / 119 total; 24 can run
I0629 14:07:08.950832   16172 executor.go:91] Tasks: 65 done / 119 total; 37 can run
I0629 14:07:09.958368   16172 executor.go:91] Tasks: 102 done / 119 total; 10 can run
I0629 14:07:10.096697   16172 dnsname.go:108] AliasTarget for "bastion.k8s.test.phennex.com." is "bastion-k8s-48060442.eu-west-1.elb.amazonaws.com."
I0629 14:07:10.231168   16172 dnsname.go:108] AliasTarget for "api.k8s.test.phennex.com." is "api-k8s-920095612.eu-west-1.elb.amazonaws.com."
I0629 14:07:10.339993   16172 executor.go:91] Tasks: 112 done / 119 total; 7 can run
I0629 14:07:10.512442   16172 executor.go:91] Tasks: 119 done / 119 total; 0 can run
Will create resources:
  Keypair/kops
  	Subject             	o=system:masters,cn=kops
  	Type                	client

  Keypair/kube-controller-manager
  	Subject             	cn=system:kube-controller-manager
  	Type                	client

  Keypair/kube-proxy
  	Subject             	cn=system:kube-proxy
  	Type                	client

  Keypair/kube-scheduler
  	Subject             	cn=system:kube-scheduler
  	Type                	client

  ManagedFile/k8s.test.phennex.com-addons-dns-controller.addons.k8s.io-k8s-1.6
  	Location            	addons/dns-controller.addons.k8s.io/k8s-1.6.yaml

  ManagedFile/k8s.test.phennex.com-addons-dns-controller.addons.k8s.io-pre-k8s-1.6
  	Location            	addons/dns-controller.addons.k8s.io/pre-k8s-1.6.yaml

  ManagedFile/k8s.test.phennex.com-addons-kube-dns.addons.k8s.io-k8s-1.6
  	Location            	addons/kube-dns.addons.k8s.io/k8s-1.6.yaml

  ManagedFile/k8s.test.phennex.com-addons-kube-dns.addons.k8s.io-pre-k8s-1.6
  	Location            	addons/kube-dns.addons.k8s.io/pre-k8s-1.6.yaml

  ManagedFile/k8s.test.phennex.com-addons-networking.weave-k8s-1.6
  	Location            	addons/networking.weave/k8s-1.6.yaml

  ManagedFile/k8s.test.phennex.com-addons-networking.weave-pre-k8s-1.6
  	Location            	addons/networking.weave/pre-k8s-1.6.yaml

  ManagedFile/k8s.test.phennex.com-addons-storage-aws.addons.k8s.io
  	Location            	addons/storage-aws.addons.k8s.io/v1.6.0.yaml

  SecurityGroupRule/node-to-master-tcp-1-4000
  	SecurityGroup       	name:masters.k8s.test.phennex.com id:sg-d762a7af
  	Protocol            	tcp
  	FromPort            	1
  	ToPort              	4000
  	SourceGroup         	name:nodes.k8s.test.phennex.com id:sg-6c60a514

  SecurityGroupRule/node-to-master-tcp-4003-65535
  	SecurityGroup       	name:masters.k8s.test.phennex.com id:sg-d762a7af
  	Protocol            	tcp
  	FromPort            	4003
  	ToPort              	65535
  	SourceGroup         	name:nodes.k8s.test.phennex.com id:sg-6c60a514

  SecurityGroupRule/node-to-master-udp-1-65535
  	SecurityGroup       	name:masters.k8s.test.phennex.com id:sg-d762a7af
  	Protocol            	udp
  	FromPort            	1
  	ToPort              	65535
  	SourceGroup         	name:nodes.k8s.test.phennex.com id:sg-6c60a514

Will modify resources:
  IAMRolePolicy/masters.k8s.test.phennex.com
  	PolicyDocument
  	                    	...
  	                    	        "Effect": "Allow",
  	                    	        "Action": [
  	                    	+         "elasticloadbalancing:*"
  	                    	-         "route53:*"
  	                    	        ],
  	                    	        "Resource": [
  	                    	...
  	                    	        "Effect": "Allow",
  	                    	        "Action": [
  	                    	-         "elasticloadbalancing:*"
  	                    	-       ],
  	                    	-       "Resource": [
  	                    	-         "*"
  	                    	-       ]
  	                    	-     },
  	                    	-     {
  	                    	-       "Effect": "Allow",
  	                    	-       "Action": [
  	                    	-         "autoscaling:DescribeAutoScalingGroups",
  	                    	+         "autoscaling:DescribeAutoScalingGroups",
  	                    	+         "autoscaling:DescribeAutoScalingInstances",
  	                    	+         "autoscaling:SetDesiredCapacity",
  	                    	+         "autoscaling:TerminateInstanceInAutoScalingGroup"
  	                    	+       ],
  	                    	+       "Resource": [
  	                    	+         "*"
  	                    	+       ]
  	                    	+     },
  	                    	+     {
  	                    	+       "Effect": "Allow",
  	                    	+       "Action": [
  	                    	+         "route53:ChangeResourceRecordSets",
  	                    	+         "route53:ListResourceRecordSets",
  	                    	+         "route53:GetHostedZone"
  	                    	+       ],
  	                    	+       "Resource": [
  	                    	+         "arn:aws:route53:::hostedzone/ZF3HEQO28R8JR"
  	                    	+       ]
  	                    	+     },
  	                    	-         "autoscaling:DescribeAutoScalingInstances",
  	                    	+     {
  	                    	-         "autoscaling:SetDesiredCapacity",
  	                    	+       "Effect": "Allow",
  	                    	+       "Action": [
  	                    	+         "route53:GetChange"
  	                    	+       ],
  	                    	+       "Resource": [
  	                    	+         "arn:aws:route53:::change/*"
  	                    	+       ]
  	                    	+     },
  	                    	+     {
  	                    	+       "Effect": "Allow",
  	                    	+       "Action": [
  	                    	+         "route53:ListHostedZones"
  	                    	-         "autoscaling:TerminateInstanceInAutoScalingGroup"
  	                    	        ],
  	                    	        "Resource": [
  	                    	...


  IAMRolePolicy/nodes.k8s.test.phennex.com
  	PolicyDocument
  	                    	...
  	                    	        "Effect": "Allow",
  	                    	        "Action": [
  	                    	-         "route53:*"
  	                    	-       ],
  	                    	-       "Resource": [
  	                    	-         "*"
  	                    	-       ]
  	                    	-     },
  	                    	-     {
  	                    	-       "Effect": "Allow",
  	                    	-       "Action": [
  	                    	+         "ecr:GetAuthorizationToken",
  	                    	-         "ecr:GetAuthorizationToken",
  	                    	          "ecr:BatchCheckLayerAvailability",
  	                    	          "ecr:GetDownloadUrlForLayer",
  	                    	+         "ecr:GetRepositoryPolicy",
  	                    	+         "ecr:DescribeRepositories",
  	                    	+         "ecr:ListImages",
  	                    	+         "ecr:BatchGetImage"
  	                    	+       ],
  	                    	+       "Resource": [
  	                    	+         "*"
  	                    	+       ]
  	                    	+     },
  	                    	+     {
  	                    	+       "Effect": "Allow",
  	                    	+       "Action": [
  	                    	+         "route53:ChangeResourceRecordSets",
  	                    	+         "route53:ListResourceRecordSets",
  	                    	+         "route53:GetHostedZone"
  	                    	+       ],
  	                    	+       "Resource": [
  	                    	+         "arn:aws:route53:::hostedzone/ZF3HEQO28R8JR"
  	                    	+       ]
  	                    	+     },
  	                    	-         "ecr:GetRepositoryPolicy",
  	                    	+     {
  	                    	-         "ecr:DescribeRepositories",
  	                    	+       "Effect": "Allow",
  	                    	+       "Action": [
  	                    	+         "route53:GetChange"
  	                    	+       ],
  	                    	+       "Resource": [
  	                    	+         "arn:aws:route53:::change/*"
  	                    	+       ]
  	                    	+     },
  	                    	-         "ecr:ListImages",
  	                    	+     {
  	                    	+       "Effect": "Allow",
  	                    	+       "Action": [
  	                    	+         "route53:ListHostedZones"
  	                    	-         "ecr:BatchGetImage"
  	                    	        ],
  	                    	        "Resource": [
  	                    	...


  Keypair/kubecfg
  	Subject             	 cn=kubecfg -> o=system:masters,cn=kubecfg

  Keypair/kubelet
  	Subject             	 cn=kubelet -> o=system:nodes,cn=kubelet

  Keypair/master
  	AlternateNames      	 [100.64.0.1, api.internal.k8s.test.phennex.com, api.k8s.test.phennex.com, kubernetes, kubernetes.default, kubernetes.default.svc, kubernetes.default.svc.cluster.local] -> [100.64.0.1, 127.0.0.1, api.internal.k8s.test.phennex.com, api.k8s.test.phennex.com, kubernetes, kubernetes.default, kubernetes.default.svc, kubernetes.default.svc.cluster.local]

  LaunchConfiguration/bastions.k8s.test.phennex.com
  	ImageID             	 ami-93a9abf5 -> kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02

  LaunchConfiguration/master-eu-west-1a.masters.k8s.test.phennex.com
  	UserData
  	                    	...
  	                    	  set -o pipefail

  	                    	+ NODEUP_URL=https://kubeupv2.s3.amazonaws.com/kops/1.6.2/linux/amd64/nodeup
  	                    	- NODEUP_URL=https://kubeupv2.s3.amazonaws.com/kops/1.5.1/linux/amd64/nodeup
  	                    	  NODEUP_HASH=

  	                    	+
  	                    	+
  	                    	  function ensure-install-dir() {
  	                    	    INSTALL_DIR="/var/cache/kubernetes-install"
  	                    	+   # On ContainerOS, we install to /var/lib/toolbox install (because of noexec)
  	                    	+   if [[ -d /var/lib/toolbox ]]; then
  	                    	+     INSTALL_DIR="/var/lib/toolbox/kubernetes-install"
  	                    	+   fi
  	                    	    mkdir -p ${INSTALL_DIR}
  	                    	    cd ${INSTALL_DIR}
  	                    	...
  	                    	    done

  	                    	+   echo "Running nodeup"
  	                    	-   echo "Running release install script"
  	                    	+   # We can't run in the foreground because of https://github.com/docker/docker/issues/23793
  	                    	-   # We run in the background to work around https://github.com/docker/docker/issues/23793
  	                    	-   run-nodeup &
  	                    	- }
  	                    	-
  	                    	- function run-nodeup() {
  	                    	-   sleep 1
  	                    	+   ( cd ${INSTALL_DIR}; ./nodeup --install-systemd-unit --conf=${INSTALL_DIR}/kube_env.yaml --v=8  )
  	                    	-   ( cd ${INSTALL_DIR}; ./nodeup --conf=/var/cache/kubernetes-install/kube_env.yaml --v=8 )
  	                    	  }

  	                    	...
  	                    	  cat > kube_env.yaml << __EOF_KUBE_ENV
  	                    	  Assets:
  	                    	+ - 57afca200aa6cec74fcc3072cae12385014f59c0@https://storage.googleapis.com/kubernetes-release/release/v1.6.2/bin/linux/amd64/kubelet
  	                    	- - 5e486d4a2700a3a61c4edfd97fb088984a7f734f@https://storage.googleapis.com/kubernetes-release/release/v1.5.2/bin/linux/amd64/kubelet
  	                    	+ - 984095cd0fe8a8172ab92e2ee0add49dfc46e0c2@https://storage.googleapis.com/kubernetes-release/release/v1.6.2/bin/linux/amd64/kubectl
  	                    	- - 10e675883b167140f78ddf7ed92f936dca291647@https://storage.googleapis.com/kubernetes-release/release/v1.5.2/bin/linux/amd64/kubectl
  	                    	+ - 1d9788b0f5420e1a219aad2cb8681823fc515e7c@https://storage.googleapis.com/kubernetes-release/network-plugins/cni-0799f5732f2a11b329d9e3d51b9c8f2e3759f2ff.tar.gz
  	                    	+ - e783785020d85426e1d12a7f78aaacc511ffaf0e@https://kubeupv2.s3.amazonaws.com/kops/1.6.2/linux/amd64/utils.tar.gz
  	                    	- - 19d49f7b2b99cd2493d5ae0ace896c64e289ccbb@https://storage.googleapis.com/kubernetes-release/network-plugins/cni-07a8a28637e97b22eb8dfe710eeae1344f69d16e.tar.gz
  	                    	  ClusterName: k8s.test.phennex.com
  	                    	  ConfigBase: s3://phennex-test-k8s-state-store/k8s.test.phennex.com
  	                    	...
  	                    	  - _automatic_upgrades
  	                    	  - _aws
  	                    	- - _cni_bridge
  	                    	- - _cni_host_local
  	                    	- - _cni_loopback
  	                    	- - _cni_ptp
  	                    	  - _kubernetes_master
  	                    	- - _kubernetes_pool
  	                    	  - _networking_cni
  	                    	- - _protokube
  	                    	  channels:
  	                    	  - s3://phennex-test-k8s-state-store/k8s.test.phennex.com/addons/bootstrap-channel.yaml
  	                    	  protokubeImage:
  	                    	+   hash: e24d27eda265e42a6efb47f969ec1fccde218cd6
  	                    	-   hash: 6805cba0ea13805b2fa439914679a083be7ac959
  	                    	+   name: protokube:1.6.2
  	                    	-   name: protokube:1.5.1
  	                    	+   source: https://kubeupv2.s3.amazonaws.com/kops/1.6.2/images/protokube.tar.gz
  	                    	-   source: https://kubeupv2.s3.amazonaws.com/kops/1.5.1/images/protokube.tar.gz

  	                    	  __EOF_KUBE_ENV
  	                    	...

  	ImageID             	 ami-fa5c7489 -> kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02

  LaunchConfiguration/master-eu-west-1b.masters.k8s.test.phennex.com
  	UserData
  	                    	...
  	                    	  set -o pipefail

  	                    	+ NODEUP_URL=https://kubeupv2.s3.amazonaws.com/kops/1.6.2/linux/amd64/nodeup
  	                    	- NODEUP_URL=https://kubeupv2.s3.amazonaws.com/kops/1.5.1/linux/amd64/nodeup
  	                    	  NODEUP_HASH=

  	                    	+
  	                    	+
  	                    	  function ensure-install-dir() {
  	                    	    INSTALL_DIR="/var/cache/kubernetes-install"
  	                    	+   # On ContainerOS, we install to /var/lib/toolbox install (because of noexec)
  	                    	+   if [[ -d /var/lib/toolbox ]]; then
  	                    	+     INSTALL_DIR="/var/lib/toolbox/kubernetes-install"
  	                    	+   fi
  	                    	    mkdir -p ${INSTALL_DIR}
  	                    	    cd ${INSTALL_DIR}
  	                    	...
  	                    	    done

  	                    	+   echo "Running nodeup"
  	                    	-   echo "Running release install script"
  	                    	+   # We can't run in the foreground because of https://github.com/docker/docker/issues/23793
  	                    	-   # We run in the background to work around https://github.com/docker/docker/issues/23793
  	                    	-   run-nodeup &
  	                    	- }
  	                    	-
  	                    	- function run-nodeup() {
  	                    	-   sleep 1
  	                    	+   ( cd ${INSTALL_DIR}; ./nodeup --install-systemd-unit --conf=${INSTALL_DIR}/kube_env.yaml --v=8  )
  	                    	-   ( cd ${INSTALL_DIR}; ./nodeup --conf=/var/cache/kubernetes-install/kube_env.yaml --v=8 )
  	                    	  }

  	                    	...
  	                    	  cat > kube_env.yaml << __EOF_KUBE_ENV
  	                    	  Assets:
  	                    	+ - 57afca200aa6cec74fcc3072cae12385014f59c0@https://storage.googleapis.com/kubernetes-release/release/v1.6.2/bin/linux/amd64/kubelet
  	                    	- - 5e486d4a2700a3a61c4edfd97fb088984a7f734f@https://storage.googleapis.com/kubernetes-release/release/v1.5.2/bin/linux/amd64/kubelet
  	                    	+ - 984095cd0fe8a8172ab92e2ee0add49dfc46e0c2@https://storage.googleapis.com/kubernetes-release/release/v1.6.2/bin/linux/amd64/kubectl
  	                    	- - 10e675883b167140f78ddf7ed92f936dca291647@https://storage.googleapis.com/kubernetes-release/release/v1.5.2/bin/linux/amd64/kubectl
  	                    	+ - 1d9788b0f5420e1a219aad2cb8681823fc515e7c@https://storage.googleapis.com/kubernetes-release/network-plugins/cni-0799f5732f2a11b329d9e3d51b9c8f2e3759f2ff.tar.gz
  	                    	+ - e783785020d85426e1d12a7f78aaacc511ffaf0e@https://kubeupv2.s3.amazonaws.com/kops/1.6.2/linux/amd64/utils.tar.gz
  	                    	- - 19d49f7b2b99cd2493d5ae0ace896c64e289ccbb@https://storage.googleapis.com/kubernetes-release/network-plugins/cni-07a8a28637e97b22eb8dfe710eeae1344f69d16e.tar.gz
  	                    	  ClusterName: k8s.test.phennex.com
  	                    	  ConfigBase: s3://phennex-test-k8s-state-store/k8s.test.phennex.com
  	                    	...
  	                    	  - _automatic_upgrades
  	                    	  - _aws
  	                    	- - _cni_bridge
  	                    	- - _cni_host_local
  	                    	- - _cni_loopback
  	                    	- - _cni_ptp
  	                    	  - _kubernetes_master
  	                    	- - _kubernetes_pool
  	                    	  - _networking_cni
  	                    	- - _protokube
  	                    	  channels:
  	                    	  - s3://phennex-test-k8s-state-store/k8s.test.phennex.com/addons/bootstrap-channel.yaml
  	                    	  protokubeImage:
  	                    	+   hash: e24d27eda265e42a6efb47f969ec1fccde218cd6
  	                    	-   hash: 6805cba0ea13805b2fa439914679a083be7ac959
  	                    	+   name: protokube:1.6.2
  	                    	-   name: protokube:1.5.1
  	                    	+   source: https://kubeupv2.s3.amazonaws.com/kops/1.6.2/images/protokube.tar.gz
  	                    	-   source: https://kubeupv2.s3.amazonaws.com/kops/1.5.1/images/protokube.tar.gz

  	                    	  __EOF_KUBE_ENV
  	                    	...

  	ImageID             	 ami-fa5c7489 -> kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02

  LaunchConfiguration/master-eu-west-1c.masters.k8s.test.phennex.com
  	UserData
  	                    	...
  	                    	  set -o pipefail

  	                    	+ NODEUP_URL=https://kubeupv2.s3.amazonaws.com/kops/1.6.2/linux/amd64/nodeup
  	                    	- NODEUP_URL=https://kubeupv2.s3.amazonaws.com/kops/1.5.1/linux/amd64/nodeup
  	                    	  NODEUP_HASH=

  	                    	+
  	                    	+
  	                    	  function ensure-install-dir() {
  	                    	    INSTALL_DIR="/var/cache/kubernetes-install"
  	                    	+   # On ContainerOS, we install to /var/lib/toolbox install (because of noexec)
  	                    	+   if [[ -d /var/lib/toolbox ]]; then
  	                    	+     INSTALL_DIR="/var/lib/toolbox/kubernetes-install"
  	                    	+   fi
  	                    	    mkdir -p ${INSTALL_DIR}
  	                    	    cd ${INSTALL_DIR}
  	                    	...
  	                    	    done

  	                    	+   echo "Running nodeup"
  	                    	-   echo "Running release install script"
  	                    	+   # We can't run in the foreground because of https://github.com/docker/docker/issues/23793
  	                    	-   # We run in the background to work around https://github.com/docker/docker/issues/23793
  	                    	-   run-nodeup &
  	                    	- }
  	                    	-
  	                    	- function run-nodeup() {
  	                    	-   sleep 1
  	                    	+   ( cd ${INSTALL_DIR}; ./nodeup --install-systemd-unit --conf=${INSTALL_DIR}/kube_env.yaml --v=8  )
  	                    	-   ( cd ${INSTALL_DIR}; ./nodeup --conf=/var/cache/kubernetes-install/kube_env.yaml --v=8 )
  	                    	  }

  	                    	...
  	                    	  cat > kube_env.yaml << __EOF_KUBE_ENV
  	                    	  Assets:
  	                    	+ - 57afca200aa6cec74fcc3072cae12385014f59c0@https://storage.googleapis.com/kubernetes-release/release/v1.6.2/bin/linux/amd64/kubelet
  	                    	- - 5e486d4a2700a3a61c4edfd97fb088984a7f734f@https://storage.googleapis.com/kubernetes-release/release/v1.5.2/bin/linux/amd64/kubelet
  	                    	+ - 984095cd0fe8a8172ab92e2ee0add49dfc46e0c2@https://storage.googleapis.com/kubernetes-release/release/v1.6.2/bin/linux/amd64/kubectl
  	                    	- - 10e675883b167140f78ddf7ed92f936dca291647@https://storage.googleapis.com/kubernetes-release/release/v1.5.2/bin/linux/amd64/kubectl
  	                    	+ - 1d9788b0f5420e1a219aad2cb8681823fc515e7c@https://storage.googleapis.com/kubernetes-release/network-plugins/cni-0799f5732f2a11b329d9e3d51b9c8f2e3759f2ff.tar.gz
  	                    	+ - e783785020d85426e1d12a7f78aaacc511ffaf0e@https://kubeupv2.s3.amazonaws.com/kops/1.6.2/linux/amd64/utils.tar.gz
  	                    	- - 19d49f7b2b99cd2493d5ae0ace896c64e289ccbb@https://storage.googleapis.com/kubernetes-release/network-plugins/cni-07a8a28637e97b22eb8dfe710eeae1344f69d16e.tar.gz
  	                    	  ClusterName: k8s.test.phennex.com
  	                    	  ConfigBase: s3://phennex-test-k8s-state-store/k8s.test.phennex.com
  	                    	...
  	                    	  - _automatic_upgrades
  	                    	  - _aws
  	                    	- - _cni_bridge
  	                    	- - _cni_host_local
  	                    	- - _cni_loopback
  	                    	- - _cni_ptp
  	                    	  - _kubernetes_master
  	                    	- - _kubernetes_pool
  	                    	  - _networking_cni
  	                    	- - _protokube
  	                    	  channels:
  	                    	  - s3://phennex-test-k8s-state-store/k8s.test.phennex.com/addons/bootstrap-channel.yaml
  	                    	  protokubeImage:
  	                    	+   hash: e24d27eda265e42a6efb47f969ec1fccde218cd6
  	                    	-   hash: 6805cba0ea13805b2fa439914679a083be7ac959
  	                    	+   name: protokube:1.6.2
  	                    	-   name: protokube:1.5.1
  	                    	+   source: https://kubeupv2.s3.amazonaws.com/kops/1.6.2/images/protokube.tar.gz
  	                    	-   source: https://kubeupv2.s3.amazonaws.com/kops/1.5.1/images/protokube.tar.gz

  	                    	  __EOF_KUBE_ENV
  	                    	...

  	ImageID             	 ami-fa5c7489 -> kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02

  LaunchConfiguration/nodes.k8s.test.phennex.com
  	UserData
  	                    	...
  	                    	  set -o pipefail

  	                    	+ NODEUP_URL=https://kubeupv2.s3.amazonaws.com/kops/1.6.2/linux/amd64/nodeup
  	                    	- NODEUP_URL=https://kubeupv2.s3.amazonaws.com/kops/1.5.1/linux/amd64/nodeup
  	                    	  NODEUP_HASH=

  	                    	+
  	                    	+
  	                    	  function ensure-install-dir() {
  	                    	    INSTALL_DIR="/var/cache/kubernetes-install"
  	                    	+   # On ContainerOS, we install to /var/lib/toolbox install (because of noexec)
  	                    	+   if [[ -d /var/lib/toolbox ]]; then
  	                    	+     INSTALL_DIR="/var/lib/toolbox/kubernetes-install"
  	                    	+   fi
  	                    	    mkdir -p ${INSTALL_DIR}
  	                    	    cd ${INSTALL_DIR}
  	                    	...
  	                    	    done

  	                    	+   echo "Running nodeup"
  	                    	-   echo "Running release install script"
  	                    	+   # We can't run in the foreground because of https://github.com/docker/docker/issues/23793
  	                    	-   # We run in the background to work around https://github.com/docker/docker/issues/23793
  	                    	-   run-nodeup &
  	                    	- }
  	                    	-
  	                    	- function run-nodeup() {
  	                    	-   sleep 1
  	                    	+   ( cd ${INSTALL_DIR}; ./nodeup --install-systemd-unit --conf=${INSTALL_DIR}/kube_env.yaml --v=8  )
  	                    	-   ( cd ${INSTALL_DIR}; ./nodeup --conf=/var/cache/kubernetes-install/kube_env.yaml --v=8 )
  	                    	  }

  	                    	...
  	                    	  cat > kube_env.yaml << __EOF_KUBE_ENV
  	                    	  Assets:
  	                    	+ - 57afca200aa6cec74fcc3072cae12385014f59c0@https://storage.googleapis.com/kubernetes-release/release/v1.6.2/bin/linux/amd64/kubelet
  	                    	- - 5e486d4a2700a3a61c4edfd97fb088984a7f734f@https://storage.googleapis.com/kubernetes-release/release/v1.5.2/bin/linux/amd64/kubelet
  	                    	+ - 984095cd0fe8a8172ab92e2ee0add49dfc46e0c2@https://storage.googleapis.com/kubernetes-release/release/v1.6.2/bin/linux/amd64/kubectl
  	                    	- - 10e675883b167140f78ddf7ed92f936dca291647@https://storage.googleapis.com/kubernetes-release/release/v1.5.2/bin/linux/amd64/kubectl
  	                    	+ - 1d9788b0f5420e1a219aad2cb8681823fc515e7c@https://storage.googleapis.com/kubernetes-release/network-plugins/cni-0799f5732f2a11b329d9e3d51b9c8f2e3759f2ff.tar.gz
  	                    	+ - e783785020d85426e1d12a7f78aaacc511ffaf0e@https://kubeupv2.s3.amazonaws.com/kops/1.6.2/linux/amd64/utils.tar.gz
  	                    	- - 19d49f7b2b99cd2493d5ae0ace896c64e289ccbb@https://storage.googleapis.com/kubernetes-release/network-plugins/cni-07a8a28637e97b22eb8dfe710eeae1344f69d16e.tar.gz
  	                    	  ClusterName: k8s.test.phennex.com
  	                    	  ConfigBase: s3://phennex-test-k8s-state-store/k8s.test.phennex.com
  	                    	...
  	                    	  - _automatic_upgrades
  	                    	  - _aws
  	                    	- - _cni_bridge
  	                    	- - _cni_host_local
  	                    	- - _cni_loopback
  	                    	- - _cni_ptp
  	                    	- - _kubernetes_pool
  	                    	  - _networking_cni
  	                    	- - _protokube
  	                    	  channels:
  	                    	  - s3://phennex-test-k8s-state-store/k8s.test.phennex.com/addons/bootstrap-channel.yaml
  	                    	  protokubeImage:
  	                    	+   hash: e24d27eda265e42a6efb47f969ec1fccde218cd6
  	                    	-   hash: 6805cba0ea13805b2fa439914679a083be7ac959
  	                    	+   name: protokube:1.6.2
  	                    	-   name: protokube:1.5.1
  	                    	+   source: https://kubeupv2.s3.amazonaws.com/kops/1.6.2/images/protokube.tar.gz
  	                    	-   source: https://kubeupv2.s3.amazonaws.com/kops/1.5.1/images/protokube.tar.gz

  	                    	  __EOF_KUBE_ENV
  	                    	...

  	ImageID             	 ami-fa5c7489 -> kope.io/k8s-1.6-debian-jessie-amd64-hvm-ebs-2017-05-02

  LoadBalancer/api.k8s.test.phennex.com
  	ConnectionSettings  	 {"IdleTimeout":60} -> {"IdleTimeout":300}

  ManagedFile/k8s.test.phennex.com-addons-bootstrap
  	Contents
  	                    	...
  	                    	        k8s-addon: core.addons.k8s.io
  	                    	      version: 1.4.0
  	                    	+   - id: pre-k8s-1.6
  	                    	+     kubernetesVersion: <1.6.0
  	                    	+     manifest: kube-dns.addons.k8s.io/pre-k8s-1.6.yaml
  	                    	+     name: kube-dns.addons.k8s.io
  	                    	+     selector:
  	                    	+       k8s-addon: kube-dns.addons.k8s.io
  	                    	+     version: 1.6.1-alpha.2
  	                    	+   - id: k8s-1.6
  	                    	+     kubernetesVersion: '>=1.6.0'
  	                    	+     manifest: kube-dns.addons.k8s.io/k8s-1.6.yaml
  	                    	-   - manifest: kube-dns.addons.k8s.io/v1.5.1.yaml
  	                    	      name: kube-dns.addons.k8s.io
  	                    	      selector:
  	                    	        k8s-addon: kube-dns.addons.k8s.io
  	                    	+     version: 1.6.1-alpha.2
  	                    	-     version: 1.5.1
  	                    	    - manifest: limit-range.addons.k8s.io/v1.5.0.yaml
  	                    	      name: limit-range.addons.k8s.io
  	                    	...
  	                    	        k8s-addon: limit-range.addons.k8s.io
  	                    	      version: 1.5.0
  	                    	+   - id: pre-k8s-1.6
  	                    	+     kubernetesVersion: <1.6.0
  	                    	+     manifest: dns-controller.addons.k8s.io/pre-k8s-1.6.yaml
  	                    	+     name: dns-controller.addons.k8s.io
  	                    	+     selector:
  	                    	+       k8s-addon: dns-controller.addons.k8s.io
  	                    	+     version: 1.6.1
  	                    	+   - id: k8s-1.6
  	                    	+     kubernetesVersion: '>=1.6.0'
  	                    	+     manifest: dns-controller.addons.k8s.io/k8s-1.6.yaml
  	                    	-   - manifest: dns-controller.addons.k8s.io/v1.5.1.yaml
  	                    	      name: dns-controller.addons.k8s.io
  	                    	      selector:
  	                    	        k8s-addon: dns-controller.addons.k8s.io
  	                    	+     version: 1.6.1
  	                    	-     version: 1.5.1
  	                    	+   - manifest: storage-aws.addons.k8s.io/v1.6.0.yaml
  	                    	-   - manifest: storage-aws.addons.k8s.io/v1.5.0.yaml
  	                    	      name: storage-aws.addons.k8s.io
  	                    	      selector:
  	                    	        k8s-addon: storage-aws.addons.k8s.io
  	                    	+     version: 1.6.0
  	                    	-     version: 1.5.0
  	                    	+   - id: pre-k8s-1.6
  	                    	+     kubernetesVersion: <1.6.0
  	                    	+     manifest: networking.weave/pre-k8s-1.6.yaml
  	                    	+     name: networking.weave
  	                    	+     selector:
  	                    	+       role.kubernetes.io/networking: "1"
  	                    	+     version: 1.9.8-kops.2
  	                    	+   - id: k8s-1.6
  	                    	+     kubernetesVersion: '>=1.6.0'
  	                    	+     manifest: networking.weave/k8s-1.6.yaml
  	                    	-   - manifest: networking.weave/v1.8.2.yaml
  	                    	      name: networking.weave
  	                    	      selector:
  	                    	        role.kubernetes.io/networking: "1"
  	                    	+     version: 1.9.8-kops.2
  	                    	-     version: 1.8.2


  Subnet/eu-west-1a.k8s.test.phennex.com
  	Tags                	 {KubernetesCluster: k8s.test.phennex.com, Name: eu-west-1a.k8s.test.phennex.com} -> {KubernetesCluster: k8s.test.phennex.com, Name: eu-west-1a.k8s.test.phennex.com, kubernetes.io/cluster/k8s.test.phennex.com: owned}

  Subnet/eu-west-1b.k8s.test.phennex.com
  	Tags                	 {KubernetesCluster: k8s.test.phennex.com, Name: eu-west-1b.k8s.test.phennex.com} -> {kubernetes.io/cluster/k8s.test.phennex.com: owned, KubernetesCluster: k8s.test.phennex.com, Name: eu-west-1b.k8s.test.phennex.com}

  Subnet/eu-west-1c.k8s.test.phennex.com
  	Tags                	 {KubernetesCluster: k8s.test.phennex.com, Name: eu-west-1c.k8s.test.phennex.com} -> {KubernetesCluster: k8s.test.phennex.com, Name: eu-west-1c.k8s.test.phennex.com, kubernetes.io/cluster/k8s.test.phennex.com: owned}

  Subnet/utility-eu-west-1a.k8s.test.phennex.com
  	Tags                	 {Name: utility-eu-west-1a.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com} -> {KubernetesCluster: k8s.test.phennex.com, Name: utility-eu-west-1a.k8s.test.phennex.com, kubernetes.io/cluster/k8s.test.phennex.com: owned}

  Subnet/utility-eu-west-1b.k8s.test.phennex.com
  	Tags                	 {KubernetesCluster: k8s.test.phennex.com, Name: utility-eu-west-1b.k8s.test.phennex.com} -> {Name: utility-eu-west-1b.k8s.test.phennex.com, kubernetes.io/cluster/k8s.test.phennex.com: owned, KubernetesCluster: k8s.test.phennex.com}

  Subnet/utility-eu-west-1c.k8s.test.phennex.com
  	Tags                	 {Name: utility-eu-west-1c.k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com} -> {KubernetesCluster: k8s.test.phennex.com, Name: utility-eu-west-1c.k8s.test.phennex.com, kubernetes.io/cluster/k8s.test.phennex.com: owned}

  VPC/k8s.test.phennex.com
  	Tags                	 {Name: k8s.test.phennex.com, KubernetesCluster: k8s.test.phennex.com} -> {KubernetesCluster: k8s.test.phennex.com, Name: k8s.test.phennex.com, kubernetes.io/cluster/k8s.test.phennex.com: owned}

Will delete items:
  SecurityGroupRule    sg-d762a7af: port=443 protocol=tcp group=sg-6c60a514
Must specify --yes to apply changes
```
That looks fine! Let's push this to the cloud!

Outputs the following:

```
$ kops update cluster k8s.test.phennex.com --yes
I0629 14:08:25.808470   16227 dns.go:91] Private DNS: skipping DNS validation
I0629 14:08:26.787541   16227 executor.go:91] Tasks: 0 done / 119 total; 41 can run
I0629 14:08:27.846731   16227 vfs_castore.go:422] Issuing new certificate: "kube-scheduler"
I0629 14:08:27.963209   16227 vfs_castore.go:422] Issuing new certificate: "kube-controller-manager"
I0629 14:08:27.996337   16227 vfs_castore.go:422] Issuing new certificate: "kubecfg"
I0629 14:08:28.003142   16227 vfs_castore.go:422] Issuing new certificate: "kube-proxy"
I0629 14:08:28.037994   16227 vfs_castore.go:422] Issuing new certificate: "kops"
I0629 14:08:28.067684   16227 vfs_castore.go:422] Issuing new certificate: "kubelet"
I0629 14:08:28.232018   16227 vfs_castore.go:422] Issuing new certificate: "master"
I0629 14:08:30.593750   16227 executor.go:91] Tasks: 41 done / 119 total; 24 can run
I0629 14:08:31.388001   16227 executor.go:91] Tasks: 65 done / 119 total; 37 can run
I0629 14:08:32.989339   16227 logging_retryer.go:59] Retryable error (Throttling: Rate exceeded
	status code: 400, request id: abd65d60-5cc3-11e7-858f-c5695e107912) from autoscaling/CreateLaunchConfiguration - will retry after delay of 756ms
I0629 14:08:33.284397   16227 logging_retryer.go:59] Retryable error (Throttling: Rate exceeded
	status code: 400, request id: abf186d6-5cc3-11e7-96d5-c9ffc7a01a7b) from autoscaling/CreateLaunchConfiguration - will retry after delay of 796ms
I0629 14:08:34.906887   16227 executor.go:91] Tasks: 102 done / 119 total; 10 can run
I0629 14:08:35.034462   16227 dnsname.go:108] AliasTarget for "bastion.k8s.test.phennex.com." is "bastion-k8s-48060442.eu-west-1.elb.amazonaws.com."
I0629 14:08:35.183942   16227 dnsname.go:108] AliasTarget for "api.k8s.test.phennex.com." is "api-k8s-920095612.eu-west-1.elb.amazonaws.com."
I0629 14:08:35.970019   16227 executor.go:91] Tasks: 112 done / 119 total; 7 can run
I0629 14:08:36.135950   16227 executor.go:91] Tasks: 119 done / 119 total; 0 can run
I0629 14:08:36.135991   16227 dns.go:152] Pre-creating DNS records
I0629 14:08:36.824130   16227 update_cluster.go:229] Exporting kubecfg for cluster
Kops has set your kubectl context to k8s.test.phennex.com

Cluster changes have been applied to the cloud.


Changes may require instances to restart: kops rolling-update cluster
```

## Rolling Update the cluster

Let's check if Kops things we should update:

```
$ kops rolling-update cluster
Using cluster from kubectl context: k8s.test.phennex.com

NAME			STATUS		NEEDUPDATE	READY	MIN	MAX	NODES
bastions		NeedsUpdate	1		0	1	1	0
master-eu-west-1a	NeedsUpdate	1		0	1	1	1
master-eu-west-1b	NeedsUpdate	1		0	1	1	1
master-eu-west-1c	NeedsUpdate	1		0	1	1	1
nodes			NeedsUpdate	3		0	3	3	3

Must specify --yes to rolling-update.
```

and of course we need to update! Let's execute:

```
$ kops rolling-update cluster --yes
Using cluster from kubectl context: k8s.test.phennex.com

NAME			STATUS		NEEDUPDATE	READY	MIN	MAX	NODES
bastions		NeedsUpdate	1		0	1	1	0
master-eu-west-1a	NeedsUpdate	1		0	1	1	1
master-eu-west-1b	NeedsUpdate	1		0	1	1	1
master-eu-west-1c	NeedsUpdate	1		0	1	1	1
nodes			NeedsUpdate	3		0	3	3	3
I0629 14:09:59.713756   16435 instancegroups.go:349] Stopping instance "i-0aaac2d1c87726df1", in AWS ASG "bastions.k8s.test.phennex.com".
I0629 14:09:59.974996   16435 instancegroups.go:255] Deleted a bastion instance, i-0aaac2d1c87726df1, and continuing with rolling-update.
I0629 14:09:59.975036   16435 instancegroups.go:347] Stopping instance "i-0d60be7540050b007", node "ip-172-20-51-232.eu-west-1.compute.internal", in AWS ASG "master-eu-west-1a.masters.k8s.test.phennex.com".
I0629 14:15:00.272958   16435 instancegroups.go:347] Stopping instance "i-056ccabdf4508e7fb", node "ip-172-20-73-196.eu-west-1.compute.internal", in AWS ASG "master-eu-west-1b.masters.k8s.test.phennex.com".
I0629 14:20:00.679622   16435 instancegroups.go:347] Stopping instance "i-01e09885a7df8041a", node "ip-172-20-116-20.eu-west-1.compute.internal", in AWS ASG "master-eu-west-1c.masters.k8s.test.phennex.com".
I0629 14:25:01.118656   16435 instancegroups.go:347] Stopping instance "i-000abbd80f1e828de", node "ip-172-20-38-130.eu-west-1.compute.internal", in AWS ASG "nodes.k8s.test.phennex.com".
I0629 14:27:01.555704   16435 instancegroups.go:347] Stopping instance "i-01546dd29f96938d0", node "ip-172-20-123-210.eu-west-1.compute.internal", in AWS ASG "nodes.k8s.test.phennex.com".
I0629 14:29:02.159736   16435 instancegroups.go:347] Stopping instance "i-0cc77c5db3b8d4771", node "ip-172-20-83-179.eu-west-1.compute.internal", in AWS ASG "nodes.k8s.test.phennex.com".
I0629 14:31:02.556227   16435 rollingupdate.go:174] Rolling update completed!
```

Rolling-update with different versions of the masters - potential problem?:
```
NAME                                           STATUS         AGE       VERSION
ip-172-20-116-20.eu-west-1.compute.internal    Ready,master   24m       v1.5.2
ip-172-20-123-210.eu-west-1.compute.internal   Ready          23m       v1.5.2
ip-172-20-38-130.eu-west-1.compute.internal    Ready          24m       v1.5.2
ip-172-20-60-13.eu-west-1.compute.internal     Ready,master   3m        v1.6.2
ip-172-20-73-196.eu-west-1.compute.internal    Ready,master   25m       v1.5.2
ip-172-20-83-179.eu-west-1.compute.internal    Ready          24m       v1.5.2
```

Anyway, didn't seem to be a problem, everything started as it should:

```
$ kubectl get nodes
NAME                                          STATUS         AGE       VERSION
ip-172-20-106-50.eu-west-1.compute.internal   Ready,node     4m        v1.6.2
ip-172-20-120-61.eu-west-1.compute.internal   Ready,master   10m       v1.6.2
ip-172-20-38-107.eu-west-1.compute.internal   Ready,node     6m        v1.6.2
ip-172-20-60-13.eu-west-1.compute.internal    Ready,master   21m       v1.6.2
ip-172-20-76-217.eu-west-1.compute.internal   Ready,node     2m        v1.6.2
ip-172-20-93-56.eu-west-1.compute.internal    Ready,master   16m       v1.6.2
```
