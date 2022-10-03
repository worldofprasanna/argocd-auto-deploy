# POC to auto deploy master to Dev environment

These are the steps did in command line to simulate auto deployment

1. Tag the docker image to latest,
`docker tag wordofprasanna/myapp:v1 wordofprasanna/myapp:latest`
2. Push the latest image,
`docker push wordofprasanna/myapp:latest`
3. Take the Image digest of latest image,
`IMAGE_DIGEST_1=$(docker image inspect wordofprasanna/myapp:latest -f '{{join .RepoDigests ","}}')`
4. Set the Image digest as annotation in ArgoCD
`argocd app set myapp -p podAnnotations.imageDigest=$IMAGE_DIGEST_1`

This can be done for v1, v2, v3 versions of the image `wordofprasanna/myapp`

To expose the service use port forwarding,
Note: mk is the alias for `microk8s kubectl`

`mk port-forward deploy/myapp-myapp-chart 8082:80`

To expose the ArgoCD

`mk port-forward svc/argocd-server -n argocd 8080:443`