# Scaffold para Gestão de Documentação do Projeto GAD

Este diretório contém a estrutura de organização para documentos de pesquisa relacionados ao projeto GAD.

## Estrutura de Diretórios

```
Fontes_Pesquisa/
├── docs_ifms/                              # Documentos específicos do IFMS (PDFs e Markdowns)
├── Artigos/                                # Pasta que contém todos os artigos gerais
│   └── [Nome_Normalizado_do_Artigo]/       # Uma pasta para cada artigo (sem acentos/espaços)
│       └── [Nome_Normalizado_do_Artigo].pdf # Versão PDF do artigo
├── Conversas_IAs/                          # Conversas com IAs relacionadas ao projeto
└── Diversos/                               # Outros arquivos diversos
```

## Scripts de Reorganização

Foram utilizados scripts para reorganizar a estrutura de diretórios e normalizar os nomes dos arquivos:

1. `normalizar_nomes.sh` - Remove acentos, caracteres especiais e substitui espaços por underscores
2. `finalizar_reorganizacao.sh` - Reorganiza os diretórios na estrutura final

## Fluxo de Trabalho para Novos Documentos

### Adicionar Novo Artigo/Documento

1. Para documentos do IFMS:
   - Adicione o arquivo PDF diretamente em `/Fontes_Pesquisa/docs_ifms/`
   - Não é necessário normalizar seus nomes

2. Para artigos gerais:
   - Crie uma nova pasta dentro de `/Fontes_Pesquisa/Artigos/` com o nome normalizado (sem acentos, espaços substituídos por underscores)
   - Adicione o arquivo PDF com o mesmo nome normalizado que a pasta

### Conversão para Markdown (usando Python com Docling)

Após adicionar novos PDFs, você pode usar o script Python com Docling para converter os documentos para Markdown automaticamente.

Exemplo de fluxo de trabalho:
```python
# Exemplo (você implementará com o Docling)
import docling  # Sua implementação

# Identificar PDFs sem versão Markdown
pdfs_pendentes = buscar_pdfs_sem_markdown()

# Converter cada PDF para Markdown
for pdf_path in pdfs_pendentes:
    md_path = pdf_path.replace('.pdf', '.md')
    docling.converter_pdf_para_markdown(pdf_path, md_path)
```
