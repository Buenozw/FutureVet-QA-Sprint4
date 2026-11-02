# FutureVet — Compliance, Quality Assurance & Tests (4ª Sprint)

Challenge FIAP 2026 · parceria **Clyvo Vet**

| Integrante | RM |
|---|---|
| João Victor Caetano Alves da Silva | RM562074 |
| João Victor Bueno Castelini da Silva | RM564115 |
| Ryan Vetoriano | RM565667 |
| Felipe Furlanetto | RM562766 |
| Raul Rezende Iemini Aguiar | RM564002 |

## Sistema testado

- Aplicação web e API REST: https://challenge-sprint3-java.onrender.com
- Código-fonte: https://github.com/joaocaetano1310/Challenge_Sprint4_Java

## Parte A — Teste manual de software

Plano de testes manuais de validação no nível de sistema, com 15 casos de teste. Cada caso traz objetivo, dados de entrada, dados de saída esperados e o procedimento passo a passo, com rastreabilidade para as funcionalidades das Sprints 3 e 4.

- 📄 [Plano_de_Testes_FutureVet.pdf](Plano_de_Testes_FutureVet.pdf)
- 📝 [PLANO_DE_TESTES.md](PLANO_DE_TESTES.md) (mesmo conteúdo, para leitura direto no GitHub)

## Parte B — Automação de teste (Postman)

Testes automatizados de integração e desempenho da API REST: 19 requisições e 61 verificações, cobrindo autenticação JWT, animais, carteira de vacinação, agenda, regras de negócio, isolamento entre tutores e tempo de resposta.

- 🎬 **Vídeo (configuração, execução e resultados):** COLE_AQUI_O_LINK_DO_YOUTUBE
- Collection: [`postman/FutureVet.postman_collection.json`](postman/FutureVet.postman_collection.json)
- Ambiente: [`postman/FutureVet-Render.postman_environment.json`](postman/FutureVet-Render.postman_environment.json)

### Como executar

1. No Postman, importe os dois arquivos da pasta `postman/`.
2. Selecione o ambiente **FutureVet - Render**.
3. Abra `/actuator/health` no navegador para acordar o servidor do Render.
4. Rode a collection pelo **Collection Runner**, na ordem.

Pela linha de comando (opcional):

```bash
npm install -g newman
newman run postman/FutureVet.postman_collection.json -e postman/FutureVet-Render.postman_environment.json
```
