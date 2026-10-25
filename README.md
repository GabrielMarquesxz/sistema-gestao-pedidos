# Sistema de Gestão de Pedidos — API REST

API REST para cadastrar, consultar, atualizar e excluir pedidos.

## Tecnologias

- Java 17
- Spring Boot 3.2.3
- Maven
- Spring Web
- Spring Data JPA
- H2
- Bean Validation
- Springdoc OpenAPI 2.5.0

## Arquitetura

A aplicação organiza o gerenciamento de pedidos nas seguintes camadas:

- **Entity (`Pedido`):** representa os pedidos persistidos.
- **Repository (`PedidoRepository`):** acesso aos dados usando Spring Data JPA.
- **Service (`PedidoService`):** operações de consulta, cadastro, atualização e exclusão.
- **Controller (`PedidoController`):** expõe os endpoints REST em `/pedidos`.
- **Tratamento global de erros (`GlobalExceptionHandler`):** converte erros de validação em respostas HTTP 400 com os detalhes dos campos inválidos.

## Entidade `Pedido`

| Campo | Tipo | Detalhes |
|---|---|---|
| `id` | `Long` | Identificador gerado pelo banco (`IDENTITY`). |
| `cliente` | `String` | Obrigatório e não pode estar em branco (`@NotBlank`). |
| `valorTotal` | `BigDecimal` | Deve ser maior que zero (`@Positive`). |

As validações são aplicadas ao cadastrar e atualizar pedidos. As mensagens definidas no código são “O nome do cliente é obrigatório” e “O valor total deve ser maior que zero”. `id` é gerado na persistência; o código não define validação `@NotNull` para `valorTotal`.

## Endpoints

Todos os endpoints usam o caminho base `/pedidos` e recebem ou retornam JSON quando aplicável.

| Método | Caminho | Descrição | Respostas principais |
|---|---|---|---|
| `GET` | `/pedidos` | Lista todos os pedidos. | `200 OK` |
| `POST` | `/pedidos` | Cadastra um pedido. | `201 Created`; `400 Bad Request` se houver falha de validação |
| `GET` | `/pedidos/{id}` | Busca um pedido pelo ID. | `200 OK`; `404 Not Found` se não existir |
| `PUT` | `/pedidos/{id}` | Atualiza `cliente` e `valorTotal` de um pedido. | `200 OK`; `404 Not Found` se não existir; `400 Bad Request` se houver falha de validação |
| `DELETE` | `/pedidos/{id}` | Exclui um pedido pelo ID. | `204 No Content`; `404 Not Found` se não existir |

Exemplo de corpo para cadastro e atualização:

```json
{
  "cliente": "Maria",
  "valorTotal": 250.50
}
```

## Como executar

É necessário ter o Java 17 instalado. Na raiz do projeto, inicie a aplicação com o Maven Wrapper:

**Windows (PowerShell):**

```powershell
.\mvnw.cmd spring-boot:run
```

**Linux:**

```bash
./mvnw spring-boot:run
```

A aplicação é iniciada na porta padrão `8080`.

## Banco de dados H2

A aplicação usa H2 em memória com o endereço JDBC `jdbc:h2:mem:pedidosdb`. O Hibernate atualiza o esquema com base nas entidades (`spring.jpa.hibernate.ddl-auto=update`) e o SQL é exibido no console.

Com a aplicação em execução, o console H2 está disponível em:

`http://localhost:8080/h2-console`

Use os dados configurados em `application.properties`:

- **JDBC URL:** `jdbc:h2:mem:pedidosdb`
- **Usuário:** `sa`
- **Senha:** em branco

## Documentação Swagger

Com a aplicação em execução, acesse a interface Swagger UI em:

`http://localhost:8080/swagger-ui/index.html`

## Testes

Execute a suíte de testes com o Maven Wrapper na raiz do projeto:

**Windows (PowerShell):**

```powershell
.\mvnw.cmd test
```

**Linux:**

```bash
./mvnw test
```
