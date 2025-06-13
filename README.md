# Sistema GAD - Gerenciamento de Atividades Diversificadas

## 📋 Descrição do Projeto

O Sistema GAD (Gerenciamento de Atividades Diversificadas) é uma solução inovadora desenvolvida para automatizar e otimizar o processo de gerenciamento de certificados de atividades complementares para estudantes do Instituto Federal de Mato Grosso do Sul (IFMS), Campus Naviraí.

O sistema utiliza tecnologias de Inteligência Artificial, especificamente OCR (Reconhecimento Óptico de Caracteres) e NLP (Processamento de Linguagem Natural), para extrair, classificar e validar informações de certificados digitalizados de forma automática e eficiente.

## 🎯 Objetivos

- **Automatizar** o processo de análise de certificados de atividades complementares
- **Otimizar** o tempo de validação e classificação de documentos
- **Melhorar** a precisão na extração de informações relevantes
- **Facilitar** o acompanhamento do progresso acadêmico dos estudantes
- **Reduzir** a carga de trabalho manual dos avaliadores

## 👥 Usuários do Sistema

### Estudante

- Submete certificados digitalizados
- Acompanha o progresso nas atividades complementares
- Visualiza o status de validação dos documentos

### Avaliador

- Valida certificados processados pelo sistema
- Ajusta classificações quando necessário
- Configura regras do sistema
- Gerencia processos administrativos

## 🔧 Funcionalidades Principais

### Gerenciamento de Certificados

- ✅ Submissão de documentos digitalizados
- ✅ Extração automática de texto via OCR
- ✅ Classificação automática por categoria
- ✅ Edição e correção de informações extraídas

### Validação de Atividades

- ✅ Aprovação/rejeição de certificados
- ✅ Ajustes manuais pelos avaliadores
- ✅ Histórico de alterações

### Gestão de Regras

- ✅ Configuração de categorias de atividades
- ✅ Definição de limites de horas por categoria
- ✅ Parametrização específica por curso

### Gerenciamento de Usuários

- ✅ Autenticação segura
- ✅ Perfis diferenciados (estudante/avaliador)
- ✅ Controle de acesso

## 🏗️ Arquitetura Tecnológica

### Tecnologias de IA Utilizadas

- **OCR (Optical Character Recognition)**: Para extração de texto de imagens
- **NLP (Natural Language Processing)**: Para classificação e análise semântica
- **Modelos de Linguagem**: Para processamento de texto em português

### Stack Tecnológico Considerado

- **Backend**: Java com Spring Framework
- **AI/ML**: Modelos de NLP em português (BERT, DeBERTa)
- **OCR**: Tesseract OCR
- **Banco de Dados**: Estruturado para armazenamento de documentos e metadados

## 📁 Estrutura do Repositório

```text
├── Diagramas/                    # Diagramas do sistema
│   ├── diagrama_caso_uso.puml   # Diagrama de casos de uso
│   └── README_diagrama.md       # Documentação dos diagramas
├── Fontes_Pesquisa/             # Material de pesquisa e referências
│   ├── Artigos/                 # Artigos acadêmicos relevantes
│   ├── Conversas_IAs/           # Conversas com IAs para desenvolvimento
│   ├── Diversos/                # Documentos diversos
│   └── docs_ifms/               # Documentação oficial do IFMS
└── README.md                    # Este arquivo
```

## 📚 Base de Conhecimento

O projeto conta com uma extensa base de pesquisa incluindo:

### Artigos Acadêmicos

- Reconhecimento de Entidades Nomeadas para o Português
- Fine-tuning de Large Language Models
- Processamento de Linguagem Natural para Code-mixing
- Questões abertas em pesquisa de NLP

### Documentação Técnica

- Regulamentos do IFMS
- Projetos pedagógicos dos cursos
- Especificações técnicas

### Conversas e Análises

- Análises de bibliotecas de extração de documentos
- Discussões sobre arquitetura do sistema
- Estratégias de implementação

## 🚀 Status do Projeto

Este repositório contém principalmente:

- ✅ **Pesquisa e Análise**: Documentação completa da fase de pesquisa
- ✅ **Arquitetura**: Definição da estrutura do sistema
- ✅ **Casos de Uso**: Mapeamento completo das funcionalidades
- 🔄 **Implementação**: Em fase de planejamento

## 📋 Próximos Passos

1. **Estruturação de Dados**: Implementar o esquema de dados unificado
2. **Desenvolvimento do MVP**: Criar versão mínima viável
3. **Integração de IA**: Implementar modelos de OCR e NLP
4. **Testes e Validação**: Validar com documentos reais do IFMS
5. **Deploy e Treinamento**: Implementar e treinar usuários

## 🎓 Contexto Acadêmico

Este projeto está sendo desenvolvido como parte dos estudos no Curso Superior de Tecnologia em Análise e Desenvolvimento de Sistemas do IFMS - Campus Naviraí, visando aplicar conhecimentos de:

- Desenvolvimento de Software
- Inteligência Artificial
- Processamento de Linguagem Natural
- Análise de Sistemas
- Engenharia de Software
