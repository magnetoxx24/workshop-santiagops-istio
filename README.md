WorkShop Managing Microservices With Istio Service Mesh

Istio es una plataforma abierta para conectar, asegurar, controlar y observar microservicios, también conocido como malla de servicio, en plataformas en la nube como Kubernetes.

Con Istio, puede administrar el tráfico de red, equilibrar la carga entre microservicios, aplicar políticas de acceso,verificar la identidad del servicio, asegurar la comunicación del servicio y observar exactamente qué sucede con sus servicios.

Requisitos

- Docker
- Kubernetes
- 3 MV con 8GB RAM, 4 CPU
- Conocimientos de Kubernetes

Objetivos
Después de completar este curso, podrás:

- Descargue e instale Istio en su clúster
- Implemente la aplicación de muestra Libro de visitas
- Utilice métricas, registros y rastreo para observar los servicios.
- Configurar la puerta de entrada de Istio
- Realice una gestión de tráfico simple, como pruebas A / B e implementaciones canarias
- Asegure su malla de servicio
- Aplicar políticas para sus microservicios

Workshop
Realizará los siguientes ejercicios en el laboratorio:

Infraestructura requerida para implementar la Malla de Servicio para el Despliegue y Gestión de Microservicios

- Cluster Kubernetes
- Servidor de Métricas
- Docker
- Kubectl
- Istio
- Nginx

Diagrama Infraestructura
![Diagrama de la Infraestructura Cluster Kubernetes con Malla de Servicio Istio](https://github.com/magnetoxx24/workshop-santiagops-istio/blob/master/imagenes/Diagrama.png)

Presentación de Gestión y Despliegue Microservicios Usando la Malla de Servicio Istio.

Instalación de Istio en el Cluster de Kubernetes

Descargar el repositorio del Workshop Istio Santiago's
https://github.com/magnetoxx24/workshop-santiagops-istio.git

Comandos para Desplegar y Gestionar Istio.

```$ cd santiagops/istio-1.4.3/install/kubernetes```

```$ kubectl apply -f istio-demo.yaml```

```$ kubectl get pods -n istio-system```

```$ kubectl get po,svc -n istio-system```

```$ kubectl get namespace -L istio-injection```

```$ kubectl label namespace santiagops istio-injection=enabled```

Despliegue de Microservicio BookInfo

```$ kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml```

```$ kubectl get services```

```$ kubectl get pods```

```$ kubectl exec -it $(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}') -c ratings -- curl productpage:9080/productpage | grep -o "<title>.\*</title>"```

<title>Simple Bookstore App</title>

```$ kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml```

``` $ kubectl get gateway```

```NAME AGE bookinfo-gateway 32s```

```$ curl -s http://${GATEWAY_URL}/productpage | grep -o "<title>.\*</title>"```

```<title>Simple Bookstore App</title> ```

```$ kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml```

```$ kubectl get destinationrules -o yaml```

Visualización de Metricas de Recursos en el Cluster de Kubernetes

Comandos para visualizar las métricas de los nodos del cluster de Kubernetes

```$ kubectl top nodes ```

Comando para visualizar las métricas de los nodos y los pods que están en ejecución con sus recursos que actualmente están consumiendo.

```$ kubectl describe nodes -o wide```

Comando para visualizar todos los pods de mayor consumo de recursos del cluster de Kubernetes
```$ kubectl top pods --all-namespaces```

Comando para visualizar el total de recursos por nodos que esta consumiendo los recursos activos

```$ kubectl get nodes --no-headers | awk '{print \$1}' | xargs -I {} sh -c 'echo {}; kubectl describe node {} | grep Allocated -A 5 | grep -ve Event -ve Allocated -ve percent -ve -- ; echo' ```

Comando para ver los nodos por role definidos dentro del cluster

```$ kubectl get node --selector='!node-role.kubernetes.io/node' -o wide```

Comando para visualizar los Namespaces creado dentro del cluster
```$ kubectl get namespaces```

Comandos para crear un nuevo Namespaces
```$ kubectl create ns santiagops```

Comando para visualizar los Namespaces y estatus de Injection Automatica de Istio
```$ kubectl get namespace -L istio-injection```

Comando para habilitar la Injection Manual del Sidecard de Istio
```$ kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/platform/kube/bookinfo.yaml)```

Comando para activar la injection automatica de Istio a un Namespaces especificado
\$ kubectl label namespace santiagops istio-injection=enabled

Cambiar el namespaces por default

```$ kubectl config set-context --current —namespace=santiagops```

Guarde permanentemente el espacio de nombres para todos los comandos kubectl posteriores en ese contexto.

```$ kubectl config set-context --current —namespace=santiagops```

```$ kubectl config view | grep namespace```

