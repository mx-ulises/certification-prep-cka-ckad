# Run and fixes vulnerabilities with kube-bench

1. Run kube-bench as a Pod
    ````
    kubectl apply -f https://raw.githubusercontent.com/aquasecurity/kube-bench/main/job.yaml
    ````
1. Review logs
    ```
    # Read the whole log
    kubectl logs kube-bench-bzplf | less

    # Grep fails only
    kubectl logs kube-bench-bzplf | grep FAIL | less
    ```
