# Elasticsearch en K8s Bare Metal

## Nodos Elasticsearch

- Master.- Su rol principal es tener la licencia y donde se deberán conectar el resto de los nodos para la administración de los mismos, se implementa en K8s como un deployment.

- Data.- El rola de este nodo es donde se guardara la información que se inyecta desde el nodo con el rol "Ingest", sus datos son persistentes por lo que se implementa como StatefulSet.

- Ingest.- La función de este nodo es recibir la conectividad desde logstash así como consultas de Kibana, por el puerto 9200.

Nota: La comunicación entre los nodos descritos se realiza usando el puerto 9300.
      La instalación se hace en el namespace por default.

## Nodo Master

Para levantar el primero nodo (Configmap, service & Deployment) ejecutamos el siguiente comando:
```
$ kubectl apply -f 1000_elastic_master.yaml 
```

Observamos que el pod este como "running"
```
$ watch kubectl get pods
```
##### (Se oprime Ctrl+C para regresar al CLI nuevamente)


## Nodo Data

Para levantar el primero nodo (Configmap, service & StatefulSet) ejecutamos el siguiente comando:
```
$ kubectl apply -f 1001_elastic_data.yaml 
```

Observamos que el pod este como "running"
```
$ watch kubectl get pods
```
(Se oprime Ctrl+C para regresar al CLI nuevamente)




## Nodo Ingest

Para levantar el primero nodo (Configmap, service & StatefulSet) ejecutamos el siguiente comando:
```
$ kubectl apply -f 1002_elastic_ingest.yaml 
```

Observamos que el pod este como "running"
```
$ watch kubectl get pods
```
(Se oprime Ctrl+C para regresar al CLI nuevamente)