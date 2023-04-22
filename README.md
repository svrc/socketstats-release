# Socket stats gatherer

## What does this do?

Dumps socket stats for certain processes  onto the node.  Then a DaemonSet is available for reading these logs.    This is not usable for most in its current form, but could be modified for your needs.    Ideally I'll turn this into a Prometheus exporter.

## How do I install it?

1. Open a shell prompt on a BOSH CLI with access to your TKGI bosh director, such as Ops Manager.
2. Export your BOSH credentials to the enviornment.  These can be accessed via the Ops Manager GUI -> BOSH Director Tile -> Credentials Tab -> Bosh Commandline Credentials.    

e.g.
```
export BOSH_CLIENT=ops_manager BOSH_CLIENT_SECRET=fakesecret BOSH_CA_CERT=/var/tempest/workspaces/default/root_ca_certificate  BOSH_ENVIRONMENT=10.0.0.10
```
3. Copy or clone this repository onto this BOSH CLI workstation and create+upload the BOSH release to the director

```
git clone https://github.com/svrc/socketstats-release && cd socketstats-release
bosh create-release
bosh upload-release ./dev_releases/socketstats/socketstats-0+dev.1.yml 

```
4. Configure the addon from this repo
```
bosh -n update-config --name=socketstats-release --type=runtime ./socketstats.yaml
```
5. Update your TKGI clusters via the PKS CLI and/or Ops Manager "Apply Pending Changes" button with the TKGI upgrade errand enabled.  This addon will automatically be installed on all worker nodes with the default manifest `pks-watch-tl-manifest.yml`

6. Apply the Daemonset `ds.yaml` to your cluster as one means of reading these logs.

