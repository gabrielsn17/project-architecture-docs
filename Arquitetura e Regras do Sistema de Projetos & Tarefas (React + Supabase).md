# **Arquitetura e Regras do Sistema de Projetos & Tarefas (React \+ Supabase)**

# 

# **PARTE 1 — VISÃO ESTRATÉGICA DO PRODUTO**

## **1.1 Contexto e Natureza do Projeto**

Este sistema nasceu como um **projeto de teste orientado à automação com IA**, cujo objetivo principal é validar e evoluir um pipeline automatizado de:

* Geração de telas (GPT → Figma Make)

* Ingestão automatizada no código (Cursor \+ MCP)

* Integração com backend (Supabase BaaS)

* Padronização arquitetural e governança técnica

Apesar de ser um projeto de teste, ele já opera como um **sistema funcional de gestão de projetos e tarefas**, com regras claras de autorização, estrutura modular e separação formal de responsabilidades.

O sistema não é atualmente multi-tenant (não há conceito de organização ou empresa), mas sua arquitetura permite futura evolução para esse modelo.

---

## **1.2 Problema que o Sistema Resolve**

O sistema resolve dois problemas principais:

### **1️⃣ Gestão estruturada de projetos e tarefas**

Permite que um administrador:

* Crie projetos

* Atribua usuários a projetos

* Gerencie tarefas dentro de cada projeto

* Acompanhe métricas executivas

Permite que usuários:

* Visualizem apenas os projetos aos quais estão atribuídos

* Criem e administrem tarefas nesses projetos

* Marquem tarefas como concluídas

* Trabalhem de forma organizada com prioridade definida

---

### **2️⃣ Teste e validação de automação arquitetural com IA**

O projeto também serve como laboratório controlado para validar:

* Geração automática de UI

* Normalização automática de código

* Padronização estrutural via regras formais

* Governança arquitetural automatizada

Nesse contexto, o sistema funciona como **base operacional para testes de automação técnica avançada**.

---

## **1.3 Conceito Central do Sistema**

O sistema é estruturado sobre três pilares fundamentais:

### **🔹 Projetos**

Unidade organizadora macro.

Um projeto:

* É criado exclusivamente por um administrador.

* Pode ter um ou mais usuários atribuídos.

* Contém tarefas executáveis.

* Pode ser monitorado por métricas agregadas.

---

### **🔹 Tarefas**

Unidade operacional de execução.

Cada tarefa:

* Pertence a um projeto.

* Possui prioridade: **Baixa, Média ou Alta**.

* Pode ser marcada como concluída.

* Representa uma ação executável concreta.

A prioridade permite:

* Organização estratégica.

* Métricas de criticidade.

* Análise de carga de trabalho.

---

### **🔹 Usuários**

Entidades autenticadas via Supabase Auth.

Usuários:

* Podem estar atribuídos a múltiplos projetos.

* Trabalham apenas nos projetos permitidos.

* Não criam projetos.

* Não gerenciam outros usuários.

---

## **1.4 Perfis e Modelo de Autoridade**

O sistema possui dois papéis formais:

### **👤 Usuário (user)**

Pode:

* Visualizar projetos atribuídos

* Criar, editar e concluir tarefas nesses projetos

* Atuar operacionalmente

Não pode:

* Criar projetos

* Excluir projetos

* Gerenciar usuários

* Acessar métricas globais da plataforma

---

### **👑 Administrador (admin)**

É o papel de autoridade máxima operacional.

Pode:

* Criar, editar e excluir projetos

* Atribuir e desatribuir usuários a projetos

* Criar, editar e excluir tarefas

* Excluir usuários

* Visualizar métricas globais

* Acompanhar desempenho agregado

O administrador possui uma visão executiva com indicadores como:

* Total de projetos

* Total de usuários

* Total de tarefas

* Percentual de tarefas concluídas vs pendentes

* Top usuários por projetos atribuídos

* Projetos com maior volume de tarefas

* Atividade recente da plataforma

---

## **1.5 O “Coração” do Sistema**

O núcleo estrutural do sistema é a relação:

**Projeto → Usuários atribuídos → Tarefas com prioridade → Métricas**

A lógica central é:

1. O administrador cria um projeto.

2. O administrador atribui usuários ao projeto.

3. Dentro do projeto, tarefas são criadas.

4. Cada tarefa possui prioridade e status.

5. Métricas agregadas são derivadas desse conjunto.

Essa relação permite:

* Controle organizacional.

* Monitoramento de carga de trabalho.

* Análise de desempenho.

* Evolução futura para estruturas mais sofisticadas.

---

## **1.6 Direção de Evolução Planejada**

O sistema está sendo preparado para evoluir de um modelo simples de tarefas soltas para um modelo estruturado por **pipelines**.

### **Evolução prevista:**

* Introdução de **Pipelines** dentro de projetos.

* Cada pipeline representará uma iniciativa estruturada (ex: “Atualização Node 20”).

* Dentro do pipeline:

  * Tarefas organizadas por responsável.

  * Fase de execução.

  * Encerramento e histórico.

* Histórico permanente de pipelines concluídos.

Além disso, está planejado:

* Dashboard individual por usuário.

* Monitoramento de produtividade.

* Métricas de desempenho.

* Histórico de atuação.

* Volume de tarefas resolvidas.

* Projetos ativos e participação.

Essa evolução aproxima o sistema de modelos como:

* Azure DevOps

* Plataformas corporativas de gestão estruturada

---

## **1.7 Princípios Arquiteturais Fundamentais**

O sistema segue princípios claros:

### **1️⃣ Separação de Responsabilidades**

* Frontend não decide autorização.

* Backend (RLS) é a autoridade final de acesso.

### **2️⃣ Segurança Estrutural**

* Nenhuma lógica sensível depende apenas da UI.

* Supabase RLS é a camada final de defesa.

### **3️⃣ Fonte Única de Verdade**

* Autenticação centralizada.

* Perfil e role definidos no backend.

* Métricas derivadas de dados reais.

### **4️⃣ Evolução Controlada**

* Estrutura preparada para expansão.

* Sem acoplamento prematuro.

* Base sólida antes de adicionar complexidade (pipelines, auditoria, etc.).

---

## **1.8 Limites Atuais do Sistema**

Atualmente, o sistema:

* Não possui conceito de organização ou empresa.

* Não possui times.

* Não possui controle granular por permissão (apenas role user/admin).

* Não possui auditoria detalhada de ações.

* Não possui billing.

* Não é multi-tenant.

Essas decisões são conscientes e mantêm o foco na estabilidade arquitetural antes de expansão funcional.

# **PARTE 2 — ARQUITETURA GERAL DO SISTEMA**

## **2.1 Visão Arquitetural em Camadas**

O sistema é estruturado em três macrocamadas bem definidas:

### **1️⃣ Camada de Interface (Frontend)**

Responsável por:

* Renderização da interface

* Orquestração de navegação

* Disparo de ações de domínio

* Gestão de estado assíncrono

* Tratamento de erros de integração

Essa camada **não é responsável por decisões finais de autorização**.  
 Ela reflete e respeita as regras impostas pelo backend.

---

### **2️⃣ Camada de Plataforma (Backend como Serviço — BaaS)**

Responsável por:

* Autenticação e sessão

* Persistência de dados

* Autorização por linha (Row Level Security)

* Execução de regras sensíveis

* Armazenamento de arquivos

Essa camada é a **autoridade final sobre segurança e integridade dos dados**.

---

### **3️⃣ Camada de Automação de Desenvolvimento**

Embora não faça parte do runtime do sistema, ela é parte estrutural do projeto.

Essa camada envolve:

* Geração assistida de interface

* Ingestão automatizada no código

* Normalização estrutural

* Resolução automatizada de assets

* Aplicação de padrões obrigatórios

Essa disciplina reduz divergência entre design e código e mantém consistência arquitetural.

---

## **2.2 Stack Tecnológica (Visão Estrutural)**

### **Frontend**

* React \+ TypeScript

* Bundler moderno (Vite)

* Roteamento estruturado

* Camada de serviços para acesso a dados

* Gerenciamento de estado assíncrono

* Testes automatizados por página

### **Backend**

* Supabase Auth (gestão de identidade)

* PostgreSQL (modelo relacional)

* Row Level Security (controle de acesso)

* Triggers para regras sensíveis

* Storage com políticas próprias

---

## **2.3 Princípio Estrutural Central**

O sistema adota o seguinte princípio:

O backend é a fonte única de verdade para autorização e integridade.

Isso implica:

* O frontend nunca assume permissões.

* O frontend pode ocultar elementos por UX, mas não substitui regras do backend.

* O banco de dados impõe controle real via RLS.

* Regras que dependem de comparação de estado (ex.: imutabilidade de campo) vivem em triggers, nunca em policies.

---

## **2.4 Modelo Arquitetural de Domínio**

O domínio do sistema é estruturado sobre:

* Usuários autenticados

* Projetos

* Associação de usuários a projetos

* Tarefas vinculadas a projetos

* Prioridades e estados de execução

A arquitetura permite:

* Associação múltipla de usuários a projetos

* Separação clara entre criação administrativa e execução operacional

* Evolução para estruturas mais complexas (como pipelines)

---

## **2.5 Fluxo Arquitetural de Execução**

### **Autenticação**

1. Usuário autentica.

2. O sistema identifica o papel (admin ou user).

3. A navegação inicial é determinada pelo papel.

### **Operação**

* O administrador atua em nível estrutural (projetos e atribuições).

* O usuário atua em nível operacional (tarefas dentro de projetos permitidos).

### **Autorização**

* Toda leitura ou modificação passa por regras de RLS.

* Nenhuma ação depende exclusivamente de validação no frontend.

---

## **2.6 Separação de Responsabilidades**

### **Frontend**

* Interface

* Experiência do usuário

* Composição de dados

* Orquestração de chamadas

### **Backend**

* Persistência

* Segurança

* Autorização

* Regras estruturais

Essa separação evita:

* Acoplamento indevido

* Lógica de autorização no cliente

* Inconsistência entre ambientes

---

## **2.7 Governança Estrutural do Projeto**

O projeto adota regras formais de organização:

* Estrutura modular por páginas e features

* Camada de serviços isolando acesso a dados

* Padronização de estilos

* Testes obrigatórios por página

* Separação entre regra de negócio e camada visual

Isso garante:

* Escalabilidade

* Manutenção previsível

* Redução de dívida técnica

* Consistência entre telas

---

## **2.8 Arquitetura de Segurança**

A segurança é baseada em:

* Autenticação robusta

* RLS como camada final de defesa

* Proibição de uso de chaves privilegiadas no frontend

* Storage com políticas explícitas

* Separação entre autorização e validação de regra de negócio

O modelo evita:

* Controle apenas visual

* Lógica sensível exposta no cliente

* Permissões implícitas

---

## **2.9 Evolução Arquitetural Planejada**

O sistema está sendo preparado para evoluir de:

Modelo atual:

* Projetos com tarefas atribuídas

Para um modelo mais estruturado:

* Pipelines dentro de projetos

* Fases de execução

* Histórico de ciclos concluídos

* Monitoramento individual de desempenho

* Métricas por usuário

* Análise de produtividade

A arquitetura atual já está organizada de forma a permitir essa expansão sem reescrever a base estrutural.

---

## **2.10 Limites Arquiteturais Atuais**

Atualmente o sistema:

* Não possui conceito de organização multi-tenant

* Não possui times formais

* Trabalha com dois papéis fixos (admin e user)

* Não possui auditoria completa de ações

* Não possui billing ou modelo comercial

Essas decisões mantêm o foco na estabilidade estrutural antes da expansão funcional.

# **PARTE 3 — ARQUITETURA DO FRONTEND**

*(Estrutura de Pastas, Providers, Services, Routing e Regras Estruturais)*

## **3.1 Papel do Frontend no Sistema**

O frontend é responsável por materializar a experiência do produto, garantindo:

* composição de telas e fluxos (user/admin)

* navegação determinística entre rotas

* integração direta com Supabase (Auth/DB/Storage) por camadas internas

* consistência arquitetural entre páginas e features

* governança de padrões para suportar ingestão recorrente de telas via automação

**Princípio central:**  
 O frontend pode refletir regras (ex.: esconder ações), mas **não é autoridade final de autorização**. A segurança real é imposta pelo backend (RLS/Storage policies).

---

## **3.2 Stack e Convenções de Implementação**

* React \+ TypeScript

* Vite

* React Router DOM (roteamento)

* TanStack React Query (estado assíncrono/cache)

* Ícones: `lucide-react` (única fonte permitida)

* Estilos: **CSS Modules** como padrão por página

* Testes: Vitest \+ React Testing Library, com cobertura mínima por página

**Importante:**

* Código vindo do Figma Make é tratado como **input semântico** e passa por normalização.

* A base do projeto privilegia previsibilidade, tipagem e testabilidade.

---

## **3.3 Estrutura de Pastas (Contrato do Projeto)**

A estrutura é organizada para separar “UI de página” de “lógica de domínio” e “integração”.

```textproto
src/
  main.tsx
  App.tsx

  app/
    providers/
    routes/

  pages/
    <PageName>/
      <PageName>.tsx
      <PageName>.module.css
      <PageName>.test.tsx

  features/
    (domínios e regras de negócio)

  components/
    ui/
    layout/

  services/
    (clientes e wrappers de integração)

  lib/
    (helpers e utilitários)

  assets/
    (imagens exportadas via MCP)

  styles/
    (estilos globais mínimos, se necessários)

  tests/
    setup/
    utils/


```

### **Regras estruturais**

* **pages/**: compõe UI e layout da tela (sem regra complexa).

* **features/**: concentra lógica, hooks, validações e regras por domínio.

* **components/ui**: componentes genéricos, reutilizáveis e previsíveis.

* **components/layout**: componentes estruturais (ex.: headers reutilizados, wrappers de seção), sem assumir um “AppShell” obrigatório.

* **services/**: acesso externo encapsulado (Supabase direto, e eventual infraestrutura HTTP para integrações futuras).

* **assets/**: sempre resolvido via MCP quando a origem é Figma.

---

## **3.4 Imports Absolutos e Organização de Código**

O projeto utiliza imports absolutos com alias:

* `@/*` aponta para `src/*`

Objetivo:

* evitar paths longos (`../../../`)

* melhorar legibilidade e refatoração

* padronizar importação entre páginas/features

---

## **3.5 Bootstrap e Providers Globais**

A configuração global do app fica concentrada em `app/providers`, tipicamente englobando:

* Router (ex.: BrowserRouter)

* QueryClientProvider (React Query)

**Regra:**

* `main.tsx` injeta um **único** provider root (ex.: `<AppProviders>`).

* Páginas e features não criam providers globais por conta própria.

---

## **3.6 Roteamento e Guards (Controle de Acesso)**

### **Rotas centralizadas**

* Rotas são definidas em um único módulo (ex.: `app/routes`).

* `App.tsx` deve manter-se minimalista e previsível (apenas renderiza o roteamento).

### **Proteção por sessão e papel (role)**

O roteamento separa:

* rotas públicas (login/signup/recovery)

* rotas privadas (fluxos user e admin)

**Regras essenciais:**

* A decisão de redirecionamento deve ser determinística e centralizada.

* Guards não redirecionam durante “loading” de sessão/role (evita loop e flicker).

* Navegação é parte do contrato de UI: listas de entidades devem navegar para detalhes quando aplicável, mesmo com dados mockados.

---

## **3.7 Integração com Supabase (Direto, via Camadas Internas)**

O frontend consome Supabase **diretamente**, porém a chamada não deve ficar espalhada pela UI.

**Padrão recomendado:**

* `services/` encapsula operações (auth, queries, storage).

* `features/` oferece hooks e “use-cases” que orquestram chamadas e estados.

* `pages/` consome hooks e apenas compõe a interface.

**Objetivo:**

* consistência de erros/loading

* reduzir duplicação

* facilitar testes

* facilitar mudanças futuras (ex.: introduzir Edge Functions ou API intermediária sem reescrever UI)

Observação: existe infraestrutura HTTP/Axios disponível, mas não é a camada obrigatória no fluxo atual do Supabase. Ela permanece como base para integrações externas e/ou evolução arquitetural.

---

## **3.8 Organização por Domínios (Features)**

A pasta `features/` organiza regras por domínio, por exemplo:

* **auth**: sessão, role, validações de formulário, fluxos de login/signup/recovery.

* **projects**: listagem, criação/edição, atribuição de usuários, acesso permitido.

* **tasks**: CRUD, prioridade, status de conclusão.

* **admin**: operações administrativas e visões agregadas (sem fixar layouts específicos).

**Regra:**  
 Validações e regras puras não ficam em páginas; páginas só compõem UI.

---

## **3.9 Padrões de UI e Estilo (CSS Modules)**

* CSS Modules é padrão por página.

* Não manter Tailwind/utility classes no output final.

* Evitar inline styles.

* Styled-components apenas se explicitamente adotado (e isolado em arquivo próprio).

**Contratos críticos de UI (reutilizáveis):**

* Inputs com ícone seguem wrapper único (ícone absoluto com `pointer-events: none`).

* Inputs com ação (ex.: toggle senha) não podem roubar clique/foco do input.

* Tipografia é contrato: tamanho/peso/line-height devem refletir a origem (Figma Make).

---

## **3.10 Testes (Contrato Mínimo por Página)**

Cada página possui testes mínimos para garantir:

* renderização básica

* existência do container principal

* presença de pelo menos um elemento-chave da tela

Objetivo:

* impedir regressões silenciosas durante ingestões sucessivas

* aumentar confiabilidade do pipeline de automação

---

## **3.11 Regra de Ouro: Ingestão Figma Make como Input Semântico**

Quando telas chegam via Figma Make:

* tratar como “intenção visual e estrutural”

* normalizar para o padrão do projeto:

  * remover UI kits externos

  * migrar estilos para CSS Modules

  * tipar corretamente

  * criar arquivos padrão da página (tsx/css/test)

  * registrar rotas

  * resolver assets via MCP

* manter idempotência (não sobrescrever silenciosamente)

---

## **3.12 Ausência de Layout Global Único (por decisão)**

O projeto **não assume** um AppShell global único aplicado a todas as rotas.

Em vez disso:

* utiliza componentes reutilizáveis (ex.: Header) quando apropriado

* permite que cada página (ou conjunto de páginas) defina seu próprio arranjo

* evita acoplamento prematuro de layout, especialmente importante em um projeto que evolui rapidamente via ingestão automatizada

# **PARTE 4 — ARQUITETURA DO BACKEND**

*(Supabase: Auth, Database, RLS, Triggers, Storage e Regras de Segurança)*

## **4.1 Papel do Backend na Arquitetura**

O backend é implementado como **Backend as a Service (BaaS)** utilizando Supabase.

Ele é responsável por:

* Autenticação e gestão de sessão

* Persistência relacional

* Autorização por linha (Row Level Security)

* Execução de regras sensíveis via triggers

* Armazenamento de arquivos com políticas próprias

**Princípio fundamental:**  
 O backend é a autoridade final sobre acesso e integridade dos dados.

O frontend nunca decide permissões reais.

---

## **4.2 Camadas do Backend**

O backend está organizado conceitualmente em quatro camadas:

### **1️⃣ Autenticação (Auth)**

* Gestão de identidade do usuário

* Login, signup e recuperação de senha

* Sessões persistentes

* Emissão e validação de tokens

A autenticação é independente do modelo de domínio, mas integrada a ele por meio do vínculo entre identidade e perfil.

---

### **2️⃣ Modelo Relacional (Database)**

O domínio principal é composto por:

* Usuários autenticados

* Perfis com papel (role)

* Projetos

* Associação de usuários a projetos

* Tarefas vinculadas a projetos

* Arquivos associados a projetos e tarefas

A estrutura relacional garante:

* Integridade referencial

* Associação múltipla entre usuários e projetos

* Separação entre criação administrativa e execução operacional

---

### **3️⃣ Autorização por Linha (RLS)**

O sistema utiliza Row Level Security como camada de controle real de acesso.

RLS determina:

* Quem pode visualizar uma linha

* Quem pode inserir

* Quem pode atualizar

* Quem pode excluir

Regras estruturais obrigatórias:

* Policies nunca utilizam `OLD` ou `NEW`

* Policies não validam imutabilidade de campos

* Policies baseiam-se apenas em:

  * `auth.uid()`

  * colunas da linha atual

  * funções auxiliares (ex.: verificação de admin)

Separação clara:

* **RLS decide “quem pode acessar”**

* **Triggers decidem “se a alteração é válida”**

Essa separação evita lógica confusa e falhas de segurança.

---

### **4️⃣ Triggers (Validação de Regras Sensíveis)**

Triggers são utilizados quando é necessário:

* Comparar estado anterior e novo

* Impedir alteração de campos sensíveis

* Garantir regras de consistência complexas

Triggers são executados no banco e não dependem do frontend.

---

## **4.3 Modelo de Papéis (Roles)**

O sistema possui dois papéis fixos:

* `user`

* `admin`

### **Usuário (user)**

* Atua apenas em projetos aos quais está associado

* Interage operacionalmente com tarefas

* Não cria projetos

* Não gerencia usuários

### **Administrador (admin)**

* Pode criar, editar e excluir projetos

* Pode atribuir e desatribuir usuários a projetos

* Pode excluir usuários

* Possui visão global da plataforma

O papel é armazenado no perfil associado ao usuário autenticado.

---

## **4.4 Modelo de Associação**

O sistema suporta:

* Projetos com múltiplos usuários associados

* Usuários associados a múltiplos projetos

Essa associação é parte central da autorização:

* Acesso a tarefas depende do acesso ao projeto.

* Acesso a projeto depende da associação ou do papel admin.

---

## **4.5 Storage e Arquivos**

O sistema utiliza armazenamento de arquivos para:

* Capas de projetos

* Arquivos associados a tarefas

O armazenamento é separado do banco relacional, mas integrado por metadados.

### **Regras estruturais de Storage:**

* Buckets possuem políticas próprias (independentes do RLS do banco).

* Upload, leitura e exclusão são controlados por policies em `storage.objects`.

* O path do arquivo deve seguir padrão consistente.

* O frontend nunca utiliza credenciais privilegiadas.

* A chave utilizada no cliente é sempre a chave pública (anon).

Erros de upload normalmente decorrem de:

* Ausência de policy

* Path incompatível com regra da policy

* Falha de associação do usuário ao projeto

---

## **4.6 Segurança Arquitetural**

O modelo de segurança é baseado em:

* Autenticação robusta

* Autorização por linha

* Políticas explícitas no Storage

* Separação entre autorização e validação

* Proibição absoluta de uso de chaves privilegiadas no frontend

O sistema não depende de:

* Flags de UI

* Controle visual de botões

* Confiança no cliente

A segurança é estrutural.

---

## **4.7 Modelo de Sessão e Papel**

Após autenticação:

1. O usuário obtém sessão válida.

2. O sistema identifica seu papel.

3. O acesso às rotas e aos dados passa a respeitar esse papel.

A decisão de role é backend-driven, não frontend-driven.

---

## **4.8 Escalabilidade Arquitetural**

A estrutura atual permite evoluir para:

* Pipelines dentro de projetos

* Fases estruturadas

* Histórico fechado de ciclos

* Monitoramento individual de produtividade

* Métricas agregadas por usuário

* Auditoria de eventos

A modelagem relacional e a separação de responsabilidades permitem essa expansão sem ruptura estrutural.

---

## **4.9 Limites Atuais**

O sistema atualmente:

* Não é multi-tenant

* Não possui organizações formais

* Não possui times hierárquicos

* Não possui controle granular além de user/admin

* Não possui auditoria detalhada por evento

* Não possui billing

Esses limites são conscientes e mantêm o foco na estabilidade arquitetural antes da expansão funcional.

# **PARTE 5 — MODELO CONCEITUAL DE DADOS**

*(Entidades e Relações — sem dependência de nomes específicos de tabelas)*

## **5.1 Visão Geral do Modelo**

O modelo de dados do sistema é relacional e foi desenhado para suportar:

* Autenticação centralizada (identidade do usuário)

* Separação entre identidade e perfil de aplicação (papel/role)

* Gestão de projetos com múltiplos usuários associados

* Gestão de tarefas vinculadas a projetos

* Classificação de tarefas por prioridade e status

* Suporte a arquivos (capas de projeto e anexos de tarefas)

* Evolução futura para entidades adicionais (ex.: pipelines)

O foco do modelo é permitir governança administrativa e execução operacional com segurança.

---

## **5.2 Entidades Principais**

### **1️⃣ Usuário (Identidade)**

Representa a identidade autenticada (login/sessão).

Características:

* Criado e mantido pelo provedor de autenticação

* Serve como âncora para todo o resto do domínio

* Não deve ser diretamente utilizado como “fonte de dados de aplicação” na UI (por estabilidade e segurança)

---

### **2️⃣ Perfil de Aplicação (Perfil/Role)**

Representa informações de aplicação associadas ao usuário.

Inclui:

* Papel (role): `user` ou `admin`

* Informações úteis para exibição/gestão (ex.: email espelhado para listagens administrativas)

Racional:

* Evita depender de estruturas internas do sistema de autenticação no frontend

* Centraliza decisões de role e permissões do produto

---

### **3️⃣ Projeto**

Unidade macro de organização.

Características:

* Criado e gerido por admin

* Pode ter atributos descritivos (ex.: título/descrição)

* Pode ter uma capa (arquivo) opcional

* É a unidade à qual usuários são atribuídos

* É a unidade à qual tarefas pertencem

---

### **4️⃣ Associação Usuário ↔ Projeto (Membership)**

Entidade de relacionamento que define quais usuários participam de quais projetos.

Características:

* Um usuário pode estar em múltiplos projetos

* Um projeto pode ter múltiplos usuários

* Essa associação é o principal insumo de autorização para usuários não-admin

Observação:

* A associação pode evoluir para incluir atributos adicionais (ex.: função no projeto, datas, status), sem alterar a arquitetura base.

---

### **5️⃣ Tarefa**

Unidade operacional executável.

Características:

* Sempre pertence a um projeto

* Pode ser criada/gerida dentro do escopo do projeto

* Possui:

  * título/descrição (conceitual)

  * prioridade (Baixa/Média/Alta)

  * status (ex.: concluída vs pendente)

A prioridade é um contrato funcional importante, pois sustenta:

* organização do trabalho

* filtros e ordenações

* métricas e visão gerencial

---

### **6️⃣ Arquivo de Projeto (Capa)**

Arquivo opcional associado ao projeto.

Características:

* armazenado em Storage

* referenciado por metadados (no domínio)

* regras de acesso seguem o acesso ao projeto (membership ou admin)

---

### **7️⃣ Arquivo de Tarefa (Anexo)**

Arquivo associado a uma tarefa.

Características:

* armazenado em Storage

* referenciado por metadados (nome, tipo, tamanho, caminho)

* regras de acesso seguem o acesso ao projeto ao qual a tarefa pertence

---

## **5.3 Relações (Mapa Conceitual)**

Abaixo, as relações essenciais:

* **Usuário (Identidade) 1 — 1 Perfil**

  * Todo usuário autenticado possui um perfil de aplicação.

* **Usuário N — N Projeto** *(via Membership)*

  * Associação explícita define acesso e participação.

* **Projeto 1 — N Tarefa**

  * Tarefas são sempre filhas de projetos.

* **Projeto 0..1 — 1 Capa**

  * Capa é opcional; quando existe, aponta para Storage.

* **Tarefa 0..N — N Anexo**

  * Tarefas podem ter zero ou múltiplos anexos.

---

## **5.4 Regras de Integridade (Conceituais)**

### **Integridade de escopo**

* Uma tarefa não existe sem um projeto válido.

* Um anexo não existe sem uma tarefa válida (ou sem o projeto, no caso de capa).

* Um usuário não deve acessar projetos/tarefas fora do seu membership, exceto admin.

### **Integridade de consistência**

* Prioridade é um conjunto fechado de valores (Baixa/Média/Alta).

* Status de tarefa é um estado controlado (ex.: pendente/concluída).

---

## **5.5 Autorização Derivada do Modelo**

A autorização de acesso é derivada diretamente das relações:

* **Admin**: acesso global (projetos, tarefas e arquivos).

* **User**: acesso apenas quando existe associação ao projeto.

Para tarefas e anexos, a regra é transitiva:

* Acesso à tarefa depende do projeto.

* Acesso ao anexo depende da tarefa (e portanto do projeto).

---

## **5.6 Evolução Prevista do Modelo (Sem Quebra)**

O modelo foi pensado para suportar evolução incremental, especialmente:

### **Introdução de “Pipelines”**

Uma nova entidade “Pipeline” pode ser adicionada como estrutura organizadora intermediária:

* **Projeto 1 — N Pipeline**

* **Pipeline 1 — N Tarefa**

Isso permite:

* ciclos de trabalho fecháveis

* histórico de iniciativas concluídas

* tarefas agrupadas por iniciativa

A mudança pode ser feita de forma progressiva:

* mantendo tarefas diretamente no projeto inicialmente

* e depois permitindo associar tarefas a pipelines quando o recurso estiver ativo

---

## **5.7 Métricas e Dashboards como Derivação do Modelo**

O modelo suporta métricas como consequência das relações (sem acoplamento a UI específica), por exemplo:

* volume de projetos por usuário

* volume de tarefas por projeto

* distribuição por prioridade

* progresso (concluídas vs pendentes)

* atividade recente (criações/atualizações)

Essas métricas podem ser derivadas via agregações, views ou funções, sem alterar o modelo conceitual.

---

## **5.8 Limites Atuais do Modelo**

Atualmente, o modelo não inclui:

* organização/empresa (multi-tenant)

* times internos

* permissão granular além de user/admin

* auditoria detalhada por evento

Esses elementos podem ser adicionados como camadas futuras, mantendo a base consistente.

# **PARTE 6 — FLUXOS DE AUTENTICAÇÃO**

*(Login, Signup, Recuperação de Senha e Redirecionamento por Role — com guardrails)*

## **6.1 Objetivo e Princípios**

A autenticação tem como objetivos:

* Identificar o usuário de forma segura

* Manter sessão válida no cliente

* Determinar o papel (role) do usuário para guiar o fluxo (user/admin)

* Garantir uma experiência previsível (sem loops de redirecionamento)

**Princípios arquiteturais:**

* O provedor de autenticação é a fonte de verdade da sessão.

* O papel (role) é definido no backend (perfil de aplicação), não no cliente.

* Guards de rota nunca redirecionam durante estados intermediários (loading).

* O sistema deve evitar estados ambíguos como “usuário logado mas sem role resolvido”.

---

## **6.2 Visão Geral dos Fluxos**

O sistema suporta quatro fluxos principais:

1. **Login**

2. **Signup (criação de conta)**

3. **Recuperação de senha (recovery link)**

4. **Encerramento de sessão (logout)**

Cada fluxo possui:

* telas públicas específicas

* transições controladas

* tratamento de erros consistente

---

## **6.3 Login**

### **Objetivo**

Permitir que o usuário autentique-se com credenciais e acesse o produto conforme seu papel.

### **Comportamento esperado**

1. Usuário informa email e senha.

2. A autenticação é validada pelo Supabase Auth.

3. Uma sessão válida é estabelecida no cliente.

4. O sistema resolve o papel do usuário (admin/user).

5. O sistema redireciona determinísticamente para o destino apropriado.

### **Regras (guardrails)**

* Após login, a navegação deve ser dirigida por um único ponto decisor (ex.: uma rota/home “baseada em role”).

* A UI pode exibir loading enquanto a sessão/role são resolvidos.

* Evitar “redirecionamentos em cascata” em múltiplos lugares.

---

## **6.4 Signup (Criação de Conta)**

### **Objetivo**

Criar um novo usuário autenticado e preparar seu perfil de aplicação com role padrão.

### **Comportamento esperado**

1. Usuário informa dados de cadastro.

2. O Supabase Auth cria a identidade (usuário autenticado).

3. O backend garante a criação do perfil de aplicação associado ao usuário.

4. O role inicial é definido pelo backend (tipicamente `user`), nunca pelo frontend.

5. Após sucesso, o usuário é direcionado ao fluxo principal (que decidirá o destino conforme role).

### **Regras (guardrails)**

* O cliente não define roles.

* A integridade do vínculo “usuário autenticado ↔ perfil de aplicação” deve ser garantida no backend.

* Validações de formulário devem ser consistentes e mantidas fora da UI de página (camada de feature/hook/validator).

---

## **6.5 Recuperação de Senha (Padrão Supabase via Recovery Link)**

### **Objetivo**

Permitir que o usuário redefina sua senha com segurança usando o mecanismo padrão de link de recuperação do Supabase.

### **Característica estrutural**

Este projeto utiliza o fluxo padrão baseado em **recovery link**:

* O usuário solicita a recuperação informando o email.

* O Supabase envia um email com um link de recuperação.

* O link direciona para uma rota do próprio aplicativo (via `redirectTo`).

* Ao abrir a rota, o app permite definir a nova senha.

* A nova senha é confirmada via chamada de atualização de credenciais.

### **Observação de UX (etapa adicional informativa)**

Além do fluxo padrão, existe uma etapa intermediária de UX (ex.: uma página de “código” ou “instruções/reenvio”) que:

* **não substitui** o mecanismo do Supabase

* serve como tela informativa de confirmação/reenvio e orientação ao usuário

* mantém o reset real baseado no link do email

Essa etapa é tratada como camada de experiência, não como camada de segurança.

### **Regras (guardrails)**

* Não implementar “código de 6 dígitos” como fonte de verdade do reset, pois o mecanismo real é o link de recuperação.

* A tela de definição de nova senha deve:

  * validar entrada (mínimos de segurança definidos pelo produto)

  * tratar erros de sessão/recovery expirado

  * conduzir o usuário para um estado estável após sucesso (ex.: login ou home)

---

## **6.6 Redirecionamento por Role e Proteção de Rotas**

### **Objetivo**

Garantir que usuários vejam apenas o que seu papel permite.

**Estrutura:**

* Rotas públicas: autenticação (login/signup/recovery)

* Rotas privadas: área do usuário e área administrativa

* Um mecanismo de guarda controla:

  * existência de sessão

  * role carregado

  * acesso permitido

### **Guardrails críticos**

* Guards não devem redirecionar enquanto estiverem “carregando” sessão/role.

* Deve haver um estado explícito de loading para evitar flicker e loops.

* A decisão de destino pós-login deve ser centralizada.

---

## **6.7 Logout**

### **Objetivo**

Encerrar sessão no cliente e retornar a um estado público.

Comportamento esperado:

* invalidar sessão no Supabase

* limpar caches relevantes no cliente (ex.: queries sensíveis)

* redirecionar para rota pública estável (ex.: login)

---

## **6.8 Tratamento de Erros (Padrão)**

O frontend deve diferenciar erros típicos:

* credenciais inválidas (login)

* email já existente (signup)

* link expirado/inválido (recovery)

* falhas de rede/instabilidade

* falhas de autorização (RLS) após login

Regra:

* erros devem ser apresentados ao usuário de forma legível

* logs internos podem existir, mas não substituem UX clara

* evitar loaders eternos: todo fluxo deve ter encerramento (sucesso ou erro)

---

## **6.9 Evolução Planejada e Impacto na Autenticação**

As evoluções previstas (pipelines e monitoramento por usuário) não alteram o núcleo de autenticação, mas exigem:

* consistência forte na resolução de role

* estrutura de acesso baseada em associação a projetos e, futuramente, a pipelines

* previsibilidade no roteamento e guards

A base arquitetural atual é compatível com essas expansões.

# **PARTE 7 — FLUXOS FUNCIONAIS DO SISTEMA**

*(User vs Admin — visão estrutural, sem acoplamento a UI específica)*

## **7.1 Objetivo desta Seção**

Descrever como o sistema é usado na prática, separando claramente:

* Fluxo operacional do **usuário comum (user)**

* Fluxo de governança do **administrador (admin)**

* Interações centrais (projetos, associação, tarefas, arquivos)

* Regras estruturais que garantem consistência e segurança

Esta seção evita detalhes de interface (cards, gráficos, textos de botão etc.) e foca em comportamento e responsabilidades.

---

## **7.2 Fluxo do Usuário (user)**

### **7.2.1 Premissas**

O usuário comum atua apenas dentro do escopo de acesso concedido (associação a projetos).  
 Ele não cria projetos nem gerencia usuários.

### **7.2.2 Entradas do Usuário**

O usuário entra no sistema via autenticação e é direcionado para sua área operacional.

### **7.2.3 Ações Operacionais Principais**

Dentro de projetos aos quais está associado, o usuário pode:

* Visualizar projetos atribuídos

* Acessar um projeto específico

* **Criar tarefas**

* **Editar tarefas**

* **Excluir tarefas**

* Marcar tarefas como concluídas (e reabrir, se suportado)

* Classificar e operar tarefas respeitando prioridade (Baixa/Média/Alta)

* Anexar arquivos a tarefas (quando aplicável)

* Visualizar e remover anexos dentro do escopo permitido

### **7.2.4 Regras de Escopo**

O usuário não deve conseguir:

* acessar projetos não atribuídos

* acessar tarefas fora de projetos atribuídos

* manipular arquivos fora do escopo permitido

Essas regras são impostas pelo backend (RLS/Storage policies).

---

## **7.3 Fluxo do Administrador (admin)**

### **7.3.1 Premissas**

O admin é o papel de governança do sistema.  
 Ele gerencia a estrutura do trabalho e a distribuição de responsabilidades.

### **7.3.2 Ações Estruturais (Governança)**

O administrador pode:

* Criar, editar e excluir projetos

* Atribuir e desatribuir usuários a projetos

* Visualizar o panorama geral da plataforma (visão executiva)

* Gerenciar tarefas em qualquer projeto (criar/editar/excluir)

* Excluir usuários (operação administrativa)

### **7.3.3 Responsabilidades Implícitas**

O admin é responsável por:

* manter a estrutura de projetos coerente

* garantir que usuários tenham acesso apenas ao necessário

* acompanhar o andamento geral e identificar gargalos

A forma como isso aparece em UI pode variar, mas o comportamento e as permissões são constantes.

---

## **7.4 Fluxo de Projetos (ciclo funcional)**

A vida de um projeto segue uma sequência típica:

1. Admin cria o projeto.

2. Admin atribui usuários ao projeto.

3. Tarefas são criadas e executadas (por admin e/ou usuários atribuídos).

4. Tarefas evoluem entre estados (pendente/concluída).

5. Arquivos podem ser associados (capa do projeto e anexos de tarefas).

6. O projeto permanece ativo enquanto houver trabalho, e pode ser encerrado/arquivado conforme decisões futuras do produto.

---

## **7.5 Fluxo de Tarefas (ciclo funcional)**

Uma tarefa segue um ciclo comum:

* Criação (com prioridade definida)

* Edição (título, prioridade e demais atributos)

* Marcação de conclusão

* Possível reabertura (se suportado)

* Exclusão (quando aplicável)

* Associação de anexos (opcional)

**Contrato funcional:**  
 Prioridade é parte do domínio e não apenas um atributo visual.

---

## **7.6 Fluxo de Arquivos (capa e anexos)**

O sistema suporta dois tipos de arquivos no domínio:

* **Capa de projeto** (associada ao projeto)

* **Anexos de tarefa** (associados a uma tarefa)

Regras estruturais:

* Arquivos são armazenados no Storage e referenciados por metadados no banco.

* Permissões de acesso a arquivos devem seguir o escopo do projeto:

  * admin: global

  * user: somente em projetos atribuídos

---

## **7.7 Regras de Navegação (Comportamento Obrigatório)**

Sempre que houver listagens de entidades que possuem rota de detalhe:

* o clique principal deve navegar de fato

* a URL deve refletir o estado e o contexto (ex.: id do recurso)

* o fluxo deve funcionar mesmo quando dados ainda forem mockados

Isso é parte do contrato de produto: navegação funcional não depende de “integração futura”.

---

## **7.8 Tratamento de Erros e Estados (Regra de Produto)**

O sistema deve lidar com:

* estados de carregamento (loading) previsíveis

* erros de rede

* erros de autorização (negado pelo backend)

* inconsistências de sessão/role

* falhas de upload e políticas de storage

Regra:

* nenhum fluxo deve travar em loading infinito

* mensagens devem ser legíveis

* falhas de autorização devem ser tratadas como comportamento esperado (não “bug inesperado”)

---

## **7.9 Evolução Planejada dos Fluxos (Pipelines e Monitoramento)**

A evolução planejada introduz dois eixos:

### **1\) Pipelines (ciclos de execução)**

* Um pipeline representa uma iniciativa estruturada dentro de um projeto.

* Tarefas passam a ser agrupadas por pipeline.

* Pipelines podem ser encerrados e mantidos como histórico.

Impacto nos fluxos:

* Usuários passam a operar tarefas dentro de pipelines.

* Admin cria/encerra pipelines e acompanha execução por iniciativa.

### **2\) Monitoramento por usuário**

* Visões por usuário para acompanhar:

  * participação em projetos

  * volume de tarefas concluídas

  * andamento e produtividade (métricas derivadas)

Impacto nos fluxos:

* Admin ganha capacidade de visão focada por usuário.

* Produto evolui de “lista de tarefas” para “gestão operacional com desempenho”.

# **PARTE 9 — REGRAS ARQUITETURAIS CRÍTICAS**

*(Governança Técnica e Contratos do Projeto)*

Esta seção consolida as regras estruturais que sustentam a integridade do sistema.  
 Ela funciona como um “estatuto arquitetural”.

---

## **9.1 Separação Obrigatória: Autorização vs Validação**

### **RLS (Row Level Security)**

Define:

* Quem pode acessar quais linhas

* Quem pode inserir, atualizar ou excluir

RLS:

* Nunca utiliza `OLD` ou `NEW`

* Não valida imutabilidade de campos

* Baseia-se apenas em:

  * `auth.uid()`

  * colunas da linha atual

  * funções auxiliares (ex.: verificação de admin)

---

### **Triggers**

São obrigatórias quando:

* É necessário comparar estado anterior vs novo

* Um campo não pode ser alterado após criação

* Existe regra sensível de integridade

Separação formal:

* **RLS decide acesso**

* **Trigger decide validade da alteração**

Misturar essas responsabilidades é proibido.

---

## **9.2 Segurança no Frontend**

É proibido:

* Utilizar chave privilegiada (service role) no frontend.

* Confiar em flags de UI para autorizar ações.

* Implementar validação crítica apenas no cliente.

* Ignorar erros de policy como se fossem exceção inesperada.

Permitido:

* Esconder ações por UX.

* Tratar erros de autorização como fluxo esperado.

---

## **9.3 Variáveis de Ambiente**

Regras:

* Variáveis do frontend devem usar prefixo `VITE_`.

* Arquivo `.env` é local e não versionado.

* `.env.example` deve existir e conter as chaves necessárias.

* O client Supabase deve falhar claramente se variáveis obrigatórias estiverem ausentes.

Nunca:

* Versionar `.env`.

* Expor chaves privilegiadas no frontend.

---

## **9.4 Contrato de Ingestão Figma → Código**

Código vindo de ferramentas de design é tratado como:

Representação semântica de intenção visual.

Obrigatório:

* Normalizar estrutura para padrão do projeto.

* Remover dependências externas não aprovadas.

* Migrar estilos para CSS Modules.

* Tipar corretamente.

* Gerar teste mínimo por página.

* Resolver assets via MCP.

* Registrar rotas conforme padrão.

* Manter idempotência (não sobrescrever silenciosamente).

Proibido:

* Manter UI kits externos no output final.

* Deixar `figma:asset` no código.

* Introduzir bibliotecas sem justificativa técnica.

---

## **9.5 Idempotência e Evolução Controlada**

Sempre que uma página já existir:

* Não sobrescrever silenciosamente.

* Apresentar diff ou gerar versão alternativa.

* Manter histórico previsível de alterações.

O projeto deve evoluir de forma incremental, não disruptiva.

---

## **9.6 Navegação como Contrato**

Sempre que uma entidade possuir detalhe:

* O clique deve navegar de fato.

* A URL deve refletir estado real.

* Não utilizar console.log como placeholder de navegação.

A navegação faz parte da funcionalidade, não da integração futura.

---

## **9.7 Fidelidade Visual**

É proibido:

* Alterar pesos tipográficos arbitrariamente.

* “Embelezar” design sem solicitação.

* Normalizar estilos sem referência.

A fonte da verdade visual é a origem do design.

---

# **PARTE 10 — DIRETRIZES DE EVOLUÇÃO DO PROJETO**

*(Crescimento Sustentável e Expansão Estrutural)*

## **10.1 Como Adicionar Nova Feature**

Ao introduzir nova funcionalidade:

1. Definir claramente o domínio.

2. Criar entidade conceitual (se necessário).

3. Implementar modelo relacional com integridade.

4. Definir RLS primeiro.

5. Implementar triggers (se houver regra sensível).

6. Criar camada de serviço.

7. Criar hooks na feature.

8. Criar página e teste mínimo.

9. Integrar rota.

A ordem importa:  
 Segurança e modelo vêm antes da UI.

---

## **10.2 Introdução de Pipelines (Exemplo de Evolução)**

A futura entidade “Pipeline” deve:

* Pertencer a um projeto.

* Conter múltiplas tarefas.

* Permitir estado de aberto/encerrado.

* Permitir histórico de ciclos concluídos.

Impacto arquitetural:

* Nova entidade relacional.

* Ajuste de RLS (escopo baseado em projeto).

* Ajuste opcional na UI.

* Nenhuma quebra estrutural necessária.

---

## **10.3 Monitoramento por Usuário**

A evolução para dashboards individuais deve:

* Derivar métricas de dados existentes.

* Não duplicar estado.

* Utilizar agregações consistentes.

* Respeitar escopo de acesso (admin vs user).

---

## **10.4 Escalabilidade Estrutural**

O sistema está preparado para:

* Multi-tenant futuro

* Times internos

* Permissões mais granulares

* Auditoria detalhada

* Histórico de alterações

* Integrações externas

Essas expansões devem respeitar:

* Separação de responsabilidades

* Segurança como base

* Evolução incremental

---

## **10.5 Regra Final de Governança**

Antes de adicionar complexidade:

* Validar necessidade real.

* Avaliar impacto no modelo.

* Garantir compatibilidade com RLS.

* Evitar acoplamento desnecessário.

A arquitetura deve permanecer:

* previsível

* segura

* escalável

* governável

