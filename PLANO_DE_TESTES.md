# Plano de Testes Manuais — FutureVet

**FIAP · Challenge 2026 (parceria Clyvo Vet) · Compliance, Quality Assurance & Tests · 4ª Sprint — Parte A**

| Integrante | RM |
|---|---|
| João Victor Caetano Alves da Silva | RM562074 |
| João Victor Bueno Castelini da Silva | RM564115 |
| Ryan Vetoriano | RM565667 |
| Felipe Furlanetto | RM562766 |
| Raul Rezende Iemini Aguiar | RM564002 |

## 1. Objetivo do plano

Validar, no nível de sistema, as funcionalidades principais do FutureVet entregues nas Sprints 3 e 4, usando dados controlados: os valores de entrada e a saída esperada de cada teste são definidos antes da execução.

## 2. Escopo e ambiente

- **Sistema sob teste:** aplicação web FutureVet (Spring Boot + Thymeleaf), publicada em https://challenge-sprint3-java.onrender.com
- **Tipo de teste:** manual, funcional, caixa-preta, nível de sistema (pelas telas da aplicação).
- **Navegador:** Google Chrome (versão atual), janela normal e janela anônima quando indicado.
- **Perfis usados:** TUTOR (contas criadas nos próprios testes) e ADMIN (conta da clínica do grupo).
- **Fora do escopo:** testes de API REST e de carga (cobertos pela Parte B, com Postman), app mobile e hardware IoT.

## 3. Estratégia e dados controlados

- Os casos são executados **em sequência (CT01 → CT15)**: cada caso usa os dados criados pelos anteriores.
- Todos os valores são fixos. Dados principais usados em todo o plano:

| Dado | Valor |
|---|---|
| Tutor principal | Tutor Teste QA · tutor.qa@futurevet.com · CPF 123.456.789-09 · senha SenhaQa123! |
| Segundo tutor | Outro Tutor QA · outro.qa@futurevet.com · CPF 987.654.321-00 · senha SenhaQa123! |
| Animal | Thor · Cão · Labrador · 3 anos · Grande · 28.50 kg |
| Consulta | Thor · Rotina · 10/12/2026 · 14:00 · Consultório 1 |
| Vacinas | V10 (01/10/2026 → 01/10/2027) e Antirrábica (01/06/2025 → 01/06/2026) |

- **Critério de aprovação:** o caso passa quando todos os passos apresentam exatamente o resultado esperado. Qualquer diferença é registrada como falha, com evidência (print) e descrição.
- **Reexecução:** para repetir o plano em uma base que já tem os dados, troque o prefixo dos e-mails (ex.: tutor.qa2@...) e os últimos dígitos dos CPFs.

## 4. Rastreabilidade com o plano de release

| Funcionalidade (item do backlog) | Sprint | Casos de teste |
|---|---|---|
| Autenticação e cadastro | Sprint 3 | CT01, CT02, CT03 |
| Gestão de animais | Sprint 3 | CT04, CT11 |
| Agenda de consultas | Sprint 3 | CT05, CT06, CT08 |
| Notificações (mensageria) | Sprint 4 | CT07 |
| Carteira de vacinação | Sprint 3 | CT09, CT10 |
| Segurança e perfis de acesso | Sprint 3 | CT12 |
| Monitoramento (Actuator) | Sprint 4 | CT13 |
| Internacionalização | Sprint 4 | CT14 |
| Assistente Vet (IA generativa) | Sprint 4 | CT15 |

## 5. Lista de testes planejados e objetivos

| ID | Teste | Objetivo | Sprint |
|---|---|---|---|
| CT01 | Cadastro de tutor com dados válidos | Validar que um novo tutor consegue criar a conta pela tela pública de cadastro e é direcionado ao login. | Sprint 3 |
| CT02 | Cadastro de tutor com dados inválidos | Validar que o cadastro recusa senhas divergentes, CPF incompleto e e-mail já existente, sem criar conta. | Sprint 3 |
| CT03 | Login com credenciais inválidas e válidas | Validar que o login recusa senha errada e que, com a senha certa, o tutor acessa o painel e consegue sair. | Sprint 3 |
| CT04 | Cadastro e edição de animal | Validar que o tutor cadastra um pet com todos os campos e consegue editar o peso depois. | Sprint 3 |
| CT05 | Solicitação de consulta pelo tutor | Validar que o tutor solicita uma consulta em horário válido e ela fica aguardando a confirmação da clínica. | Sprint 3 |
| CT06 | Regras de horário da agenda | Validar que a agenda recusa domingo, data no passado e horário já ocupado. | Sprint 3 |
| CT07 | Confirmação da consulta pela clínica e notificações | Validar o fluxo tutor → clínica → tutor: a clínica recebe o aviso da solicitação, confirma, e o tutor é notificado. | Sprint 4 |
| CT08 | Cancelamento de consulta e liberação do horário | Validar que o tutor cancela uma consulta informando o motivo e o horário volta a ficar disponível. | Sprint 3 |
| CT09 | Registro de vacinas e situação na carteira | Validar o registro de doses, a classificação automática (Em dia / Atrasada) e as regras de data. | Sprint 3 |
| CT10 | Registro de reforço de vacina | Validar que o reforço calcula a próxima dose pelo intervalo e preserva a dose anterior como histórico. | Sprint 3 |
| CT11 | Exclusão de animal e preservação do histórico | Validar que animal com consultas ou vacinas não pode ser excluído e que animal sem histórico pode. | Sprint 3 |
| CT12 | Isolamento de dados entre tutores e área restrita | Validar que um tutor não vê nem acessa os registros de outro tutor e não entra na área da clínica. | Sprint 3 |
| CT13 | Painel de monitoramento da clínica | Validar que a clínica vê a saúde da plataforma e as métricas, e que o health check é público. | Sprint 4 |
| CT14 | Troca de idioma (português / inglês) | Validar que a interface e as mensagens de regra mudam para inglês e voltam para português. | Sprint 4 |
| CT15 | Pergunta ao Assistente Vet com dados do pet | Validar que o assistente responde usando os dados reais do tutor (animais e vacinas). | Sprint 4 |

## 6. Casos de teste

### CT01 — Cadastro de tutor com dados válidos

**Funcionalidade:** Autenticação e cadastro (Sprint 3)  
**Objetivo:** Validar que um novo tutor consegue criar a conta pela tela pública de cadastro e é direcionado ao login.

**Pré-condições**

- Aplicação no ar (abrir a URL e aguardar até 60 s no primeiro acesso do Render).
- E-mail tutor.qa@futurevet.com e CPF 123.456.789-09 ainda não cadastrados.

**Dados de entrada**

| Campo | Valor |
|---|---|
| Nome completo | `Tutor Teste QA` |
| E-mail | `tutor.qa@futurevet.com` |
| CPF | `123.456.789-09` |
| Telefone | `(11) 98765-4321` |
| Senha | `SenhaQa123!` |
| Confirme sua senha | `SenhaQa123!` |

**Dados de saída esperados**

- Redirecionamento para /login?cadastro
- Mensagem: "Cadastro concluído. Entre com sua nova conta."
- Conta criada com perfil TUTOR

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Acessar https://challenge-sprint3-java.onrender.com/login | Tela de login exibe "Vamos cuidar juntos?" e o link "Cadastre-se". |
| 2 | Clicar em "Cadastre-se" | Tela "Criar conta" com os campos Nome completo, E-mail, CPF, Telefone, Senha e Confirme sua senha. |
| 3 | Preencher Nome completo = Tutor Teste QA; E-mail = tutor.qa@futurevet.com; CPF = 123.456.789-09; Telefone = (11) 98765-4321 | Campos aceitam os valores, sem destaque de erro. |
| 4 | Preencher Senha = SenhaQa123! e Confirme sua senha = SenhaQa123! e clicar em "Criar minha conta →" | Redireciona para /login?cadastro com a mensagem "Cadastro concluído. Entre com sua nova conta." |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT02 — Cadastro de tutor com dados inválidos

**Funcionalidade:** Autenticação e cadastro (Sprint 3)  
**Objetivo:** Validar que o cadastro recusa senhas divergentes, CPF incompleto e e-mail já existente, sem criar conta.

**Pré-condições**

- CT01 executado (conta tutor.qa@futurevet.com existente).

**Dados de entrada**

| Campo | Valor |
|---|---|
| Nome completo | `Teste Validacao` |
| E-mail (passos 2 e 3) | `validacao.qa@futurevet.com` |
| CPF (passos 2 e 4) | `111.222.333-44` |
| CPF (passo 3) | `123.456.789-0 (10 dígitos)` |
| Senha | `SenhaQa123!` |
| Confirmação (passo 2) | `OutraSenha1!` |
| E-mail (passo 4) | `tutor.qa@futurevet.com` |

**Dados de saída esperados**

- Passo 2: "As senhas não conferem."
- Passo 3: "Informe os 11 dígitos do CPF."
- Passo 4: "E-mail ou CPF já cadastrado."
- Nenhuma conta nova criada; usuário permanece em /registro

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Acessar https://challenge-sprint3-java.onrender.com/registro | Tela "Criar conta" exibida. |
| 2 | Preencher Nome = Teste Validacao; E-mail = validacao.qa@futurevet.com; CPF = 111.222.333-44; Senha = SenhaQa123!; Confirme sua senha = OutraSenha1! e clicar em "Criar minha conta →" | Permanece no cadastro com a mensagem "As senhas não conferem." |
| 3 | Corrigir Confirme sua senha = SenhaQa123!, trocar CPF = 123.456.789-0 e clicar em "Criar minha conta →" | Permanece no cadastro com a mensagem "Informe os 11 dígitos do CPF." |
| 4 | Trocar CPF = 111.222.333-44 e E-mail = tutor.qa@futurevet.com e clicar em "Criar minha conta →" | Permanece no cadastro com a mensagem "E-mail ou CPF já cadastrado." |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT03 — Login com credenciais inválidas e válidas

**Funcionalidade:** Autenticação e cadastro (Sprint 3)  
**Objetivo:** Validar que o login recusa senha errada e que, com a senha certa, o tutor acessa o painel e consegue sair.

**Pré-condições**

- CT01 executado.

**Dados de entrada**

| Campo | Valor |
|---|---|
| E-mail | `tutor.qa@futurevet.com` |
| Senha incorreta | `SenhaErrada1` |
| Senha correta | `SenhaQa123!` |

**Dados de saída esperados**

- Senha incorreta: "E-mail ou senha inválidos."
- Senha correta: painel "Olá, Tutor Teste QA."
- Sair: "Você saiu da conta."

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Acessar https://challenge-sprint3-java.onrender.com/login | Tela de login exibida. |
| 2 | Informar E-mail = tutor.qa@futurevet.com e Senha = SenhaErrada1 e clicar em "Entrar na minha conta →" | Permanece no login com a mensagem "E-mail ou senha inválidos." |
| 3 | Informar E-mail = tutor.qa@futurevet.com e Senha = SenhaQa123! e clicar em "Entrar na minha conta →" | Abre o painel "Visão geral" com a saudação "Olá, Tutor Teste QA." e "Animais acompanhados" = 0. |
| 4 | Clicar em "Sair da conta ↗" | Volta ao login com a mensagem "Você saiu da conta." |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT04 — Cadastro e edição de animal

**Funcionalidade:** Gestão de animais (Sprint 3)  
**Objetivo:** Validar que o tutor cadastra um pet com todos os campos e consegue editar o peso depois.

**Pré-condições**

- Logado como tutor.qa@futurevet.com (CT03).
- Nenhum animal cadastrado para esse tutor.

**Dados de entrada**

| Campo | Valor |
|---|---|
| Nome | `Thor` |
| Espécie | `Cão` |
| Raça | `Labrador` |
| Idade | `3 anos` |
| Porte | `Grande` |
| Peso (cadastro) | `28.50` |
| Peso (edição) | `29.00` |

**Dados de saída esperados**

- Mensagem "Animal salvo com sucesso." nas duas gravações
- Thor listado com Cão · Labrador · Grande
- Peso final exibido: 29,00 kg
- Anotar o ID do Thor (aparece na URL ao editar) para o CT12

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | No menu, clicar em "Animais" | Tela "Animais" com o texto "Nenhum animal cadastrado. Comece pelo botão acima." |
| 2 | Clicar em "+ Cadastrar animal" | Formulário "Perfil do animal" exibido. |
| 3 | Preencher Nome = Thor; Espécie = Cão; Raça = Labrador; Idade = 3 anos; Porte = Grande; Peso = 28.50 e clicar em "Salvar e continuar →" | Volta para "Animais" com "Animal salvo com sucesso." e o card do Thor (Cão, Labrador, Grande, 28,50 kg). |
| 4 | Clicar em "Editar" no Thor, anotar o número da URL (/animais/{ID}/editar), alterar Peso = 29.00 e salvar | Mensagem "Animal salvo com sucesso." e o Thor exibe 29,00 kg. |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT05 — Solicitação de consulta pelo tutor

**Funcionalidade:** Agenda de consultas (Sprint 3)  
**Objetivo:** Validar que o tutor solicita uma consulta em horário válido e ela fica aguardando a confirmação da clínica.

**Pré-condições**

- Logado como tutor.qa@futurevet.com.
- Thor cadastrado (CT04).

**Dados de entrada**

| Campo | Valor |
|---|---|
| Animal | `Thor` |
| Tipo de consulta | `Rotina` |
| Data | `10/12/2026 (quinta-feira)` |
| Horário | `14:00` |
| Local / consultório | `Consultório 1` |
| Observações | `Check-up anual` |

**Dados de saída esperados**

- Mensagem "Solicitação enviada. Aguarde a confirmação da clínica."
- Consulta listada com situação "Aguardando confirmação"
- Painel: "1 consulta aguarda a confirmação da clínica."

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | No menu, clicar em "Consultas" e depois em "+ Agendar consulta" | Formulário de agendamento exibido, com o Thor na lista de animais. |
| 2 | Preencher Animal = Thor; Tipo = Rotina; Data = 10/12/2026; Horário = 14:00; Local = Consultório 1; Observações = Check-up anual e clicar em "Salvar e continuar →" | Volta para "Consultas" com "Solicitação enviada. Aguarde a confirmação da clínica." e a consulta do Thor em 10/12/2026 14:00 com situação "Aguardando confirmação". |
| 3 | No menu, clicar em "Visão geral" | "Consultas pela frente" = 1 e o aviso "1 consulta aguarda a confirmação da clínica." |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT06 — Regras de horário da agenda

**Funcionalidade:** Agenda de consultas (Sprint 3)  
**Objetivo:** Validar que a agenda recusa domingo, data no passado e horário já ocupado.

**Pré-condições**

- Logado como tutor.qa@futurevet.com.
- CT05 executado (Thor ocupa 10/12/2026 14:00 no Consultório 1).

**Dados de entrada**

| Campo | Valor |
|---|---|
| Animal / Tipo / Local | `Thor / Rotina / Consultório 1` |
| Tentativa 1 | `13/12/2026 (domingo) às 10:00` |
| Tentativa 2 | `01/10/2026 (passado) às 10:00` |
| Tentativa 3 | `10/12/2026 às 14:00 (já ocupado)` |

**Dados de saída esperados**

- Tentativa 1: "Agendamentos de segunda a sábado, das 08:00 às 18:00."
- Tentativa 2: "Escolha uma data e hora futuras."
- Tentativa 3: "Este horário já está ocupado para o animal ou local escolhido."
- Nenhuma consulta nova criada

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Em "Consultas", clicar em "+ Agendar consulta" | Formulário de agendamento exibido. |
| 2 | Preencher Thor; Rotina; Data = 13/12/2026; Horário = 10:00; Local = Consultório 1 e salvar | Permanece no formulário com "Agendamentos de segunda a sábado, das 08:00 às 18:00." |
| 3 | Trocar Data = 01/10/2026 (Horário 10:00) e salvar | Permanece no formulário com "Escolha uma data e hora futuras." |
| 4 | Trocar Data = 10/12/2026 e Horário = 14:00 e salvar | Permanece no formulário com "Este horário já está ocupado para o animal ou local escolhido." |
| 5 | Voltar para "Consultas" | Só existe a consulta do CT05 (10/12/2026 14:00). |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT07 — Confirmação da consulta pela clínica e notificações

**Funcionalidade:** Notificações (mensageria) (Sprint 4)  
**Objetivo:** Validar o fluxo tutor → clínica → tutor: a clínica recebe o aviso da solicitação, confirma, e o tutor é notificado.

**Pré-condições**

- CT05 executado.
- Conta da clínica (perfil ADMIN) do grupo disponível.

**Dados de entrada**

| Campo | Valor |
|---|---|
| Conta clínica | `e-mail e senha da conta ADMIN do grupo` |
| Consulta | `Thor · 10/12/2026 · 14:00 · Consultório 1` |
| Conta tutor | `tutor.qa@futurevet.com / SenhaQa123!` |

**Dados de saída esperados**

- Clínica recebe "Nova solicitação de consulta" (Thor, 10/12/2026, 14:00)
- "Consulta confirmada. O tutor foi avisado." e situação "Confirmada"
- Tutor recebe "Consulta confirmada"
- "Todos os avisos foram marcados como lidos."

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Entrar com a conta da clínica e abrir "Notificações" | Aviso "Nova solicitação de consulta" marcado como "Nova", citando Thor, 10/12/2026 e 14:00. |
| 2 | Abrir "Consultas", localizar a consulta do Thor de 10/12/2026 14:00 e clicar em "Confirmar" | Mensagem "Consulta confirmada. O tutor foi avisado." e situação "Confirmada". |
| 3 | Sair, entrar com tutor.qa@futurevet.com / SenhaQa123! e abrir "Notificações" | Aviso "Consulta confirmada" citando Thor, 10/12/2026 e 14:00. |
| 4 | Clicar em "Marcar todas como lidas" | Mensagem "Todos os avisos foram marcados como lidos." e os avisos deixam de aparecer como "Nova". |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT08 — Cancelamento de consulta e liberação do horário

**Funcionalidade:** Agenda de consultas (Sprint 3)  
**Objetivo:** Validar que o tutor cancela uma consulta informando o motivo e o horário volta a ficar disponível.

**Pré-condições**

- Logado como tutor.qa@futurevet.com.
- Thor cadastrado.

**Dados de entrada**

| Campo | Valor |
|---|---|
| Consulta | `Thor · Vacinação · 11/12/2026 · 09:00 · Consultório 2` |
| Motivo | `Tutor estará viajando` |

**Dados de saída esperados**

- "Consulta cancelada. Horário liberado." e situação "Cancelada"
- Novo agendamento no mesmo horário aceito: "Solicitação enviada. Aguarde a confirmação da clínica."

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Em "Consultas", agendar Thor; Tipo = Vacinação; Data = 11/12/2026; Horário = 09:00; Local = Consultório 2 | "Solicitação enviada. Aguarde a confirmação da clínica." e a consulta aparece como "Aguardando confirmação". |
| 2 | Na consulta de 11/12/2026, abrir "Cancelar consulta", preencher Motivo = Tutor estará viajando e clicar em "Confirmar cancelamento" | "Consulta cancelada. Horário liberado." e situação "Cancelada". |
| 3 | Agendar novamente Thor; Vacinação; 11/12/2026; 09:00; Consultório 2 | Agendamento aceito com "Solicitação enviada. Aguarde a confirmação da clínica." (o horário foi liberado). |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT09 — Registro de vacinas e situação na carteira

**Funcionalidade:** Carteira de vacinação (Sprint 3)  
**Objetivo:** Validar o registro de doses, a classificação automática (Em dia / Atrasada) e as regras de data.

**Pré-condições**

- Logado como tutor.qa@futurevet.com.
- Thor cadastrado e sem vacinas.

**Dados de entrada**

| Campo | Valor |
|---|---|
| Vacina 1 | `V10 · aplicação 01/10/2026 · próxima 01/10/2027 · Clínica FutureVet` |
| Vacina 2 | `Antirrábica · aplicação 01/06/2025 · próxima 01/06/2026 · Clínica FutureVet` |
| Inválida A | `Giárdia · aplicação 10/12/2026 (futuro)` |
| Inválida B | `Giárdia · aplicação 01/10/2026 · próxima 01/09/2026` |

**Dados de saída esperados**

- V10: "Vacina salva na carteira." e situação "Em dia"
- Antirrábica: situação "Atrasada" e "Precisam de atenção" = 1
- Inválida A: "A aplicação não pode estar no futuro."
- Inválida B: "A próxima dose deve ser posterior à aplicação."

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | No menu, abrir "Carteira de vacinação" e clicar em "+ Registrar vacina" | Formulário de vacina exibido. |
| 2 | Preencher Animal = Thor; Nome = V10; Data de aplicação = 01/10/2026; Próxima dose = 01/10/2027; Local = Clínica FutureVet e salvar | "Vacina salva na carteira." e a V10 aparece com situação "Em dia". |
| 3 | Registrar Thor; Antirrábica; aplicação = 01/06/2025; próxima dose = 01/06/2026; Local = Clínica FutureVet | Antirrábica aparece com situação "Atrasada" e o card "Precisam de atenção" mostra 1. |
| 4 | Registrar Thor; Giárdia; aplicação = 10/12/2026; próxima dose vazia | Formulário recusa com "A aplicação não pode estar no futuro." |
| 5 | Trocar aplicação = 01/10/2026 e próxima dose = 01/09/2026 e salvar | Formulário recusa com "A próxima dose deve ser posterior à aplicação." |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT10 — Registro de reforço de vacina

**Funcionalidade:** Carteira de vacinação (Sprint 3)  
**Objetivo:** Validar que o reforço calcula a próxima dose pelo intervalo e preserva a dose anterior como histórico.

**Pré-condições**

- CT09 executado (Antirrábica do Thor com situação "Atrasada").

**Dados de entrada**

| Campo | Valor |
|---|---|
| Reforço inválido | `data 01/05/2025 · intervalo 12 · Clínica FutureVet` |
| Reforço válido | `data 20/10/2026 · intervalo 12 meses · Clínica FutureVet` |

**Dados de saída esperados**

- Inválido: "O reforço deve ser posterior à dose anterior."
- Válido: "Reforço registrado. Próxima dose calculada e dose anterior preservada no histórico."
- Nova dose com próxima dose prevista para 20/10/2027 ("Em dia")
- Dose de 01/06/2025 com situação "Histórico"

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Na Antirrábica (Atrasada), clicar em "Registrar reforço →" | Tela "Registrar reforço" exibida. |
| 2 | Preencher Data = 01/05/2025; Intervalo = 12; Local = Clínica FutureVet e clicar em "Registrar e calcular próxima dose →" | Recusa com "O reforço deve ser posterior à dose anterior." |
| 3 | Preencher Data = 20/10/2026; Intervalo = 12; Local = Clínica FutureVet e confirmar | "Reforço registrado. Próxima dose calculada e dose anterior preservada no histórico." |
| 4 | Conferir a carteira | Nova Antirrábica de 20/10/2026 com próxima dose em 20/10/2027 ("Em dia"); a dose de 01/06/2025 aparece como "Histórico" com "Esta aplicação já possui reforço registrado."; "Precisam de atenção" = 0. |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT11 — Exclusão de animal e preservação do histórico

**Funcionalidade:** Gestão de animais (Sprint 3)  
**Objetivo:** Validar que animal com consultas ou vacinas não pode ser excluído e que animal sem histórico pode.

**Pré-condições**

- Logado como tutor.qa@futurevet.com.
- Thor com consultas e vacinas (CT05 a CT10).

**Dados de entrada**

| Campo | Valor |
|---|---|
| Animal com histórico | `Thor` |
| Animal novo | `Mel · Gato · Siamês · 1 ano · Pequeno · 4.20` |

**Dados de saída esperados**

- Thor: "Este animal possui histórico de consultas ou vacinas. Preserve o histórico; a exclusão foi bloqueada."
- Mel: "Animal salvo com sucesso." e depois "Animal excluído."

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Em "Animais", clicar em "Excluir" no Thor e em "Confirmar exclusão" | Mensagem "Este animal possui histórico de consultas ou vacinas. Preserve o histórico; a exclusão foi bloqueada." e o Thor continua listado. |
| 2 | Cadastrar Nome = Mel; Espécie = Gato; Raça = Siamês; Idade = 1 ano; Porte = Pequeno; Peso = 4.20 | "Animal salvo com sucesso." e Mel listada. |
| 3 | Clicar em "Excluir" na Mel e em "Confirmar exclusão" | "Animal excluído." e Mel some da lista. |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT12 — Isolamento de dados entre tutores e área restrita

**Funcionalidade:** Segurança e perfis de acesso (Sprint 3)  
**Objetivo:** Validar que um tutor não vê nem acessa os registros de outro tutor e não entra na área da clínica.

**Pré-condições**

- CT04 executado e ID do Thor anotado.

**Dados de entrada**

| Campo | Valor |
|---|---|
| Nome completo | `Outro Tutor QA` |
| E-mail | `outro.qa@futurevet.com` |
| CPF | `987.654.321-00` |
| Telefone | `(11) 91234-5678` |
| Senha / confirmação | `SenhaQa123!` |
| URL proibida 1 | `/animais/{ID do Thor}/editar` |
| URL proibida 2 | `/admin/monitoramento` |

**Dados de saída esperados**

- Lista de animais do outro tutor vazia (Thor não aparece)
- URL 1: "Você não tem permissão para acessar este registro."
- URL 2: "Esta área é exclusiva da clínica."

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Sair da conta e cadastrar Outro Tutor QA / outro.qa@futurevet.com / 987.654.321-00 / (11) 91234-5678 / SenhaQa123! | "Cadastro concluído. Entre com sua nova conta." |
| 2 | Entrar com outro.qa@futurevet.com / SenhaQa123! e abrir "Animais" | "Nenhum animal cadastrado. Comece pelo botão acima." (o Thor não aparece). |
| 3 | Digitar na barra de endereço https://challenge-sprint3-java.onrender.com/animais/{ID do Thor}/editar | Página de erro com "Você não tem permissão para acessar este registro." |
| 4 | Digitar na barra de endereço https://challenge-sprint3-java.onrender.com/admin/monitoramento | Página de erro com "Esta área é exclusiva da clínica." |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT13 — Painel de monitoramento da clínica

**Funcionalidade:** Monitoramento (Actuator) (Sprint 4)  
**Objetivo:** Validar que a clínica vê a saúde da plataforma e as métricas, e que o health check é público.

**Pré-condições**

- Conta da clínica (ADMIN) do grupo disponível.

**Dados de entrada**

| Campo | Valor |
|---|---|
| Conta clínica | `e-mail e senha da conta ADMIN do grupo` |
| Endpoint público | `https://challenge-sprint3-java.onrender.com/actuator/health` |

**Dados de saída esperados**

- Aplicação = "Operando" e Banco de dados = "Operando"
- Memória, CPU, tempo no ar e requisições com valores preenchidos
- /actuator/health responde {"status":"UP"}

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Entrar com a conta da clínica e clicar em "Monitoramento" | Tela "Monitoramento" com o quadro "Situação dos serviços". |
| 2 | Conferir os cards de serviço | "Aplicação" e "Banco de dados" com "Operando"; "Mensageria" mostra o canal em uso (RabbitMQ ou "Entrega local (sem broker)"). |
| 3 | Clicar em "Atualizar ↻" | Memória da JVM, CPU, "No ar há" e o número de requisições HTTP são exibidos, e o total de requisições aumenta. |
| 4 | Em uma aba anônima (sem login), acessar https://challenge-sprint3-java.onrender.com/actuator/health | Resposta {"status":"UP"}. |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT14 — Troca de idioma (português / inglês)

**Funcionalidade:** Internacionalização (Sprint 4)  
**Objetivo:** Validar que a interface e as mensagens de regra mudam para inglês e voltam para português.

**Pré-condições**

- Conta tutor.qa@futurevet.com com o Thor cadastrado.

**Dados de entrada**

| Campo | Valor |
|---|---|
| Parâmetro de idioma | `?lang=en e ?lang=pt-BR` |
| Agendamento inválido | `Thor · Rotina · 13/12/2026 (domingo) · 10:00 · Consultório 1` |

**Dados de saída esperados**

- Login em inglês: "Shall we care together?"
- Menu: "Pets" no lugar de "Animais"
- Regra em inglês: "Appointments from Monday to Saturday, 8 AM to 6 PM."
- De volta ao português: "Vamos cuidar juntos?"

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Sair da conta e acessar https://challenge-sprint3-java.onrender.com/login?lang=en | Tela de login em inglês com "Shall we care together?" |
| 2 | Entrar com tutor.qa@futurevet.com / SenhaQa123! | Menu em inglês, com "Pets" no lugar de "Animais". |
| 3 | Tentar agendar Thor; Rotina; 13/12/2026; 10:00; Consultório 1 | Mensagem em inglês: "Appointments from Monday to Saturday, 8 AM to 6 PM." |
| 4 | Sair e acessar https://challenge-sprint3-java.onrender.com/login?lang=pt-BR | Tela volta ao português com "Vamos cuidar juntos?" |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

### CT15 — Pergunta ao Assistente Vet com dados do pet

**Funcionalidade:** Assistente Vet (IA generativa) (Sprint 4)  
**Objetivo:** Validar que o assistente responde usando os dados reais do tutor (animais e vacinas).

**Pré-condições**

- CT09 e CT10 executados.
- Microsserviço de IA no ar e com a chave da API configurada no Render.

**Dados de entrada**

| Campo | Valor |
|---|---|
| Conta | `tutor.qa@futurevet.com / SenhaQa123!` |
| Pergunta | `Alguma vacina do Thor está atrasada?` |

**Dados de saída esperados**

- Resposta em português citando o Thor
- Informa que não há vacina atrasada (Antirrábica com reforço; próxima dose em 20/10/2027)
- "Nova conversa" limpa o histórico

**Procedimento (passos)**

| # | Ação | Resultado esperado |
|---|---|---|
| 1 | Entrar como tutor.qa@futurevet.com e clicar em "Assistente Vet" | Tela do Assistente Vet com o aviso de que não substitui o veterinário e as sugestões de pergunta. |
| 2 | Digitar "Alguma vacina do Thor está atrasada?" e clicar em "Enviar ✦" | A resposta aparece em português, cita o Thor e diz que nenhuma vacina está atrasada (Antirrábica reforçada, próxima dose em 20/10/2027). |
| 3 | Clicar em "Nova conversa" | O histórico do chat é limpo. |

**Execução:** Data: ____/____/2026 · Responsável: __________ · Resultado: ☐ Passou ☐ Falhou · Evidência: ______

## 7. Registro de execução

| ID | Data | Responsável | Resultado | Observação / defeito |
|---|---|---|---|---|
| CT01 | | | | |
| CT02 | | | | |
| CT03 | | | | |
| CT04 | | | | |
| CT05 | | | | |
| CT06 | | | | |
| CT07 | | | | |
| CT08 | | | | |
| CT09 | | | | |
| CT10 | | | | |
| CT11 | | | | |
| CT12 | | | | |
| CT13 | | | | |
| CT14 | | | | |
| CT15 | | | | |
