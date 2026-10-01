# Network Load Balancing and IIS

## Objective

Practice using Windows Network Load Balancing with two IIS web servers.

The goal was to install IIS and NLB on two server VMs, create an NLB cluster, add both hosts, and configure HTTP traffic on port `80`.

## Lab Environment

- VMware-based Windows Server lab.
- Management server shown in the script: `S1`.
- NLB nodes shown in the screenshots: `S2` and `S3`.
- Cluster name: `NLB.Ahmed.edu`.
- Cluster IP: `10.0.0.100`.
- Node IPs visible in the script/screenshots: `10.0.0.2` and `10.0.0.3`.
- Tools used: Server Manager, PowerShell ISE, and Network Load Balancing Manager.

## Implementation

### Install IIS and NLB features

I used PowerShell to define the management server and the two NLB nodes. The script installs the Web Server and NLB features on both nodes and imports the NLB module.

![NLB and IIS install script](<screenshots/2026-10-01/NLBInstallationinserversandIIS.png>)

### Create the NLB cluster

I created a new NLB cluster with the cluster IP `10.0.0.100` and the full name `NLB.Ahmed.edu`. The screenshot shows the cluster wizard using unicast mode.

![Creating NLB cluster](<screenshots/2026-09-28/CreatingAclusterForNetworkLoadBalancing.png>)

### Configure the HTTP port rule

I edited the port rule for HTTP traffic. The rule is for TCP port `80`, using multiple-host filtering mode.

![HTTP port rule](<screenshots/2026-09-28/EditedPortrulesforwebandhttpandtcp.png>)

### Add both servers to the cluster

I added `S2` and `S3` to the cluster. The NLB Manager screenshot shows `S2` and `S3` under `NLB.Ahmed.edu (10.0.0.100)`. One host is still converging in the screenshot and the other is already converged.

My filename note says I split the load as `60%` for `S2` and `40%` for `S3`. The visible part of the screenshot mainly proves that both hosts were added to the cluster, so I still need a cleaner screenshot for the exact load weights.

![NLB nodes added](<screenshots/2026-09-28/Hereiaddedthetwoserversandwecanseetheyareupandconvergingisplittheworkas60percentforS2and40%forS3.png>)

## Commands and Scripts

Commands visible in the PowerShell screenshot include:

```powershell
$ManagementServer = "S1"
$Node1IP = "10.0.0.2"
$Node2IP = "10.0.0.3"
$Nodes = @("S2", "S3")

foreach ($Node in $Nodes) {
    Install-WindowsFeature -ComputerName $Node -Name Web-Server, NLB -IncludeManagementTools
}

Import-Module NetworkLoadBalancingClusters
```

The script file itself is not committed yet. Right now this is documented from the screenshot.

## Verification and Testing

- Network Load Balancing Manager showed the cluster name and cluster IP.
- The port rule was set for TCP `80`.
- `S2` and `S3` were visible under the same NLB cluster.
- The host state showed convergence/converged status while the cluster updated.

## Results

The screenshots show the NLB cluster build process for two IIS nodes. The next thing I need is a clean browser test against `http://NLB.Ahmed.edu` or `http://10.0.0.100` to prove client-side access through the cluster.

## Lessons Learned

- NLB setup is not just installing the feature; the cluster IP, port rules, host membership, and convergence state all matter.
- IIS and NLB can be installed remotely with PowerShell, which is faster than doing each server by hand.
- A clean verification screenshot is important because the NLB Manager view can hide useful columns like weight/priority.

## Documentation TODO

- Add the actual NLB PowerShell script.
- Add browser testing against the cluster name/IP.
- Add `Get-NlbCluster`, `Get-NlbClusterNode`, and `Get-NlbClusterPortRule` output.
- Add a clearer screenshot showing the 60/40 load split.
