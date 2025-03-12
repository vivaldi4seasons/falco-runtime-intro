# falco-runtime-intro


## Goal

Demonstrate the detection capabilities of Falco Runtime Security through two common data exfiltration scenarios:

- Information Exfiltration via SSH Tunneling in Kubernetes.

- Data Exfiltration through Memory Dump.

The goal is to showcase how Falco can identify and alert on suspicious behaviors in both Kubernetes and cloud environments, focusing on runtime threats and data breach attempts.



## Requirements

- A Kubernetes cluster with at least one running pod.

- A cloud instance with Docker and Docker Compose installed.


## Installation

    kubectl run -i --tty --image vivaldi4seasons/falco-runtime-intro:0.0.1 ubuntu

    kubectl apply -f eks-pod-deployment.yaml

    kubectl delete -f eks-pod-deployment.yaml

