# Tools zum Aufsetzen eines Clusters 

## Hintergrund 

  * Um ein Cluster einzurichten, muss ich mich für ein Tool entscheiden.

## kubeadm

  * Most vanilla - Tool (d.h. am komplexesten)
  * Das erste Tool, dass es zum Einrichten eines Clusters gab.

## microk8s (ubuntu) 

  * Einfach zu bedienen
  * Reines commandline - tool (microk8s status) 
  * Bietet plugins, die bestimmte features an und abschalten (Ingress,Metrics-Server) (microk8s enable ingress) 
    * Dieses Features, die im Hintergrund manifeste ausführen, können so einfach konfiguriert werden 
  * Es läßt sich sehr einfach ein Cluster aufbauen (3 Server hochziehen mit microk8s und verheiraten (node2, node3) 

## Rancher 

  * Von der 3 Varianten das komfortabelste Tool
  * Bietet einen gui um ein Cluster einzurichten 

