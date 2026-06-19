# Validação de Acessos em Assembly 8085

Sistema de **validação de acessos** (utilizador + *password*) escrito em *assembly* para
microprocessador **Intel 8085**, desenvolvido no âmbito da unidade curricular de
**Sistemas com Microprocessadores** (Licenciatura em Engenharia Eletrotécnica e das
Telecomunicações, EST – IPCB).

O programa pede um utilizador, valida-o contra uma base de dados em memória, pede a
respetiva *password* e, em caso de sucesso, permite ainda **alterar a password** e leva
registo do número de acessos.

---

## Funcionalidades

- Ecrã de boas-vindas e mensagens formatadas no *display*.
- Leitura e **validação do utilizador** contra uma tabela de 6 utilizadores.
- Leitura e **validação da password** correspondente ao utilizador autenticado.
- Mensagens de **erro** distintas para utilizador inválido e password inválida, com
  repetição do pedido em vez de bloquear.
- **Mudança de password** após login válido (escreve a nova password na memória).
- Contador de acessos: ao fim de 4 acessos o "espaço" é considerado cheio e é pedido
  um *reset* antes de continuar.
- Funcionamento em ciclo contínuo (`LOOP`) após o primeiro acesso válido.

---

## Mapa de memória

| Endereço | Conteúdo |
|----------|----------|
| `3000h`–`3005h` | Base de dados de **utilizadores** (`01h`…`06h`) |
| `4000h`–`4005h` | Base de dados de **passwords** (`06h`…`01h`) |
| `2020h` | Utilizador lido do teclado |
| `2021h` | Password lida do teclado |
| `2022h` | Contador de acessos |
| `2023h` | Cópia do utilizador (para o aviso `ULT=`) |
| `2030h` | Password esperada do utilizador autenticado |
| `2031h` / `2032h` | Endereço (high/low) da password na zona `4000h` |

> Cada utilizador em `3000h+n` corresponde à password em `4000h+n`. O programa obtém o
> endereço da password somando `10h` ao *byte* alto do ponteiro (`3000h` → `4000h`).

---

## Portos de E/S

| Porto | Direção | Função |
|-------|---------|--------|
| `00h` | OUT | Define a **posição do cursor** no *display* |
| `01h` | OUT | Escreve um **carácter ASCII** na posição atual |
| `03h` | IN  | Lê o **teclado** |

---

## Fluxo do programa

1. `BEM` – mensagem de boas-vindas.
2. `LER` – aguarda que o utilizador pressione a tecla de continuação.
3. `UTIL` + `LE_U` – pede e valida o utilizador (`ERR_U` em caso de erro).
4. `UTL_X` – calcula o endereço da password e guarda os ponteiros.
5. `INT_P` + `LE_P` – pede e valida a password (`INV` em caso de erro).
6. `VAL_P` – acesso válido: mostra o último acesso (`ULTIM`), incrementa o contador e
   oferece a mudança de password (`MUDPW`).
7. `CHEIO` – ao 4.º acesso pede *reset* antes de retomar o `LOOP`.

---

## Como executar

O código foi escrito para um **simulador de 8085** que usa a sintaxe com etiquetas
prefixadas por `:` (por exemplo `:BEM`) e os portos descritos acima.

1. Abrir o simulador de 8085 usado nas aulas.
2. Carregar o ficheiro [`src/FINAL.asm`](src/FINAL.asm) (o conteúdo é o mesmo do
   ficheiro original; a extensão `.asm` é apenas para melhor leitura no GitHub).
3. Montar (*assemble*) e correr a partir do endereço inicial do código.

> As tabelas em `3000h` e `4000h` (`ORG`/`DB`) são carregadas junto com o programa, pelo
> que não é necessário introduzir os dados manualmente.

---

## Estrutura do repositório

```
validacao-acessos-8085/
├── README.md
├── LICENSE
├── .gitignore
├── .gitattributes
├── docs/
│   └── ROTINAS.md      # descrição rotina a rotina
└── src/
    └── FINAL.asm       # código-fonte 8085
```

---

## Autor

Projeto académico — Sistemas com Microprocessadores, EST/IPCB.
Echarme Kaya · 
Paulo Kiesse Victor ·  
Patrício Leonardo Samuel · 

## Licença

Distribuído sob a licença MIT. Ver [`LICENSE`](LICENSE).
