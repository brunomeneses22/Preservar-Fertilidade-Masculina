# Plano de 12 semanas — PDA web de preservação da fertilidade masculina

Oct 28, 2026 · @Bruno

## Visão geral

A meta é lançar até 22 de janeiro de 2027 a versão 1.0 de um app web que informa homens com câncer sobre preservação da fertilidade antes do tratamento e os encaminha, sem identificação, ao serviço de andrologia. O plano começa em 2 de novembro de 2026 e cumpre os 7 critérios qualificadores e os 7 essenciais do IPDAS 5.0.

- **Decisão apoiada:** congelar sêmen antes do tratamento gonadotóxico ou não congelar agora.
- **Público-alvo:** homens e adolescentes pós-púberes com câncer recém-diagnosticado e tratamento potencialmente gonadotóxico planejado (quimioterapia, radioterapia pélvica ou testicular, transplante de medula, cirurgia que afete a ejaculação). Pré-púberes ficam fora da PDA e são encaminhados direto.
- **Quem usa:** qualquer profissional da linha oncológica (oncologia, hematologia, radio-oncologia, cirurgia, enfermagem, navegação).
- **Dois modos:** triagem rápida pelo profissional (menos de 2 minutos) e PDA para o paciente, na consulta ou no celular por link ou QR code.
- **Encaminhamento anônimo:** o app gera um código aleatório e um link de WhatsApp para o serviço; o paciente decide se envia. Nada é armazenado.
- **Fora da v1.0:** prontuário, agendamento, coleta de dados do paciente, painel do serviço, calculadora de risco personalizada.

## Arquitetura mínima

O app é um único arquivo HTML com JavaScript, sem servidor e sem banco de dados; por isso cabe no prazo e quase não tem custo LGPD.

&#91;embedded content: fluxo do encaminhamento anônimo · 3 papéis, 7 passos\]

- **Hospedagem:** página estática (artifact publicado, ou Netlify ou GitHub Pages com domínio próprio), custo zero ou próximo disso.
- **Código anônimo:** gerado no navegador por gerador criptográfico (ex.: OF-7K3P); não deriva de nenhum dado do paciente.
- **Link do paciente:** abre a PDA com o código no fragmento da URL (depois do #), que o navegador não envia ao servidor.
- **QR code:** gerado localmente na tela, para o paciente fotografar.
- **WhatsApp:** link wa.me com mensagem pronta contendo apenas o código e a urgência; o paciente envia se quiser.
- **Manutenção:** o conteúdo fica num bloco de texto dentro do arquivo; revisar é editar e republicar.

## Conteúdo e base de evidências

O protótipo já traz as estimativas abaixo, todas como "melhor estimativa" e marcadas para validação. Nas semanas 2 a 5, cada desfecho recebe busca documentada, leitura do texto completo e nível de certeza GRADE, num documento técnico anexo à PDA (o rigor que Durand 2019 propõe e o IPDAS 5.0 recomenda).

| Informação mostrada ao paciente | Melhor estimativa atual | Fonte |
| --- | --- | --- |
| Recomendação geral | Discutir fertilidade no diagnóstico e encaminhar interessados ou indecisos | [ASCO 2025](https://ascopubs.org/doi/10.1200/JCO-24-02782) |
| Homens que conseguem congelar | Cerca de 90% (falha em \~10%) | [Li 2024](https://pmc.ncbi.nlm.nih.gov/articles/PMC10882264) |
| Homens que usam o sêmen congelado | 8–9%, provavelmente subestimado pelo seguimento curto | [Ferrari 2016](https://air.unimi.it/handle/2434/453889); [Li 2024](https://pmc.ncbi.nlm.nih.gov/articles/PMC10882264) |
| Paternidade entre quem usou | Cerca de 49% | [Ferrari 2016](https://air.unimi.it/handle/2434/453889) |
| Gestação e parto por ciclo de ICSI | 34% e 23% | [Li 2024](https://pmc.ncbi.nlm.nih.gov/articles/PMC10882264) |
| Recuperação após cisplatina (testículo) | \~50% em 2 anos, \~80% em 5 anos | Howell & Shalet 2005 |
| Azoospermia após esquemas com alquilantes em dose alta | Acima de 90% em esquemas como MVPP | Howell & Shalet 2005 |
| Risco por agente | Alto: ciclofosfamida, ifosfamida, procarbazina, bussulfano, melfalano, clorambucil. Médio: cisplatina, carboplatina, doxorrubicina. Baixo: vincristina, vimblastina, metotrexato, bleomicina | [Tabela de risco por agente](https://pmc.ncbi.nlm.nih.gov/articles/PMC5904848) |
| Extração testicular antes do tratamento (onco-TESE) | Congelamento possível em 43–58% | [Ogouma 2022](https://doaj.org/article/635f39bfeb9a4f7794c656b5405db53e) |
| Micro-TESE anos após quimioterapia | Espermatozoides em 37% (85% após câncer de testículo; 26–36% após alquilantes) | [Hsiao 2011](https://doi.org/10.1200/JCO.2010.33.7808) |

**Faltam dados locais, a definir pelo serviço na semana 2:** custo da coleta e da anuidade de armazenamento, prazo entre o contato e a coleta, acesso pelo SUS e convênios. Custo e prazo pesam muito na decisão e precisam aparecer com o mesmo destaque para as duas opções.

## Checklist IPDAS 5.0

Os 14 critérios obrigatórios já têm lugar definido no app; 4 dependem de trabalho fora do código (evidência, codesign e financiamento).

| Critério | Nível | Onde aparece no app | Status |
| --- | --- | --- | --- |
| Descreve a condição de saúde | Qualificador | Tela "Por que falar disso agora" | No protótipo |
| Explicita a decisão | Qualificador | Título e abertura da PDA | No protótipo |
| Identifica o público-alvo | Qualificador | Bloco "Para quem é este guia" | No protótipo |
| Lista as opções, incluindo esperar | Qualificador | "Congelar agora" e "Não congelar agora" | No protótipo |
| Descreve benefícios | Qualificador | Comparação lado a lado | No protótipo |
| Descreve danos | Qualificador | Comparação lado a lado | No protótipo |
| Clarificação de valores | Qualificador | Exercício "O que pesa para você" + como é viver cada opção | No protótipo |
| Melhor evidência diretamente aplicável | Essencial | Documento técnico de evidências (seção anterior) | Pendente |
| Usuários envolvidos no desenvolvimento | Essencial | Paciente parceiro no comitê + 2 ciclos de teste alfa | Pendente |
| Apresentação balanceada | Essencial | Mesma estrutura, ordem e detalhe nas duas opções | No protótipo |
| Financiamento declarado com destaque | Essencial | Seção "Quem fez este guia" | Pendente |
| Citações completas | Essencial | Seção "Referências" | No protótipo |
| Data de produção | Essencial | Rodapé e seção de transparência | No protótipo |
| Política de atualização | Essencial | Revisão anual ou a cada nova diretriz | No protótipo |

**Potencializadores incluídos na v1.0:** probabilidades em "de 100 homens" com ícones e mesmo denominador; incerteza como "nossa melhor estimativa"; lista de perguntas para levar à consulta; conflito de interesse dos autores declarado; meta de PEMAT acima de 70% e leitura nível 8ª série; GRADE por desfecho; observação de usuários com mudanças documentadas entre ciclos.

## Cronograma

O protocolo vai ao CEP na primeira semana porque a aprovação é o único passo que não depende da equipe.

&#91;embedded content: cronograma · 12 semanas, 7 frentes, 1 lançamento\]

- [ ] Semana 1 (2–6 nov): montar o comitê e submeter o protocolo ao CEP
- [ ] Semana 2: serviço define custos, prazos e o número de WhatsApp; busca documentada por desfecho
- [ ] Semanas 3–4: GRADE por desfecho e revisão dos números do protótipo
- [ ] Semana 5: texto revisado pelo comitê, teste de legibilidade e PEMAT
- [ ] Semanas 6–7: protótipo v0.5 em domínio próprio e revisão técnica pelo comitê
- [ ] Semanas 8–9: recesso e resposta a pendências do CEP
- [ ] Semana 10: teste alfa, ciclo 1, e ajustes
- [ ] Semana 11: teste alfa, ciclo 2
- [ ] Semana 12 (18–22 jan): autoavaliação IPDAS 5.0, documento técnico, v1.0 no ar, treinamento de 15 minutos e QR nos consultórios

## Equipe mínima

Seis papéis bastam; o comitê se reúne a cada duas semanas por 45 minutos.

| Papel | Quem | Dedicação |
| --- | --- | --- |
| Coordenação e conteúdo andrológico | Dr. Bruno Hállan | 4–6 h/semana |
| Oncologista ou hematologista revisor (externo ao serviço de andrologia) | A definir | 2 h/semana nas semanas 3–6 |
| Enfermagem ou navegação oncológica | A definir | Revisão da triagem e implantação |
| Paciente parceiro (sobrevivente que viveu a decisão) | A definir | Reuniões do comitê e revisão de linguagem |
| Laboratório do banco de sêmen | A definir | Custos, prazos e logística da coleta |
| Apoio técnico | Claude + quem cuidar da hospedagem e do domínio | Ajustes no app e publicação |

O paciente parceiro integra a equipe e não é participante de pesquisa; isso permite codesign desde a semana 1, antes da aprovação do CEP.

## Teste alfa com usuários

Dois ciclos curtos nas semanas 10 e 11, com ajustes documentados entre eles, atendem ao critério essencial de envolvimento de usuários.

- **Amostra por ciclo:** 5–8 profissionais da linha oncológica e 5–8 pacientes ou sobreviventes; idealmente parte dos pacientes ainda indecisos.
- **Método:** sessão de 20–30 minutos com observação e pensar em voz alta, seguida de questionário.
- **Instrumentos:** escala de usabilidade SUS (versão em português), questionário de aceitabilidade no modelo Ottawa, 5 perguntas de compreensão, pergunta de equilíbrio ("as informações puxam para alguma opção?"), PEMAT aplicado pela equipe.
- **Critérios de sucesso:** SUS de 68 ou mais; 70% ou mais acham o conteúdo equilibrado; 80% ou mais acertam as perguntas-chave; triagem concluída em menos de 2 minutos.
- **Registro:** o que mudou entre o ciclo 1 e o ciclo 2, com o motivo, em tabela no documento técnico.

## Ética, LGPD, CFM e conflito de interesse

O caminho crítico é o CEP: a submissão precisa sair na semana 1 para o teste alfa caber nas semanas 10–11.

- **CEP:** submeter pela Plataforma Brasil na semana 1 (Resoluções CNS 466/2012 e 510/2016), com o protocolo do teste alfa e os questionários. Como a intenção é publicar o desenvolvimento, trate o teste como pesquisa.
- **LGPD:** o app não coleta, transmite nem armazena dados pessoais. A triagem roda no navegador e desaparece ao fechar; o código é aleatório; quem envia a mensagem é o próprio paciente. Sem cookies nem analytics na v1.0. O app declara isso na tela.
- **Natureza do software:** informativo, sem diagnóstico nem recomendação individual. Se uma versão futura personalizar risco por calculadora, reavaliar o enquadramento na RDC ANVISA 657/2022.
- **Publicidade médica:** revisar textos e identificação do serviço à luz da Resolução CFM 2.336/2023.
- **Conflito de interesse:** o serviço de destino pertence ao autor do conteúdo. O app declara isso com destaque, informa que o paciente pode procurar outro serviço e passa por revisão de um oncologista externo e por pacientes indecisos quanto ao equilíbrio.

## Riscos e mitigação

O maior risco ao prazo é a aprovação ética; o maior risco à qualidade é o viés pró-congelamento.

| Risco | Mitigação |
| --- | --- |
| CEP não aprova até a semana 9 | Submeter na semana 1; usar semanas 8–9 para pendências; plano B: lançar v1.0 com codesign do comitê e fazer o alfa formal em fevereiro |
| Recesso de fim de ano | Semanas 8–9 sem entregas críticas |
| Custos e prazos locais desconhecidos | Serviço e laboratório definem na semana 2 |
| Viés pró-congelamento por conflito de interesse | Revisor oncológico externo, pergunta de equilíbrio no teste alfa, declaração destacada |
| Profissionais não usam o app | Triagem em menos de 2 minutos, QR impresso nos consultórios, treinamento de 15 minutos |
| Impossível acompanhar quem foi encaminhado (anonimato) | Indicador agregado: códigos recebidos no WhatsApp por mês |
| App abandonado após o projeto (só \~25% das PDAs testadas seguem em uso) | Hospedagem estática de baixo custo, responsável nomeado, revisão anual na agenda |

## Referências

1. Volk RJ et al. Updated International Patient Decision Aid Standards (IPDAS version 5.0). BMJ 2026;393:e088116. [doi](https://doi.org/10.1136/bmj-2025-088116)
2. Durand MA et al. Modified Delphi survey for the evidence summarisation of patient decision aids: study protocol. BMJ Open 2019;9:e026701.
3. Elwyn G et al. Developing a quality criteria framework for patient decision aids. BMJ 2006;333:417.
4. Su HI et al. Fertility preservation in people with cancer: ASCO guideline update. J Clin Oncol 2025;43(12):1488–1515. [link](https://ascopubs.org/doi/10.1200/JCO-24-02782)
5. Li Q et al. Fertility preservation in adult male patients with cancer: a systematic review and meta-analysis. Hum Reprod Open 2024;hoae006. [link](https://pmc.ncbi.nlm.nih.gov/articles/PMC10882264)
6. Ferrari S et al. Sperm cryopreservation and reproductive outcome in male cancer patients: a systematic review. Reprod Biomed Online 2016;33(1):29–38. [link](https://air.unimi.it/handle/2434/453889)
7. Howell SJ, Shalet SM. Spermatogenesis after cancer treatment: damage and recovery. J Natl Cancer Inst Monogr 2005;34:12–7.
8. Ogouma L et al. Testicular sperm extraction (TESE) outcomes in the context of malignant disease: a systematic review. Asian J Androl 2022;24(6):584–590. [link](https://doaj.org/article/635f39bfeb9a4f7794c656b5405db53e)
9. Hsiao W et al. Successful treatment of postchemotherapy azoospermia with microsurgical testicular sperm extraction. J Clin Oncol 2011. [doi](https://doi.org/10.1200/JCO.2010.33.7808)
10. Okada K, Fujisawa M. Recovery of spermatogenesis following cancer treatment with cytotoxic chemotherapy and radiotherapy. World J Mens Health 2019;37(2):166–174. [link](https://pmc.ncbi.nlm.nih.gov/articles/PMC6479085)
