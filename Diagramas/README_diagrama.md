# Diagrama de Caso de Uso - Sistema GAD

Este diagrama de caso de uso representa as principais interações e funcionalidades do Sistema GAD (Gerenciamento Automático de Documentos/Certificados).

## Visualização do Diagrama

![Diagrama de Caso de Uso - Sistema GAD](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/Vitor-rs/Gad_Pesquisa/main/Diagramas/diagrama_caso_uso.puml)

## Descrição do Sistema

O Sistema GAD é uma solução projetada para automatizar e otimizar o processo de gerenciamento de certificados de atividades complementares para estudantes do Instituto Federal de Mato Grosso do Sul (IFMS), Campus Naviraí. O sistema utiliza tecnologias de Inteligência Artificial, especificamente OCR (Reconhecimento Óptico de Caracteres) e NLP (Processamento de Linguagem Natural), para extrair, classificar e validar informações de certificados digitalizados.

## Atores

- **Estudante**: Usuário que submete certificados e acompanha seu progresso nas atividades complementares.
- **Avaliador**: Qualquer funcionário do IFMS responsável por validar certificados, ajustar classificações, configurar regras do sistema e gerenciar processos administrativos relacionados às atividades complementares.

## Principais Pacotes de Casos de Uso

### Gerenciamento de Usuários
- Autenticação e gerenciamento de perfis no sistema

### Gerenciamento de Certificados
- Submissão, extração de texto, classificação automática e edição de certificados

### Validação de Atividades
- Aprovação, rejeição e ajustes nos certificados submetidos

### Gestão de Regras
- Configuração de categorias, limites de horas e parametrização por curso

### Relatórios e Monitoramento
- Acompanhamento do progresso e geração de relatórios

### Inteligência Artificial
- Treinamento e aprimoramento dos modelos de classificação

## Visualização Manual do Diagrama

Se a imagem incorporada acima não estiver visível, você pode visualizar o diagrama manualmente:

1. Instale uma extensão PlantUML para seu editor (como VS Code)
2. Abra o arquivo `diagrama_caso_uso.puml`
3. Use a função de visualização da extensão

Alternativamente, você pode:
1. Copiar o conteúdo do arquivo `diagrama_caso_uso.puml`
2. Colar no [PlantUML Online Server](http://www.plantuml.com/plantuml/uml/)
3. Visualizar ou baixar a imagem gerada

## Notas sobre o Diagrama

- As linhas sólidas com setas representam associações entre atores e casos de uso.
- As linhas tracejadas com setas representam relacionamentos de inclusão (`<<include>>`) ou extensão (`<<extend>>`) entre casos de uso.
- Os casos de uso estão organizados em pacotes para facilitar a compreensão.
