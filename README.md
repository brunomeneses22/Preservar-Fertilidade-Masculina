# Decidir Antes

Ferramenta web de apoio à decisão (PDA, *patient decision aid*) sobre preservação da fertilidade em homens com câncer, desenvolvida segundo os padrões internacionais IPDAS 5.0.

> **Rascunho para validação.** O conteúdo ainda não foi revisado pelo comitê nem testado com pacientes. Não usar no atendimento.

## Como funciona

O app é *stateless*: não há servidor nem banco de dados. As respostas viajam dentro do link e do QR code, no fragmento da URL (depois do `#`), que o navegador não envia ao servidor.

1. **Profissional (aba Profissional):** preenche a triagem de 4 perguntas (puberdade, tratamento planejado, prazo e interesse). O app mostra a categoria de risco e gera um QR code e um link `#pda-OF-XXX-XXX-XXX` com essas respostas codificadas. Também gera um texto para o prontuário.
2. **Paciente, no próprio celular:** abre o QR ou o link, lê por que o assunto aparece agora e continua o guia de 5 passos de forma anônima: opções lado a lado, números em pictogramas, exercício de valores, inclinação, teste SURE e perguntas para a consulta. No final, toca em **Gerar meu resumo**. O app cria um novo QR e um link `#ver-OF-XXXX-XXXX-XXXX-XXXX` com a triagem e todas as respostas dele.
3. **Envio:** o botão **Abrir no WhatsApp** abre uma conversa com o número cadastrado em `CONFIG.whatsapp`, com o link já na mensagem. Enviar é opcional.
4. **Quem recebe o link** vê todas as respostas numa tela somente leitura. Na aba **Serviço**, também dá para colar o código.

O paciente pode abrir o guia sem triagem (`#pda`). Nesse caso, o resumo final indica "sem triagem profissional".

## Formato dos códigos

Os dois códigos usam base32 de Crockford e terminam em 1 caractere de conferência. Esse caractere é uma soma ponderada módulo 31, que detecta erros de digitação de um caractere e trocas entre vizinhos. Na leitura, são aceitos letras minúsculas, hífens, `O` (lido como 0) e `I`/`L` (lidos como 1).

- **Código da triagem:** 9 caracteres (`OF-XXX-XXX-XXX`). Leva puberdade, tratamentos, início, interesse, data da triagem e uma parte aleatória.
- **Código completo:** 16 caracteres (`OF-XXXX-XXXX-XXXX-XXXX`). Leva tudo da triagem, mais os dias até o preenchimento, um nível por valor, a inclinação, as respostas do SURE e as perguntas marcadas.

### Versão do questionário

Todo código registra a **versão do questionário** com que foi respondido, nos 6 bits finais do número codificado. Os textos de todas as versões ficam em `QUESTIONARIOS`, no `index.html`. Assim, um código antigo é sempre exibido com as perguntas e opções que o paciente viu. A tela de leitura mostra a versão, por exemplo "Versão 1 do questionário (02/11/2026)".

Para mudar perguntas, crie uma nova versão e nunca edite uma existente. As regras estão no [`CLAUDE.md`](CLAUDE.md).

### Privacidade e limites

**É codificação, não criptografia.** Os códigos não contêm nome, documento nem contato, mas quem tem o link consegue ler as respostas. O app informa isso ao paciente antes do envio. A tela do link é somente leitura, mas isso não impede que alguém monte outro código à mão. Garantir que um código não foi forjado exigiria assinatura com chave secreta e, portanto, um servidor.

O QR code é gerado no próprio navegador pela biblioteca qrcode-generator 1.4.4 (Kazuhiko Arase, licença MIT), incluída no `index.html`. Não há dependência de CDN para isso.

## Estrutura

| Caminho | Conteúdo |
| --- | --- |
| `index.html` | O app inteiro, num único arquivo estático |
| `docs/plano-12-semanas.md` | Plano de desenvolvimento, base de evidências e checklist IPDAS 5.0 |
| `docs/protocolo-cep-ufc.md` | Projeto de pesquisa do teste alfa, no modelo do CEP-UFC, com apêndices e TCLEs |
| `CLAUDE.md` | Regras permanentes de manutenção, inclusive o versionamento do questionário |

## Como usar

Abra `index.html` no navegador. Para publicar, qualquer hospedagem estática serve (GitHub Pages, Netlify). As fontes vêm do Google Fonts. Sem internet, o app usa as fontes do sistema e continua funcionando.

## Antes de publicar

Edite o bloco `CONFIG` no início do script do `index.html`:

- `whatsapp`: número real do serviço (DDI + DDD + número). O valor atual é um exemplo.
- `appUrl`: endereço público do app, usado nos links e nos QR codes.
- `financiamento`: declaração de quem financia o desenvolvimento e a hospedagem.
- `rascunho`: mude para `false` só depois da validação.

Os custos de coleta e de armazenamento ainda aparecem como "a definir" no passo 1 do guia ("As duas opções, lado a lado").

## Autoria

Dr. Bruno Hállan, urologista e andrologista, Fortaleza, CE (CRM-CE 16919, RQE 13791).
