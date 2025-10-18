# 3w5dChallenge

## GitOps를 통한 Argocd 환경 구축

**상황** 
1. apps/main-app 폴더에 Apllication 서버 존재
2. Argocd 설정 및 오류 yaml 파일 수정


```yaml
kubectl get namespace argocd
------
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
----
#기본 namespace 설정
kubectl config set-context --current --namespace=day5-challenge
    
```


```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: challenge-app-1
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/invalid-user/k8s-gitops-demo.git
    targetRevision: main
    path: apps/demo-app
  destination:
    server: https://kubernetes.default.svc
    namespace: day5-challenge
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
    - CreateNamespace=true
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: challenge-app-2
  namespace: day5-challenge
spec:
  replicas: 3
  selector:
    matchLabels:
      app: challenge-app-2
  template:
    metadata:
      labels:
        app: challenge-app-2
    spec:
      containers:
      - name: nginx
        image: nginxinc/nginx-unprivileged:1.21
        ports:
        - containerPort: 8080
        ## 들여쓰기 오류발생
        resources:
          requests:
            cpu: 100m
```
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: challenge-app-3
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/xaczxzz/3w3dchallenge.git
    targetRevision: develop
    path: apps/demo-app
  destination:
    server: https://kubernetes.default.svc
    namespace: day5-challenge
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: challenge-app-4
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/xaczxzz/3w3dchallenge.git
    targetRevision: main
    path: apps/demo-app
  destination:
    server: https://kubernetes.default.svc
    namespace: day5-challenge
  syncPolicy:
    syncOptions:
    - CreateNamespace=true
```