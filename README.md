# rag-from-scratch

RAG (Retrieval-Augmented Generation) em Python puro para tirar dúvidas sobre material de aula em PDF, com citação da fonte e teste de qualidade.

> **Status:** em desenvolvimento.

## O que é

Um programa de terminal que responde perguntas usando o conteúdo dos seus PDFs. Em vez de deixar o modelo "chutar", ele busca os trechos mais relevantes do material e responde com base só neles, indicando de onde tirou a informação.

O projeto é feito **sem frameworks de RAG** (sem LangChain ou similares). A ideia é implementar e entender cada etapa do pipeline.

## Como funciona

1. **Ingestão:** lê cada PDF página por página e corta o texto em pedaços pequenos, guardando o arquivo e a página de origem.
2. **Embeddings:** cada pedaço vira um vetor numérico que representa o significado do texto (API do Gemini). Os vetores são salvos em um arquivo local.
3. **Busca:** a pergunta também vira um vetor, e a similaridade (cosseno, com numpy) encontra os pedaços mais próximos.
4. **Resposta:** os melhores trechos entram em um prompt que pede ao modelo para responder usando apenas eles.
5. **Fonte:** a resposta mostra o arquivo e a página usados, por exemplo: `Fonte: aula03.pdf, p. 12`.

## Teste de qualidade

O `teste.py` roda um conjunto de perguntas com resposta conhecida (`perguntas_teste.json`) e calcula a taxa de acerto.

| Configuração | Acertos |
|---|---|
| Versão inicial | _/10 |
| Após ajustar tamanho dos pedaços e número de trechos | _/10 |

_(preencher com os resultados reais)_

## Estrutura

```
rag-from-scratch/
├── ingestao.py            # lê PDFs, gera embeddings e salva
├── perguntar.py           # pergunta no terminal e resposta com fonte
├── teste.py               # avaliação de qualidade
├── perguntas_teste.json   # perguntas e respostas esperadas
└── README.md
```

## Como rodar

```bash
# 1. Criar e ativar o ambiente virtual
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 2. Instalar dependências
pip install pypdf numpy google-genai

# 3. Configurar a chave da API do Gemini (nunca coloque no código)
export GEMINI_API_KEY="sua-chave"   # Windows: setx GEMINI_API_KEY "sua-chave"

# 4. Indexar os PDFs (coloque-os na pasta pdfs/)
python ingestao.py

# 5. Fazer perguntas
python perguntar.py

# 6. Rodar o teste de qualidade
python teste.py
```

## Tecnologias

Python, pypdf, numpy, API do Gemini (embeddings e geração de respostas).

## Próximos passos

- Testar outros tamanhos de pedaço e quantidade de trechos buscados.
- Suporte a vários PDFs com filtro por arquivo.
- Histórico de conversa.

## Autor

Jhonatan Lopes Diniz, estudante de Ciência da Computação (Mackenzie).
