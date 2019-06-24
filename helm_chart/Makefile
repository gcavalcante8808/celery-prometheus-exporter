SHELL = /bin/bash
.SHELLFLAGS = -euox pipefail -c
.POSIX: all
.RECIPEPREFIX +=


.PHONY: test
test :
    echo "Initializing Helm, Helm Tiller and Checking Chart structure."
    helm init --client-only
    helm tiller start-ci $${NAMESPACE}
    export HELM_HOST=127.0.0.1:44134
    helm tiller run helm install --tiller-namespace $${NAMESPACE:-test} --namespace $${NAMESPACE:-test} --dry-run .

.PHONY: install
install :
    echo "Upgrade Release"
    kubectl config use-context $${K8S_CLUSTER}
    helm upgrade --namespace $${NAMESPACE:-default} \
        --tiller-namespace $${NAMESPACE:-default} \
        --install --force celery-exporter-$${RELEASE} \
        --set broker_url=$${BROKER_URL} \
        --set queue_list="$${QUEUES}" \
        --set timezone="$${TIMEZONE}" \
        .
