# Bookinfo Demo Application CP4MCM/RHACM Operators

## Prerequisites
1. Create the `bookinfo-manager` namespace on the Hub cluster
2. Create an ImagePolicy or ClusterImagePolicy to allow images from `quay.io/zach_robinson*`

## Install
1. Clone the repo
2. \[Optional\] Modify the [BookInfoManager CR](hub-operator/bookinfo-hub-operator/deploy/crds/demo.cp4mcm.ibm.com_v1alpha1_bookinfomanager_cr.yaml) to set the placement rules and BookInfo spec to desired values
3. Deploy the Hub-Operator, CRD, and BookInfoManager CR to the Hub cluster using this command:
    ```
    oc apply -f hub-operator/bookinfo-hub-operator/deploy/ -R
    ```
   
 ## Troubleshooting
 - Sometimes the BookInfoManager CR will not get deployed to the Hub Cluster due to a timing issue since the CRD needs to be registered first. If this happens, just run the command from step 3 again
 - If the bookinfo-klusterlet-operator gets installed to the managed-cluster, but the corresponding BookInfo CR does not, try restarting the CP4MCM-Appmgr in the `multicluster-endpoint` namespace on the managed-cluster