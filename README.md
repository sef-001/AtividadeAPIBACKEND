# Assistência Técnica API

API REST para gerenciar ordens de serviço, clientes, técnicos e peças de uma assistência técnica.

## Tecnologias

- Python 3.11+
- FastAPI
- SQLAlchemy
- SQLite
- JWT
- HTTPX (integração ViaCEP)

## Arquitetura do Projeto

- `app/api`: rotas e controladores da API
- `app/services`: regras de negócio e operações transacionais
- `app/repositories`: acesso a dados e consultas SQLAlchemy
- `app/models`: models SQLAlchemy e relacionamentos
- `app/schemas`: DTOs Pydantic para validação e serialização
- `app/core`: configuração, segurança e geração de tokens
- `app/db`: sessão do banco e inicialização
- `app/utils`: integração externa com ViaCEP
- `app/scripts`: scripts utilitários, como exportação de OpenAPI

## Instalação

1. Clone o repositório.
2. Crie um ambiente virtual:
   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate
   ```
3. Instale as dependências:
   ```powershell
   python -m pip install -r requirements.txt
   ```
4. Copie o arquivo de ambiente:
   ```powershell
   copy .env.example .env
   ```

## Execução

Inicie a aplicação:

```powershell
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

A API estará disponível em `http://127.0.0.1:8000`.

## Testes

Execute a suíte de testes:

```powershell
python -m pytest -q
```

## OpenAPI / Documentação

A documentação interativa está disponível em:

- `http://127.0.0.1:8000/docs`
- `http://127.0.0.1:8000/redoc`

Também existe o arquivo `openapi.json` gerado na raiz do projeto.

Para regenerar o OpenAPI, execute:

```powershell
python app/scripts/export_openapi.py
```

Para gerar a coleção Postman, execute:

```powershell
python app/scripts/generate_postman_collection.py
```

O arquivo `postman_collection.json` será criado na raiz do projeto e pode ser importado diretamente no Postman ou Insomnia.

## Endpoints principais

- `POST /api/auth/login`
- `POST /api/auth/logout`
- `POST /api/customers`
- `GET /api/customers`
- `GET /api/customers/search`
- `GET /api/customers/{id}`
- `PUT /api/customers/{id}`
- `POST /api/orders`
- `GET /api/orders`
- `GET /api/orders/{id}`
- `PUT /api/orders/{id}`
- `POST /api/orders/{id}/assign`
- `POST /api/orders/{id}/status`
- `POST /api/orders/{id}/parts`
- `POST /api/parts`
- `GET /api/parts`
- `GET /api/parts/{id}`
- `PATCH /api/parts/{id}`
- `PATCH /api/parts/{id}/stock`
- `GET /api/reports/orders-by-status`
- `GET /api/reports/overdue`
- `GET /api/reports/technicians-top`
- `GET /api/reports/parts-used`
- `GET /api/reports/average-time`

## Regras de negócio implementadas

- Uma ordem não pode ser concluída sem técnico responsável.
- Ordens concluídas ou canceladas não podem ser editadas.
- Histórico de alterações de status é registrado.
- Cada técnico só pode ter até 5 ordens em andamento.
- Controle de prioridade: baixa, média, alta, urgente.
- Tempo de atendimento calculado automaticamente na conclusão.
- Ordens canceladas exigem motivo.
- Peças só podem ser vinculadas se houver estoque disponível.
- Estoque não pode ficar negativo.
- Movimentações de peças geram histórico.
- Integração externa com ViaCEP para preencher endereço do cliente pelo CEP.

## Usuários de exemplo

- Administrador: `admin@example.com` / `admin123`
- Técnico: `tech@example.com` / `tech123`
- Atendente: `attendant@example.com` / `attendant123`

## Observações

- O projeto já contém `openapi.json` gerado e pode ser importado em Postman ou Insomnia.
- O banco de dados SQLite padrão é criado em `./data/app.db`.
