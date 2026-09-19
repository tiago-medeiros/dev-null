# Prompt: Criação do Projeto "DEV-NULL LAB" — Laboratório DevOps/SRE

---

## Visão Geral do Projeto

Crie o **DEV-NULL LAB**, um laboratório completo de estudos DevOps/SRE com arquitetura multi-repositório, totalmente versionado em Git e projetado para ser gerenciado por agentes de IA.

### Stack Tecnológica
- **Terraform** — Infraestrutura como Código (IaC)
- **LocalStack** — AWS local
- **Kind** — Kubernetes local (clusters)
- **Helm** — Gerenciamento de pacotes Kubernetes (charts reutilizáveis)
- **ArgoCD** — GitOps para deploy em Kubernetes
- **Zabbix** — Monitoramento e observabilidade
- **Istio** — Service Mesh
- **Docker** — Containerização

---

## Estrutura de Repositórios

Crie os seguintes repositórios Git separados, cada um com seu próprio diretório raiz:

| Ferramenta      | Nome do Repo               | Descrição                                    |
|----------------|----------------------------|----------------------------------------------|
| Terraform       | `dev-null-terraform`       | Módulos Terraform reutilizáveis              |
| LocalStack/AWS  | `dev-null-localstack`      | Configuração do AWS local                   |
| Kind/K8s        | `dev-null-k8s`             | Definição dos clusters Kind                 |
| Helm            | `dev-null-helm`            | Charts Helm reutilizáveis                  |
| ArgoCD         | `dev-null-argocd`          | Configurações GitOps e Application CRDs      |
| Zabbix          | `dev-null-zabbix`          | Monitoramento, templates, alertas            |
| Istio           | `dev-null-istio`           | Service mesh config, virtual services        |
| Docker          | `dev-null-docker`          | Dockerfiles multi-stage, docker-compose      |

### Repositório Raiz
- `dev-null-lab` — Orquestrador principal que clona e coordena todos os sub-repositórios

---

## Estrutura de Diretórios por Repositório

### 1. `dev-null-terraform` (Terraform)

```
dev-null-terraform/
├── .qwen/INSTRUCTIONS.md          # Instruções para agente Terraform
├── README.md                      # Documentação do repositório
├── modules/
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── versions.tf
│   ├── s3/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── versions.tf
│   ├── ec2/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── versions.tf
│   ├── eks/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── versions.tf
│   ├── rds/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── versions.tf
│   └── iam/
│       ├── main.tf
│       ├── variables.tf
│       ├── outputs.tf
│       └── versions.tf
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── terraform.tfvars
│   ├── staging/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── terraform.tfvars
│   └── prod/
│       ├── main.tf
│       ├── variables.tf
│       ├── outputs.tf
│       └── terraform.tfvars
├── scripts/
│   └── init.sh
├── .gitignore
└── .tflint.hcl
```

### 2. `dev-null-localstack` (LocalStack/AWS)

```
dev-null-localstack/
├── .qwen/INSTRUCTIONS.md          # Instruções para agente LocalStack
├── README.md
├── docker-compose.yml             # LocalStack + serviços auxiliares
├── localstack-init.sh             # Scripts de inicialização
├── provisioning/
│   ├── bucket-setup.sh
│   ├── lambda-setup.sh
│   ├── eventbridge-setup.sh
│   └── cloudwatch-setup.sh
├── .gitignore
└── .env.example
```

### 3. `dev-null-k8s` (Kind/Kubernetes)

```
dev-null-k8s/
├── .qwen/INSTRUCTIONS.md          # Instruções para agente Kubernetes
├── README.md
├── clusters/
│   ├── control-plane.yaml         # Config do cluster Kind
│   ├── worker-node-1.yaml
│   ├── worker-node-2.yaml
│   └── multi-cluster.yaml         # Config para multi-cluster
├── namespaces/
│   ├── development.yaml
│   ├── staging.yaml
│   └── production.yaml
├── rbac/
│   ├── roles.yaml
│   ├── rolebindings.yaml
│   └── serviceaccounts.yaml
├── configmaps/
│   ├── global-config.yaml
│   └── app-config.yaml
├── secrets/
│   └── encrypted-secrets.yaml     # Sealed Secrets ou SOPS
├── storage/
│   ├── local-path-provisioner.yaml
│   └── pvc-templates.yaml
├── ingress/
│   ├── nginx-ingress-values.yaml
│   └── ingress-rules.yaml
├── scripts/
│   ├── create-clusters.sh
│   ├── destroy-clusters.sh
│   └── kubectl-wrapper.sh
├── .gitignore
└── .env.example
```

### 4. `dev-null-helm` (Helm Charts Reutilizáveis)

```
dev-null-helm/
├── .qwen/INSTRUCTIONS.md          # Instruções para agente Helm
├── README.md
├── charts/
│   ├── base/                       # Chart base reutilizável
│   │   ├── Chart.yaml
│   │   ├── values.yaml
│   │   ├── templates/
│   │   │   ├── _helpers.tpl
│   │   │   ├── deployment.yaml
│   │   │   ├── service.yaml
│   │   │   ├── hpa.yaml
│   │   │   ├── pvc.yaml
│   │   │   ├── configmap.yaml
│   │   │   └── secret.yaml
│   │   └── tests/
│   └── application/                # Chart para aplicações web
│       ├── Chart.yaml
│       ├── values.yaml
│       ├── values.production.yaml
│       ├── templates/
│       │   ├── _helpers.tpl
│       │   ├── deployment.yaml
│       │   ├── service.yaml
│       │   ├── ingress.yaml
│       │   ├── hpa.yaml
│       │   ├── serviceaccount.yaml
│       │   └── tests/
│       │       └── test-connection.yaml
│       └── tests/
├── reusable/
│   ├── postgresql/
│   │   ├── Chart.yaml
│   │   ├── values.yaml
│   │   ├── templates/
│   │   └── tests/
│   ├── redis/
│   │   ├── Chart.yaml
│   │   ├── values.yaml
│   │   ├── templates/
│   │   └── tests/
│   ├── nginx/
│   │   ├── Chart.yaml
│   │   ├── values.yaml
│   │   ├── templates/
│   │   └── tests/
│   ├── prometheus/
│   │   ├── Chart.yaml
│   │   ├── values.yaml
│   │   ├── templates/
│   │   └── tests/
│   └── grafana/
│       ├── Chart.yaml
│       ├── values.yaml
│       ├── templates/
│       └── tests/
├── scripts/
│   ├── lint-charts.sh
│   └── test-charts.sh
├── .gitignore
└── Chart.lock.example
```

### 5. `dev-null-argocd` (ArgoCD)

```
dev-null-argocd/
├── .qwen/INSTRUCTIONS.md          # Instruções para agente ArgoCD
├── README.md
├── applications/
│   ├── base/
│   │   ├── kustomization.yaml
│   │   └── namespace.yaml
│   ├── dev/
│   │   ├── application.yaml
│   │   └── sync-policy.yaml
│   ├── staging/
│   │   ├── application.yaml
│   │   └── sync-policy.yaml
│   └── production/
│       ├── application.yaml
│       └── sync-policy.yaml
├── apps/
│   ├── web-app/
│   │   ├── kustomization.yaml
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   ├── ingress.yaml
│   │   └── hpa.yaml
│   ├── api-service/
│   │   ├── kustomization.yaml
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── ingress.yaml
│   └── monitoring-stack/
│       ├── kustomization.yaml
│       ├── prometheus.yaml
│       └── grafana.yaml
├── clusters/
│   ├── local-kind.yaml
│   └── remote-cluster.yaml
├── sync-waves/
│   ├── 0-namespace.yaml
│   ├── 1-storage.yaml
│   ├── 2-applications.yaml
│   └── 3-validation.yaml
├── scripts/
│   └── bootstrap-argocd.sh
├── .gitignore
└── argocd-cm-example.yaml
```

### 6. `dev-null-zabbix` (Zabbix)

```
dev-null-zabbix/
├── .qwen/INSTRUCTIONS.md          # Instruções para agente Zabbix
├── README.md
├── docker-compose.yml             # Zabbix + banco de dados
├── zabbix-config/
│   ├── zabbix-server.conf
│   └── zabbix-web.conf
├── templates/
│   ├── kubernetes/
│   │   ├── hostgroup.yaml
│   │   ├── template-k8s-api.yaml
│   │   ├── template-k8s-node.yaml
│   │   └── template-k8s-pod.yaml
│   ├── applications/
│   │   ├── template-web-app.yaml
│   │   └── template-api-service.yaml
│   └── infrastructure/
│       ├── template-docker.yaml
│       └── template-localstack.yaml
├── graphs/
│   ├── cluster-overview.yaml
│   ├── node-performance.yaml
│   └── application-response.yaml
├── triggers/
│   ├── k8s-node-down.yaml
│   ├── pod-crashloop.yaml
│   ├── high-cpu.yaml
│   └── disk-space-low.yaml
├── actions/
│   ├── slack-notification.yaml
│   └── auto-restart-pod.yaml
├── media-types/
│   └── custom-webhook.yaml
├── scripts/
│   ├── import-templates.sh
│   └── setup-zabbix-agent.sh
├── .gitignore
└── .env.example
```

### 7. `dev-null-istio` (Istio)

```
dev-null-istio/
├── .qwen/INSTRUCTIONS.md          # Instruções para agente Istio
├── README.md
├── install/
│   ├── istio-minimal.yaml
│   ├── istio-demo.yaml
│   └── istio-operator.yaml
├── config/
│   ├── mesh-config.yaml
│   └── gateway.yaml
├── virtual-services/
│   ├── web-app-vs.yaml
│   ├── api-service-vs.yaml
│   └── canary-deployment.yaml
├── destination-rules/
│   ├── web-app-dr.yaml
│   ├── api-service-dr.yaml
│   └── tls-mode.yaml
├── traffic-management/
│   ├── fault-injection.yaml
│   ├── rate-limiting.yaml
│   ├── circuit-breaker.yaml
│   └── retries.yaml
├── security/
│   ├── peer-authentication.yaml
│   ├── authorization-policy.yaml
│   └── jwt-rules.yaml
├── observability/
│   ├── tracing-config.yaml
│   ├── access-logs.yaml
│   └── metrics-dashboard.json
├── scripts/
│   ├── install-istio.sh
│   ├── label-namespaces.sh
│   └── verify-mesh.sh
├── .gitignore
└── istio-operator-values.yaml
```

### 8. `dev-null-docker` (Docker)

```
dev-null-docker/
├── .qwen/INSTRUCTIONS.md          # Instruções para agente Docker
├── README.md
├── images/
│   ├── web-app/
│   │   ├── Dockerfile
│   │   ├── Dockerfile.dev
│   │   ├── Dockerfile.prod
│   │   └── .dockerignore
│   ├── api-service/
│   │   ├── Dockerfile
│   │   ├── Dockerfile.dev
│   │   ├── Dockerfile.prod
│   │   └── .dockerignore
│   ├── zabbix-agent/
│   │   ├── Dockerfile
│   │   └── zabbix_agentd.conf
│   └── custom-tools/
│       ├── Dockerfile
│       └── entrypoint.sh
├── compose/
│   ├── base.yml                   # Serviços base comuns
│   ├── dev.yml                    # Ambiente de desenvolvimento
│   ├── staging.yml                # Ambiente de staging
│   └── prod.yml                   # Ambiente de produção
├── registry/
│   └── docker-registry.yaml       # Private registry config
├── scripts/
│   ├── build-images.sh
│   ├── push-images.sh
│   └── cleanup.sh
├── .docker/
│   └── config.json                # Registry credentials (gitignored)
├── .gitignore
└── .env.example
```

### 9. `dev-null-lab` (Orquestrador Raiz)

```
dev-null-lab/
├── .qwen/INSTRUCTIONS.md          # Instruções para agente orquestrador
├── README.md                      # Documentação principal do projeto
├── ARCHITECTURE.md                # Diagrama e descrição da arquitetura
├── get-started.md                 # Guia de primeiros passos
├── scripts/
│   ├── bootstrap.sh               # Clona e inicializa todos os repositórios
│   ├── up.sh                      # Inicia todo o ambiente
│   ├── down.sh                    # Destrói todo o ambiente
│   ├── status.sh                  # Mostra status dos serviços
│   └── destroy-all.sh             # Limpeza total
├── Makefile                       # Comandos padrão (make up, make down, etc.)
├── .gitmodules                    # Submodules para cada repositório
├── .gitignore
├── .env.example
└── labs/
    ├── lab-01-kubernetes-basics.md
    ├── lab-02-helm-charts.md
    ├── lab-03-argocd-gitops.md
    ├── lab-04-istio-service-mesh.md
    ├── lab-05-terraform-modules.md
    ├── lab-06-monitoring-zabbix.md
    └── lab-07-integrated-deploy.md
```

---

## Arquivo `.qwen/INSTRUCTIONS.md` — Modelo Padrão

Cada repositório deve ter um arquivo `.qwen/INSTRUCTIONS.md` com o seguinte formato:

```markdown
# Dev-Null Lab — Agente [NOME_DA_FERRAMENTA]

## Papel
Você é um especialista em [FERRAMENTA] dentro do laboratório DEV-NULL LAB.
Seu objetivo é criar, manter e evoluir a infraestrutura definida neste repositório.

## Contexto do Projeto
O DEV-NULL LAB é um ambiente de estudos DevOps/SRE que integra múltiplas ferramentas
de orquestração, monitoramento e deploy automatizado. Este repositório gerencia
a camada de [DESCREVER_CAMADA].

## Diretrizes Gerais
- Siga as melhores práticas da comunidade para [FERRAMENTA]
- Mantenha os arquivos pequenos e modulares
- Use variáveis para tudo que pode mudar entre ambientes
- Documente decisões importantes no diretório `decisions/` se existir
- Todos os arquivos devem ser versionados em Git
- Use nomes semânticos para tags de commit (conventional commits)

## Estrutura do Repositório
[Descrever a estrutura específica deste repositório]

## Dependências
- Este repositório depende de: [listar outros repos]
- Este repositório é consumido por: [listar outros repos]

## Comandos Úteis
- `command1` — descrição
- `command2` — descrição

## Padrões de Naming
- Recursos: `devnull-{environment}-{component}-{name}`
- Namespaces: `devnull-{environment}`
- Labels: `app.kubernetes.io/name`, `app.kubernetes.io/part-of: "dev-null-lab"`

## Checklist antes de commitar
- [ ] Código segue padrões do repositório
- [ ] Variáveis estão em arquivos separados (não hardcoded)
- [ ] Documentação atualizada
- [ ] Sem sensitive data (secrets, tokens, chaves)
```

Adapte o conteúdo acima para cada repositório específico.

---

## Requisitos Técnicos Detalhados

### Terraform (`dev-null-terraform`)
- Usar **módulos** para todos os recursos reutilizáveis
- Estrutura de workspaces ou diretórios separados por ambiente
- Provider: AWS (configurado para LocalStack no ambiente local)
- State file: backend S3 (LocalStack) com locking DynamoDB
- Validar com `terraform fmt`, `terraform validate`, `tflint`
- Usar `terraform-docs` para gerar documentação dos módulos

### LocalStack (`dev-null-localstack`)
- Docker Compose com LocalStack + extensions
- Scripts de inicialização para criar buckets, Lambdas, etc.
- Variáveis de ambiente configuráveis por ambiente
- Health check no docker-compose

### Kind/Kubernetes (`dev-null-k8s`)
- Cluster Kind com 1 control plane + 2 worker nodes
- Namespaces separados: dev, staging, production
- RBAC básico configurado
- Storage class local-path-provisioner
- Ingress controller (nginx)

### Helm (`dev-null-helm`)
- **Todos** os recursos Kubernetes devem ser aplicados via Helm charts
- Charts base reutilizáveis com templates genéricos
- Charts específicos para cada tipo de aplicação
- Values files separados por ambiente
- Testes com `helm unittest`
- Linting com `helm lint`

### ArgoCD (`dev-null-argocd`)
- Application CRDs apontando para os outros repositórios
- Sync waves para ordenação de deploy
- Auto-sync habilitado para dev, manual para production
- Kustomize para sobreposição de ambientes

### Zabbix (`dev-null-zabbix`)
- Docker Compose com Zabbix server + frontend + database
- Templates para monitorar Kubernetes, Docker e aplicações
- Triggers e ações configuráveis
- Gráficos personalizados

### Istio (`dev-null-istio`)
- Install via operator ou manifestos
- Gateway configuration
- VirtualServices e DestinationRules
- Traffic splitting (canary)
- Security policies (mTLS, authorization)

### Docker (`dev-null-docker`)
- Multi-stage builds para imagens otimizadas
- Dockerfiles separados para dev/prod
- Compose files com sobreposição (base + environment)
- .dockerignore otimizado

---

## Arquivos de Direcionamento para Agentes IA

Crie os seguintes arquivos em cada repositório para guiar agentes de IA:

1. **`.qwen/INSTRUCTIONS.md`** — Instruções específicas da ferramenta (modelo acima)
2. **`.github/copilot-instructions.md`** — Instruções para GitHub Copilot (se aplicável)
3. **`AGENTS.md`** — Visão geral do repositório para qualquer agente
4. **`.qwen/prompts/`** — Prompts pré-definidos para tarefas comuns:
   - `create-module.md` — Prompt para criar novo módulo
   - `deploy-environment.md` — Prompt para deploy em ambiente
   - `debug-issue.md` — Prompt para debugar problemas

---

## Makefile Raiz (`dev-null-lab/Makefile`)

```makefile
.PHONY: bootstrap up down status destroy-all clean help

# Inicializa todos os sub-repositórios
bootstrap:
	@echo "Initializing Dev-Null Lab..."
	@./scripts/bootstrap.sh

# Inicia todo o ambiente
up:
	@echo "Starting Dev-Null Lab environment..."
	@./scripts/up.sh

# Para todo o ambiente
down:
	@echo "Stopping Dev-Null Lab environment..."
	@./scripts/down.sh

# Status dos serviços
status:
	@./scripts/status.sh

# Destrói tudo completamente
destroy-all:
	@echo "Destroying entire Dev-Null Lab..."
	@./scripts/destroy-all.sh

# Limpeza de artefatos
clean:
	@echo "Cleaning up..."
	@rm -rf .terraform* */.terraform*
	@kind delete clusters devnull-cluster 2>/dev/null || true

help:
	@echo "Dev-Null Lab — DevOps/SRE Laboratory"
	@echo ""
	@echo "Usage: make <target>"
	@echo ""
	@echo "Targets:"
	@echo "  bootstrap    - Initialize all sub-repositories"
	@echo "  up           - Start the entire environment"
	@echo "  down         - Stop the entire environment"
	@echo "  status       - Show status of all services"
	@echo "  destroy-all  - Destroy everything completely"
	@echo "  clean        - Clean up artifacts"
	@echo "  help         - Show this help message"
```

---

## Script de Bootstrap (`dev-null-lab/scripts/bootstrap.sh`)

```bash
#!/usr/bin/env bash
set -euo pipefail

LAB_ROOT="$(cd "$(dirname "${BASH_SOURCE[0]}")/.." && pwd)"
REPOS=(
  "dev-null-terraform"
  "dev-null-localstack"
  "dev-null-k8s"
  "dev-null-helm"
  "dev-null-argocd"
  "dev-null-zabbix"
  "dev-null-istio"
  "dev-null-docker"
)

echo "🏗️  Initializing Dev-Null Lab repositories..."

for repo in "${REPOS[@]}"; do
  if [ ! -d "$LAB_ROOT/$repo" ]; then
    echo "  Cloning $repo..."
    git clone git@github.com:tiago-devnull/$repo.git "$LAB_ROOT/$repo"
  else
    echo "  ✓ $repo already exists"
  fi
done

echo ""
echo "✅ Dev-Null Lab initialized successfully!"
echo "   Run 'make up' to start the environment."
```

---

## Convenções de Git

### Conventional Commits
```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

**Types:**
- `feat` — Nova funcionalidade
- `fix` — Correção de bug
- `docs` — Documentação
- `chore` — Manutenção/tarefas operacionais
- `refactor` — Refatoração de código
- `test` — Adição ou modificação de testes
- `ci` — Configuração de CI/CD

**Exemplos:**
```
feat(terraform): add base module with VPC and subnet configuration
feat(helm): create reusable chart with deployment and service templates
fix(k8s): fix ingress controller namespace configuration
docs(zabbix): update Zabbix template documentation
chore(lab): add bootstrap script for environment setup
```

### Branches
- `main` — Branch principal, sempre funcional
- `feature/<nome>` — Novas funcionalidades
- `fix/<nome>` — Correções
- `docs/<nome>` — Documentação
- `chore/<nome>` — Manutenção

---

## Arquitetura do Laboratório

```
┌─────────────────────────────────────────────────────┐
│                DEV-NULL LAB (Root)                  │
│                                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────┐  │
│  │ dev-null-    │→ │ dev-null-    │  │ dev-null │  │
│  │ terraform    │  │ localstack   │  │  k8s     │  │
│  └──────────────┘  └──────────────┘  └────┬─────┘  │
│                                            │        │
│              ┌─────────────────────────────▼──────┐  │
│              │         Kubernetes                 │  │
│              │   ┌──────────┐  ┌──────────┐      │  │
│              │   │ dev-null │→ │ dev-null │      │  │
│              │   │  helm    │  │ argocd   │      │  │
│              │   └──────────┘  └────┬─────┘      │  │
│              │   ┌──────────┐  ┌───▼──────┐      │  │
│              │   │ dev-null │  │ dev-null │      │  │
│              │   │  istio   │  │  docker  │      │  │
│              │   └──────────┘  └──────────┘      │  │
│              └────────────────────────────────────┘  │
│                            │                        │
│              ┌─────────────▼──────────┐            │
│              │   dev-null-zabbix      │            │
│              │   (Monitoramento)       │            │
│              └────────────────────────┘            │
└─────────────────────────────────────────────────────┘
```

Fluxo de Deploy:
1. **dev-null-terraform** provisiona a infraestrutura na nuvem/local
2. **dev-null-localstack** fornece serviços AWS simulados
3. **dev-null-k8s** cria os clusters Kubernetes
4. **dev-null-docker** constrói as imagens dos containers
5. **dev-null-helm** empacota as aplicações para Kubernetes
6. **dev-null-argocd** faz deploy automático baseado no Git
7. **dev-null-istio** gerencia o tráfego entre serviços
8. **dev-null-zabbix** monitora tudo

---

## Instruções Finais para a IA

Ao criar este projeto:

1. **Gere todos os arquivos listados acima** com conteúdo funcional e válido
2. **Use YAML/JSON onde apropriado** — configs do Kubernetes, Helm values, ArgoCD Application CRDs devem ser YAML; prompts de agente podem ser Markdown
3. **Crie commits semânticos** para cada conjunto de arquivos relacionados
4. **Garanta que cada repositório tenha seu `.qwen/INSTRUCTIONS.md`** completo e adaptado
5. **Use variáveis e templates** — nada hardcoded
6. **Inclua exemplos funcionais** — cada chart, módulo e config deve ser funcional quando executado
7. **Adicione `.gitignore` apropriados** para cada repositório
8. **Crie README.md** em cada repositório explicando seu propósito
9. **Use tags semânticas** para releases/versões
10. **O ambiente deve ser 100% local** — tudo rodando via Docker/Kind/LocalStack

---

## Notas Adicionais

- O projeto deve funcionar em máquinas Linux e macOS
- Requisitos mínimos: Docker, Kind, kubectl, helm, terraform, kustomize
- Todos os scripts devem ter `set -euo pipefail`
- Use cores e emojis nos scripts para melhor UX
- Inclua verificação de dependências nos scripts de bootstrap
- O tempo total de setup deve ser < 5 minutos em hardware razoável

---

**FIM DO PROMPT**
