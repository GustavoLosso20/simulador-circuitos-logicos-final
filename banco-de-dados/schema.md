# Esquema da coleção `circuits` (MongoDB / Mongoose)

Equivalente ao `schema.sql`: o banco é NoSQL, então a estrutura é definida pelo Mongoose em `codigo-fonte/src/server/models/Circuit.ts` e pelos tipos de `src/shared/types.ts`. O banco e a coleção são criados automaticamente no primeiro uso.

```
circuits (coleção)
└── Circuit (documento)
    ├── _id        ObjectId (gerado)
    ├── name       String, obrigatório, até 80 caracteres
    ├── nodes[]    Node
    ├── wires[]    Wire
    ├── createdAt  Date (automático)
    └── updatedAt  Date (automático)

Node (subdocumento, sem _id)
├── id      String, obrigatório, até 64, único no circuito
├── type    INPUT | OUTPUT | CLOCK | AND | OR | NOT | NAND | NOR | XOR | DFF | JKFF
├── x, y    Number, obrigatórios (posição no quadro)
├── label   String, até 32 (opcional)
├── value   Boolean (opcional, estado da chave INPUT)
└── period  Number, 50 a 600000 ms (opcional, período do CLOCK)

Wire (subdocumento, sem _id)
├── fromNode  String  (nó de origem)
├── fromPin   Number ≥ 0  (índice do pino de saída)
├── toNode    String  (nó de destino)
└── toPin     Number ≥ 0  (índice do pino de entrada)
```

## Pinos por componente
| Tipo | Entradas | Saídas |
|---|---|---|
| INPUT, CLOCK | — | 1 |
| OUTPUT | 1 | — |
| NOT | 1 | 1 |
| AND, OR, NAND, NOR, XOR | 2 | 1 |
| DFF | D, CLK | Q, Q̅ |
| JKFF | J, K, CLK | Q, Q̅ |

## Integridade (validação `pre('validate')`)
- IDs de componentes duplicados são rejeitados.
- Fio ligado a componente inexistente é rejeitado.
- Fio ligado a pino inexistente é rejeitado.
