# Sistema GAD - Pesquisa e Desenvolvimento

![IFMS](https://img.shields.io/badge/IFMS-Instituto%20Federal%20de%20MS-blue)
![Status](https://img.shields.io/badge/Status-Em%20Desenvolvimento-orange)
![Tecnologia](https://img.shields.io/badge/Tecnologia-IA%20%7C%20NLP%20%7C%20OCR-green)

## 📋 Sobre o Projeto

O **Sistema GAD (Gerenciamento de Atividades Diversificadas)** é um projeto de pesquisa e desenvolvimento voltado para a criação de uma solução inovadora de gerenciamento automatizado de certificados de atividades complementares no Instituto Federal de Mato Grosso do Sul (IFMS) - Campus Naviraí.

Este repositório contém o material de pesquisa, análises técnicas e documentação que fundamentam o desenvolvimento de um sistema baseado em Inteligência Artificial para classificação automática de certificados PDF através de técnicas de Machine Learning e Processamento de Linguagem Natural.

## 🎯 Objetivos

### Objetivo Principal

Automatizar o processo de leitura, classificação e validação de certificados de atividades complementares, otimizando um processo atualmente manual e propenso a inconsistências.

### Objetivos Específicos

- **Extração automatizada** de texto de documentos PDF utilizando tecnologias OCR
- **Classificação inteligente** de atividades conforme regulamentos institucionais do IFMS
- **Validação automática** de carga horária e adequação às normas acadêmicas
- **Interface intuitiva** para professores e alunos
- **Redução significativa** do tempo de processamento manual

## 🏗️ Arquitetura Proposta

### Componentes Principais

#### 1. **Document Extractor**

- Extração e limpeza de texto de arquivos PDF
- Remoção de ruídos e formatações desnecessárias com **regex**
- Bibliotecas: Apache PDFBox e Tess4J

#### 2. **Módulo de PLN (Processamento de Linguagem Natural)**

- Tokenização e lematização de texto em português
- Reconhecimento de entidades nomeadas
- Classificação automática baseada em Apache OpenNLP

#### 3. **Banco de Dados Vetorial**

- PostgreSQL com extensão pgVector
- Armazenamento de embeddings para busca por similaridade
- Dados relacionais tradicionais + capacidades vetoriais

#### 4. **Sistema de Classificação IA**

- Modelos treinados localmente
- Fine-tuning
- Classificação conforme regulamentos do IFMS

## 🔬 Base de Pesquisa

### Regulamentação Institucional

- **Resolução 063/2018**: Projeto Pedagógico do Curso Superior de Tecnologia em Análise e Desenvolvimento de Sistemas
- **Regulamento Didático-Pedagógico**: Normas para atividades complementares e validação acadêmica

### Tecnologias Investigadas

#### **Inteligência Artificial e NLP**

- Fine-tuning de modelos
- Processamento de texto em português brasileiro
- Apache OpenNLP para análise linguística

#### **OCR e Extração de Documentos**

- Biblioteca PDFBox para extração de PDFs
- Técnicas de pré-processamento de documentos
- Tratamento de layouts variados de certificados

#### **Arquitetura de Sistemas**

- Spring Framework (Java)
- PostgreSQL com pgVector
- Containerização Docker
- Arquitetura MVC em modulos

## 🛠️ Stack Tecnológica Proposta

### Backend

- **Java** com Spring Framework
- **PostgreSQL** + extensão pgVector
- **Apache OpenNLP** para PLN
- **Apache PDFBox** para manipulação de PDF

### Inteligência Artificial

- **Embeddings vetoriais** para similaridade semântica
- **Machine Learning** para classificação de documentos com NLP

### Infraestrutura

- **Docker** para containerização
- **APIs RESTful** para integração
- **Banco de dados híbrido** (relacional + vetorial)

## 📖 Documentação de Pesquisa

### Análises Técnicas Realizadas

1. **Análise da Biblioteca Docling** - Avaliação de ferramentas de extração de documentos
2. **Sistema GAD Arquitetura** - Proposta arquitetural completa

### Regulamentos Estudados

- Normas de atividades complementares do IFMS
- Classificação de atividades acadêmicas
- Limites de carga horária por categoria
- Processos de validação institucional

## 🎓 Contexto Acadêmico

### Instituição

**Instituto Federal de Educação, Ciência e Tecnologia de Mato Grosso do Sul (IFMS)**

- Campus: Naviraí
- Curso: Superior de Tecnologia em Análise e Desenvolvimento de Sistemas

### Problema Identificado

O processo manual de validação de certificados apresenta:

- **Demora excessiva** no processamento
- **Inconsistências** na interpretação de regulamentos
- **Sobrecarga** da equipe docente e administrativa
- **Dificuldades** no acompanhamento do progresso discente

## 🚀 Próximos Passos

### Fase 1: Desenvolvimento do MVP

- [ ] Implementação do módulo de extração de PDF
- [ ] Desenvolvimento do sistema de classificação básico
- [ ] Criação da interface de usuário inicial
- [ ] Testes com conjunto limitado de certificados

### Fase 2: Refinamento e IA

- [ ] Treinamento de modelos específicos para o domínio
- [ ] Otimização da precisão de classificação

### Fase 3: Produção

- [ ] Testes extensivos com dados reais
- [ ] Treinamento de usuários
- [ ] Deploy em ambiente de produção
- [ ] Monitoramento e ajustes contínuos

## 📊 Impacto Esperado

### Benefícios Quantitativos

- **Redução de 80%** no tempo de processamento
- **Diminuição de 90%** nos erros de classificação
- **Aumento de 70%** na satisfação dos usuários

### Benefícios Qualitativos

- Maior transparência no processo de validação
- Padronização na interpretação de regulamentos
- Facilidade de acompanhamento para estudantes
- Liberação da equipe para atividades estratégicas

## 🤝 Contribuição

Este projeto está em fase de pesquisa e desenvolvimento. Contribuições são bem-vindas nas seguintes áreas:

- **Análise de regulamentos** educacionais
- **Desenvolvimento de algoritmos** de classificação
- **Otimização de modelos** de IA
- **Melhorias na arquitetura** proposta
- **Testes e validação** de funcionalidades
