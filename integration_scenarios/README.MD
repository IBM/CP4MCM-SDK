## Interacting with MCM cluster remotely via CLI (cloudctl)
When implementing integration scenarios with MCM you might be required to remotely connect to MCM via commandline (cloudctl). You can use cloudctl to view information about your cluster, manage your cluster, install Helm charts and workloads, and more.
Detail info on cloudctl can be found here: https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.1/manage_cluster/icp_cli.html

### Logging into MCM via cloudctl
`cloudctl login -a https://<cluster host>:<port> -u <user> -p <pwd> -n kube-system`

Once you are logged in, you can run kubectl, cloudctl, helm and oc commands remotely. (assuming you have installed those tools locally)
