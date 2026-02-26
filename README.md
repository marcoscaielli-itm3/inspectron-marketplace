# Inspectron – Pacote Google Cloud Marketplace

Pacote Kubernetes para listagem do Inspectron no Google Cloud Marketplace.

## Estrutura (placeholder)

- `app/` – Manifests Kubernetes (Deployments, Services, ConfigMaps) e recurso Application CR.
- `schema/` – Schemas do deployer (se aplicável).
- `LICENSE` – Licença do repositório (obrigatória para o Marketplace).

## Próximos passos

1. Exportar do cluster GKE os manifests atuais (Deployments, Services, Ingress) do namespace `itm3` e colocar em `app/`.
2. Adicionar o recurso [Application](https://cloud.google.com/marketplace/docs/partners/kubernetes/create-app-package) descrevendo a aplicação.
3. Criar e publicar a imagem **deployer** que aplica esses manifests no cluster do cliente.
4. Configurar repositório público (GitHub/GitLab) e submeter no Producer Portal.

Ver [CHECKLIST_GOOGLE_MARKETPLACE.md](../CHECKLIST_GOOGLE_MARKETPLACE.md) na raiz do POC.
