# Pipeline Sample

Repositório de referência para pipelines CI/CD da organização Visualytics.

## Status Atual

Pipeline mínimo validando:
- Autenticação GCP via Workload Identity Federation
- Build e push de imagem Docker para Artifact Registry

## Estrutura

```
pipeline-sample/
├── Dockerfile                    # Imagem de exemplo
├── .github/workflows/
│   └── build.yml                 # Workflow de build e push
└── README.md
```

## Configuração Necessária

### Variáveis de Organização (GitHub)
- `WORKLOAD_IDENTITY_PROVIDER` - Provider do Workload Identity Pool
- `DOCKER_SERVICE_ACCOUNT` - Service account para push no Artifact Registry

### IAM Binding (GCP)
Para cada novo repositório, adicionar o binding:
```bash
gcloud iam service-accounts add-iam-policy-binding SERVICE_ACCOUNT@PROJECT.iam.gserviceaccount.com \
  --project=PROJECT \
  --role="roles/iam.workloadIdentityUser" \
  --member="principalSet://iam.googleapis.com/projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/POOL/attribute.repository/ORG/REPO"
```

---

## Roadmap

### CI - Qualidade de Código
- [ ] Lint e formatação (prettier, eslint, dotnet format)
- [ ] Testes unitários
- [ ] Testes de integração
- [ ] Code coverage com threshold mínimo
- [ ] Security scanning (trivy, snyk)
- [ ] SAST/DAST

### CI - Build
- [ ] Build multi-stage otimizado
- [ ] Cache de layers Docker
- [ ] Build matrix (múltiplas plataformas)

### CD - Ambientes
- [ ] Deploy para `development` em push para `main`
- [ ] Deploy para `staging` via tag `rc-*`
- [ ] Deploy para `production` via tag `v*` com aprovação manual

### Versionamento
- [ ] Semantic versioning automático
- [ ] Changelog gerado automaticamente
- [ ] Release notes

### Reusable Workflows
- [ ] Workflow de CI reutilizável (`ci.yml`)
- [ ] Workflow de CD reutilizável (`cd.yml`)
- [ ] Composite actions para steps comuns
- [ ] Importação via `uses: visualitycs/pipeline-sample/.github/workflows/ci.yml@main`

### Aplicação de Exemplo
- [ ] API simples (.NET ou Node) para demonstrar pipeline completo
- [ ] Dockerfile multi-stage com build e runtime
- [ ] Health check endpoint
- [ ] Testes automatizados

---

## Referências

- [Workload Identity Federation](https://cloud.google.com/iam/docs/workload-identity-federation)
- [GitHub Actions Reusable Workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows)
- [Artifact Registry](https://cloud.google.com/artifact-registry/docs)
