# Decidir Antes

Ferramenta web de apoio à decisão (PDA, *patient decision aid*) sobre preservação da fertilidade em homens com câncer, desenvolvida segundo os padrões internacionais IPDAS 5.0.

> **Rascunho para validação.** O conteúdo ainda não foi revisado pelo comitê nem testado com pacientes. Não usar no atendimento.

## O que faz

- **Modo profissional:** triagem de 4 perguntas (puberdade, tratamento planejado, prazo e interesse do paciente). Indica a categoria de risco para a fertilidade e gera um código de encaminhamento aleatório, um QR code e um texto pronto para o prontuário.
- **Modo paciente:** guia em 6 passos. Traz as duas opções lado a lado, as probabilidades em pictogramas de 100 homens, um exercício de valores, as perguntas do teste SURE, perguntas para levar à consulta e o contato com o serviço por WhatsApp, enviando só o código.

O app não pede nome, não salva respostas e não envia dados a nenhum servidor. O código é aleatório e só é enviado se o próprio paciente decidir mandar a mensagem.

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
