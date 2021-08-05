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



## Confirmar conectividad entre los nodos

Se observa los logs en el nodo master donde pasa de estatus a YELLOW a GREEN

```
$ kubectl logs -f $(kubectl get pods | grep elastic-master | sed -n 1p | awk '{print $1}') | grep "Cluster health status changed from \[YELLOW\] to \[GREEN\]"
```

## Generación de Contraseñas

Se ingresa al nodo master de modo interactivo de la siguienta manera:

```
$ kubectl exec -it [nombrePOD] -- /bin/bash
```

Entrar a la siguiente carpeta __"/usr/share/elasticsearch/bin"__ y ejecutar el comando:
```
$ elasticsearch-setup-passwords auto -b
```
Observar la respuesta y guardar las contraseñas para cada usuario

## Generar Secret

Con la contraseña del usuario "elastic" la vamos a guardar en una variable llamada Secret usando el siguiente ejemplo:
```
$ kubectl create secret generic elasticsearch-pw-elastic --from-literal password=WbWIkphCVHO3kL45PAEI
```