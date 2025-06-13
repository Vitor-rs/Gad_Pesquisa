# Sistema GAD - Gerenciamento de Atividades Diversificadas

## Sobre o Projeto

O Sistema GAD é uma solução inovadora para automatizar o gerenciamento de certificados de atividades complementares para estudantes do Instituto Federal de Mato Grosso do Sul (IFMS), Campus Naviraí. Utilizando tecnologias de Inteligência Artificial, como OCR (Reconhecimento Óptico de Caracteres) e NLP (Processamento de Linguagem Natural), o sistema extrai, classifica e valida informações de certificados digitalizados.

## Diagrama de Casos de Uso

O diagrama abaixo representa as principais funcionalidades e interações do sistema:

![Diagrama de Caso de Uso - Sistema GAD](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/Vitor-rs/Gad_Pesquisa/main/Diagramas/diagrama_caso_uso.puml)

## Principais Funcionalidades

### Para Estudantes

- Upload de certificados para validação
- Acompanhamento do progresso nas atividades complementares
- Visualização de certificados já enviados e seu status

### Para Avaliadores (Servidores do IFMS)

- Validação de certificados enviados pelos estudantes
- Ajuste de classificação e horas de atividades
- Geração de relatórios de atividades
- Configuração de categorias de atividades
- Definição de limites de carga horária por categoria
- Parametrização de regras específicas por curso
- Monitoramento do desempenho do sistema
- Gerenciamento dos modelos de IA

## Tecnologias Utilizadas

- **OCR (Reconhecimento Óptico de Caracteres)**: Para extrair texto de certificados digitalizados
- **NLP (Processamento de Linguagem Natural)**: Para classificar automaticamente as atividades
- **Inteligência Artificial**: Para aprimorar continuamente a precisão da classificação

## Estrutura do Projeto

```bash
Gad_Pesquisa/
├── Diagramas/             # Diagramas do projeto (UML, etc.)
├── Fontes_Pesquisa/       # Materiais de pesquisa e referência
│   ├── Artigos/           # Artigos acadêmicos relacionados
│   │   └── [Nome_do_Artigo]/  # Cada artigo em sua própria pasta
│   │       ├── [Nome_do_Artigo].pdf  # Versão PDF do artigo
│   │       └── [Nome_do_Artigo].md   # Versão Markdown do artigo
│   ├── Conversas_IAs/     # Registros de conversas com IAs sobre o projeto
│   ├── Diversos/          # Outros materiais de pesquisa
│   └── docs_ifms/         # Documentos específicos do IFMS
└── README.md              # Este arquivo
```

## Organização de Documentos de Pesquisa

### Fluxo de Trabalho para Novos Documentos

1. **Para documentos do IFMS**:
   - Adicione o arquivo PDF diretamente em `/Fontes_Pesquisa/docs_ifms/`

2. **Para artigos gerais**:
   - Crie uma pasta em `/Fontes_Pesquisa/Artigos/` com nome normalizado (sem acentos, espaços substituídos por underscores)
   - Adicione o arquivo PDF com o mesmo nome normalizado que a pasta
   - Se disponível, adicione também uma versão Markdown do documento

3. **Para conversas com IAs**:
   - Salve registros de conversas com IAs em `/Fontes_Pesquisa/Conversas_IAs/`

## Como Contribuir

Para contribuir com este projeto:

1. Faça um fork do repositório
2. Crie uma branch para sua feature (`git checkout -b feature/nome-da-feature`)
3. Faça commit de suas mudanças (`git commit -m 'Adiciona nova feature'`)
4. Faça push para a branch (`git push origin feature/nome-da-feature`)
5. Abra um Pull Request

## Documentação Adicional

Para mais detalhes sobre o projeto, consulte:

- [Documento detalhado sobre o diagrama de caso de uso](./Diagramas/README_diagrama.md)
- [Fontes de pesquisa e artigos de referência](./Fontes_Pesquisa)
