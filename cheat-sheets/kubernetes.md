## useful

run a pod in an interactive shell
```bash
kubectl run -i --tty busybox --image=busybox:1.28 -- sh
```

run a cronjob manually
```bash
kubectl create job --from=cronjob/<name of cronjob> <name of job>
```

get pods name only
```bash
kubectl get pods --no-headers -o custom-columns=":metadata.name" # my-pod
# can also use this
kubectl get pods -o=name # pod/my-pod
```

## more cheat-sheets:
- https://kubernetes.io/docs/reference/kubectl/cheatsheet/