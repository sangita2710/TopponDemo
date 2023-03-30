# Scenario 2

# Deliverables:

1. Make use of scenario 1 code and display your <your first name> using an
environment variable supplied to the container

 kubectl exec my-pod -- env | grep -E "MY_NAME"

Output: 

MY_NAME=Sangeetha Palanisamy

2a.Ensure that the Incremental and Decremental Counter is able to connect to Mysql

kubectl exec -it frontend-c999dbdfd-hbb6t -- bash
echo "10.1.0.115" >> /etc/hosts
curl http://10.1.0.115:3306
exit

Output: 

curl: (1) Received HTTP/0.9 when not allowed

2b.

kubectl exec -it mysql-5d84d6fd57-tvxxt
mysql -u root -p$(kubectl get secret db-secret -o jsonpath="{.data.db-root-password}" | base64 --decode)

mysql> CREATE DATABASE mydb;
Query OK, 1 row affected (0.01 sec)

mysql> USE mydb;
Database changed

mysql> CREATE TABLE mytable (id INT PRIMARY KEY, name VARCHAR(255));
Query OK, 0 rows affected (0.02 sec)

4. Monitor the resources using Prometheus and Grafana

Step 1: install chart 

helm install prometheus prometheus-community/kube-prometheus-stack 

Step 2: kubectl port-forward deployment/prometheus-grafana 3000

output:
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000

kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0 9090

output:
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090


Step 4:Deploy the Prometheus MySQL Exporter 

Refer mysql-exporter.yaml

Step 5: Configure Prometheus
 - configure Prometheus to scrape metrics from the MySQL Exporter
 - Add mysql job to prometheus.yml file.

   - job_name: 'mysql'
     scrape_interval: 30s
     static_configs:
        - targets: ['mysql-exporter:9104']
This configuration tells Prometheus to scrape metrics from the MySQL Exporter at the address mysql-exporter:9104

Step 6:  Adding a new Prometheus data source and create dashboards to display the MySQL metrics


File included are:
a)  db-secret.yaml: This file contains contain the sensitive details for the MySQL database, such as the root user's username and password
b)  frontend-deploy,mysql-deploy.yaml: These files contain the definition for a Kubernetes Deployment object, which specifies the desired state of the frontend  and the mysql application
c)  mysql-service.yaml,frontend-service.yaml: This file contains file defines a service for the MySQL deployment and a service for the frontend deployment.
d) mysql-pvc.yaml:  This file contains the definition of a Kubernetes PersistentVolumeClaim (PVC) object that is used to claim a certain amount of storage from a storage class for use by the MySQL database.
f) my-role.yaml, my-role-binding.yaml : These files are used to create a Kubernetes Role and RoleBinding, respectively, for granting permissions to access Kubernetes resources.
g) mysql-exporter.yaml: This file contain the deployment of mysql-exporter

3. troubleshooting commands for Kubernetes:

kubectl get pods: Lists all the running pods in the cluster.

kubectl describe pod <pod-name>: Provides detailed information about a specific pod, including its current status, events, and logs.

kubectl logs <pod-name>: Displays the logs for a specific pod.

kubectl get services: Lists all the services in the cluster.

kubectl describe service <service-name>: Provides detailed information about a specific service, including its IP address, port, and endpoints.

kubectl get nodes: Lists all the nodes in the cluster.

kubectl describe node <node-name>: Provides detailed information about a specific node, including its IP address, CPU and memory usage, and the pods running on it.

kubectl get events: Lists all the events in the cluster, such as pod creation or deletion, node failures, and service changes.

kubectl exec -it <pod-name> -- <command>: Executes a command inside a specific pod, allowing you to troubleshoot issues with the application or its dependencies.

kubectl delete pod <pod-name>: Deletes a pod, allowing you to restart it and potentially resolve any issues that may be causing it to fail.

These commands can be helpful in troubleshooting various issues in a Kubernetes cluster, including pod failures, networking problems, and resource constraints.




