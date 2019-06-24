app_name:=celery_exporter

test :
	echo "Initializing Helm, Helm Tiller and Checking Chart structure."
	helm init --client-only
	helm tiller start-ci $${NAMESPACE}
	export HELM_HOST=127.0.0.1:44134
	helm tiller run helm install --tiller-namespace $${NAMESPACE} --namespace $${NAMESPACE} --dry-run .

install :
	echo "Upgrade Release"
	kubectl config use-context $${AWS_CLUSTER}
	helm upgrade --namespace $${NAMESPACE} \
		--tiller-namespace $${NAMESPACE} \
		--install --force celery-exporter-$${RELEASE} \
		--set broker_url=$${BROKER_URL} \
		--set queue_list="default\,importacao\,integracao\,metrica\,enviarpedido" \
		.
