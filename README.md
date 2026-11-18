# bnb-lab · Meu primeiro agente de IA na BNB Chain

> 🚧 **Projeto em construção.** O agente ainda está sendo desenvolvido e testado. Funciona só na **rede de teste** (BSC testnet), com moedas que não valem dinheiro.

Este repositório é o meu laboratório do **BNB Builder Sessions · Brasil** (18/11/2026, anfitrião: Instituto Florescer Caldeira). Aqui está o agente que comecei a construir no evento e, mais abaixo, um **passo a passo para você criar o seu próprio agente**.

---

## O que é este agente

O `meuagente` é um **agente vendedor** na BNB Chain, criado com o [BNB Agent Studio](https://www.npmjs.com/package/@bnbagent/studio-cli) (comando `bag`). Ele funciona como uma loja automática:

1. Alguém (uma pessoa ou outro agente) pede um serviço.
2. O agente responde com um preço assinado pela carteira dele.
3. Quem pediu paga em moeda de teste.
4. O agente faz o trabalho com IA e entrega o resultado, registrando tudo na blockchain.

A IA **nunca mexe em dinheiro**. Preço e assinaturas são feitos por código fixo, não pela IA.

| Item | Como está hoje |
|---|---|
| Rede | BSC testnet (só treino) |
| Formas de venda | A2A (conversa entre agentes) e X402 (pagamento por pedido) |
| Preço | US$ 0,10 por pedido, em moedas de teste (TEST_U, TEST_USDC, TEST_USDT) |
| IA usada | Pieverse, modelo gratuito |
| Especialidade | ainda genérica. **Próximo passo:** definir o serviço que ele vai vender |

### Andamento

- [x] **CRIADO**: projeto montado, carteira de teste criada
- [ ] **TESTADO**: rodando no meu computador
- [ ] **PUBLICADO**: no ar na BNB Chain
- [ ] **VERIFICADO**: com RG (`agent_id`) registrado na BNB Chain

### Onde fica cada coisa

```
bnb-lab/
├── README.md               ← este arquivo
├── .gitignore              ← protege senhas e carteira
└── meuagente/
    ├── app/agent/
    │   ├── studio.toml     ← configurações do agente (rede, preço, IA)
    │   └── src/            ← código do agente
    └── agentcore/          ← configuração para publicar na nuvem
```

### Sobre segurança

A pasta `.studio/`, onde ficam a **carteira** e as **senhas**, **não está neste repositório** de propósito. Ela fica só no computador de quem criou o agente. Se você clonar este projeto, ele não vai funcionar sozinho: você precisa da sua própria carteira de teste. Para isso, siga o passo a passo abaixo ou rode `bag doctor` dentro de `meuagente/app/agent` para ver o que falta.

---

## Passo a passo: crie o seu próprio agente

Este roteiro segue o guia do laboratório. O agente de exemplo **olha o preço do BNB e explica, em português, se ele está subindo ou caindo**. **Ninguém precisa saber programar.**

Dica: façam em dupla. Uma pessoa digita e a outra lê o guia em voz alta. Basta uma pessoa da dupla ter IA instalada.

### ⚠️ Regras de ouro

- **Carteira nova:** use só uma carteira de teste criada para isso, nunca a sua carteira pessoal. Na publicação de teste, a chave da carteira é enviada para a plataforma que hospeda o agente.
- **Senha em segredo:** nunca escreva a senha nem as palavras secretas da carteira no chat da IA.
- **Leia antes de aprovar:** a IA pede licença antes de cada ação. Aprove uma de cada vez, nunca "tudo".
- **Só treino:** as moedas são de teste e não valem dinheiro.
- **Nunca suba a pasta `.studio/` para o GitHub.** Use um `.gitignore` como o deste repositório.

### Parte 1: Preparar o computador (uns 20 minutos)

**Passo 1. Abra o terminal**

O terminal é uma janela onde você escreve ordens para o computador. Você só vai colar textos prontos e apertar Enter.

- **Mac:** aperte `Command + Espaço`, digite **Terminal** e aperte Enter.
- **Windows:** clique em **Iniciar**, digite **PowerShell** e aperte Enter.

**Passo 2. Instale o Node.js**

O BNB Agent Studio precisa desse programa para funcionar.

- **Mac:** entre em [nodejs.org](https://nodejs.org), baixe a versão recomendada (LTS) e clique em **Continuar** até o fim.
- **Windows:** no PowerShell, cole o comando abaixo. Se aparecer uma pergunta, digite `S` (ou `Y`) e aperte Enter.
  ```powershell
  winget install OpenJS.NodeJS.LTS
  ```

Terminou? **Feche o terminal e abra de novo**, para o computador "enxergar" o programa novo.

**Passo 3. Instale a IA (uma pessoa por dupla)**

Escolha uma das duas:

- **Cursor** (mais fácil para quem nunca usou): baixe em [cursor.com](https://cursor.com), instale e entre com uma conta. Tem versão grátis, com limite de uso.
- **Claude Code** (para quem já assina um plano pago do Claude): siga as instruções em [claude.com/claude-code](https://claude.com/claude-code).

**Passo 4. Instale o BNB Agent Studio**

No terminal, rode:

```bash
npm install -g @bnbagent/studio-cli@latest
```

Leva alguns minutos e muitas letras vão passar na tela. É normal.

Depois, ensine a sua IA a usar o Studio. Escolha a linha que combina com a IA que você instalou:

```bash
bag skills install --scope user --target cursor        # se usa Cursor
bag skills install --scope user --target claude-code   # se usa Claude Code
```

**Deu certo?** Feche e abra o terminal e rode:

```bash
bag --version
```

Se aparecer um número (0.0.16 ou maior), o computador está pronto.

### Parte 2: Criar o agente

**Passo 5. Abra a IA numa pasta nova**

1. Na Área de Trabalho, crie uma pasta chamada `laboratorio`.
2. **No Cursor:** abra o menu **File → Open Folder** e escolha a pasta `laboratorio`. Depois clique no ícone de chat da IA, do lado direito.
   **No Claude Code:** no terminal, entre na pasta (`cd` + caminho da pasta) e digite `claude`.

**Passo 6. Peça para a IA criar o agente**

Cole no chat da IA um pedido como o exemplo abaixo. Troque `monitorbnb` por um nome seu. O nome só pode ter **letras e números**, sem espaço, hífen ou acento, começando com letra e com até 23 caracteres. Exemplo: `monitord07`.

```text
Use o bnbagent-studio para criar um agente chamado monitorbnb na BSC testnet, com carteira nova e preço 0.
O agente deve buscar o preço do BNB e explicar, em português, se está subindo, caindo ou misto,
mostrando o preço atual, a média e a variação, com horário. No final do relatório, inclua o aviso:
"não é recomendação de investimento".
```

Se a IA perguntar:

| Pergunta | Resposta |
|---|---|
| Rede | BSC testnet |
| Carteira | a opção padrão (carteira nova) |
| Senha | invente uma senha de treino e **anote no papel**. Digite só onde a IA pedir |
| Preço | 0 |
| Outra coisa | deixe o padrão |

Deu certo se a IA disser que o projeto foi criado. Você chegou a **CRIADO** ✅

**Passo 7. Teste o agente**

No chat da IA, escreva só esta frase:

```text
Inicie meu agente e me mostre um relatório de teste.
```

Confira:

- [ ] Veio um relatório em português.
- [ ] Aparecem o preço do BNB, a média e a variação, com horário.
- [ ] A conclusão (alta, baixa ou mista) combina com os números.
- [ ] No final, vem o aviso "não é recomendação de investimento".

Tudo certo? Você chegou a **TESTADO** ✅ O agente funciona no seu computador.

### Parte 3: Colocar no ar

**Passo 8. Publique o agente**

Agora você vai trabalhar direto no terminal, sem a IA. No Cursor, abra o menu **Terminal → New Terminal**. Rode **uma linha por vez**, sempre trocando `monitorbnb` pelo nome do seu agente:

| # | O que faz | Comando |
|---|---|---|
| 1 | Entrar na pasta do agente | `cd monitorbnb/app/agent` |
| 2 | Colocar moedas de teste na carteira | `bag wallet fund` |
| 3 | Entrar com a conta do GitHub | `bag platform login` |
| 4 | Liberar o teste grátis de 48 horas | `bag platform credit` |
| 5 | Conferir se está tudo pronto | `bag deploy prepare` |
| 6 | Publicar | `bag deploy --provider bnb` |
| 7 | Registrar o RG do agente | `bag deploy verify --provider bnb` |

**Pediu senha?** Digite a senha de treino e aperte Enter. As letras não aparecem enquanto você digita, e isso é normal.

O teste grátis dura **48 horas**. Depois disso, o agente sai do ar.

**Passo 9. Dê um nome e anote o RG**

1. O agente nasce com o nome `studio-agent`. No chat da IA, peça para mudar o nome dele para o nome que você escolheu.
2. A IA mostra um número chamado **`agent_id`**. É o **RG do seu agente** na BNB Chain. Anote.

Conseguiu? Você chegou a **VERIFICADO** ✅ O agente está no ar, com RG na BNB Chain.

### Deu errado? Calma!

1. **Pergunte à IA:** copie só a mensagem de erro, cole no chat e escreva "O que eu faço?".
2. **Rode o diagnóstico:** dentro da pasta `app/agent`, rode `bag doctor`.
3. **Plano B:** não conseguiu colocar no ar? Tudo bem! Se o agente funciona no seu computador, você já chegou a **TESTADO**, e o agente é seu.

**Até onde você chegou?** CRIADO → TESTADO → PUBLICADO → VERIFICADO

---

## Avisos

- Projeto educacional, feito para treino na **rede de teste** da BNB Chain.
- Nada aqui é recomendação de investimento.
- Ainda em construção: o código e as instruções podem mudar.
