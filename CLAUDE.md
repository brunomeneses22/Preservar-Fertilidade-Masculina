# Instruções do projeto Decidir Antes

App web estático de apoio à decisão sobre preservação da fertilidade masculina, com um único arquivo, `index.html`. Não há servidor nem banco de dados. As respostas viajam codificadas no link e no QR code. Leia o `README.md` para o fluxo completo.

## Regra permanente: versões do questionário

Todo código gerado registra a **versão do questionário** com que foi respondido. Códigos já entregues a pacientes e enviados ao serviço precisam continuar legíveis para sempre, com os textos exatos daquela versão. Por isso:

1. **Nunca edite, reordene ou remova** perguntas, opções ou ids de uma versão que já existe em `QUESTIONARIOS` no `index.html`. Isso vale também para correções de texto, porque o texto mostrado ao serviço precisa ser o mesmo que o paciente viu.
2. **Para mudar qualquer pergunta ou opção:**
   - copie a versão atual inteira para uma nova chave (`N + 1`);
   - atualize o campo `data` da cópia;
   - faça as mudanças só na cópia;
   - aponte `Q_ATUAL` para a nova versão.
3. **Nunca apague versões antigas** de `QUESTIONARIOS`, mesmo que ninguém mais gere códigos com elas.
4. **Registre a mudança** na tabela "Histórico de versões do questionário" deste arquivo.

### O que é versionado

Exige nova versão qualquer mudança em `pub`, `trat` (texto, `nivel` ou ordem), `urg`, `int`, `valores`, `niveis`, `lean`, `sure`, `yn` e `perguntas`. A ordem das listas e os `id` definem a codificação.

Não exige nova versão o que não está codificado: textos explicativos do guia, números de evidência, referências, layout, `CONFIG` e textos de botões.

## Estrutura fixa dos códigos (não alterar)

- A versão do questionário ocupa sempre os **6 bits menos significativos** do número codificado (base 64, versões de 1 a 63). O decodificador lê a versão primeiro e só depois usa as listas dela.
- O alfabeto é o base32 de Crockford (`0123456789ABCDEFGHJKMNPQRSTVWXYZ`), com prefixo `OF-`.
- Os tamanhos são fixos: 9 caracteres para o código da triagem e 16 para o código completo. O último caractere é a conferência: soma ponderada dos valores, com pesos de 1 a n, módulo 31.
- A data é o dia desde 01/01/2026, em ciclo de 1024 dias.
- Os campos são derivados das listas da versão por `basesTriagem(q)` e `basesPaciente(q)`. Uma versão nova pode ter mais ou menos itens, desde que caiba nos tamanhos fixos. O app confere isso ao carregar e mostra um erro no console se não couber.
- Se algum dia for preciso mudar essa estrutura, use outro prefixo, por exemplo `OG-`, e mantenha o decodificador de `OF-` funcionando.

## Antes de cada commit que mexa no código

1. Abra o `index.html` no navegador e rode `testarCodigos()` no console. O resultado deve terminar com `falhas: 0`.
2. Gere uma triagem, abra o link do paciente, gere o resumo e abra o link `#ver-`. Confira se a versão do questionário aparece na tela.
3. Se criou uma versão nova, decodifique um código da versão anterior (aba Serviço) e confira se aparecem os textos antigos.

## Histórico de versões do questionário

| Versão | Data | Mudança |
| --- | --- | --- |
| 1 | 02/11/2026 | Versão inicial: triagem com 9 tratamentos e guia com 6 valores, 4 perguntas SURE e 6 perguntas para a consulta |
