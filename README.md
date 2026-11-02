# Decidir Antes

Ferramenta web de apoio à decisão (PDA, *patient decision aid*) sobre preservação da fertilidade em homens com câncer, desenvolvida segundo os padrões internacionais IPDAS 5.0.

> **Rascunho para validação.** O conteúdo ainda não foi revisado pelo comitê nem testado com pacientes. Não usar no atendimento.

## Como funciona

O app é *stateless*: não há servidor nem banco de dados. As respostas viajam dentro do link e do QR code, no fragmento da URL (depois do `#`), que o navegador não envia ao servidor.

1. **Profissional (aba Profissional):** preenche a triagem de 4 perguntas (puberdade, tratamento planejado, prazo e interesse). O app mostra a categoria de risco e gera um QR code e um link `#pda-OF-XXX-XXX-XXX` com essas respostas codificadas. Também gera um texto para o prontuário.
2. **Paciente, no próprio celular:** abre o QR ou o link e continua o guia de 6 passos de forma anônima: opções lado a lado, números em pictogramas, exercício de valores, inclinação, teste SURE e perguntas para a consulta. No final, toca em **Gerar meu resumo**. O app cria um novo QR e um link `#ver-OF-XXXX-XXXX-XXXX-XXXX` com a triagem e todas as respostas dele.
3. **Envio:** o botão **Abrir no WhatsApp** abre uma conversa com o número cadastrado em `CONFIG.whatsapp`, com o link já na mensagem. Enviar é opcional.
4. **Quem recebe o link** vê todas as respostas numa tela somente leitura. Na aba **Serviço**, também dá para colar o código.

O paciente pode abrir o guia sem triagem (`#pda`). Nesse caso, o resumo final indica "sem triagem profissional".

## Formato dos códigos

Os dois códigos usam base32 de Crockford e terminam em 1 caractere de conferência. Esse caractere é uma soma ponderada módulo 31, que detecta erros de digitação de um caractere e trocas entre vizinhos. Na leitura, são aceitos letras minúsculas, hífens, `O` (lido como 0) e `I`/`L` (lidos como 1).

**Código da triagem** (versão 1, 9 caracteres, `OF-XXX-XXX-XXX`), com 40 bits:

| Campo | Valores |
| --- | --- |
| Versão | 1 |
| Puberdade | Sim, não ou não sabe |
| Tratamentos | Um bit por opção da triagem (9 opções) |
| Início | Até 3 dias, 4 a 7, 8 a 14, mais de 14, já começou ou não informado |
| Interesse | Quer conversar, indeciso, não agora ou sem triagem profissional |
| Data | Dia da triagem (ciclo de 1024 dias a partir de 01/01/2026) |
| Aleatório | 11 bits, para que dois casos iguais gerem códigos diferentes |

**Código completo** (versão 2, 16 caracteres, `OF-XXXX-XXXX-XXXX-XXXX`), com todos os campos da triagem e mais estes, em até 75 bits:

| Campo | Valores |
| --- | --- |
| Preenchimento | Dias entre a triagem e o preenchimento (0 a 63) |
| Valores | 6 itens, de 0 (pouco importante) a 4 (muito importante) |
| Inclinação | Congelar, não congelar agora, ainda não sei ou sem resposta |
| SURE | 4 perguntas: sim, não ou sem resposta |
| Perguntas para a consulta | Um bit por pergunta (6 perguntas) |

**É codificação, não criptografia.** Os códigos não contêm nome, documento nem contato, mas quem tem o link consegue ler as respostas. O app informa isso ao paciente antes do envio. A tela do link é somente leitura, mas nada impede que alguém monte outro código à mão. Garantir que um código não foi forjado exigiria assinatura com chave secreta e, portanto, um servidor. Mudar a ordem ou a quantidade de opções exige uma nova versão de código.

O QR code é gerado no próprio navegador pela biblioteca qrcode-generator 1.4.4 (Kazuhiko Arase, licença MIT), incluída no `index.html`. Não há dependência de CDN para isso.

## Estrutura

| Caminho | Conteúdo |
| --- | --- |
| `index.html` | O app inteiro, num único arquivo estático |
| `docs/plano-12-semanas.md` | Plano de desenvolvimento, base de evidências e checklist IPDAS 5.0 |
| `docs/protocolo-cep-ufc.md` | Projeto de pesquisa do teste alfa, no modelo do CEP-UFC, com apêndices e TCLEs |

## Como usar

Abra `index.html` no navegador. Para publicar, qualquer hospedagem estática serve (GitHub Pages, Netlify). As fontes vêm do Google Fonts. Sem internet, o app usa as fontes do sistema e continua funcionando.

## Antes de publicar

Edite o bloco `CONFIG` no início do script do `index.html`:

- `whatsapp`: número real do serviço (DDI + DDD + número). O valor atual é um exemplo.
- `appUrl`: endereço público do app, usado nos links e nos QR codes.
- `financiamento`: declaração de quem financia o desenvolvimento e a hospedagem.
- `rascunho`: mude para `false` só depois da validação.

Os custos de coleta e de armazenamento ainda aparecem como "a definir" no passo 2 do guia.

## Autoria

Dr. Bruno Hállan, urologista e andrologista, Fortaleza, CE (CRM-CE 16919, RQE 13791).
