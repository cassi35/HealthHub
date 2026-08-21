# Sistema Hospitalar

Backend modular para gestão hospitalar desenvolvido com Clean Architecture, com separação clara entre camadas, repositórios, casos de uso e pontos de extensão para e-mail, automação e analytics.

---

# 1. Visão Geral

Fornece uma API REST consistente para fluxos clínicos, administrativos, financeiros, monitoramento operacional e relatórios analíticos.

---

# 2. Escopo do Projeto

* Gestão de pacientes
* Gestão de médicos
* Operadoras de planos de saúde
* Departamentos hospitalares
* Leitos
* Medicamentos
* Especialidades médicas
* Agendamentos
* Internações
* Prescrições
* Exames médicos
* Faturamento
* Notificações por e-mail
* Analytics
* Automação de processos

---

# 3. Requisitos Funcionais

| Código | Requisito                  | Descrição                                                            |
| ------ | -------------------------- | -------------------------------------------------------------------- |
| RF01   | Gestão de Pacientes        | CRUD completo com associação de endereço                            |
| RF02   | Gestão de Médicos          | CRUD completo com especialidades                                     |
| RF03   | Especialidades Médicas     | CRUD                                                                 |
| RF04   | Agendamento de Consultas   | Data deve ser hoje ou posterior. Médico e paciente devem existir     |
| RF05   | Exames Médicos             | Registrar e listar exames por paciente ou médico                     |
| RF06   | Prescrições                | Associar medicamentos a médicos e pacientes                          |
| RF07   | Medicamentos               | CRUD do catálogo de medicamentos                                     |
| RF08   | Internações                | Internar, atualizar e dar alta em pacientes                          |
| RF09   | Leitos                     | Gerenciamento de ocupação                                            |
| RF10   | Departamentos              | CRUD                                                                 |
| RF11   | Planos de Saúde            | CRUD                                                                 |
| RF12   | Gestão Financeira          | Registros financeiros validados                                      |
| RF13   | Endereços                  | Associados aos pacientes                                             |
| RF14   | Validação                  | Schemas utilizando Cerberus                                           |
| RF15   | Tratamento de Erros        | Respostas HTTP padronizadas                                          |
| RF16   | Notificações por E-mail    | E-mails de boas-vindas, recuperação de senha e tokens de verificação |
| RF17   | Relatórios de Ocupação     | Ocupação de leitos por departamento e período                       |
| RF18   | Estatísticas Hospitalares  | Estatísticas por médico, especialidade e período                     |
| RF19   | Analytics Financeiro       | Receita, ticket médio e pagamentos pendentes                          |
| RF20   | Tempo Médio de Internação  | Duração média das internações                                        |
| RF21   | Indicadores de Readmissão  | Pacientes readmitidos dentro de um período configurável             |
| RF22   | Lembrete de Consulta       | Lembretes automatizados por e-mail/webhook                           |
| RF23   | Evento de Leito Disponível | Publicar evento quando um leito ficar disponível                     |
| RF24   | Faturamento Automático     | Gerar faturamento na alta do paciente                                |
| RF25   | Relatório Diário           | Gerar relatórios PDF/CSV automaticamente                             |
| RF26   | Monitoramento de Exceções  | Logs estruturados e alertas                                          |

---

# 4. Requisitos Não Funcionais

| Código | Categoria                  | Descrição                                   |
| ------ | -------------------------- | ------------------------------------------- |
| NFR01  | Arquitetura                | Clean Architecture                          |
| NFR02  | Testabilidade              | Repositórios e casos de uso simuláveis      |
| NFR03  | Manutenibilidade           | Domínio isolado da infraestrutura            |
| NFR04  | Extensibilidade            | Injeção de dependência através de composers |
| NFR05  | Consistência               | Modelo de resposta HTTP padronizado          |
| NFR06  | Observabilidade            | Logs estruturados                            |
| NFR07  | Segurança                  | Autenticação e RBAC (futuro)                 |
| NFR08  | Portabilidade              | Configuração baseada em ambiente             |
| NFR09  | Integridade dos Dados      | Validações de domínio e chaves estrangeiras |
| NFR10  | Analytics                  | Camada de analytics extensível               |
| NFR11  | Isolamento SMTP            | Abstração do provedor de e-mail              |
| NFR12  | Automação Escalável        | Futura integração com filas                  |

---

# 5. Arquitetura

Fluxo:

Route
    ↓
Adapter
    ↓
Controller
    ↓
Use Case
    ↓
Repository Interface
    ↓
Infrastructure Repository
    ↓
Database

Camadas do projeto:

domain/
data/
infra/
presentation/
main/
validation/
errors/

Módulos futuros:

* analytics/
* automation/
* event bus

---

# 6. Modelos de Dados

Exemplos de entidades:

* Paciente
* Médico
* Agendamento
* Internação
* Leito
* Registro Financeiro

Os analytics agregados são calculados dinamicamente e não são persistidos.

---

# 7. Analytics

Métricas implementadas:

* Taxa de ocupação de leitos
* Consultas por especialidade
* Receita por operadora de plano de saúde
* Faturamento médio
* Duração média das internações
* Taxa de readmissão

---

# 8. Automação

Fluxos automatizados:

* Lembretes de consultas
* Faturamento automático
* Eventos de disponibilidade de leitos
* Geração diária de relatórios
* Monitoramento de exceções

Projetado para ser compatível com agendadores e futuras integrações com sistemas de filas.

---

# 9. Tratamento de Erros

Formato padrão de resposta:

{
  "error": [
    {
      "title": "HttpBadRequestError",
      "message": "Detalhes do erro"
    }
  ]
}

O mapeamento centralizado de erros é tratado por errors/error_handler.py.

---

# 10. Serviço de E-mail

Interface:

SMTPServiceInterface

Implementação:

SMTPEmailService

Templates suportados:

* Boas-vindas
* Token de verificação
* Recuperação de senha
* Reenvio de token

Suporte futuro:

* Lembretes de consultas
* Relatórios diários

---

# 11. Estrutura do Projeto

src/
├── domain/
├── data/
├── infra/
├── presentation/
├── main/
├── validation/
├── errors/
└── analytics/ (planejado)

---

# 12. Variáveis de Ambiente

DB_USER=
DB_PASSWORD=
DB_HOST=localhost
DB_PORT=3306
DB_NAME=

MAIL_USERNAME=
MAIL_PASS=
MAIL_FROM=
MAIL_FROM_NAME=Hospital
MAIL_PORT=587
MAIL_SERVER=smtp.gmail.com

---

# 13. Execução do Projeto

python3 -m venv venv

source venv/bin/activate

pip install -r requirements.txt

cp src/.env.example src/.env

python run.py

URL base:

http://127.0.0.1:8000/v1/

---

# 14. Testes

pytest -q

Estratégia de testes:

* Spies de repositórios
* Testes isolados dos casos de uso
* Analytics e automações simulados

---

# 15. Roadmap

| Fase | Objetivo                     |
| ---- | ---------------------------- |
| 1    | Módulos CRUD                 |
| 2    | Serviço de e-mail            |
| 3    | Analytics                    |
| 4    | Automação                    |
| 5    | Relatórios diários           |
| 6    | Tarefas assíncronas e filas  |
| 7    | Autenticação e RBAC          |

---

# 16. Licença

Uso educacional e interno.

---

# 17. Resumo Técnico

Backend modular desenvolvido para escalabilidade e manutenibilidade através de Clean Architecture. O sistema separa as regras de negócio da infraestrutura, facilitando a extensão com analytics, automações, fluxos orientados a eventos e futuras integrações sem impactar o domínio principal.

---

# 18. Analytics Planejados

| Código | Análise                      | Objetivo                          |
| ------ | ---------------------------- | --------------------------------- |
| AD01   | Tendência de Ocupação        | Ocupação de leitos ao longo do tempo |
| AD02   | Consultas por Especialidade  | Análise da demanda hospitalar     |
| AD03   | Analytics de Receita         | Análise financeira                |
| AD04   | Análise de Readmissões       | Monitoramento de readmissões      |
| AD05   | Consumo de Medicamentos      | Previsão de estoque               |

Estratégia de implementação:

* Módulo dedicado analytics/
* Endpoints somente para leitura
* Consultas SQL de agregação
* Funções puras para facilitar os testes

---

# 19. Automações Planejadas

| Código | Automação                              |
| ------ | -------------------------------------- |
| AUTO01 | Alerta de baixa disponibilidade de leitos |
| AUTO02 | Lembrete de pagamentos pendentes       |
| AUTO03 | Notificação de exames atrasados        |
| AUTO04 | Detecção de internações prolongadas    |
| AUTO05 | Alerta de reposição de medicamentos   |

Implementação inicial:

* Agendador
* Serviços de automação
* Dispatcher central
* Futuro suporte a Redis/Celery

---

# 20. Plano de Desenvolvimento Incremental

| Etapa | Entregável              |
| ----- | ----------------------- |
| 1     | Módulo de analytics     |
| 2     | Analytics financeiro    |
| 3     | Agendador               |
| 4     | Fluxos de automação     |
| 5     | Métricas materializadas |
| 6     | Abstração de filas      |

Princípios de design:

* Sem dependências de Big Data
* Analytics baseado em SQL
* Notificações baseadas em SMTP
* Funções puras altamente testáveis
