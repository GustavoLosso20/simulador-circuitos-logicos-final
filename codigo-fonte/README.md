# Simulador Virtual de Circuitos Digitais e Registro de Eventos

## 👥 Integrantes
- Gustavo Afonso Losso — RA 02310201 — [função: Back-End]
- João Paulo Guinancio Ferreira — RA 02310231 — [função: Front-End]

Orientador(a): Hélio · Instituição: Universidade do Vale do Paraíba (Univap)

## 📌 Descrição
Ambiente gráfico em navegador para posicionar portas lógicas (AND, OR, NOT, NAND, NOR, XOR), chaves, clocks, LEDs e flip-flops (D e JK), ligar os pinos, simular em tempo real (contínuo ou passo a passo) e salvar/carregar esquemáticos na nuvem. Inclui diagrama de tempo.

## 🛠️ Tecnologias
- Frontend: TypeScript + HTML5 Canvas 2D (`src/client`, `public/`)
- Backend: Node.js 24+, Express 4, CORS, dotenv (`src/server`), executado com `tsx`
- Motor de simulação: TypeScript compartilhado entre front e back (`src/shared`)
- Banco de dados: MongoDB (local ou Atlas) com Mongoose 8

```
Navegador (Canvas 2D + TS) ──HTTP/JSON──▶ Express (API REST) ──Mongoose──▶ MongoDB
        └──────── src/shared (tipos + motor de simulação) ────────┘
```

| Pasta | Conteúdo |
|---|---|
| `src/shared/` | `types.ts` (grafo: nós, fios, pinos) e `engine.ts` (motor de simulação) |
| `src/server/` | `index.ts`, `models/Circuit.ts`, `routes/circuits.ts` |
| `src/client/` | `main.ts` (interface e diagrama de tempo), `api.ts`, `examples.ts` |
| `public/` | `index.html`, `style.css` e JS compilado em `public/js` (gerado no build) |

> Frontend e backend ficam no mesmo projeto porque compartilham `src/shared`; por isso não há pastas `frontend/` e `backend/` separadas.

## ⚙️ Como executar
Requisitos: Node.js 24 ou superior e um MongoDB (local ou Atlas).

1. Clone o repositório: `git clone [link do repositório]`
2. Instale as dependências: `npm install`
3. Configure o ambiente: `cp .env.example .env` (no Windows: `copy .env.example .env`) e ajuste `MONGODB_URI`
4. Banco: o MongoDB cria a coleção `circuits` sozinho. Para carregar exemplos, veja `../banco-de-dados/instrucoes-banco.md`
5. Rode o projeto: `npm start` (compila o cliente e inicia o servidor) ou `npm run dev` (servidor com *watch*)
6. Abra `http://localhost:3000`

MongoDB local rápido com Docker: `docker run -d -p 27017:27017 mongo:7`

Outros scripts: `npm run build:client`, `npm run dev:client`, `npm run typecheck`.

### Variáveis de ambiente (`.env.example`)
| Variável | Descrição | Padrão |
|---|---|---|
| `PORT` | Porta do servidor | `3000` |
| `MONGODB_URI` | String de conexão do MongoDB | `mongodb://127.0.0.1:27017/logic-sim` |
| `CORS_ORIGIN` | Origem permitida pelo CORS | `*` |

## 🔌 API REST
| Método | Rota | Descrição |
|---|---|---|
| GET | `/api/health` | Estado do servidor e do banco |
| GET | `/api/circuits` | Lista (id, nome, datas) |
| GET | `/api/circuits/:id` | Circuito completo (nós + fios) |
| POST | `/api/circuits` | Cria |
| PUT | `/api/circuits/:id` | Atualiza |
| DELETE | `/api/circuits/:id` | Remove |

Corpo: `{ "name": "...", "nodes": [{id,type,x,y,label?,value?,period?}], "wires": [{fromNode,fromPin,toNode,toPin}] }`.
O servidor valida tipos de componente, IDs únicos e fios ligados a pinos existentes (erros 400; 404 para circuito inexistente).

## 🖱️ Uso
- **Adicionar:** clique num componente da barra lateral ou arraste-o para o quadro.
- **Ligar:** arraste de um pino de saída (direita) até um pino de entrada (esquerda).
- **Chaves:** clique para alternar 0/1. **Clock:** duplo clique altera o período.
- **Passo:** inverte os clocks manualmente, útil para analisar cada estágio.
- **Diagrama de tempo:** abaixo do quadro, mostra chaves, clocks e LEDs.
- **Exemplos:** meio somador, latch SR e contador assíncrono de 2 bits.

## 🗄️ Banco de dados
Instruções em `../banco-de-dados/instrucoes-banco.md`

## 📚 Referências
Ver `../referencias/referencias-abnt.pdf`

## 🔗 Repositório
[link do Git]
