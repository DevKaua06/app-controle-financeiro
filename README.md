# Caderneta

Aplicativo de **controle de gastos pessoais** que funciona direto no celular, sem loja de aplicativos. Você registra cada gasto ou receita em poucos toques, os dados vão para uma **planilha do Google Sheets** e o app mostra um resumo mensal com comparação entre meses.

Feito com HTML, CSS e JavaScript puro, sem frameworks e sem dependências externas. Funciona como **PWA**: instala na tela inicial e abre mesmo sem internet.

> Projeto pessoal, em evolução.

**Acesse:** `https://SEU-USUARIO.github.io/caderneta/`

---

## Funcionalidades

### Lançar rápido
- Receita ou despesa, com valor em destaque na tela
- Categoria, forma de pagamento e pessoa em botões de um toque
- Data já preenchida com hoje, com atalhos para "Hoje" e "Ontem"
- Descrição opcional (se ficar em branco, usa o nome da categoria)
- Autocompletar de descrições já usadas e sugestão automática de categoria
- Status **Pago** ou **Pendente**
- **Compras parceladas** no crédito: o app divide o total em parcelas (sem perder centavos), cria uma linha por mês e marca como pendentes as parcelas futuras
- Lista dos últimos lançamentos logo abaixo do formulário

### Consultar, editar e excluir
- Navegação por mês, com busca por texto e filtro por categoria
- Lançamentos agrupados por dia, com o total gasto em cada dia
- Toque em um lançamento para editar ou excluir
- Em compras parceladas, escolha excluir só uma parcela, ela e as próximas, ou todas

### Resumo mensal
- Despesas, receitas, saldo e valor a pagar
- Gasto por categoria, com percentual e variação em relação ao mês anterior
- Gráfico dos últimos 6 meses (toque em uma barra para abrir o mês)
- Maiores gastos do mês
- **Comparação justa:** no mês em andamento, o app compara com o **mesmo período** do mês anterior (por exemplo, do dia 1 ao dia 12 contra do dia 1 ao dia 12), em vez de comparar um mês incompleto com um mês fechado

### Funciona sem internet
- Sem sinal, os lançamentos ficam guardados no aparelho
- A barra do topo mostra o estado: *Sincronizado*, *N para enviar*, *Offline* ou *Falha ao sincronizar*
- Quando a conexão volta, o app envia sozinho

### Ajustes
- Conexão com a planilha (endereço do script e senha)
- Listas editáveis de categorias de despesa, categorias de receita, formas de pagamento e pessoas
- Exportação dos dados em CSV (cópia de segurança)
- Tema claro e escuro automáticos, seguindo o celular

---

## Como funciona

```
 Celular (PWA)                         Google
┌──────────────────────┐   HTTPS    ┌──────────────────┐      ┌────────────────┐
│ Caderneta            │  ───────►  │ Google Apps      │ ───► │ Google Sheets  │
│ - guarda no aparelho │  ◄───────  │ Script           │ ◄─── │ (aba           │
│ - fila de envio      │  lista     │ (Web App + senha)│      │  "Lançamentos")│
└──────────────────────┘            └──────────────────┘      └────────────────┘
```

1. Cada lançamento é salvo primeiro **no aparelho** (`localStorage`) e entra numa fila de envio.
2. Quando há internet, o app envia a fila para o **Apps Script**, que grava, edita ou apaga as linhas na planilha.
3. O script devolve a lista completa e o app atualiza sua cópia local.
4. Cada lançamento tem um **ID único**, o que evita duplicação mesmo se o envio for repetido.

A planilha fica com estas colunas:

| Data | Valor | Descrição | Categoria | Pagamento | Quem | Tipo | Status | Parcela | Total parcelas | Grupo | ID | Criado em | Atualizado em |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

A data é uma data real e o valor é numérico, então dá para criar tabelas dinâmicas e gráficos direto no Google Sheets. Você também pode editar a planilha à mão, e as mudanças chegam ao app na próxima sincronização. Não apague nem altere a coluna **ID**.

---

## Tecnologias

- HTML, CSS e JavaScript (sem frameworks, sem bibliotecas)
- **PWA:** Web App Manifest + Service Worker
- **localStorage** para guardar os dados no aparelho
- **Google Apps Script** (Web App) como ponte com a planilha
- **Google Sheets** como armazenamento na nuvem
- **GitHub Pages** para hospedagem

---

## Arquivos do repositório

| Arquivo | Função |
|---|---|
| `index.html` | O app inteiro: telas, estilos e lógica |
| `manifest.webmanifest` | Permite instalar o app na tela inicial |
| `sw.js` | Service Worker: abre o app sem internet |
| `icon-192.png`, `icon-512.png` | Ícones do app |

O script da planilha (`Code.gs`) **não fica neste repositório de propósito**: ele contém a senha de acesso à sua planilha, e este repositório é público. Ele é colado apenas no editor do Apps Script.

---

## Como usar

### 1. Publicar no GitHub Pages
1. Crie um repositório público e envie os 5 arquivos acima para a raiz.
2. Em **Settings > Pages**, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)` e salve.
3. Após 1 a 3 minutos, o app estará em `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.

### 2. Criar a ponte com a planilha
1. Crie uma planilha no Google Sheets.
2. Abra **Extensões > Apps Script** e cole o conteúdo do `Code.gs`.
3. Troque o valor de `SENHA` por uma frase longa e só sua.
4. Execute a função `autorizar` uma vez e conceda as permissões.
5. Vá em **Implantar > Nova implantação > App da Web**, com *Executar como: Eu* e *Quem tem acesso: Qualquer pessoa*.
6. Copie o endereço que termina em `/exec`.

### 3. Instalar no celular
- **Android (Chrome):** menu ⋮ > **Instalar app**.
- **iPhone (Safari):** botão de compartilhar > **Adicionar à Tela de Início**.

Abra o app pelo ícone, vá em **Ajustes**, cole o endereço `/exec` e a senha, e toque em **Salvar e testar conexão**.

---

## Privacidade e segurança

- Os dados ficam **na sua planilha e no seu aparelho**. O repositório público contém só o código do app, nenhum dado pessoal.
- O endereço do script e a senha são digitados no app e ficam **apenas no aparelho**, nunca no código.
- Quem tiver o endereço `/exec` **e** a senha consegue ler e gravar na planilha. Não compartilhe os dois.
- Não publique o `Code.gs` com a sua senha real.

---

## Limitações

- A sincronização acontece com o app aberto (ao abrir, ao voltar para ele e ao recuperar a conexão), não em segundo plano.
- Se dois aparelhos editarem o mesmo lançamento ao mesmo tempo, vale a última sincronização.
- Limpar os dados do navegador apaga a cópia local; lançamentos ainda não enviados se perdem.
- No iPhone, use o ícone instalado: o Safari comum pode limpar dados de sites pouco usados.

---

## Próximos passos

- [ ] Lançamentos recorrentes (aluguel, assinaturas)
- [ ] Metas de gasto por categoria, com alerta
- [ ] Lançar por mensagem (bot do Telegram)
- [ ] Compartilhar com mais uma pessoa
- [ ] Versão Android nativa (Kotlin, Jetpack Compose e Room) com banco local e sincronização com o Sheets
