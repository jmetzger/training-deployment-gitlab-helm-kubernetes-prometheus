# PodAffinity und PodAntiAffinity 

## PodAffinity - Was ist das ? 

```
Situation: Wir wissen nicht, auf welchem Node ein Pod läuft,
aber wir wollen sicherstellen das ein "verwandter Pod" auf dem gleichen
Node läuft.

Es ist auch denkbar für:
o im gleichen Rechenzentrum 
o im gleichen Rack etc.


```

## PodAntiAffinity - Was ist das ? 

```
Das genaue Gegenteil zu PodAffinity:
Ein weitere Pod B, soll eben gerade nicht dort laufen wo Pod A 
läuft.
Im einfachsten Fall gerade nicht auf dem gleichen Host/Node 
```

## PodAntiAffinity - auch möglich für pods mit gleichem Label 

```
# Spezialfall: Ich möchte sicherstellen, des jeder meiner Pods mit gleich label
# auf einem anderen Node/Host 
```
