# Decidir Antes

Ferramenta web de apoio à decisão (PDA, *patient decision aid*) sobre preservação da fertilidade em homens com câncer, desenvolvida segundo os padrões internacionais IPDAS 5.0.

> **Rascunho para validação.** O conteúdo ainda não foi revisado pelo comitê nem testado com pacientes. Não usar no atendimento.

## O que faz

- **Modo profissional:** triagem de 4 perguntas (puberdade, tratamento planejado, prazo e interesse do paciente). Indica a categoria de risco para a fertilidade e gera um código de encaminhamento com as respostas, um QR code e um texto pronto para o prontuário.
- **Modo paciente:** guia em 6 passos. Traz as duas opções lado a lado, as probabilidades em pictogramas de 100 homens, um exercício de valores, as perguntas do teste SURE, perguntas para levar à consulta e o contato com o serviço por WhatsApp, enviando só o código.
- **Modo serviço:** o serviço de andrologia cola o código recebido e lê as respostas da triagem.

## Código de encaminhamento sem banco de dados

O app é *stateless*: não há servidor nem banco de dados. As respostas da triagem viajam dentro do próprio código, por exemplo `OF-40A-AK2-A3V`, e do link e do QR code (`#pda-OF-40A-AK2-A3V`). Qualquer cópia do app consegue decodificá-lo.

São 40 bits em 8 caracteres base32 de Crockford, mais 1 caractere de conferência:

| Campo | Bits | Conteúdo |
| --- | --- | --- |
| Versão | 3 | Formato do código (atual: 1) |
| Puberdade | 2 | Sim, não ou não sabe |
| Tratamentos | 9 | Um bit por opção da triagem |
| Início | 3 | Até 3 dias, 4 a 7, 8 a 14, mais de 14, já começou, não informado |
| Interesse | 2 | Quer conversar, indeciso, não agora ou código gerado pelo paciente |
| Data | 10 | Dia da triagem (ciclo de 1024 dias a partir de 01/01/2026) |
| Aleatório | 11 | Para que dois casos iguais gerem códigos diferentes |

O caractere de conferência é uma soma ponderada módulo 31. Ele detecta erros de digitação de um caractere e trocas entre vizinhos. Na leitura, letras minúsculas, hífens, `O` (lido como 0) e `I`/`L` (lidos como 1) são aceitos.

**É codificação, não criptografia.** O código não contém nome, documento nem contato, mas quem o tiver consegue ler as respostas da triagem. O app informa isso ao paciente antes do envio. Mudar a ordem das opções de tratamento ou de início exige incrementar `VERSAO_COD`.

O app não pede nome, não salva respostas e não envia dados a nenhum servidor. O código só chega ao serviço se o próprio paciente decidir mandar a mensagem.

## Estrutura

| Caminho | Conteúdo |
| --- | --- |
| `index.html` | O app inteiro, num único arquivo estático |
| `docs/plano-12-semanas.md` | Plano de desenvolvimento, base de evidências e checklist IPDAS 5.0 |
| `docs/protocolo-cep-ufc.md` | Projeto de pesquisa do teste alfa, no modelo do CEP-UFC, com apêndices e TCLEs |

## Como usar

Abra `index.html` no navegador. Para publicar, qualquer hospedagem estática serve (GitHub Pages, Netlify). Os links e o QR code abrem o guia com o código no fragmento da URL (`#pda-OF-XXX-XXX`), que o navegador não envia ao servidor.

## Antes de publicar

Edite o bloco `CONFIG` no início do script em `index.html`:

- `whatsapp`: número real do serviço (DDI + DDD + número). O valor atual é um exemplo.
- `appUrl`: endereço público do app.
- `financiamento`: declaração de quem financia o desenvolvimento e a hospedagem.
- `rascunho`: mude para `false` só depois da validação.

Os custos de coleta e de armazenamento ainda aparecem como "a definir" no passo 2 do guia.

## Autoria

Dr. Bruno Hállan, urologista e andrologista, Fortaleza, CE (CRM-CE 16919, RQE 13791).
