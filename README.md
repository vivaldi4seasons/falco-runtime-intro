# falco-runtime-intro


## Goal

Esto es similar a ngrok.
Seria como para bypassear un puerto.




## Configuration

    kubectl run -i --tty --image vivaldi4seasons/falco-runtime-intro:0.0.1 ubuntu

    kubectl apply -f eks-pod-deployment.yaml

    kubectl delete -f eks-pod-deployment.yaml


### Findings

✅ Métodos recomendados (seguros)
1️⃣ Montar la llave privada en tiempo de ejecución (más seguro)
En lugar de incluir la llave en la imagen, móntala como un volumen cuando inicies el contenedor:

### Opportunities

# Establecer el nombre del host
ENV HOSTNAME=falco-runtime-intro
