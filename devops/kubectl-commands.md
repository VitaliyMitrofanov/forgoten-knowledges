# Kubectl commands    
#### Копирование секрета между NS    
kubectl get secret <secret name> --namespace=<NS from> -oyaml | grep -v '^\s*namespace:\s' | kubectl apply --namespace=<NS to> -f -