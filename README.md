# dev-manifests

## Admin
- admin 서비스는 EKS 클러스터 내에서 Kubernetes의 Deployment, Service, Ingress 리소스를 통해 배포됩니다.
- 컨테이너 이미지는 mangobutter/mangoboss-admin:{tag}로 빌드되어 사용되며, ClusterIP 타입의 서비스로 내부에 노출됩니다.
- NGINX Ingress를 통해 외부 도메인 admin.api.mangoboss.store로 접근할 수 있도록 구성되어 있습니다.
- TLS 인증서는 cert-manager와 letsencrypt-prod 클러스터 이슈어를 사용하여 자동으로 발급 및 갱신됩니다.
- 해당 서비스는 CPU 100m, 메모리 128Mi 수준의 경량 리소스를 요청하도록 설정되어 있으며,
- 필요한 환경 변수와 시크릿은 Kubernetes의 ConfigMap과 Secret을 통해 주입됩니다.


## App
- app 서비스는 EKS 클러스터 내에서 Kubernetes의 Deployment, Service, Ingress, HorizontalPodAutoscaler 리소스를 통해 배포됩니다.
- 컨테이너 이미지는 mangobutter/mangoboss-app:{tag} 빌드되어 사용되며, ClusterIP 타입의 서비스로 내부에 노출됩니다.
- NGINX Ingress를 통해 외부 도메인 api.mangoboss.store로 접근할 수 있도록 구성되어 있습니다.
- TLS 인증서는 cert-manager와 letsencrypt-prod 클러스터 이슈어를 사용하여 자동으로 발급 및 갱신됩니다.
- 해당 서비스는 CPU 250m, 메모리 256Mi 수준의 리소스를 요청하도록 설정되어 있으며, 최대 500m / 512Mi로 제한됩니다.
- HorizontalPodAutoscaler를 통해 CPU 사용률이 50%를 초과하면 파드 수를 2개에서 최대 4개까지 자동 확장합니다.
- 필요한 환경 변수와 시크릿은 Kubernetes의 ConfigMap과 Secret을 통해 주입됩니다.

## Batch
- batch 서비스는 EKS 클러스터 내에서 Kubernetes의 Deployment와 Service 리소스를 통해 배포됩니다.
- 컨테이너 이미지는 mangobutter/mangoboss-batch:{tag} 빌드되어 사용되며, ClusterIP 타입의 서비스로 내부에 노출됩니다.
- 서비스는 80번 포트로 외부 노출 없이 클러스터 내부 통신을 위해 구성되어 있으며, 컨테이너는 8081번 포트를 사용합니다.
- 해당 서비스는 CPU 250m, 메모리 256Mi 수준의 리소스를 요청하도록 설정되어 있으며, 최대 500m / 512Mi로 제한됩니다.
- 필요한 환경 변수와 시크릿은 Kubernetes의 ConfigMap과 Secret을 통해 주입됩니다.
