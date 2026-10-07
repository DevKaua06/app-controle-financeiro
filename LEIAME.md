# Caderneta

App de controle de gastos pessoais. Você lança no celular, os dados vão para uma planilha do Google Sheets e o app mostra consulta, edição, exclusão e um resumo mensal com comparação entre meses.

## O que tem na pasta

| Arquivo | Para que serve |
|---|---|
| `index.html` | O app inteiro (tela de lançar, lista, resumo e ajustes) |
| `manifest.webmanifest`, `sw.js`, `icon-*.png` | Fazem o app instalar na tela inicial e abrir sem internet |
| `Code.gs` | Script que fica na sua planilha e recebe os lançamentos do app |

## Passo 1: planilha e script (uns 5 minutos)

1. Crie uma planilha nova no Google Sheets (pode chamar de "Gastos").
2. Menu **Extensões > Apps Script**.
3. Apague o que estiver no editor, cole o conteúdo de `Code.gs` e salve.
4. No topo do código, troque `TROQUE-ESTA-SENHA-POR-UMA-FRASE-LONGA` por uma frase longa e só sua. Anote, você vai colar a mesma no app. Salve de novo.
5. No seletor de funções (ao lado de "Executar"), escolha **autorizar** e clique em **Executar**. O Google vai pedir permissão. Como o script é seu e não passou por revisão do Google, aparece um aviso: clique em **Avançado** e depois em **Acessar (nome do projeto)**.
6. Clique em **Implantar > Nova implantação**, no ícone de engrenagem escolha **App da Web** e configure:
   - Executar como: **Eu**
   - Quem tem acesso: **Qualquer pessoa**
7. Clique em **Implantar** e copie o endereço que termina em `/exec`.

> Se você mudar o código do script depois, é preciso **Implantar > Gerenciar implantações > editar > Nova versão**. Salvar sozinho não atualiza o endereço publicado.

## Passo 2: colocar o app na internet

O app é só um conjunto de arquivos estáticos. Ele precisa estar em um endereço `https` para o celular permitir a instalação. Opções gratuitas:

- **Netlify Drop** (a mais simples): acesse `app.netlify.com/drop` e arraste a pasta do app. Você recebe um endereço na hora.
- **GitHub Pages** ou **Cloudflare Pages**, se você já usa um deles.

O endereço do app ficará público, mas ele **não contém nenhum dado seu**. Seus lançamentos vivem na planilha e no aparelho.

## Passo 3: conectar e instalar

1. Abra o endereço do app no celular.
2. Vá em **Ajustes**, cole o endereço `/exec` e a senha, e toque em **Salvar e testar conexão**. Deve aparecer "Conectado".
3. Instale:
   - **Android (Chrome):** menu ⋮ > **Instalar app** (ou **Adicionar à tela inicial**).
   - **iPhone (Safari):** botão de compartilhar > **Adicionar à Tela de Início**. Use sempre o ícone instalado, porque o Safari comum pode apagar dados do site depois de uns dias sem uso.

## Como o app funciona

- **Lançar:** valor, descrição (opcional), categoria, pagamento, data, status e, no crédito, número de parcelas. O pagamento, a pessoa e a data já vêm preenchidos para ser rápido. Se você deixar a descrição em branco, ela vira o nome da categoria.
- **Parcelas:** o valor digitado é o **total** da compra. O app cria uma linha por mês, divide sem perder centavos, e marca como Pendente as parcelas com data futura.
- **Lançamentos:** navegue por mês, busque por texto, filtre por categoria. Toque em um item para editar ou excluir. Em compras parceladas você escolhe excluir só aquela parcela, ela e as próximas, ou todas.
- **Resumo:** despesas do mês, receitas, saldo, valor a pagar, gasto por categoria com a variação em relação ao mês anterior, últimos 6 meses e maiores gastos. No mês em andamento, a comparação é feita com o **mesmo período** do mês anterior (por exemplo, até o dia 2 contra o dia 1 e 2 do mês passado), para não comparar um mês incompleto com um mês fechado.
- **Offline:** sem sinal, o lançamento fica guardado no aparelho e a barra do topo mostra quantos faltam enviar. Ao voltar a conexão, o app envia sozinho.
- **Ajustes:** edite as listas de categorias, formas de pagamento e pessoas. Se houver mais de uma pessoa, aparece o campo "Quem".

## Cuidados

- Quem tiver o endereço `/exec` **e** a senha consegue ler e gravar na sua planilha. Não compartilhe os dois. A senha fica só no aparelho onde você a digitou.
- Você pode editar a planilha à mão e as mudanças chegam ao app na próxima sincronização. Mas **não apague nem altere a coluna ID**, e saiba que linhas sem ID são ignoradas pelo app.
- A planilha tem as colunas: Data, Valor, Descrição, Categoria, Pagamento, Quem, Tipo, Status, Parcela, Total parcelas, Grupo, ID, Criado em, Atualizado em. A data é uma data real e o valor é número, então dá para fazer tabelas dinâmicas e gráficos direto no Sheets.
- Se dois aparelhos editarem o mesmo lançamento ao mesmo tempo, vale a última sincronização.
- Em **Ajustes > Exportar CSV** você baixa uma cópia de segurança.

## Se algo der errado

| Mensagem no app | O que verificar |
|---|---|
| "A senha (token) não confere" | A senha no app precisa ser idêntica à do `Code.gs`, e a implantação precisa estar na versão mais recente |
| "Resposta inesperada" | Na implantação, "Quem tem acesso" precisa ser **Qualquer pessoa** |
| "Não foi possível acessar a planilha" | Conexão com a internet e o endereço (deve terminar em `/exec`) |

## Ideias para a próxima versão

Lançamentos recorrentes (aluguel, assinaturas), meta de gasto por categoria com alerta, lançar por mensagem no Telegram, e compartilhar com mais uma pessoa.
