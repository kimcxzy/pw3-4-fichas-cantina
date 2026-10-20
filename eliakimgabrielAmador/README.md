# Planejamento do Projeto Prático: Angular + Spring Boot

**Duração Total:** 12 Horas (6 encontros de 2 horas)  
**Nível:** Ensino Técnico  
**Foco:** Backend Spring Boot (1 Entidade JPA + Regras de Negócio) e Frontend Angular (Interface CRUD + Integração REST)  

---

## 📅 Cronograma de Entregas por Aula (2h por Encontro)

### Aula 1 — 20/10: Modelagem do Backend e Configuração Inicial
* **Objetivo:** Criar o projeto Spring Boot, configurar o banco H2 e criar a Entidade principal com JPA.
* **Entregáveis (Backend):**
  * Projeto gerado no *Spring Initializr* (`Web`, `JPA`, `H2`, `Lombok`).
  * Classe `@Entity` mapeada com seus atributos e anotações `@Id`, `@GeneratedValue`.
  * Interface `Repository` estendendo `JpaRepository`.
  * Aplicação executando com banco H2 ativo e tabela gerada.

### Aula 2 — 27/10: Implementação das Regras de Negócio e Endpoints REST
* **Objetivo:** Escrever a lógica de negócio na camada Service e expor a API REST.
* **Entregáveis (Backend):**
  * Classe `@Service` contendo a regra de negócio central da entidade.
  * Classe `@RestController` com os métodos `GET`, `POST`, `PUT`, `DELETE` e os métodos customizados da regra de negócio.
  * Testes locais dos endpoints via Postman, Bruno ou Swagger.

### Aula 3 — 03/11: Inicialização do Frontend Angular e Serviço REST
* **Objetivo:** Criar o projeto Angular, configurar rotas e implementar o serviço HTTP.
* **Entregáveis (Frontend):**
  * Projeto Angular criado e limpo com módulo de rotas ativado.
  * Configuração do `HttpClient` / `provideHttpClient()`.
  * Criação da interface TypeScript (model) idêntica ao DTO/Entity Java.
  * Criação do `@Injectable Service` com métodos para consumir todos os endpoints do Spring Boot.

### Aula 4 — 10/11: Interface Gráfica — Listagem e Operações Rápidas
* **Objetivo:** Desenvolver o componente principal de exibição e ação de regra de negócio.
* **Entregáveis (Frontend):**
  * Componente de Listagem com tabela ou cards renderizados dinamicamente via `*ngFor` / `@for`.
  * Integração com o Service para carregar dados do Backend (`GET`).
  * Botões de ação direta (ex: botão "Concluir", "Votar", "Reservar" ou "Debitar") consumindo os endpoints customizados.
  * Exclusão (`DELETE`) integrada na tabela.

### Aula 5 — 17/11: Interface Gráfica — Formulário e Validações
* **Objetivo:** Criar tela ou modal para cadastro e edição de registros.
* **Entregáveis (Frontend):**
  * Componente de Formulário utilizando `ReactiveFormsModule`.
  * Validações de campos no front (campos obrigatórios, valores mínimos, etc.).
  * Integração do formulário com as rotas `POST` (Criação) e `PUT` (Edição).
  * Feedback visual ao usuário (mensagens de sucesso/erro ou alertas).

### Aula 6 — 24/11: Polimento, Tratamento de Erros e Apresentação Final
* **Objetivo:** Ajustes finais, integração `CORS`, tratamento de exceções e entrega final.
* **Entregáveis (Geral):**
  * Ajuste de `CORS` no Spring Boot (`@CrossOrigin`).
  * Tratamento básico de erros no Angular (exibir mensagem quando a regra de negócio do Java bloquear algo).
  * Teste do fluxo completo (End-to-End): Cadastrar $\rightarrow$ Listar $\rightarrow$ Executar Regra $\rightarrow$ Editar $\rightarrow$ Excluir.
  * Apresentação/Avaliação do projeto finalizado.

---

## 🚀 Controle de Fichas da Cantina
* **Entidade:** `CarteirinhaAluno` (`id`, `nomeAluno`, `matricula`, `saldo`, `status`)
* **Regra de Negócio:** Endpoint específico para débito que valida se há saldo suficiente. Se o saldo for suficiente, decrementa o valor. Se for insuficiente, bloqueia o débito e lança uma exceção.
* **Endpoints:**
  * `GET /api/carteirinhas` — Lista todas as carteirinhas.
  * `GET /api/carteirinhas/{id}` — Consulta saldo e dados da carteirinha.
  * `POST /api/carteirinhas` — Cadastra nova carteirinha com saldo inicial.
  * `PUT /api/carteirinhas/{id}` — Atualiza dados cadastrais.
  * `DELETE /api/carteirinhas/{id}` — Inativa/remove a carteirinha.
  * `POST /api/carteirinhas/{id}/debitar` — Debita um valor do saldo (Valida saldo suficiente).
  * `POST /api/carteirinhas/{id}/recarregar` — Adiciona valor ao saldo da carteirinha.

* **Pseudocódigo da Regra de Negócio (Débito com Validação de Saldo):**
```text
Função debitarSaldo(carteirinhaId, valorDebito):
    carteirinha = repository.buscarPorId(carteirinhaId)
    
    Se carteirinha.status == "BLOQUEADA" Então
        LançarErro("Carteirinha bloqueada para compras.")
    FimSe
    
    Se carteirinha.saldo >= valorDebito Então
        carteirinha.saldo = carteirinha.saldo - valorDebito
        Retornar repository.salvar(carteirinha)
    Senão
        LançarErro("Saldo insuficiente para realizar a compra.")
    FimSe
FimFuncao
```
