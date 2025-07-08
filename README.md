# dev-manifests

## Admin
- admin 서비스는 EKS 클러스터 내에서 Kubernetes의 Deployment, Service, Ingress 리소스를 통해 배포됩니다.
- 컨테이너 이미지는 mangobutter/mangoboss-admin:{tag}로 빌드되어 사용되며, ClusterIP 타입의 서비스로 내부에 노출됩니다.
- NGINX Ingress를 통해 외부 도메인 admin.api.mangoboss.store로 접근할 수 있도록 구성되어 있습니다.
- TLS 인증서는 cert-manager와 letsencrypt-prod 클러스터 이슈어를 사용하여 자동으로 발급 및 갱신됩니다.
- 해당 서비스는 CPU 250m, 메모리 256Mi 수준의 매우 경량한 리소스를 요청하며, 최대 CPU 500m / 512Mi로 제한됩니다.
- readiness probe를 통해 애플리케이션 준비 상태를 확인하며, RollingUpdate 전략을 통해 무중단 배포를 지원합니다.
- 필요한 환경 변수와 시크릿은 Kubernetes의 ConfigMap과 Secret을 통해 주입됩니다.


## App
- app 서비스는 EKS 클러스터 내에서 Kubernetes의 Deployment, Service, Ingress, HorizontalPodAutoscaler 리소스를 통해 배포됩니다.
- 컨테이너 이미지는 mangobutter/mangoboss-app:{tag} 빌드되어 사용되며, ClusterIP 타입의 서비스로 내부에 노출됩니다.
- NGINX Ingress를 통해 외부 도메인 api.mangoboss.store로 접근할 수 있도록 구성되어 있습니다.
- TLS 인증서는 cert-manager와 letsencrypt-prod 클러스터 이슈어를 사용하여 자동으로 발급 및 갱신됩니다.
- 해당 서비스는 CPU 500m, 메모리 1Gi 수준의 리소스를 요청하도록 설정되어 있으며, 최대 1000m / 2Gi로 제한됩니다.
- 기본적으로 파드 2개로 운영되며, readinessProbe와 RollingUpdate 전략을 통해 무중단 배포가 가능합니다.
- /health 엔드포인트를 통해 애플리케이션의 준비 상태를 확인하며, 준비 완료된 파드에만 트래픽이 전달됩니다.
- HorizontalPodAutoscaler(HPA) 설정을 통해 CPU 사용률이 50%를 초과할 경우, 파드 수를 2개에서 최대 4개까지 자동 확장합니다.
- 필요한 환경 변수와 시크릿은 Kubernetes의 ConfigMap과 Secret을 통해 주입됩니다.

## Batch
- batch 서비스는 EKS 클러스터 내에서 Kubernetes의 Deployment와 Service 리소스를 통해 배포됩니다.
- 컨테이너 이미지는 mangobutter/mangoboss-batch:{tag} 빌드되어 사용되며, ClusterIP 타입의 서비스로 내부에 노출됩니다.
- 서비스는 80번 포트로 외부 노출 없이 클러스터 내부 통신을 위해 구성되어 있으며, 컨테이너는 8081번 포트를 사용합니다.
- 해당 서비스는 CPU 500m, 메모리 1Gi 수준의 리소스를 요청하도록 설정되어 있으며, 최대 1000m / 2Gi로 제한됩니다.
- 배포는 Recreate 전략을 사용하여, 기존 파드를 완전히 종료한 후 새 파드를 생성하는 방식으로 수행됩니다.
- 필요한 환경 변수와 시크릿은 Kubernetes의 ConfigMap과 Secret을 통해 주입됩니다.
