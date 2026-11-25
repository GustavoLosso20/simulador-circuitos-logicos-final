# Banco de dados — MongoDB

O projeto usa **MongoDB** (NoSQL), acessado pelo Mongoose. Não existe `schema.sql`; veja `schema.md`. Cada esquemático é um documento da coleção `circuits`.

## Arquivos desta pasta
| Arquivo | Equivalente pedido |
|---|---|
| `schema.md` | schema.sql (estrutura da coleção) |
| `seed.json` | seed.sql (3 circuitos de exemplo: meio somador, latch SR, contador assíncrono de 2 bits) |
| `diagrama-er.png` | Diagrama ER (modelo de documentos) |
| `backup/` | Dump (gerar com `mongodump`, ver abaixo) |

## Preparar o banco
**Opção A — MongoDB local (Docker):** `docker run -d -p 27017:27017 mongo:7` e use `MONGODB_URI=mongodb://127.0.0.1:27017/logic-sim`.

**Opção B — MongoDB Atlas:** crie um cluster, um usuário com leitura/escrita, libere o IP em *Network Access* e copie a *connection string* para `MONGODB_URI` no `.env`, incluindo o nome do banco (ex.: `.../logic-sim?...`).

## Importar os dados de exemplo
Requer o MongoDB Database Tools (`mongoimport`).
```bash
mongoimport --uri "$MONGODB_URI" --collection circuits --file banco-de-dados/seed.json --jsonArray
```
No PowerShell: `mongoimport --uri $env:MONGODB_URI --collection circuits --file banco-de-dados/seed.json --jsonArray`

Alternativa sem ferramentas: abra o simulador e escolha cada circuito em “Abrir exemplo…” e clique em “Salvar”.

## Gerar backup (dump)
```bash
mongodump --uri "$MONGODB_URI" --collection circuits --out banco-de-dados/backup
```

## Restaurar backup
```bash
mongorestore --uri "$MONGODB_URI" banco-de-dados/backup
```

> Nunca inclua senha real do Atlas nos arquivos entregues.
