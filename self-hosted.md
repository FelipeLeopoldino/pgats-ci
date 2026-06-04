# Self-Hosted em CI/CD

Este documento explora o conceito de self-hosted (auto-hospedados) em pipelines de Integração Contínua/Entrega Contínua (CI/CD), avalia sua viabilidade e confirma o suporte em plataformas como Azure Pipelines e GitHub Actions.

## O que são Self-Hosted?

Runners self-hosted são máquinas de execução que você provisiona, configura e gerencia em sua própria infraestrutura. Em contraste com os agentes hospedados pelo provedor de CI/CD (que são máquinas virtuais ou contêineres gerenciados pelo serviço), os agentes self-hosted permitem que você tenha controle total sobre o ambiente de execução dos seus pipelines.

Você instala um software de agente/runner nessas máquinas (que podem ser servidores físicos, máquinas virtuais, contêineres, etc.), e elas se conectam à sua plataforma de CI/CD para receber e executar jobs.

## Vantagens

1.  **Controle Total do Ambiente**: Permite a instalação de software específico, configuração de variáveis de ambiente personalizadas e acesso a recursos de rede internos.
2.  **Desempenho Otimizado**: Possibilidade de configurar hardware de alta performance (CPU, RAM, GPU) para acelerar builds e testes complexos.
3.  **Acesso a Recursos Privados**: Capacidade de acessar recursos em redes privadas (bancos de dados internos, sistemas de arquivos) sem exposição à internet ou configurações de VPN complexas.
4.  **Custo Potencialmente Menor**: Para cargas de trabalho muito intensas e contínuas, pode ser mais econômico do que pagar por minutos de computação em agentes hospedados, especialmente se houver infraestrutura ociosa.
5.  **Ambiente Consistente**: Garante que o ambiente de build permaneça o mesmo, evitando surpresas com atualizações ou mudanças nos ambientes dos agentes hospedados.

## Desvantagens

1.  **Manutenção e Gerenciamento**: Você é responsável por todo o ciclo de vida do agente, incluindo provisionamento, manutenção, atualizações de software, aplicação de patches de segurança e monitoramento.
2.  **Custo Inicial e Operacional**: Há um investimento inicial na infraestrutura e custos contínuos (energia, licenciamento, tempo da equipe de operações).
3.  **Escalabilidade Manual**: A escalabilidade não é automática; você precisa implementar e gerenciar sua própria lógica para escalar os agentes conforme a demanda.
4.  **Segurança**: A responsabilidade pela segurança da máquina do agente recai sobre você. Vulnerabilidades podem expor sua rede interna.
5.  **Disponibilidade**: A execução dos pipelines depende da disponibilidade e saúde dos seus agentes self-hosted.

## Suporte em Azure Pipelines e GitHub Actions

Ambas as plataformas de CI/CD oferecem suporte robusto para auto-hospedados.

### Azure Pipelines: Agentes Auto-Hospedados

O Azure Pipelines permite a configuração de "agentes auto-hospedados". Você pode instalá-los em sistemas operacionais Windows, Linux ou macOS, seja em sua infraestrutura local (on-premises) ou em qualquer provedor de nuvem (Azure, AWS, GCP, etc.).

Para usar um agente auto-hospedado em um pipeline do Azure, você especificaria o nome do seu pool de agentes no arquivo `azure-pipelines.yml`:

```yaml
pool:
  name: MeuPoolDeAgentesAutoHospedados
```

### GitHub Actions: Runners Auto-Hospedados

O GitHub Actions também oferece suporte a "runners auto-hospedados". Similar ao Azure, você pode instalá-los em máquinas Windows, Linux ou macOS, em qualquer ambiente que você controle.

Para usar um runner auto-hospedado em um workflow do GitHub Actions, você especificaria uma ou mais labels que você atribuiu ao seu runner no arquivo `.github/workflows/*.yml`:

```yaml
runs-on: [self-hosted, linux, x64]
```

## Vale a Pena Usar Agentes Self-Hosted?

A decisão de usar agentes self-hosted deve ser baseada nas necessidades específicas do seu projeto e organização:

*   **Quando vale a pena:**
    *   Você precisa de acesso a recursos de rede internos, hardware especializado (ex: GPUs) ou software licenciado que não está disponível nos agentes hospedados.
    *   Existem requisitos rigorosos de segurança ou conformidade que exigem que os builds sejam executados em sua própria infraestrutura controlada.
    *   Você tem uma carga de trabalho de CI/CD extremamente alta e contínua, onde o custo dos agentes hospedados se torna proibitivo, e você já possui infraestrutura disponível para reutilizar.
    *   A necessidade de um ambiente de build altamente personalizado e consistente é crítica.

*   **Quando não vale a pena:**
    *   Seus projetos são relativamente simples e não possuem requisitos de hardware, software ou rede muito específicos.
    *   Você prioriza a conveniência, a baixa manutenção e a escalabilidade automática oferecidas pelos agentes hospedados.
    *   Sua equipe não possui recursos ou expertise para gerenciar a infraestrutura dos agentes.
    *   A carga de trabalho é esporádica, tornando os agentes hospedados mais econômicos devido ao modelo de pagamento por uso.

Para a maioria dos cenários, os agentes hospedados são a opção mais prática e eficiente, oferecendo escalabilidade, atualizações automáticas e zero manutenção por parte do usuário. Agentes self-hosted são uma solução poderosa para casos de uso específicos que exigem controle e personalização aprofundados.
