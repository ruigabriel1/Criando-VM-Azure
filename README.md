# Criação de Máquina Virtual no Azure - Guia Completo

## Visão Geral
Este guia fornece instruções detalhadas para criar uma máquina virtual (VM) no Microsoft Azure através do portal web, incluindo considerações importantes sobre SLA e níveis de disponibilidade.

## Pré-requisitos
- Assinatura ativa do Microsoft Azure
- Permissões para criar recursos na assinatura
- Conhecimento básico de conceitos de cloud computing

## SLA e Níveis de Disponibilidade

### SLA do Azure para VMs
O Azure oferece diferentes níveis de SLA baseados na configuração de disponibilidade:

| Configuração | SLA | Descrição |
|--------------|-----|-----------|
| **VM Única** | 99.9% | Para uma única VM com Premium SSD ou Ultra SSD |
| **Conjunto de Disponibilidade** | 99.95% | VMs distribuídas em domínios de falha e atualização |
| **Zonas de Disponibilidade** | 99.99% | VMs distribuídas entre múltiplas zonas de disponibilidade |

### Tipos de Disco e SLA
- **Premium SSD**: 99.9% de SLA para VM única
- **Standard SSD**: 99.5% de SLA para VM única
- **Standard HDD**: 99.5% de SLA para VM única

## Passo a Passo - Criação via Portal Azure

### 1. Acessar o Portal Azure
1. Navegue para [portal.azure.com](https://portal.azure.com)
2. Faça login com suas credenciais

### 2. Iniciar a Criação da VM
1. Clique em "Criar um recurso" no menu superior esquerdo
2. Pesquise por "Máquina virtual"
3. Clique em "Máquina virtual" e depois em "Criar"

### 3. Configurar os Básicos
**Assinatura**: Selecione sua assinatura ativa
**Grupo de Recursos**:
- Crie novo: `rg-vm-producao`
- Ou use existente

**Detalhes da Instância**:
- **Nome da VM**: `vm-web-producao-01`
- **Região**: `East US` (ou região mais próxima)
- **Opções de Disponibilidade**:
  - *Nenhuma redundância de infraestrutura necessária* (SLA: 99.9%)
  - *Conjunto de disponibilidade* (SLA: 99.95%)
  - *Zona de disponibilidade* (SLA: 99.99%)

**Imagem**:
- Windows Server 2022 Datacenter
- Ubuntu Server 20.04 LTS
- Ou imagem personalizada

**Tamanho**:
- Standard_B2s (2 vCPUs, 4GB RAM)
- Standard_D4s_v3 (4 vCPUs, 16GB RAM)
- Consulte o [calculador de preços](https://azure.microsoft.com/pricing/calculator/)

### 4. Configurar Conta de Administrador
- **Nome de usuário**: `azureuser` (não use admin, administrator ou root)
- **Senha**: Use senha forte com mínimo 12 caracteres
- **Chave SSH pública**: Para Linux, use autenticação por chave SSH

### 5. Configurar Regras de Porta de Entrada
**Portas de entrada públicas**:
- **RDP (3389)**: Para Windows
- **SSH (22)**: Para Linux
- **HTTP (80)**: Para web servers
- **HTTPS (443)**: Para web servers seguros

⚠️ **Importante**: Restrinja o acesso por IP específico quando possível

### 6. Configurar Discos
**Disco do SO**:
- **Premium SSD**: Para produção (SLA: 99.9%)
- **Standard SSD**: Para desenvolvimento/testes
- **Standard HDD**: Para cargas de trabalho não críticas

**Dados adicionais**:
- Adicione discos conforme necessidade
- Configure cache de leitura/gravação apropriado

### 7. Configurar Rede
**Rede virtual**: Crie nova ou use existente
- **Nome**: `vnet-producao-001`
- **Espaço de endereço**: `10.0.0.0/16`

**Sub-rede**:
- **Nome**: `subnet-web-001`
- **Intervalo de endereços**: `10.0.1.0/24`

**IP Público**:
- Crie novo IP público
- SKU: Standard (recomendado)
- Atribuição: Dinâmica ou Estática

### 8. Configurar Monitoramento
- **Insights da VM**: Ative para monitoramento avançado
- **Diagnóstico de inicialização**: Ative para troubleshooting
- **Métricas básicas**: Ative por padrão

### 9. Revisar e Criar
1. Revise todas as configurações
2. Verifique o custo estimado
3. Clique em "Criar"
4. Aguarde 3-5 minutos para provisionamento

## Configurações de Alta Disponibilidade

### Conjunto de Disponibilidade
Para SLA de 99.95%:
1. Crie um Conjunto de Disponibilidade antes da VM
2. Configure mínimo 2 VMs no conjunto
3. Distribua em pelo menos 2 domínios de falha

### Zonas de Disponibilidade
Para SLA de 99.99%:
1. Selecione região que suporte Zonas de Disponibilidade
2. Distribua VMs entre zonas (ex: Zona 1, 2, 3)
3. Configure Load Balancer entre as zonas

## Pós-Criação

### 1. Configurações de Segurança
- Atualize o sistema operacional
- Configure firewall do SO
- Instale antivírus (Windows)
- Configure backups

### 2. Configurações de Rede
- Configure NSG (Network Security Group)
- Restrinja acesso por IP
- Configure VPN ou ExpressRoute se necessário

### 3. Monitoramento
- Configure alertas de CPU, memória e disco
- Configure logs de diagnóstico
- Configure Azure Backup

## Custos e Dimensionamento

### Fatores de Custo
- **Tamanho da VM**: Maior tamanho = maior custo
- **Tipo de disco**: Premium SSD > Standard SSD > HDD
- **Região**: Custos variam por região
- **Tempo de execução**: Pague por minuto de uso

### Dicas de Economia
- Use VMs Spot para cargas de trabalho não críticas
- Configure Auto-shutdown para ambientes de dev/teste
- Use Reserved Instances para produção estável
- Monitore e ajuste o tamanho conforme uso real

## Troubleshooting Comum

### VM não inicia
- Verifique logs de diagnóstico
- Confirme NSG e regras de firewall
- Verifique cotas da assinatura

### Conexão falha
- Confirme IP público e porta
- Verifique NSG e ACLs
- Teste conectividade com Azure Network Watcher

## Referências e Recursos Adicionais

- [Documentação oficial - Criar VM Windows](https://learn.microsoft.com/pt-br/azure/virtual-machines/windows/quick-create-portal)
- [Documentação oficial - Criar VM Linux](https://learn.microsoft.com/pt-br/azure/virtual-machines/linux/quick-create-portal)
- [SLA do Azure para VMs](https://azure.microsoft.com/pt-br/support/legal/sla/virtual-machines/)
- [Calculadora de Preços Azure](https://azure.microsoft.com/pt-br/pricing/calculator/)
- [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/)

## Suporte
- [Azure Support](https://azure.microsoft.com/pt-br/support/)
- [Stack Overflow - Azure](https://stackoverflow.com/questions/tagged/azure)
- [Microsoft Q&A](https://docs.microsoft.com/answers/topics/azure-virtual-machines.html)
