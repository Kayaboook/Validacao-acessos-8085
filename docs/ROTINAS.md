# Descrição das rotinas

Resumo de cada rotina/etiqueta do ficheiro [`../src/FINAL.asm`](../src/FINAL.asm).

| Rotina | Descrição |
|--------|-----------|
| `BEM` | Mostra a mensagem de boas-vindas `**BEM-VINDO**`. |
| `LIMP` | Limpa o ecrã, escrevendo espaços nas posições `00h`–`29h`. |
| `LER` | Mostra `**PRESS-> D**` e aguarda a tecla de continuação (`0Dh`). |
| `UTIL` | Mostra `**UTILIZADOR?**`. |
| `LE_U` | Lê o utilizador do teclado, guarda em `2020h` e procura na tabela `3000h`–`3005h`. Em caso de igualdade salta para `UTL_X`; senão executa `ERR_U`. |
| `ERR_U` | Mostra `**ERRO UTIL.**`, repõe o pedido de utilizador e volta a `LE_U`. |
| `LIMP1` | Limpa o ecrã (variante usada após a leitura do utilizador). |
| `UTL_X` | Utilizador válido: aponta para a password correspondente (`+10h` no *byte* alto), guarda a password esperada em `2030h` e o seu endereço em `2031h`/`2032h`, e mostra `**UTILIZADOR=n**`. |
| `INT_P` | Mostra `PASS WORD :`. |
| `LE_P` | Lê a password, guarda em `2021h` e compara com `2030h`. Igual → `VAL_P`; diferente → `INV`. |
| `INV` | Mostra `**INVALIDA**`, repõe o pedido de password e volta a `LE_P`. |
| `LIMP2` | Limpa o ecrã (variante usada após a leitura da password). |
| `VAL_P` | Acesso válido: mostra `**PASS VALIDA**`, chama `ULTIM`, incrementa o contador `2022h`; ao 4.º acesso salta para `CHEIO`, senão chama `MUDPW` e segue para `LOOP`. |
| `CHEIO` | Mostra `ESPACO CHEIO` e `**PRESS-> F**`, aguarda `0Fh`, repõe o contador e retoma o `LOOP`. |
| `LOOP` | Ciclo principal repetido após o primeiro acesso (limpar, pedir utilizador, pedir password). |
| `MUDPW` | Pergunta `MUDAR PASS?` com `A=SIM B=NAO`. `A` (`0Ah`) → `FAZMD`; `B` (`0Bh`) → `FIMMD`. |
| `FAZMD` | Lê a nova password e escreve-a no endereço guardado (`2031h`/`2032h`) com `STAX D`; mostra `PASS OK!`. |
| `FIMMD` | Termina a mudança de password (`RET`). |
| `ULTIM` | Mostra `ULT=n` (último utilizador, a partir de `2023h`) e aplica um pequeno atraso. |
