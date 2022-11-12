# commento-11-05

[operator-sdk go](https://sdk.operatorframework.io/docs/building-operators/golang/tutorial/)
[helm으로 구축시 'crds' 폴더 생성후 crd파일을 넣어줘야함](https://github.com/kedacore/charts/issues/226)

```
brew install operator-sdk

# cluter admin 자격 있는지 확인
% kubectl auth can-i create secrets
% yes
% kubectl auth can-i create deployments
% yes

# operator 생성
operator-sdk init --domain example.com --repo github.com/hkpark130/commento-11-05 --plugins go/v4-alpha

# controller 생성
operator-sdk create api --group stable --version v1 --kind MyCr --resource --controller

# CRD manifests 생성
make manifests

# build, push operator docker image
docker buildx build --platform linux/amd64 -t k8s-operator .
docker tag k8s-operator asia-northeast3-docker.pkg.dev/k8s-project-365610/gar/k8s-operator
docker push asia-northeast3-docker.pkg.dev/k8s-project-365610/gar/k8s-operator
# # make docker-build docker-push IMG=asia-northeast3-docker.pkg.dev/k8s-project-365610/gar/k8s-operator:latest # (M1 에서는 안 됨)

# Deploy the controller to the cluster
make deploy IMG=asia-northeast3-docker.pkg.dev/k8s-project-365610/gar/k8s-operator:latest

# Define & Install CRD on K8S Cluster
make generate
make install


# To delete the CRDs from the cluster:
make uninstall

# UnDeploy the controller to the cluster:
make undeploy

```

------------------------

```
메모
api/{group}/{CR}_types.go: 여기서 crd를 정의 해야함


kubectl apply -f config/samples/role.yaml -n commento-11-05-system
kubectl apply -f comment-charts/templates/mycr.yaml -n commento-11-05-system


```

