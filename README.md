referred to https://cybozu.github.io/introduction-to-kubernetes/

立てたpodの中に入る
```
kubectl exec -it my-first-pod -- bash
```
my-first-podがname


IP調べる
```
[~/myapp/k8s]  % kubectl get pod -o wide
NAME           READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
bastion        1/1     Running   0          28s     10.244.0.6   minikube   <none>           <none>
my-first-pod   1/1     Running   0          4m35s   10.244.0.5   minikube   <none>           <none>
```

調べたIPで別のpodにcurlを実行する
```
root@bastion:/# curl -i http://10.244.0.5
HTTP/1.1 200 OK
Server: nginx/1.25.1
Date: Sun, 18 Jun 2023 14:06:18 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Tue, 13 Jun 2023 15:08:10 GMT
Connection: keep-alive
ETag: "6488865a-267"
Accept-Ranges: bytes
```
curlがない場合はインストールする
```
apt update && apt install -y curl
```

replicasetの状態
```
[~/myapp/k8s]  % kubectl get replicaset
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   3         3         3       25s
```


特定のpod削除
```
kubectl delete pod my-first-pod
```

特定のreplicaset削除
```
kubectl delete replicaset nginx-replicaset
```

podのimageを表示する
```
[~/myapp/k8s]  % kubectl get pod -o 'custom-columns=NAME:.metadata.name,IMAGE:.spec.containers[*].image,PHASE:.status.phase'
NAME                                IMAGE           PHASE
bastion                             debian:stable   Running
nginx-deployment-78f688f5db-h9g2w   nginx:1.20      Running
nginx-deployment-78f688f5db-xs9v8   nginx:1.20      Running
nginx-deployment-78f688f5db-zvwtj   nginx:1.20      Running
```

podのログを確認
```
[~/myapp/k8s]  % kubectl logs nginx-deployment-65f6f979cc-hf6ng
```

ラベル単位でログを確認
```
[~/myapp/k8s]  % kubectl logs -l component=nginx
```
