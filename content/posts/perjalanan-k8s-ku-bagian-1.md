---
title: "Perjalanan K8s Ku (bagian 1)"
date: 2022-09-04T22:33:39+07:00
catogories:
  - kubernetes
tags:
  - kubernetes
  - k8s
  - kubectl
---

Ah, kubernetes, akhirnya...  
Setelah beberapa waktu yang lalu masih mainan arus deras dengan docker-swarm, kini tiba saatnya untuk menjelajahi lautan luas dengan kubernetes.  

Perkakas yang digunakan untuk menyelami luasnya lautan di kubernetes adalah kubectl (saya kubu pembaca: `ku-be-ce-te-el` :) )

Berikut adalah yang sudah saya pelajari

**Daftar Resource:**  
- node                  -> list node yang tergabung dalam cluster kubernetes  
- pod                   -> unit terkecil di k8s  
- deployment            -> terdiri dari banyak pod  
- daemonset             -> terdiri dari banyak pod yang auto tersebar masing-masing 1 pod di tiap node  
- service               -> sebagai loadbalancer dari beberapa pod yang ada  
- ingress               -> sebagai gerbang masuk dari eksternal host/path menuju internal service-service di dalam kubernetes  
- statefulset  
- job  
- cronjob  
- serviceaccount  
- role  
- rolebinding  
- clusterrole  
- clusterrolebinding  

## Add Resources
Sebelum kita bikin resource baru, alangkah baiknya kalau dicek terlebih dahulu apakah sintaksnya sudah
benar atau belum dengan menambahkan flag `--dry-run=client` di akhir command. Jika kita bikin resource
dengan cara ngecer, misal bikin 1 pod pakai kubectl run, dan ingin menyimpan yaml nya untuk disunting di lain
waktu, gunakan flag `-o yaml`

```bash
# pod
kubectl run nama_pod -n nama_namespace --image=nginx:1.19
# pod (dry-run) ---> tidak akan benar-benar dibikin, tapi cuma simulasi apakah nanti sukses atau tidak
kubectl run nama_pod -n nama_namespace --image=nginx:1.19 --dry-run=client
# pod (dry-run dan disimpan dalam yaml)
kubectl run nama_pod -n nama_namespace --image=nginx:1.19 --dry-run=client -o yaml > nama_pod.yaml
# deployment dengan pod memiliki 2 container
kubectl create deployment nama_deployment -n nama_namespace --image=nginx:1.19 --image=busybox:3.25
# expose deployment sebagai service
kubectl expose deployment nama_deployment -n nama_namespace --name nama_service
# expose pod sebagai service
kubectl expose pod nama_pod -n nama_namespace --name nama_service
# serviceaccount
kubectl create serviceaccount nama_service_account -n nama_namespace

# membuat resource dari file
kubectl apply -f nama_file.yaml
```

## Modify Resources
```bash
kubectl edit nama_resource -n nama_namespace
```

## Get Resources
```bash
# format:
kubectl get nama_resource -n nama_namespace nama_objek

# list all
kubectl get all -n nama_namespace
# list semua pod yang ada di 1 namespace dengan informasi yang diperluas
kubectl get pod -n nama_namespace -o wide
# list semua pod semua namespace dengan informasi yang diperluas
kubectl get pod -A -o wide

kubectl describe nama_resource -n nama_namespace nama_objek
```

## Log tracing
```bash
# melihat event yang terjadi
kubectl get events -n nama_namespace

# melihat status pod  dan lokasi node nya
kubectl get pod -n nama_namespace -o wide | grep katakunci

# melihat log pod/deployment/service
kubectl logs -n nama_namespace pod/nama_pod --tail=100 --timestamps
kubectl logs -n nama_namespace deployment/nama_pod --tail=100 --timestamps
kubectl logs -n nama_namespace service/nama_pod --tail=100 --timestamps
# berdasar label app=testing (ambil dari describe bagian label)
kubectl logs -n nama_namespace -l app=testing --tail=100 --timestamps
```

## Delete Resources
```bash
# format
kubectl delete nama_resource -n nama_namespace nama_objek

# contoh:
# pod
kubectl delete pod -n nama_namespace nama_pod
# deployment
kubectl delete deployment -n nama_namespace nama_deployment
# service
kubectl delete service -n nama_namespace nama_deployment

# delete resource dari file
kubectl delete -f nama_file.yaml
```
### Delete Evicted pods
```bash
pods=$(kubectl get pod -A | grep Evicted | awk '{print $1,$2}')
for delpod in $pods
do
    ns=$(echo $delpod | cut -d ',' -f 1)
    p=$(echo $delpod | cut -d ',' -f 2)
    kubectl -n $ns delete pod $p
done
```

## Node Operations
### Taint dan Untaint
Taint untuk membatasi node untuk tidak melakukan aksi tertentu jika
pod tidak memiliki toleransi pada node yang memiliki label_node tertentu.  
Untaint untuk melepaskan taint pada node.  
Daftar Taint:
- NoSchedule
- NoExecute
- ???

```bash
# taint
kubectl taint node nama_node label_node:NoSchedule

# untaint (tambahkan dash di belakang)
kubectl taint node nama_node label_node:NoSchedule-
```

### Cordon/Drain dan Uncordon
Cordon berarti tidak akan ada scheduling baru pada node. Ini berbeda dengan drain,
kalau kita drain, maka semua pod akan ditendang dari node yang di-drain, setelah itu,
node tidak akan menerima scheduling pod.  
;tldr:  
- drain  = tendang semua pod existing di node + set noschedule pod baru  
- cordon = set noschedule pod baru  

```bash
# cordon
kubectl cordon nama_node
# uncordon
kubectl uncordon nama_node

# drain
kubectl drain nama_node
# undrain (pakai uncordon, tidak ada perintah undrain)
kubectl uncordon nama_node
```

## MISC
### cek mtu semua pod dalam satu cluster
```bash
PODS=$(kubectl get pod -A --no-headers --template '{{ range .items }}{{ printf "%s,%s\n" .metadata.namespace .metadata.name }}{{ end }}')
for pod in ${PODS[@]}
do
    ns=$(echo $pod | cut -d ',' -f 1)
    p=$(echo $pod | cut -d ',' -f 2)
    echo -n "$ns/$p mtu: "
    kubectl -n $ns exec $p -- cat /sys/class/net/eth0/mtu 
done
```
