## Desafio da Escola da Nuvem 
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/Amazon_Web_Services_Logo.svg/2560px-Amazon_Web_Services_Logo.svg.png" width="180" alt="AWS Logo"><img src="https://github.com/HalleyVeras/Escola_da_Nuvem/blob/main/Documentos/download%20(4)_processed.png?raw=true" width="210" alt="Second Image">


A startup Nova Tech, está criando um e-commerce. O time responsável pela infraestrutura decidiu contratar uma consultoria para evoluir sua arquitetura. Tendo disponível para investimento um aporte inicial de até $10.000,00 para compromissos de longo prazo, além de um orçamento mensal de $500,00 para gastos adicionais recorrentes na nuvem AWS. A Nova Tech deseja uma arquitetura baseada nas melhores práticas da AWS.

**Foco técnico:** Alcance Global


## Situação atual

![](https://github.com/HalleyVeras/Escola_da_Nuvem/blob/main/Documentos/imagem_2024-11-05_141524831.png?raw=true)

## ARQUITETURA PROPOSTA PELA EQUIPE 

#####A Nova Tech nos contratou para desenvolver uma estratégia que permita ao seu e-commerce alcançar o público global de forma eficiente.
Após uma análise detalhada, identificamos serviços cruciais, como o **Amazon CloudFront**, que oferece uma rede de distribuição de conteúdo com locais de borda estratégicos para garantir baixa latência, e o **Amazon Route 53**, que proporciona gerenciamento de DNS de forma rápida e confiável. Esses serviços são essenciais para melhorar o desempenho e a disponibilidade global do site da Nova Tech.

A equipe utilizou o **AWS Well-Architected Framework** como base para a construção do e-commerce, garantindo que a solução seguisse as melhores práticas em termos de segurança, desempenho, confiabilidade, eficiência operacional, otimização de custos e sustentabilidade.

Durante o desenvolvimento do diagrama do projeto, o Well-Architected Framework foi aplicado em todas as etapas, desde o planejamento até a implementação, assegurando que a arquitetura fosse não apenas escalável e resiliente, mas também que estivesse em conformidade com os seis pilares principais:

**Excelência Operacional:** A automação da infraestrutura foi garantida através do uso de CloudFormation e práticas de monitoramento contínuo com CloudWatch.

**Segurança:** O AWS IAM, WAF e o Route 53 garantiram um nível elevado de proteção e controle de acesso, além de conformidade com padrões de segurança globais.

**Confiabilidade:** A arquitetura foi distribuída entre múltiplas regiões e Zonas de Disponibilidade, com failover configurado, assegurando alta disponibilidade e resiliência.

**Eficiência de Desempenho:** O CloudFront garantiu baixa latência na entrega de conteúdo para clientes globais, e a escalabilidade automática foi implementada com Auto Scaling.

**Otimização de Custos:** Ferramentas como o AWS Cost Explorer foram usadas para monitorar os custos e garantir que a solução permanecesse dentro do orçamento, ajustando a infraestrutura conforme a demanda.

**Sustentabilidade:** Utilizando uma arquitetura serverless, algumas funções foram implementadas com AWS Lambda, o que permitiu o uso de recursos computacionais apenas quando necessário, reduzindo o desperdício e otimizando o consumo de energia.

---

## Objetivo do Documento
Este documento visa detalhar a arquitetura da Nova Tech, incluindo a descrição dos serviços utilizados, regiões, zonas de disponibilidade, e a interconexão entre os componentes. A estrutura foi projetada para garantir a escalabilidade automática da aplicação de e-commerce, mantendo os custos controlados e a disponibilidade global.

## Estrutura da Arquitetura

![](https://raw.githubusercontent.com/HalleyVeras/Escola_da_Nuvem/refs/heads/main/Documentos/Projeto_AlcanceGlobal_alteracao.png)

### 2.1. Regiões e Zonas de Disponibilidade AZs
- **Região Primária:** `us-east-1` (Norte da Virgínia)
  - Zonas de Disponibilidade:
    - `us-east-1a`
    - `us-east-1b`
- **Região Secundária (Backup):** `eu-west-1` (Irlanda)
  - Zonas de Disponibilidade:
    - `eu-west-1a`
    - `eu-west-1b`

> A escolha dessas regiões se baseia em sua alta disponibilidade e confiabilidade, além de oferecer baixa latência para usuários globais.

### 2.2. VPCs e Sub-redes
<img src="https://000003.awsstudygroup.com/images/1-Introduce/serviceicon.png?featherlight=false&width=10pc" width="40" alt="AWS Logo">- **VPC Primária:** `vpc-nova-tech-primary`
  - Sub-redes Públicas:
    - `us-east-1a`
    - `us-east-1b`
  - Sub-redes Privadas:
    - `us-east-1a`
    - `us-east-1b`
- **VPC Secundária:** `vpc-nova-tech-secondary`
  - Sub-redes Públicas:
    - `eu-west-1`
    - `eu-west-2`
  - Sub-redes Privadas:
    - `eu-west-1a`
    - `eu-west-1b`

> As VPCs fornecem isolamento da rede para o ambiente da Nova Tech, com sub-redes separadas para os recursos públicos e privados. A replicação entre regiões garante recuperação de desastres.

## Serviços Utilizados

### 3.1. Gerenciamento de Tráfego e APIs
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS20_xZrbCjYfjYxmEkO8jslZJgGDICDleZdQ&s" width="40" alt="AWS Logo">- **Amazon Route 53**
  - Gerenciamento global de DNS, com failover entre `us-east-1` e `eu-west-1`.
  - Domínio Principal: `www.novatech.com`
  
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRQqvRViRcB2VPDy1p5uJ2NHwbDjzNEbDdBeg&s" width="40" alt="AWS Logo">- **Amazon API Gateway**
  - Exposição de APIs RESTful, integração com Lambda para funcionalidades do e-commerce.
  - Nome do Endpoint: `api-gateway-ecommerce`

### 3.2. Balanceamento de Carga e Entrega de Conteúdo
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQWKBnZfRFMMRrW0i2-b2i5D9sOyHoW-vipGA&s" width="40" alt="AWS Logo">- **Elastic Load Balancer (ALB)**
  - Balanceamento de tráfego HTTP/HTTPS entre instâncias EC2.
  - Configuração SSL via HTTPS com certificado SSL.
  
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTuYoLlp6rFCekZcKfLrkJeApZe_hqUCkTulQ&s" width="40" alt="AWS Logo">- **Amazon CloudFront**
  - Rede de entrega de conteúdo (CDN) para arquivos estáticos.
  - Integração com S3 para latência global otimizada.

### 3.3. Computação Escalável
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSVS48VeIElXRoqXfCQ4uD0Ct9D5i9jDNLvbw&s" width="40" alt="AWS Logo">- **Amazon EC2**
  - Instâncias escaláveis automaticamente com Auto Scaling.
  - Tipo de Instância: `t3.medium` e `t3.small`
  
<img src="https://upload.wikimedia.org/wikipedia/commons/e/e9/Amazon_Lambda_architecture_logo.png" width="40" alt="AWS Logo">- **AWS Lambda**
  - Funções serverless para pedidos, estoque e notificações.

### 3.4. Banco de Dados e Armazenamento
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRH-qrjtCxTfNCFRAMkLrrDVb4KbvTYtCKvGg&s" width="40" alt="AWS Logo">- **Amazon RDS (MySQL)**
  - Banco de dados relacional com replicação multi-AZ.
  
<img src="https://cdn.worldvectorlogo.com/logos/amazon-s3-simple-storage-service.svg" width="40" alt="AWS Logo">- **Amazon S3**
  - Armazenamento de objetos para conteúdo estático e backups.

### 3.5. Segurança e Controle de Acesso
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ0rh_MHs8A_3bqf6v-xsH7OYkNnvp0WOooDA&s" width="40" alt="AWS Logo">- **AWS IAM**
  - Controle de acesso com MFA.

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTBfI2E9SFfpNJ_8Kf3ujVI9LVC098LWFyyawI4FMeo1uRnndTwQVqz6e5J3ZfAkqik7us&usqp=CAU" width="40" alt="AWS Logo">- **AWS WAF**
  - Proteção contra ataques DDoS no ALB.

### 3.6. Monitoramento e Logs
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSmJWNXq74Q-5TP0-VGhxGhKiO6yCOx0LoGpw&s" width="40" alt="AWS Logo">- **Amazon CloudWatch**
  - Coleta métricas de performance e logs dos serviços.

### 3.7. Otimização de Custos
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRKYWM9vGnX0mQRTrbbj9wfqZ8rOUtUQb0maQ&s" width="40" alt="AWS Logo">- **AWS Cost Explorer**
  - Controle de custos com alertas financeiros.

### 3.8. Automação de Infraestrutura
<img src="https://seeklogo.com/images/A/aws-cloudformation-logo-11F173F931-seeklogo.com.png" width="40" alt="AWS Logo">- **AWS CloudFormation**
  - Automação do provisionamento de infraestrutura para replicação em outras regiões.


## Diagrama Hierárquico de Interconexão

### Fluxo e Interconexão entre os Serviços

#### Região Primária (`us-east-1`)
1. **Route 53:** Gerencia o DNS e roteia para API Gateway ou ALB.
2. **API Gateway:** Exposição de APIs RESTful que acionam funções Lambda.
3. **Lambda Functions:** Executam lógica de backend e atualizam o banco de dados.
4. **ALB:** Balanceia o tráfego entre instâncias EC2.
5. **EC2:** Instâncias de aplicação web escaláveis automaticamente.
6. **RDS:** Banco de dados multi-AZ replicado.
7. **S3:** Armazenamento de arquivos estáticos e backups.

#### Região Secundária (`eu-west-1`)
1. **RDS (Backup):** Réplica para recuperação de desastres.
2. **S3 (Backup):** Replicação dos backups do bucket principal.

## Conclusão
A integração com os seis pilares do Well-Architected Framework garante que a arquitetura ofereça alta disponibilidade, eficiência, segurança, otimização de custos, sustentabilidade e excelência operacional. Todos os componentes foram selecionados para atender às demandas globais sem comprometer a performance, com os custos controlados em $10.000.

[Acesse o AWS Pricing Calculator](https://calculator.aws/#/estimate?id=273402d9beda54cbad07bb8660ae7df228331fce)

--- 
##Agradecimentos
Gostaríamos de expressar nossa gratidão à Escola da Nuvem e ao professor Thiago Kurimoto pelo apoio e orientação ao longo deste projeto. Agradecemos também aos colegas de equipe pela colaboração e comprometimento em construir uma solução de infraestrutura robusta e inovadora.

## Links Utéis


| [Amazon EC2](https://aws.amazon.com/ec2/)  |  [Documentação do Amazon EC2](https://docs.aws.amazon.com/ec2/index.html)  |
| :------------: | :------------: |
|  [Amazon S3](https://aws.amazon.com/s3/)  |  [Documentação do Amazon S3](https://docs.aws.amazon.com/s3/index.html)  |
|  [Amazon RDS](https://aws.amazon.com/rds/)  |  [Documentação do Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) |
|  [Amazon VPC](https://aws.amazon.com/vpc/)  |   [Documentação do Amazon VPC](https://docs.aws.amazon.com/vpc/index.html) |
|  [AWS Lambda](https://aws.amazon.com/lambda/)  |  [Documentação do AWS Lambda](https://docs.aws.amazon.com/lambda/index.html)  |
|  [Amazon CloudFront](https://aws.amazon.com/cloudfront/)  | [Documentação do Amazon CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Welcome.html)  |
| [Amazon API Gateway](https://aws.amazon.com/api-gateway/)  | [Documentação do Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)  |
| [AWS IAM](https://aws.amazon.com/iam/)  | [Documentação do AWS IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)  |
|  [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)  | [Documentação do Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)  |
|  [Amazon Route 53](https://aws.amazon.com/route53/)  |  [Documentação do Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html)  |
|  [AWS Auto Scaling](https://aws.amazon.com/autoscaling/)  |   [Documentação do AWS Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-as.html) |
|  [Amazon ELB](https://aws.amazon.com/elasticloadbalancing/)  |  [Documentação do Amazon ELB](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html)  |
|  [AWS CloudFormation](https://aws.amazon.com/cloudformation/)  | [Documentação do AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)  |
| [AWS Cost Management](https://aws.amazon.com/aws-cost-management/)  |  [Documentação do AWS Cost Management](https://docs.aws.amazon.com/cost-management/latest/userguide/what-is-cost-management.html) |
| [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)  |  [AWS Architecture Center](https://aws.amazon.com/architecture/) |



Esses links incluem tanto a página principal dos serviços quanto a documentação detalhada, que pode ser muito útil ao desenvolver seu projeto. Se precisar de mais informações sobre algum serviço específico, fique à vontade para perguntar!

![](https://img.shields.io/github/stars/pandao/editor.md.svg) ![](https://img.shields.io/github/forks/pandao/editor.md.svg) 


