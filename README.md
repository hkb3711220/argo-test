# Argo CD検証用のブランチ

### レポジトリ追加
```
GITHUB_USER=hkb3711220
GITHUB_TOKEN=
GITHUB_REPO="https://github.com/hkb3711220/argo_test.git"

argocd repo add ${GITHUB_REPO} --name argo_test --username ${GITHUB_USER} --password ${GITHUB_TOKEN}

```
### APP Projects作成
```
GITHUB_REPO="https://github.com/hkb3711220/argo_test.git"
ARGO_CD_PROJECT="argo-test-stg"
NAMESPACE="stg"
argocd proj create ${ARGO_CD_PROJECT} --src ${GITHUB_REPO} --dest https://kubernetes.default.svc,${NAMESPACE}

```
### Application作成
```
GITHUB_REPO="https://github.com/hkb3711220/argo_test.git"
ARGO_CD_PROJECT="argo-test-stg"
NAMESPACE="stg"
argocd app create ${NAMESPACE}-nginx \
  --label app=nginx \
  --label environment=${NAMESPACE} \
  --project ${ARGO_CD_PROJECT} \
  --dest-namespace ${NAMESPACE} \
  --dest-name in-cluster \
  --repo ${GITHUB_REPO} \
  --revision HEAD \
  --path overlays/${NAMESPACE}
```