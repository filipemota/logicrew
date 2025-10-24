# LogiCrew

**LogiCrew** é um sistema completo de **gestão logística de equipes**, desenvolvido para otimizar a administração de colaboradores, escalas de trabalho, deslocamentos e recursos operacionais em ambientes corporativos e offshore.

O sistema oferece módulos integrados que garantem **eficiência, conformidade e segurança**, facilitando a tomada de decisão e a comunicação entre gestores e colaboradores.

---

## 🚀 Funcionalidades por Módulo

### 1. Gestão de Colaboradores
- Cadastro completo de colaboradores (nome, matrícula, função, contato).  
- Controle de documentação obrigatória (certificados, exames, treinamentos).  
- Alertas de validade de documentos.  
- Histórico de cargos, escalas e transferências.  

### 2. Escalas e Turnos
- Criação e gestão de escalas de trabalho.  
- Alocação automática ou manual de colaboradores.  
- Controle de folgas, substituições e revezamento.  
- Visualização em calendário ou lista.  

### 3. Manifesto de Voo
- Registro de voos (origem, destino, horários).  
- Inclusão de colaboradores como passageiros.  
- Controle de check-in e embarque.  
- Relatórios de ocupação de voos.  

### 4. Logística e Requisições
- Solicitação de transporte, hospedagem ou materiais.  
- Aprovação ou rejeição de requisições por gestores.  
- Rastreamento de requisições em andamento.  
- Relatórios de custos e uso de recursos.  

### 5. Relatórios e Dashboards
- Relatórios de documentação, escalas, voos e logística.  
- Indicadores de conformidade e ocupação.  
- Exportação de relatórios em PDF, Excel ou CSV.  

### 6. Autenticação e Permissões
- Login de usuários e gestores.  
- Perfis de acesso (admin, manager, collaborator).  
- Controle de permissões por módulo.  
- Registro de atividades (logs).

---

## 🗄️ Estrutura do Banco de Dados (MariaDB/MySQL)

### Tabelas e Campos

#### 1. `users`
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INT AUTO_INCREMENT | Chave primária |
| name | VARCHAR(100) | Nome do usuário |
| email | VARCHAR(100) UNIQUE | Email do usuário |
| password | VARCHAR(255) | Senha criptografada |
| role | ENUM('admin','manager','collaborator') | Perfil de acesso |
| created_at | TIMESTAMP | Data de criação |
| updated_at | TIMESTAMP | Data de atualização |

#### 2. `collaborators`
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INT AUTO_INCREMENT | Chave primária |
| user_id | INT | Referência para `users.id` |
| employee_number | VARCHAR(50) UNIQUE | Número funcional |
| position | VARCHAR(100) | Cargo ou função |
| department | VARCHAR(100) | Departamento |
| phone | VARCHAR(20) | Telefone |
| email | VARCHAR(100) | Email alternativo |
| status | ENUM('active','inactive') | Situação do colaborador |
| created_at | TIMESTAMP | Data de criação |
| updated_at | TIMESTAMP | Data de atualização |

#### 3. `documents`
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INT AUTO_INCREMENT | Chave primária |
| collaborator_id | INT | Referência para `collaborators.id` |
| type | VARCHAR(100) | Tipo de documento |
| document_number | VARCHAR(100) | Número do documento |
| issue_date | DATE | Data de emissão |
| expiry_date | DATE | Data de validade |
| file_path | VARCHAR(255) | Caminho do arquivo |
| status | ENUM('valid','expired','warning') | Situação do documento |
| created_at | TIMESTAMP | Data de criação |
| updated_at | TIMESTAMP | Data de atualização |

#### 4. `shifts`
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INT AUTO_INCREMENT | Chave primária |
| collaborator_id | INT | Referência para `collaborators.id` |
| start_datetime | DATETIME | Início do turno |
| end_datetime | DATETIME | Fim do turno |
| shift_type | VARCHAR(50) | Tipo de turno |
| location | VARCHAR(100) | Local de trabalho |
| status | ENUM('scheduled','completed','canceled') | Status da escala |
| created_at | TIMESTAMP | Data de criação |
| updated_at | TIMESTAMP | Data de atualização |

#### 5. `flights`
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INT AUTO_INCREMENT | Chave primária |
| flight_number | VARCHAR(50) | Número do voo |
| origin | VARCHAR(100) | Origem |
| destination | VARCHAR(100) | Destino |
| departure_datetime | DATETIME | Horário de partida |
| arrival_datetime | DATETIME | Horário de chegada |
| created_at | TIMESTAMP | Data de criação |
| updated_at | TIMESTAMP | Data de atualização |

#### 6. `flight_passengers`
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INT AUTO_INCREMENT | Chave primária |
| flight_id | INT | Referência para `flights.id` |
| collaborator_id | INT | Referência para `collaborators.id` |
| seat_number | VARCHAR(10) | Assento |
| status | ENUM('scheduled','boarded','canceled') | Status do passageiro |

#### 7. `logistics_requests`
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INT AUTO_INCREMENT | Chave primária |
| collaborator_id | INT | Referência para `collaborators.id` |
| request_type | ENUM('transport','accommodation','material') | Tipo de requisição |
| description | TEXT | Descrição da requisição |
| status | ENUM('pending','approved','rejected','completed') | Status da requisição |
| created_at | TIMESTAMP | Data de criação |
| updated_at | TIMESTAMP | Data de atualização |

#### 8. `approvals`
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INT AUTO_INCREMENT | Chave primária |
| request_id | INT | Referência para `logistics_requests.id` |
| approved_by | INT | Referência para `users.id` |
| approval_status | ENUM('approved','rejected') | Status da aprovação |
| approval_date | TIMESTAMP | Data da aprovação |
| comments | TEXT | Observações |

---

### Relacionamentos principais
- **users → collaborators** (1:1)  
- **collaborators → documents** (1:N)  
- **collaborators → shifts** (1:N)  
- **flights → flight_passengers → collaborators** (N:M)  
- **collaborators → logistics_requests** (1:N)  
- **logistics_requests → approvals** (1:N)  

---

