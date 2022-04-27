# Umdenken in der Administration 

## Vorher (old-school) - Imperativ

  * Ich setze Server auf
  * Auf dem Server läuft eine Anwendung 

## Jetzt (Kubernetes) - Declarative 

  * Ich definiere, wieviele Nginx - Server laufen sollen (Beispiel) 
  * Überlasse Kubernetes, wo diese laufen
  * Kubernetes entscheidend anhand der Ressourcen 
  * Ich kann aber contraints festlegen (d.h. ich sage, nur in bestimmten Rechenzentrum) 

## Was ist anders ? 

  * Ich weiss nicht genau, auf welchem node ein container(pod) läuft

## Was ist die Konsequenz 

  * Logs müssen anders ausgewertet werden (Logs sammeln) 
    * innerhalb der Container 
    * cluster-wide 
    * einzelne Nodes 
  * Backup (Wie lasse ich backups laufen bzw. führe diese durch) 
    * Kubernetes aware backup (solution), e.g. kasten.io 
  * Havarie (wie setze ich das ganze wieder auf - worst case)   

## Wo muss mich strecken als Admin 

  * Aufbau von Vertrauen auf Kubernetes (Vertrauen darauf, dass Kubernetes das in meinem Sinne macht)

## Sicherheitsaspekt (Server vs. Kubernetes) 

  * Komplexität und Durchschaubarkeit steigt, weil
    * kann keine einfachen Firewall - Regeln mehr machen 
    * Was macht Kubernetes auf ? (Port)
    * Läuft vielleicht ein Ingress-Objekct, was mein System aufmacht (ungewollt) 


