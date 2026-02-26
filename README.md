# Inspectron – Pacote Google Cloud Marketplace

Pacote Kubernetes para listagem do **Inspectron** no Google Cloud Marketplace (ITM3).

---

## Visão geral

- **Inspectron** – Sistema de vistoria e inspeção com IA
- **Componentes:** API (.NET) + Portal (SPA Angular servido por Nginx)
- **Imagens:** `inspectron-api`, `inspectron-portal`, `inspectron/deployer` (Artifact Registry)

---

## Estrutura

```
marketplace-package/
├── LICENSE
├── README.md           (este arquivo)
├── USER_GUIDE.md       (guia do usuário para implantação e uso)
├── inspectron/
│   └── app/
│       ├── application.yaml
│       ├── inspectron-api-deployment.yaml
│       ├── inspectron-api-service.yaml
│       ├── inspectron-portal-deployment.yaml
│       └── inspectron-portal-service.yaml
```

---

## Para usuários finais

Consulte o **[USER_GUIDE.md](USER_GUIDE.md)** para:

- Instalação via Console ou kubectl
- Configuração pós-instalação
- Uso básico, backup e atualização

---

## Para parceiros / desenvolvedores

- **Deployer:** Imagem `inspectron/deployer:1.0.0` – aplica os manifests no cluster do cliente
- **Repositório deployer:** `C:\sistemas\POC\deployer\` (schema, manifest templates, Dockerfile)
- **Checklist e próximos passos:** `C:\sistemas\POC\CHECKLIST_GOOGLE_MARKETPLACE.md`
