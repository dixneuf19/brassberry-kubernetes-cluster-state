HELM_RELEASE=kube-prometheus-stack
HELM_REPOSITORY=prometheus-community
HELM_CHART=kube-prometheus-stack

NAMESPACE=monitoring

get-latest-value:
	helm show values "${HELM_REPOSITORY}/${HELM_CHART}" > values.yaml

secret:
	kubectl -n "${NAMESPACE}" create secret generic grafana-admin --from-env-file=grafana-admin.secret

template:
	helm template -n "${NAMESPACE}" "${HELM_RELEASE}" . -f values.yaml > manifests.yaml

dependency:
	helm dependency update

deploy:
	kubectl create namespace "${NAMESPACE}" || echo "Namespace ${NAMESPACE} already exists"
	helm upgrade --install -n "${NAMESPACE}" "${HELM_RELEASE}" . -f values.yaml
