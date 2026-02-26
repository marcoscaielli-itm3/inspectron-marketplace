# Inspectron – Guia do usuário

Sistema de vistoria e inspeção com IA (ITM3), implantado no Google Kubernetes Engine (GKE).

---

## Visão geral

O Inspectron é composto por:

- **Portal** – Interface web (SPA) para uso do sistema
- **API** – Backend .NET que processa as requisições e integra com banco de dados e serviços

A implantação cria Deployments e Services de API e Portal em um namespace de sua escolha. A API requer configuração de banco de dados (PostgreSQL) e variáveis de ambiente; o Portal serve conteúdo estático.

---

## Pré-requisitos

- Cluster GKE (ou Kubernetes compatível) com nodes x86
- `kubectl` configurado para o cluster
- (Opcional) Application CRD instalado para visualização na UI do GKE

---

## Instalação

### Via Google Cloud Console (Marketplace)

1. Acesse o [Google Cloud Marketplace](https://console.cloud.google.com/marketplace).
2. Localize o **Inspectron** e clique em **Instalar**.
3. Selecione o cluster e o namespace de destino.
4. Informe o **nome da instância** e o **namespace** (ou use os valores sugeridos).
5. Clique em **Implantar**.

### Via linha de comando (kubectl)

1. **Criar o namespace** (se ainda não existir):
   ```bash
   kubectl create namespace inspectron
   ```

2. **Instalar o Application CRD** (opcional, para visualização na UI):
   ```bash
   kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/application/master/config/crd/bases/app.k8s.io_applications.yaml
   ```

3. **Aplicar os manifests**:
   ```bash
   kubectl apply -f inspectron/app/application.yaml -n inspectron
   kubectl apply -f inspectron/app/inspectron-api-deployment.yaml -n inspectron
   kubectl apply -f inspectron/app/inspectron-api-service.yaml -n inspectron
   kubectl apply -f inspectron/app/inspectron-portal-deployment.yaml -n inspectron
   kubectl apply -f inspectron/app/inspectron-portal-service.yaml -n inspectron
   ```

4. **Verificar o status**:
   ```bash
   kubectl get pods,svc -n inspectron
   ```

---

## Configuração pós-instalação

### API – Variáveis de ambiente e secrets

A API Inspectron espera variáveis de ambiente para:

- `ConnectionStrings__Itm3` – String de conexão PostgreSQL
- `Jwt__SecretKey`, `Jwt__Issuer`, `Jwt__Audience` – Configuração de JWT
- Outras configurações conforme documentação do produto

**Recomendação:** usar um Secret do Kubernetes e montar como variáveis no Deployment:

```yaml
# Exemplo: criar Secret com dados sensíveis
kubectl create secret generic inspectron-api-secrets -n inspectron \
  --from-literal=ConnectionStrings__Itm3='Server=HOST;Port=5432;Database=DB;...' \
  --from-literal=Jwt__SecretKey='sua-chave-secreta'

# Atualizar o Deployment para usar envFrom (descomente e ajuste no manifest)
# envFrom:
#   - secretRef:
#       name: inspectron-api-secrets
```

### Portal – URL da API

O Portal consome a API. Em implantações padrão, a URL da API deve ser configurável (via variável de ambiente ou arquivo de configuração). Consulte a documentação do Inspectron para o método suportado na sua versão.

### Expor os serviços (Ingress / LoadBalancer)

Para acesso externo, crie um Ingress ou Service tipo LoadBalancer apontando para os serviços `inspectron-api` e `inspectron-portal`. Exemplo mínimo:

```yaml
# Exemplo: Service tipo LoadBalancer para o portal
kubectl expose deployment inspectron-portal -n inspectron --type=LoadBalancer --port=80 --name=inspectron-portal-lb
```

---

## Uso básico

1. Obtenha o endereço do Portal (IP ou hostname do LoadBalancer/Ingress):
   ```bash
   kubectl get svc -n inspectron
   ```

2. Acesse o Portal no navegador usando o endereço retornado.

3. Faça login com as credenciais configuradas para o seu ambiente.

---

## Backup e restauração

- **Banco de dados:** o Inspectron usa PostgreSQL. Configure backups periódicos do banco (pg_dump, backups gerenciados do Cloud SQL ou ferramenta equivalente).
- **Configurações:** mantenha cópias dos Secrets e ConfigMaps relevantes para restauração em outro cluster ou namespace.

---

## Atualização de imagens

Para atualizar as imagens da API ou do Portal:

1. Obtenha a nova tag de imagem (ex.: `1.0.1`).
2. Atualize o Deployment:
   ```bash
   kubectl set image deployment/inspectron-api api=NOVA_IMAGEM_API -n inspectron
   kubectl set image deployment/inspectron-portal portal=NOVA_IMAGEM_PORTAL -n inspectron
   ```
3. Aguarde o rollout:
   ```bash
   kubectl rollout status deployment/inspectron-api -n inspectron
   kubectl rollout status deployment/inspectron-portal -n inspectron
   ```

---

## Remoção

Para remover o Inspectron do cluster:

```bash
kubectl delete namespace inspectron
```

Se houver PersistentVolumeClaims ou outros recursos fora do namespace, remova-os manualmente.

---

## Suporte

- **Documentação:** [https://inspectron.com.br](https://inspectron.com.br)
- **Contato:** Suporte ITM3
