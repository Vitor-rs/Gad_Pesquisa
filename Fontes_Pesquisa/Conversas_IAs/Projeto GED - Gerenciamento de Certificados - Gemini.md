# Análise e Pesquisa para o Desenvolvimento de um Sistema de Gerenciamento de Certificados de Atividades Complementares com IA no IFMS Campus Naviraí

## 1. Introdução

### 1.1 Propósito do Relatório

Este relatório tem como objetivo fornecer uma base de pesquisa abrangente para apoiar o desenvolvimento de um sistema inovador, impulsionado por Inteligência Artificial (IA), destinado ao gerenciamento de certificados de atividades complementares para estudantes do Instituto Federal de Mato Grosso do Sul (IFMS), Campus Naviraí. A proposta central é automatizar a leitura, classificação e validação dessas atividades, otimizando um processo atualmente manual e propenso a inconsistências. O documento busca preencher a lacuna entre a concepção do projeto e sua implementação prática, explorando detalhadamente os regulamentos institucionais, as tecnologias habilitadoras (como Reconhecimento Óptico de Caracteres - OCR e Processamento de Linguagem Natural - NLP), as abordagens arquitetônicas e os desafios inerentes à implementação.

### 1.2 Declaração do Problema

O gerenciamento manual de certificados de atividades complementares representa um desafio significativo tanto para os professores quanto para os alunos do IFMS. A validação individual de documentos, a classificação das atividades segundo as normas institucionais e a contabilização das horas demandam tempo considerável e estão sujeitas a erros e interpretações subjetivas. A ausência de um sistema automatizado dificulta o acompanhamento do progresso dos alunos no cumprimento dessa exigência curricular e sobrecarrega a equipe docente e administrativa. A introdução de tecnologias de IA, especificamente OCR para extração de texto de documentos e NLP para análise e classificação semântica, oferece uma oportunidade promissora para automatizar essas tarefas, aumentando a eficiência, a precisão e a transparência do processo.

### 1.3 Estrutura do Relatório

Para fornecer uma análise completa, este relatório está estruturado da seguinte forma:

* **Seção 2:** Analisa em profundidade os regulamentos do IFMS, com foco no Campus Naviraí, que regem as atividades complementares, incluindo classificações, limites de carga horária e regras específicas.
* **Seção 3:** Apresenta um levantamento de sistemas existentes, como Sistemas de Gestão Acadêmica (AMS) e Gerenciamento Eletrônico de Documentos (GED), avaliando suas funcionalidades e limitações no contexto do projeto.
* **Seção 4:** Explora as tecnologias de IA fundamentais – OCR e NLP – detalhando seus princípios, desafios na aplicação a certificados e as principais ferramentas e bibliotecas disponíveis, especialmente no ecossistema Python.
* **Seção 5:** Discute a construção do modelo de classificação de IA, abordando requisitos de dados, técnicas de pré-processamento, seleção de modelos (com ênfase em fine-tuning) e melhores práticas para pipelines de Machine Learning.
* **Seção 6:** Propõe uma arquitetura de sistema e estratégias de integração, considerando a interação entre os módulos de OCR, NLP, motor de regras e interface do usuário, além de abordar técnicas para extração de informação robusta a variações de layout.
* **Seção 7:** Identifica e discute os desafios de implementação mais comuns, como erros de OCR, ambiguidades na classificação, escalabilidade, segurança e a importância da experiência do usuário, sugerindo estratégias de mitigação.
* **Seção 8:** Conclui com recomendações sobre tecnologias, considerações de design, áreas para investigação adicional e uma abordagem de desenvolvimento por fases (MVP).

### 1.4 Relevância da Pesquisa

A pesquisa detalhada apresentada neste relatório é fundamental para o sucesso do projeto proposto. Ela fornece o conhecimento necessário para tomar decisões informadas sobre a escolha de tecnologias (OCR, NLP, bibliotecas), o design da arquitetura do sistema e, crucialmente, a correta implementação das regras de negócio específicas do IFMS Naviraí. Compreender as nuances dos regulamentos, as capacidades e limitações das ferramentas de IA, e os desafios práticos enfrentados por sistemas similares permitirá o desenvolvimento de uma solução mais robusta, precisa e alinhada às necessidades da instituição e de seus alunos.

## 2. Decodificando as Atividades Complementares no IFMS Naviraí

### 2.1 A Importância dos Regulamentos Institucionais

Antes de iniciar o desenvolvimento de qualquer sistema que envolva regras acadêmicas, é imperativo compreender profundamente as normativas da instituição. No caso do projeto em questão, o sistema de gerenciamento de atividades complementares deve refletir com precisão as políticas e procedimentos estabelecidos pelo IFMS, com atenção especial às particularidades do Campus Naviraí. Qualquer desvio ou interpretação incorreta dos regulamentos pode levar a erros na contabilização de horas, prejudicando os alunos e invalidando a utilidade do sistema. Portanto, a análise detalhada dos documentos normativos é o pilar fundamental para o design da lógica de negócio do software.

### 2.2 Análise dos Regulamentos do IFMS

A pesquisa identificou documentos chave que regem as atividades complementares no IFMS:

* **Regulamento da Organização Didático Pedagógica (ROD) do IFMS:** Uma minuta revisada deste regulamento ^1^ oferece insights valiosos sobre a estrutura e propósito das atividades complementares. Elas são definidas como componentes curriculares de enriquecimento, visando implementar o perfil do egresso através de estudos e vivências independentes, transversais e contextualizadas. A participação é obrigatória se prevista no Projeto Pedagógico do Curso (PPC). O aluno é responsável por participar e apresentar a documentação comprobatória, que deve descrever a atividade, carga horária e período. A avaliação considera a compatibilidade e relevância da atividade com os objetivos do curso e as horas dedicadas. O regulamento estabelece que cada ponto equivale a uma hora de atividade e que o Trabalho de Conclusão de Curso (TCC) e estágios obrigatórios não contam como atividades complementares.^1^ Atividades que se encaixam em mais de uma categoria são pontuadas conforme a escolha do aluno. A validação é feita por um professor responsável, indicado pelo Coordenador do Curso.^1^
* **Projetos Pedagógicos de Curso (PPCs) - Campus Naviraí:** Os PPCs especificam os requisitos para cada curso.
  * *Agronomia:* Exige um total de 240 horas de atividades complementares, que podem ser iniciadas desde o primeiro semestre. As atividades aceitas incluem iniciação à pesquisa e extensão, participação em eventos, visitas técnicas, dias de campo, entre outras detalhadas em tabela anexa ao regulamento. Um docente específico é designado para validar as atividades. Atividades não previstas na tabela são analisadas pelo colegiado do curso.^2^
  * *Análise e Desenvolvimento de Sistemas (ADS):* Requer 50 horas de atividades complementares. O foco está em experiências que vão além da sala de aula tradicional, como visitas técnicas, palestras, semanas acadêmicas e desenvolvimento de projetos. O PPC remete ao ROD do IFMS para a lista detalhada de atividades válidas.^3^
  * *Técnico em Informática para Internet:* Embora seja um curso técnico, o PPC menciona que atividades complementares podem ser usadas para complementar a validação do estágio obrigatório, sujeitas à aprovação do Núcleo Docente Estruturante (NDE) ou Colegiado do Curso e a regulamentos específicos. Cita "Projetos de Ensino" como exemplo.^4^
* **Outros Regulamentos (Contexto Comparativo):** Para fins de comparação e entendimento de práticas comuns, foram analisados regulamentos de outros Institutos Federais. O IFMT Campus Sorriso ^5^ detalha categorias e limites de horas fixos por sub-atividade, exige participação em pelo menos quatro categorias distintas e, importantemente, estabelece que horas excedentes aos limites são desconsideradas. O IF Sudeste MG (Campus Bom Sucesso) ^6^ também define categorias (Ensino, Pesquisa, Extensão, Geral) e estabelece limites máximos como percentuais da carga horária total exigida no PPC. Essas variações reforçam a necessidade de aderir estritamente às normas específicas do IFMS Naviraí.

### 2.3 Detalhamento das Classificações de Atividades

O sistema proposto deve classificar as atividades conforme a tabela fornecida pelo usuário:

| **Sigla** | **Descrição**                                         |
| --------------- | ------------------------------------------------------------- |
| UC              | Unidades Curriculares optativas/eletivas                      |
| PE              | Projetos de Ensino, pesquisa e extensão                      |
| PP              | Prática Profissional Integradora                             |
| PD              | Práticas Desportivas                                         |
| PA              | Práticas Artístico Culturais                                |
| PV              | Práticas de vivência acadêmica e profissional complementar |
| TCC             | Trabalho de Conclusão de Curso                               |

É crucial mapear essas siglas às categorias oficiais definidas no Anexo A do ROD do IFMS ^1^:

1. **Atividades de aperfeiçoamento e enriquecimento cultural e esportivo:** (Provavelmente abrange PA e PD)
2. **Atividades de divulgação científica e de iniciação à docência:** (Pode se relacionar com PE em alguns casos)
3. **Atividades de vivência acadêmica e profissional complementar:** (Provavelmente abrange PV e talvez PP)
4. **Atividades de Pesquisa ou Extensão e publicações:** (Claramente relacionado a PE)

A categoria UC (Unidades Curriculares) parece referir-se a disciplinas cursadas, enquanto TCC é explicitamente excluído como atividade complementar pelo ROD.^1^ O sistema precisará de uma lógica clara para mapear o conteúdo do certificado (identificado por NLP) a uma ou mais dessas siglas, respeitando as definições oficiais do IFMS.

### 2.4 Compreendendo Limites de Carga Horária

A carga horária total exigida varia significativamente entre os cursos no Campus Naviraí, como visto nos 50 horas para ADS ^3^ e 240 horas para Agronomia.^2^ Essa variação demonstra que o sistema não pode ter uma configuração única; ele deve ser parametrizado por curso.

Além do total, o Anexo A do ROD do IFMS ^1^ sugere limites máximos de pontos (equivalentes a horas) por categoria ao longo do curso (ex: 120 pontos para atividades culturais/esportivas, 100 para divulgação científica, 100 para vivência acadêmica, 100 para pesquisa/extensão). É fundamental verificar se esses limites são apenas sugestões da minuta ou se foram oficializados no ROD final ou nos PPCs específicos de Naviraí, pois eles impactam diretamente a lógica de validação. A prática em outros IFs, como IF Sudeste MG ^6^ (limites percentuais) ou IFMT ^5^ (limites por sub-atividade), mostra que diferentes abordagens existem.

### 2.5 Abordando Cenários Complexos

A solicitação do usuário inclui cenários de validação que requerem atenção especial, pois podem não estar explicitamente cobertos ou podem até contradizer os regulamentos encontrados:

* **Distribuição de Horas Excedentes:** O usuário exemplifica uma situação onde 70 horas de uma atividade são submetidas para uma categoria com teto de 60 horas, e as 10 horas restantes deveriam ser redistribuídas. No entanto, a análise do ROD do IFMS ^1^ não encontrou menção a essa possibilidade de redistribuição. Pelo contrário, o regulamento enfatiza a observância dos limites por categoria. O regulamento do IFMT ^5^ é ainda mais explícito ao afirmar que horas excedentes são  *desconsideradas* . Essa divergência é crítica. O sistema deve ser projetado com base nas regras oficiais confirmadas. Se a redistribuição for uma regra específica de Naviraí não documentada na pesquisa, ou uma funcionalidade desejada além do regulamento, isso precisa ser validado com a coordenação do curso. A implementação dessa lógica adiciona complexidade significativa ao motor de regras.
* **Classificações Múltiplas por Atividade:** Outro exemplo do usuário sugere que uma única atividade (certificado de 200h) poderia ser classificada simultaneamente como PA e PV, totalizando as 200h. Contudo, o ROD do IFMS ^1^ indica que atividades que se encaixam em múltiplas categorias serão pontuadas conforme a  *escolha expressa do aluno* , sugerindo que apenas *uma* classificação é atribuída por item submetido. Novamente, há uma aparente contradição que precisa ser resolvida. O sistema precisa de clareza sobre se uma atividade pode, de fato, ser dividida entre categorias ou se o aluno deve escolher a categoria mais apropriada ao submeter.
* **Atividades em Grupo:** A menção a atividades realizadas em dupla ou grupo levanta uma questão funcional importante: como dividir as horas de um único certificado entre múltiplos participantes? A pesquisa nos regulamentos do IFMS ^1^, em descrições de sistemas acadêmicos ^7^ e em artigos gerais sobre atividades complementares ^9^ não revelou regras claras para essa situação. O snippet ^10^ discute múltiplos participantes em sistemas complexos, mas não no contexto de certificados acadêmicos. Esta ausência de diretrizes representa uma lacuna significativa. O sistema precisará de uma abordagem definida: exigir submissões individuais (mesmo para atividades em grupo), implementar uma lógica de divisão padrão (ex: divisão igualitária), ou, mais provavelmente, incluir uma interface para que o professor responsável aloque manualmente as horas para cada participante do grupo. A escolha impactará o fluxo de trabalho e o esforço de desenvolvimento.

A variabilidade nos requisitos de horas entre cursos como ADS e Agronomia no mesmo campus ^2^ sublinha a necessidade de um sistema configurável por curso, não apenas em termos de horas totais, mas potencialmente também nos limites por categoria, caso estes também variem.

### 2.6 Tabela Resumo: Regras de Atividades Complementares - IFMS Naviraí (Baseado na Pesquisa)

A tabela a seguir consolida as informações regulatórias encontradas e destaca as áreas que necessitam de clarificação junto ao IFMS Naviraí.

| **Característica**             | **ADS (Tecnologia)**                                  | **Agronomia (Bacharelado)**                           | **Regulamento Geral IFMS (ROD - Minuta)**                                                                                                                                        | **Necessidade de Clarificação / Implicações para o Sistema**                                                                                                                          |
| ------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Horas Totais Exigidas**       | 50 horas                                                    | 240 horas                                                   | Definido no PPC de cada curso.                                                                                                                                                         | **Sistema deve ser configurável por curso.**                                                                                                                                             |
| **Categorias Oficiais**         | Remete ao ROD                                               | Remete ao ROD (Ex: Pesquisa, Extensão, Eventos, Visitas)   | 1. Aperfeiçoamento Cultural/Esportivo`<br>`2. Divulgação Científica/Iniciação Docência`<br>`3. Vivência Acadêmica/Profissional`<br>`4. Pesquisa/Extensão/Publicações | Mapear categorias do usuário (UC, PE, etc.) para as categorias oficiais do ROD. Validar se TCC é realmente uma categoria de*atividade complementar*(ROD diz que não).                      |
| **Limites Máx. / Categoria**   | Não especificado no PPC (remete ao ROD)                    | Não especificado no PPC (remete ao ROD)                    | Sugestão de limites máx. de pontos/horas por categoria (ex: 120 Cult/Esp; 100 Cien/Doc; 100 Viv; 100 Pesq/Ext).                                                                      | **Confirmar se os limites do ROD são oficiais e se aplicam a Naviraí.**Sistema deve implementar os limites confirmados.                                                                       |
| **Regra Horas Excedentes**      | Não mencionado (usuário sugere redistribuição)          | Não mencionado (usuário sugere redistribuição)          | Não menciona redistribuição; enfatiza observância dos limites. (Comparativo: IFMT^5^desconsidera excesso).                                                                         | **CRÍTICO: Confirmar regra oficial.**A lógica de redistribuição do usuário difere das normas encontradas. O motor de regras do sistema depende dessa definição.                          |
| **Múltiplas Classificações** | Não mencionado (usuário sugere divisão entre categorias) | Não mencionado (usuário sugere divisão entre categorias) | Atividade pontuada na categoria escolhida pelo aluno (implica uma única classificação).                                                                                             | **CRÍTICO: Confirmar regra oficial.**A lógica de divisão do usuário difere da norma encontrada. O sistema precisa saber se permite uma ou múltiplas classificações por certificado.      |
| **Atividades em Grupo**         | Não mencionado (usuário menciona necessidade)             | Não mencionado (usuário menciona necessidade)             | Não abordado especificamente.                                                                                                                                                         | **CRÍTICO: Definir processo.**Como as horas de um certificado serão atribuídas a múltiplos alunos? Requer definição de funcionalidade (divisão auto, alocação manual pelo professor?). |

Esta análise detalhada dos regulamentos e a identificação das ambiguidades são cruciais. O próximo passo essencial antes de solidificar a lógica do sistema é a validação dessas regras complexas (horas excedentes, múltiplas classificações, atividades em grupo) diretamente com a coordenação ou setor responsável no IFMS Campus Naviraí.

## 3. Panorama de Sistemas Existentes

Para contextualizar o desenvolvimento do sistema proposto, é útil analisar as soluções existentes no mercado e em outras instituições de ensino, focando em Sistemas de Gestão Acadêmica (AMS) e Gerenciamento Eletrônico de Documentos (GED).

### 3.1 Sistemas de Gestão Acadêmica (AMS)

Os AMS são plataformas abrangentes usadas por instituições de ensino para gerenciar diversos aspectos da vida acadêmica, desde matrículas e notas até horários e comunicação.

* **SIGAA (Exemplo da UFC):** O Sistema Integrado de Gestão de Atividades Acadêmicas (SIGAA), utilizado por diversas universidades federais no Brasil, como a UFC ^11^, possui um módulo específico para o cadastro e integralização de horas complementares e de extensão.^11^ As funcionalidades incluem:
  * Registro da carga horária pelo próprio aluno via sistema.
  * Solicitação formal de creditação das horas.
  * Análise e aprovação (ou rejeição) pelo docente responsável (coordenador, orientador).
  * Integralização das horas aprovadas no histórico acadêmico pelo aluno.
  * Acompanhamento do status de cada atividade registrada.
    É crucial notar que, conforme a descrição dessa funcionalidade no SIGAA ^11^, **não há menção ao uso de automação ou Inteligência Artificial para a classificação** das atividades. O processo depende da submissão do aluno e da validação manual por um professor. O foco da ferramenta é otimizar a supervisão, reduzir a burocracia e centralizar o processo, mas não automatizar a análise do conteúdo dos certificados.
* **Outros AMS:** Existem soluções comerciais no mercado brasileiro, como o Mannesoft Prime ^12^, que se posiciona como especializado em todos os segmentos educacionais e menciona ferramentas para gestão educacional, incluindo a possibilidade de atividades em grupo via videoconferência e gamificação ^7^, e o Abaris da Stoque ^13^, que oferece "Automação as a Service". Embora essas plataformas possam ter módulos de gestão acadêmica, os detalhes específicos sobre o gerenciamento automatizado de certificados de atividades complementares com IA não foram encontrados nos materiais pesquisados. Soluções mais simples, como templates no Notion ^14^, também existem, indicando uma gama de abordagens, muitas delas manuais ou semi-manuais.

A análise do SIGAA ^11^, um sistema amplamente utilizado, demonstra que a automação baseada em IA para classificação de certificados, como a proposta pelo projeto do IFMS, ainda não é uma funcionalidade padrão em grandes AMS brasileiros. Isso reforça o caráter inovador do projeto, mas também significa que haverá menos exemplos diretos ou soluções prontas para adaptar. O desenvolvimento exigirá uma abordagem mais customizada, focada na integração das tecnologias de IA (OCR/NLP) com a lógica de regras acadêmicas.

### 3.2 Gerenciamento Eletrônico de Documentos (GED)

Sistemas GED são focados na gestão de documentos em formato digital, abrangendo captura, armazenamento, indexação, recuperação e controle de versão.^15^

* **Conceito e Funcionalidades:** O GED visa organizar e facilitar o acesso a documentos eletrônicos, substituindo arquivos físicos.^15^ Funcionalidades comuns incluem a digitalização de documentos, organização em pastas virtuais, busca por metadados ou conteúdo (se indexado), controle de acesso e, em alguns casos, automação de fluxos de trabalho (workflows).^16^ Empresas como Arquivar ^18^, Rede Imagem ^15^, Zeev ^16^, Impeto ^17^ e Taugor ^19^ oferecem soluções GED no mercado.
* **Relevância para o Projeto:** O sistema proposto para o IFMS lidará intrinsecamente com documentos (certificados). Portanto, os princípios e funcionalidades de um GED são relevantes para a parte de **armazenamento, organização e recuperação** dos certificados enviados pelos alunos. O sistema precisará armazenar os arquivos digitais (PDFs, imagens) e associá-los aos respectivos alunos e atividades. No entanto, um sistema GED padrão geralmente não possui a inteligência específica necessária para:
  1. **Extrair texto automaticamente** de imagens de certificados usando OCR avançado.
  2. **Analisar o conteúdo** do texto extraído usando NLP para classificar a atividade segundo as categorias do IFMS (UC, PE, PP, etc.).
  3. **Aplicar as regras complexas** de carga horária, limites por categoria e tratamento de horas excedentes específicas do IFMS Naviraí.

Embora os sistemas GED forneçam a infraestrutura necessária para o manuseio dos documentos digitais, eles não resolvem o cerne do problema que o projeto visa atacar: a classificação automática baseada em conteúdo e a aplicação de regras acadêmicas. O componente de IA (OCR/NLP) e o motor de regras são os elementos distintivos e customizados que precisarão ser desenvolvidos e integrados a uma estrutura de gerenciamento de documentos, que pode ser inspirada em princípios GED ou utilizar soluções de armazenamento mais simples.

### 3.3 Estudos de Caso e Práticas Institucionais

A pesquisa buscou exemplos de como outras instituições de ensino gerenciam atividades complementares, especialmente no que tange à classificação, limites e distribuição de horas.

* **Regulamentos e Manuais:** Documentos de instituições como UFABC ^20^, IFFarroupilha ^21^, IFRS ^22^, IF Sudeste MG ^6^, UFBA ^24^, UFMG ^25^, UENP ^26^, UFSCar ^27^, Unisul ^28^, UFPE ^29^ e Universidade de Lisboa ^30^ confirmam a existência e a obrigatoriedade das atividades complementares em diversos cursos e níveis. Eles detalham categorias, cargas horárias e procedimentos, mas os snippets encontrados não descrevem sistemas automatizados baseados em IA para gerenciamento.
* **Publicações Acadêmicas:** Buscas em bases como SciELO ^31^ por artigos sobre automação na gestão de atividades complementares não retornaram, nos snippets disponíveis, descrições detalhadas de sistemas implementados com IA para classificação de certificados.

A ausência de estudos de caso detalhados sobre sistemas semelhantes ao proposto sugere que esta é uma área com espaço para inovação e desenvolvimento. As práticas atuais, mesmo em instituições bem estabelecidas, parecem ainda depender significativamente de processos manuais ou de sistemas com automação limitada (como o SIGAA ^11^).

Em suma, a pesquisa de sistemas existentes indica que, embora haja ferramentas para gestão acadêmica e de documentos, a solução específica que utiliza IA para ler, classificar certificados e aplicar regras complexas de atividades complementares, como a idealizada para o IFMS Naviraí, representa um avanço em relação às práticas comuns encontradas. O projeto tem potencial para ser um diferencial, mas exigirá um desenvolvimento customizado dos componentes de inteligência e regras.

## 4. Tecnologias de IA Essenciais para Processamento de Certificados

O coração do sistema proposto reside na aplicação inteligente de tecnologias de Inteligência Artificial para processar os certificados submetidos pelos alunos. Duas tecnologias são fundamentais: Reconhecimento Óptico de Caracteres (OCR) e Processamento de Linguagem Natural (NLP).

### 4.1 Reconhecimento Óptico de Caracteres (OCR): Extraindo o Texto

* **Propósito:** A primeira etapa essencial é converter a imagem do certificado (seja um arquivo PDF escaneado, uma foto ou outro formato de imagem) em texto que possa ser processado por um computador.^33^ O OCR é a tecnologia que realiza essa "leitura" da imagem e extrai os caracteres.^36^ Sem um texto digital, a análise de conteúdo via NLP é impossível.
* **Desafios Intrínsecos aos Certificados:** Certificados acadêmicos apresentam desafios particulares para a tecnologia OCR:
  * **Qualidade Variável da Imagem:** Os documentos podem ser digitalizados com baixa resolução, iluminação inadequada, sombras, reflexos ou contraste insuficiente. Certificados mais antigos podem ter tinta desbotada ou papel amarelado.^37^ Fotos tiradas com celular podem estar desalinhadas (skew) ou distorcidas.^37^
  * **Diversidade de Layouts e Formatos:** Não existe um padrão único para certificados. Eles variam enormemente em estrutura, disposição dos elementos (nome do aluno, nome do curso/evento, carga horária, data, instituição emissora), uso de tabelas, colunas ou caixas de texto.^37^ Sistemas baseados em regras ou templates fixos falham diante dessa variabilidade.^37^
  * **Fontes e Elementos Não Textuais:** Podem ser usadas fontes não padrão, decorativas ou até mesmo texto manuscrito (assinaturas, preenchimentos manuais), que são mais difíceis para o OCR.^38^ Além disso, logotipos, selos, carimbos, linhas decorativas e outros elementos gráficos podem interferir na detecção do texto.^37^
  * **Caracteres Especiais e Idiomas:** Embora o foco seja o português, certificados podem conter nomes estrangeiros, termos técnicos ou símbolos que alguns motores OCR podem ter dificuldade em reconhecer corretamente.^37^
* **A Importância Crucial do Pré-processamento:** Dada a variabilidade e os potenciais problemas de qualidade, aplicar técnicas de pré-processamento à imagem *antes* de enviá-la ao motor OCR é fundamental para maximizar a precisão da extração de texto.^39^ As técnicas mais relevantes identificadas incluem:
  * **Garantir Alta Resolução:** Sempre que possível, orientar os usuários a escanear ou fotografar com alta resolução (mínimo de 300 dpi recomendado).^37^
  * **Otimização da Imagem:** Conversão para escala de cinza ^47^, binarização (transformar em preto e branco para aumentar contraste) ^39^, ajuste de brilho/contraste.^40^
  * **Correção Geométrica:** Detecção e correção de inclinação (deskewing) para alinhar o texto horizontalmente.^39^
  * **Limpeza da Imagem:** Redução de ruído (remoção de pontos, manchas ou padrões de fundo) ^39^, remoção de bordas.^39^
  * **Melhora da Nitidez:** Aplicação de filtros de sharpening para textos borrados ou de baixa resolução.^47^
* **Comparativo de Bibliotecas e Ferramentas OCR (Foco em Python e Código Aberto):** A escolha da ferramenta OCR impacta diretamente a qualidade do texto extraído. Diversas opções estão disponíveis, principalmente no ecossistema Python:
  * **Tesseract (via `pytesseract`):** Motor OCR de código aberto mantido pelo Google, amplamente considerado um padrão pela sua maturidade e precisão.^33^ Suporta mais de 100 idiomas, incluindo português.^48^ Requer instalação separada do motor Tesseract.^33^ Sua precisão é sensível à qualidade da imagem, sendo comum o uso conjunto com a biblioteca OpenCV para pré-processamento.^47^ Pode fornecer informações detalhadas sobre a localização do texto (bounding boxes).^47^
  * **EasyOCR:** Biblioteca Python de código aberto que utiliza modelos de deep learning (PyTorch).^34^ É conhecida pela facilidade de uso (API simples, instalação direta via pip) e boa precisão em textos com diferentes estilos, fontes e orientações (horizontal/vertical).^33^ Suporta mais de 80 idiomas.^34^ Uma forte candidata pela combinação de simplicidade e performance.
  * **Keras-OCR:** Biblioteca de código aberto baseada nos frameworks Keras e TensorFlow.^33^ Oferece modelos pré-treinados e permite customização e treinamento de novos modelos.^33^ Requer familiaridade com esses frameworks de deep learning.
  * **Doctr:** Biblioteca Python de código aberto focada em "document understanding", indo além do OCR básico para analisar layout e estrutura do documento.^33^ Pode ser vantajosa para certificados com layouts complexos, mas sua implementação pode ser mais avançada.^33^
  * **Outras Opções Open Source:** OCRopus (focado em documentos históricos) ^48^, PyOCR (wrapper para múltiplos motores como Tesseract) ^48^, OCRmyPDF (adiciona camada de texto OCR a PDFs usando Tesseract) ^34^, Kraken.^48^
  * **Serviços de Nuvem (Comerciais):** Soluções como AWS Textract ^33^ e Google Cloud Vision OCR ^36^ oferecem OCR como serviço gerenciado. Podem ter alta precisão, escalabilidade e funcionalidades adicionais (como extração de formulários e tabelas no Textract ^53^), mas implicam custos e dependência de provedores de nuvem.

A qualidade do texto gerado pelo OCR é um pré-requisito absoluto para a etapa seguinte de NLP. Erros de OCR, como palavras mal reconhecidas, caracteres trocados ou texto fora de ordem, irão inevitavelmente prejudicar a capacidade do modelo de NLP de classificar corretamente a atividade ou extrair informações.^54^ Portanto, a escolha de uma ferramenta OCR robusta (como EasyOCR ou Tesseract) combinada com um pipeline de pré-processing eficaz ^37^ é um investimento crítico para o sucesso do sistema como um todo.

### 4.2 Processamento de Linguagem Natural (NLP): Classificação e Extração

Uma vez que o texto do certificado foi extraído pelo OCR, entra em ação o NLP para interpretar esse texto e realizar as tarefas principais do sistema.

* **Propósito:** O NLP permite que o sistema "entenda" o conteúdo do texto extraído para:
  1. **Classificar a Atividade:** Atribuir a etiqueta correta (UC, PE, PP, PD, PA, PV) com base no teor do certificado (nome do evento/curso, descrição, instituição promotora).^55^ Esta é a funcionalidade central.
  2. **Extrair Informações Chave (Opcional, mas Valioso):** Identificar e extrair dados específicos como nome do aluno, nome da atividade, carga horária mencionada, data de realização, instituição emissora, etc..^54^ Isso pode ser usado para validação cruzada, preenchimento de banco de dados e auditoria.
* **Tarefas NLP Relevantes:**
  * **Classificação de Texto:** É a tarefa de associar um texto (neste caso, o conteúdo do certificado) a uma ou mais categorias pré-definidas (as siglas do IFMS).^55^ Modelos de Machine Learning são treinados para reconhecer padrões textuais associados a cada categoria.
  * **Reconhecimento de Entidades Nomeadas (NER):** Identifica e classifica palavras ou sequências de palavras que representam entidades específicas, como nomes de pessoas (PERSON), organizações (ORG), locais (LOC), datas (DATE), quantidades (QUANTITY, como horas).^54^ Por exemplo, poderia extrair "João Silva" (PERSON), "IFMS Campus Naviraí" (ORG), "60 horas" (QUANTITY), "15 de maio de 2025" (DATE).
  * **Pré-processamento NLP:** Antes da classificação ou NER, o texto OCRizado geralmente passa por etapas como: *Tokenização* (dividir o texto em palavras ou sub-palavras) ^54^, *Limpeza* (remover caracteres indesejados, corrigir erros óbvios de OCR se possível), *Normalização* (converter para minúsculas), *Remoção de Stopwords* (remover palavras comuns como "o", "a", "de"), e *Stemming/Lemmatization* (reduzir palavras à sua forma base para agrupar variações).^55^
* **Comparativo de Bibliotecas NLP (Foco em Python e Português):**
  * **spaCy:** Biblioteca de código aberto projetada para uso em produção, conhecida pela performance, eficiência e API amigável.^55^ Possui modelos pré-treinados para português e suporta tarefas como tokenização, etiquetagem de Part-of-Speech (POS), NER e classificação de texto.^55^ É frequentemente considerada mais adequada para implantação em produção do que NLTK devido à sua velocidade e facilidade de uso.^62^
  * **NLTK (Natural Language Toolkit):** Biblioteca muito abrangente, mais voltada para pesquisa e ensino.^55^ Oferece uma vasta gama de algoritmos e recursos textuais (corpora), mas pode ser mais lenta e ter uma API mais complexa para tarefas de produção.^62^ Suporta português.^62^
  * **Hugging Face Transformers:** Plataforma e biblioteca de código aberto que fornece acesso a milhares de modelos pré-treinados, incluindo muitos para português (como BERTimbau ^65^, Albertina ^66^, mDistilBERT ^67^, etc.).^68^ É o estado da arte para muitas tarefas de NLP, incluindo classificação de texto e NER. A abordagem principal é o *fine-tuning* (ajuste fino) desses modelos para a tarefa específica (classificação de certificados) usando um dataset próprio.^58^ Requer compreensão dos modelos Transformer e do processo de fine-tuning.
  * **Scikit-learn:** Uma biblioteca fundamental de Machine Learning em Python, não específica para NLP, mas amplamente utilizada para classificação de texto. Fornece ferramentas para extração de features (como `TfidfVectorizer` ^63^) e diversos algoritmos de classificação (SVM ^63^, Naive Bayes, Regressão Logística ^45^, etc.) que podem ser aplicados aos dados textuais pré-processados.

Considerando a necessidade de classificar os certificados com base em seu conteúdo semântico, a abordagem mais promissora parece ser o uso de modelos baseados em Transformers (via Hugging Face) ^68^, fine-tuned especificamente para as categorias do IFMS. Isso aproveita o conhecimento linguístico pré-treinado em grandes volumes de texto em português ^65^, adaptando-o para a tarefa de nicho de classificar certificados acadêmicos. Essa abordagem geralmente supera métodos tradicionais (como TF-IDF + SVM) em tarefas de classificação de texto complexas.^70^ A biblioteca spaCy ^62^ seria uma excelente escolha para as etapas de pré-processamento e, potencialmente, para tarefas de NER mais simples, dada sua eficiência e facilidade de uso em produção.

A utilização de NER ^60^, embora secundária à classificação, pode agregar um valor considerável. Extrair automaticamente o nome do aluno, a carga horária e a data do certificado permite verificações automáticas de consistência (o nome bate com o do usuário logado? A carga horária confere com a informada? A data é válida?) e a criação de um registro estruturado da atividade, facilitando consultas e auditorias futuras. Isso transformaria o sistema de um simples classificador para uma ferramenta de validação de dados mais completa.

### 4.3 Tabelas Comparativas de Ferramentas

**Tabela 1: Comparativo de Ferramentas OCR para Processamento de Certificados**

| **Ferramenta**              | **Características Principais**                                                        | **Facilidade de Uso**                              | **Potencial de Precisão**                         | **Suporte a Português** | **Qualidade Variável** | **Código Aberto / Custo** | **Complexidade Setup** | **Snippets Relevantes** |
| --------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------- | ------------------------------ | ----------------------------- | -------------------------------- | ---------------------------- | ----------------------------- |
| **Tesseract (pytesseract)** | Motor maduro, +100 idiomas, grande comunidade, saída detalhada (bboxes).                    | Requer instalação do motor; wrapper Python é usável. | Alta, mas sensível à qualidade; melhora com pré-proc. | Sim                            | Moderada                      | Sim / Gratuito                   | Média                       | ^33^                          |
| **EasyOCR**                 | Baseado em Deep Learning (PyTorch), API simples, +80 idiomas, bom com estilos/orientações. | Alta (instalação pip, API intuitiva).                  | Alta (Deep Learning).                                    | Sim                            | Boa                           | Sim / Gratuito                   | Baixa                        | ^33^                          |
| **Keras-OCR**               | Baseado em Deep Learning (Keras/TF), modelos pré-treinados, customizável.                  | Média (requer familiaridade com Keras/TF).              | Alta (Deep Learning).                                    | Sim (configurável)            | Boa                           | Sim / Gratuito                   | Média/Alta                  | ^33^                          |
| **Doctr**                   | Foco em "document understanding" (layout), análise estrutural.                              | Média/Alta (voltado para ML/NLP).                       | Potencialmente alta para layouts complexos.              | Não detalhado                 | Boa (layout)                  | Sim / Gratuito                   | Média/Alta                  | ^33^                          |
| **AWS Textract**            | Serviço de nuvem gerenciado, OCR + extração de dados (formulários, tabelas), escalável. | Média (requer integração AWS SDK).                    | Muito Alta (otimizado para documentos).                  | Sim                            | Muito Boa                     | Não / Pago (AWS)                | Média                       | ^34^                          |
| **Google Cloud Vision**     | Serviço de nuvem gerenciado, OCR geral e otimizado para documentos (PDF/TIFF).              | Média (requer integração Google Cloud SDK).           | Muito Alta.                                              | Sim                            | Muito Boa                     | Não / Pago (GCP)                | Média                       | ^36^                          |

**Tabela 2: Comparativo de Bibliotecas NLP para Classificação de Texto em Português**

| **Biblioteca**           | **Características Principais**                                                  | **Facilidade de Uso** | **Performance / Velocidade** | **Suporte/Modelos PT** | **Adequação Produção** | **Tarefas Chave (Classificação, NER)** | **Snippets Relevantes** |
| ------------------------------ | -------------------------------------------------------------------------------------- | --------------------------- | ---------------------------------- | ---------------------------- | -------------------------------- | ---------------------------------------------- | ----------------------------- |
| **spaCy**                | Foco em produção, API eficiente, rápida, modelos pré-treinados.                    | Alta                        | Alta                               | Sim (bom suporte)            | Alta                             | Sim (ambas)                                    | ^55^                          |
| **NLTK**                 | Abrangente, foco acadêmico/pesquisa, muitos algoritmos/recursos.                      | Média/Baixa                | Média/Baixa                       | Sim                          | Média                           | Sim (ambas)                                    | ^55^                          |
| **Hugging Face Transf.** | Acesso a SOTA (BERT, etc.), fine-tuning, vasta comunidade, muitos modelos PT.          | Média/Alta                 | Variável (model dep.)             | Sim (excelente)              | Alta                             | Sim (ambas, via fine-tuning)                   | ^58^                          |
| **Scikit-learn**         | Biblioteca ML geral, integra com NLP (TF-IDF), muitos classificadores (SVM, NB, etc.). | Alta                        | Boa                                | N/A (usa features)           | Alta                             | Sim (Classificação, após features)          | ^45^                          |

## 5. Construindo o Modelo de Classificação de IA

Desenvolver o núcleo de IA que classifica os certificados exige uma abordagem metodológica, desde a obtenção e preparação dos dados até a escolha e avaliação do modelo de Machine Learning (ML).

### 5.1 Requisitos de Dados

A performance de qualquer modelo de ML depende fundamentalmente da qualidade e quantidade dos dados de treinamento. Para este projeto, o ideal seria um dataset composto por certificados reais (ou muito semelhantes) aos que os alunos do IFMS submetem. Este dataset precisa ser:

* **Representativo:** Deve incluir exemplos de todas as categorias de atividades complementares (UC, PE, PP, etc.) que o sistema precisa classificar.
* **Diverso:** Deve abranger a variedade de layouts, fontes, qualidades de imagem (bons e ruins) e instituições emissoras encontradas na prática.
* **Rotulado:** Cada certificado no dataset de treinamento/fine-tuning precisa ser corretamente rotulado com a(s) categoria(s) do IFMS a que pertence.

A obtenção e rotulagem de um dataset grande e representativo de certificados reais de alunos pode ser um desafio significativo para um projeto estudantil, devido a questões de privacidade, acesso e o esforço de anotação manual.^37^ Estratégias alternativas ou complementares podem ser necessárias, como:

* **Data Augmentation:** Criar variações sintéticas dos certificados existentes (ex: adicionar ruído, alterar brilho/contraste, aplicar pequenas rotações) para aumentar artificialmente o tamanho do dataset.
* **Uso de Datasets Públicos:** Utilizar datasets públicos de documentos (faturas, recibos, artigos científicos) para pré-treinamento ou como parte do fine-tuning, embora sejam menos específicos que certificados acadêmicos.
* **Transfer Learning / Fine-tuning:** Focar em modelos pré-treinados em grandes volumes de dados (como os da Hugging Face) que já possuem um bom entendimento da linguagem (português) e da estrutura de documentos, exigindo um dataset rotulado menor para adaptação à tarefa específica.^58^

### 5.2 Preparação de Dados e Engenharia de Features

Uma vez que o texto é extraído via OCR, ele precisa ser preparado para alimentar o modelo de ML.

* **Input:** O texto bruto extraído pelo OCR.
* **Pré-processamento NLP:** Aplicação das técnicas já mencionadas: tokenização, limpeza (remoção de caracteres especiais irrelevantes, potencial correção de erros comuns de OCR), normalização (lowercase), remoção de stopwords e stemming/lemmatization.^54^
* **Representação Textual (Feature Engineering):** Converter o texto limpo em vetores numéricos que o modelo de ML possa entender:
  * **TF-IDF:** Cria vetores esparsos baseados na frequência de termos, ponderada pela sua raridade no corpus. É uma técnica clássica, eficaz para modelos como SVM.^59^
  * **Word Embeddings:** Cria vetores densos (Word2Vec, GloVe, FastText) que capturam relações semânticas entre palavras.^57^ São tipicamente usados como camada de entrada em modelos de deep learning (CNNs, LSTMs).
  * **Transformer Embeddings:** Embeddings contextuais gerados por modelos como BERT, RoBERTa, etc. (disponíveis via Hugging Face).^68^ Capturam o significado da palavra no contexto da frase/documento, geralmente levando a melhor performance em tarefas de compreensão textual.^70^ São a base para o fine-tuning de Transformers.

### 5.3 Seleção do Modelo: Treinamento vs. Fine-tuning

Existem duas abordagens principais para criar o modelo de classificação:

* **Treinamento a partir do Zero (From Scratch):** Envolve treinar um modelo (seja tradicional como SVM ^45^ ou deep learning como CNNs ^70^) usando apenas o dataset de certificados rotulados. Esta abordagem geralmente requer um dataset consideravelmente grande para alcançar boa performance, o que pode ser inviável [Insight 9].
* **Ajuste Fino de Modelos Pré-treinados (Fine-tuning):** Esta é a **abordagem recomendada** [Insight 7]. Utiliza-se um modelo de linguagem grande (LLM), como BERTimbau ^65^ ou Albertina ^66^, que já foi treinado em um vasto corpus de texto em português (e talvez até dados de layout, no caso de modelos multimodais). Esse modelo já "entende" a língua portuguesa. O processo de fine-tuning consiste em continuar o treinamento desse modelo por um curto período, usando o dataset específico de certificados rotulados do IFMS. Isso adapta o conhecimento geral do modelo para a tarefa específica de classificar certificados, exigindo muito menos dados rotulados do que treinar do zero.^58^ A biblioteca Hugging Face Transformers ^68^ é a ferramenta padrão para realizar fine-tuning.

### 5.4 Melhores Práticas para o Pipeline de Machine Learning

Construir um pipeline de ML robusto e sustentável envolve seguir boas práticas de engenharia de software e MLOps (Machine Learning Operations):

* **Modularidade:** Estruturar o pipeline em componentes independentes e bem definidos (ex: módulo de carregamento de dados, pré-processamento de imagem, OCR, pré-processamento de texto, classificação, aplicação de regras). Isso facilita o desenvolvimento, teste, manutenção e substituição de partes do sistema.^73^
* **Automação:** Automatizar tarefas repetitivas como o pré-processamento, treinamento e avaliação do modelo sempre que possível.^73^
* **Versionamento:** Utilizar controle de versão (como Git) não apenas para o código, mas também para os dados (ou referências a eles) e os modelos treinados. Isso garante reprodutibilidade e rastreabilidade.^74^
* **Avaliação Criteriosa:** Definir métricas de avaliação que realmente reflitam o sucesso no contexto do problema (não apenas acurácia geral). Analisar a matriz de confusão ^63^, precisão, recall e F1-score por categoria.^59^ Usar um conjunto de teste separado e representativo que não foi visto durante o treinamento ou validação.^75^
* **Simplicidade:** Começar com a solução mais simples que pode funcionar (Princípio da Navalha de Occam) e adicionar complexidade apenas se comprovadamente necessário para melhorar a performance.^74^ Um modelo excessivamente complexo pode consumir mais recursos e ser mais difícil de manter.
* **Monitoramento:** Planejar como monitorar a performance do modelo após a implantação (em produção) para detectar degradação (model drift) devido a mudanças nos dados de entrada ao longo do tempo.^73^ Isso pode acionar processos de retreinamento.
* **Documentação:** Manter documentação clara e completa sobre as fontes de dados, etapas de pré-processamento, arquitetura do modelo, hiperparâmetros, resultados de avaliação e decisões de design. Isso é crucial para manutenção, colaboração e entendimento futuro do sistema.^74^

A performance do classificador não deve ser medida apenas pela acurácia geral. Dado que o sistema impacta diretamente a validação de créditos dos alunos, é fundamental entender *onde* o modelo erra. Uma análise detalhada da matriz de confusão ^63^ pode revelar se o modelo tem dificuldade em distinguir categorias específicas (por exemplo, PA vs. PV, se forem semanticamente próximas). Métricas por classe (precisão, recall, F1) são mais informativas que a acurácia global, especialmente se houver desbalanceamento entre as categorias. Além disso, explorar técnicas de interpretabilidade de modelos (explicar *por que* uma classificação foi feita) pode ser valioso para ganhar confiança no sistema e depurar erros [Insight 10].

## 6. Arquitetura do Sistema e Estratégia de Integração

A concepção da arquitetura do sistema é crucial para garantir que todos os componentes (interface, processamento de IA, regras de negócio, armazenamento) funcionem de forma coesa e eficiente.

### 6.1 Esboço Conceitual da Arquitetura

Uma arquitetura de alto nível para o sistema proposto pode ser visualizada com os seguintes componentes principais:

**Snippet de código**

```
graph TD
    A[Frontend (Interface Web)] --> B(Backend Server);
    B --> C{Fila de Processamento (Opcional)};
    C --> D;
    D -- Texto Extraído --> E[Módulo NLP];
    E -- Classificação/Entidades --> F;
    F -- Horas Validadas/Status --> G;
    B <--> G;
    D -- Imagem Processada --> H[Armazenamento de Documentos];
    B --> H;
    A --> H(Upload);
    G --> A(Exibição de Resultados);

    subgraph "Processamento Assíncrono (Recomendado)"
        direction LR
        C; D; E; F;
    end

    subgraph "Camada de Persistência"
        direction LR
        G; H;
    end

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#ccf,stroke:#333,stroke-width:2px
    style G fill:#9cf,stroke:#333,stroke-width:2px
    style H fill:#9cf,stroke:#333,stroke-width:2px
    style D fill:#fcc,stroke:#333,stroke-width:2px
    style E fill:#fcc,stroke:#333,stroke-width:2px
    style F fill:#fcc,stroke:#333,stroke-width:2px
```

* **Frontend (Interface Web):** Portal para os alunos realizarem o upload dos certificados (PDF, imagens) e visualizarem o status e os resultados do processamento. Também deve prover uma interface para os professores revisarem classificações, gerenciarem regras (se aplicável) e realizarem overrides manuais.
* **Backend Server:** Aplicação central que recebe as requisições do frontend, gerencia o fluxo de trabalho, interage com os outros módulos e com o banco de dados. Pode ser desenvolvido usando frameworks como Flask ou Django em Python.
* **Armazenamento de Documentos:** Local para guardar os arquivos originais dos certificados enviados. Pode ser um sistema de arquivos local, um serviço de armazenamento em nuvem (como AWS S3 ^34^ ou Google Cloud Storage) ou um banco de dados com suporte a BLOBs.
* **Módulo OCR:** Componente responsável por receber a imagem do certificado, aplicar pré-processamento e utilizar a biblioteca OCR escolhida (ex: EasyOCR, Tesseract) para extrair o texto.^33^
* **Módulo NLP:** Recebe o texto extraído do OCR, realiza pré-processamento NLP e aplica o modelo de ML treinado/fine-tuned (ex: via Hugging Face Transformers ou spaCy) para classificar a atividade e, opcionalmente, extrair entidades nomeadas (NER).^55^
* **Motor de Regras:** Implementa a lógica de negócio específica do IFMS Naviraí. Recebe a classificação do módulo NLP e a carga horária (extraída via NER ou informada pelo aluno/OCR), aplica os limites máximos por categoria, calcula as horas a serem creditadas e implementa a lógica (confirmada) para tratamento de horas excedentes e múltiplas classificações.^1^
* **Banco de Dados:** Armazena informações dos usuários (alunos, professores), metadados dos certificados (nome do arquivo, data de upload, status), o texto extraído pelo OCR, os resultados da classificação NLP, as horas validadas, o histórico de processamento e as regras configuráveis do curso (carga horária total, limites por categoria). PostgreSQL ou MySQL são opções robustas.
* **Fila de Processamento (Opcional, mas Recomendado):** Como o processamento OCR e NLP pode ser computacionalmente intensivo e demorado, usar uma fila de mensagens (como RabbitMQ ou Celery com Redis/RabbitMQ) para processamento assíncrono é recomendado. O backend colocaria a tarefa na fila, e workers independentes consumiriam da fila para executar OCR, NLP e regras, atualizando o status no banco de dados sem bloquear a interface do usuário.

### 6.2 Desenho do Fluxo de Trabalho (Workflow)

O processo ponta-a-ponta, desde o upload até a validação, seguiria estes passos:

1. **Upload:** Aluno acessa o sistema via interface web, seleciona o curso e faz upload do arquivo do certificado (PDF ou imagem).
2. **Recepção e Enfileiramento:** O backend recebe o arquivo, salva-o no Armazenamento de Documentos, registra metadados iniciais no Banco de Dados (aluno, arquivo, status "Pendente") e publica uma mensagem na Fila de Processamento contendo o ID do certificado.
3. **Processamento Assíncrono (Worker):**
   a.  Um worker consome a mensagem da fila.
   b.  Recupera o arquivo do Armazenamento de Documentos.
   c.  **Módulo OCR:** Aplica pré-processamento na imagem e extrai o texto bruto. Armazena o texto extraído no Banco de Dados.
   d.  **Módulo NLP:** Pré-processa o texto extraído. Aplica o modelo de classificação para determinar a(s) categoria(s) (PE, PA, PV, etc.). Opcionalmente, aplica NER para extrair carga horária, data, nome, etc. Armazena a classificação e entidades extraídas no Banco de Dados.
   e.  **Motor de Regras:** Recupera as regras do curso do aluno (horas totais, limites por categoria) do Banco de Dados. Valida a classificação, aplica limites, calcula horas creditadas (considerando regras de excesso/múltiplas classificações). Armazena as horas validadas e o status final (ex: "Validado Automaticamente", "Requer Revisão") no Banco de Dados.
4. **Notificação/Exibição:** O sistema atualiza a interface do aluno mostrando o status final. Se o status for "Requer Revisão" (devido a baixa confiança do modelo, erro de OCR, regra complexa), notifica o professor responsável.
5. **Revisão do Professor (se necessário):** Professor acessa interface de revisão, visualiza o certificado, o texto extraído, a classificação automática e as horas calculadas. Pode confirmar a validação ou realizar um override manual da classificação e/ou horas. A decisão final é registrada no Banco de Dados.

### 6.3 Estratégias de Integração

A comunicação entre os módulos pode ser feita através de chamadas de API (RESTful) se forem serviços separados, ou chamadas de função direta se estiverem no mesmo processo/aplicação. O uso de uma fila de mensagens para orquestrar o processamento assíncrono desacopla os componentes e melhora a resiliência e escalabilidade. Padrões de integração comuns em processamento de documentos podem ser adaptados.^35^

### 6.4 Técnicas para Extração de Informação Robusta a Variações de Layout

Um desafio chave é que a simples extração de texto OCR e análise NLP sequencial ignora a rica informação visual e de layout presente nos certificados.^42^ A posição de um texto (ex: "Carga Horária: 60h") é semanticamente importante. Para criar um sistema mais robusto e menos dependente de formatos fixos, podem ser consideradas técnicas mais avançadas:

* **Abordagens Baseadas em Templates/Regras:** Definir manualmente templates para layouts conhecidos ^45^ ou regras baseadas em palavras-chave e proximidade espacial (ex: encontrar o número próximo à palavra "horas").^50^ São frágeis e não generalizam bem para layouts imprevistos.
* **Redes Neurais de Grafos (GNNs):** Modelar o documento como um grafo onde nós são blocos de texto (com suas coordenadas) e arestas representam relações espaciais.^43^ GNNs podem aprender a interpretar essa estrutura espacial.
* **Modelos Multimodais (Layout-Aware):** Esta é a abordagem estado da arte. Modelos como LayoutLM, LayoutLMv2, LayoutXLM ^77^, LiLT e outros são pré-treinados considerando simultaneamente o  **texto** , a **posição** (bounding boxes) e, em alguns casos, as **características visuais** da imagem do documento.^43^ Eles podem ser fine-tuned para tarefas como classificação de documentos ou Extração de Informação Chave (KIE), sendo inerentemente mais robustos a variações de layout, pois aprendem a importância da disposição espacial dos elementos.^79^ Muitos desses modelos estão disponíveis na plataforma Hugging Face Transformers.^68^

A complexidade do Motor de Regras, especialmente se as regras de excesso de horas e múltiplas classificações forem confirmadas como complexas e não triviais [Insight 1], terá um impacto direto na arquitetura. Um motor de regras mais sofisticado pode exigir um design mais elaborado, talvez até o uso de bibliotecas dedicadas a regras de negócio, e demandará interações mais complexas com o banco de dados e os módulos de IA.

Além disso, a natureza semi-estruturada dos certificados, onde o layout carrega significado, sugere fortemente que uma abordagem puramente textual (OCR -> Classificador de Texto) pode ser insuficiente para alta robustez.^43^ Incorporar informações de layout, seja através de heurísticas, GNNs ou, idealmente, modelos multimodais ^43^, é provavelmente necessário para um desempenho superior, embora aumente a complexidade técnica do projeto [Insight 12]. A escolha da abordagem deve balancear a robustez desejada com a viabilidade de implementação no contexto de um projeto estudantil.

## 7. Navegando pelos Desafios de Implementação

O desenvolvimento de um sistema como o proposto envolve diversos desafios técnicos e práticos que precisam ser antecipados e gerenciados.

### 7.1 Mitigação de Erros de OCR

* **Problema:** A precisão do OCR é a base de todo o sistema. Conforme discutido (Seção 4.1), a qualidade variável das imagens de certificados (escaneamentos ruins, fotos, layouts diversos, fontes não padrão) é a principal causa de erros na extração de texto.^37^ Erros de OCR propagam-se para o NLP, levando a classificações incorretas.
* **Soluções:**
  * **Pipeline de Pré-processamento Robusto:** Implementar as técnicas de pré-processamento identificadas (alta resolução, deskewing, binarização, redução de ruído, etc.) é essencial.^37^ A biblioteca OpenCV é uma ferramenta poderosa para isso.
  * **Seleção Cuidadosa da Ferramenta OCR:** Optar por motores OCR baseados em deep learning (como EasyOCR ^33^) ou serviços de nuvem especializados (como AWS Textract ^53^) pode oferecer maior robustez a variações de qualidade em comparação com abordagens mais antigas.^48^ Avaliar diferentes ferramentas no contexto específico de certificados do IFMS é recomendado.
  * **Feedback de Confiança:** Muitos motores OCR fornecem um score de confiança para o texto reconhecido. O sistema pode usar esse score para sinalizar extrações de baixa confiança, direcionando-as automaticamente para revisão manual por um professor.
  * **Orientação ao Usuário:** Instruir os alunos sobre as melhores práticas para digitalizar ou fotografar certificados (boa iluminação, alinhamento, resolução mínima) pode melhorar a qualidade dos documentos de entrada.^37^

### 7.2 Tratamento de Ambiguidade na Classificação

* **Problema:** Mesmo com texto OCR preciso, a classificação NLP pode ser ambígua. Algumas atividades podem genuinamente pertencer a mais de uma categoria do IFMS (como a similaridade entre PA e PV mencionada pelo usuário). Além disso, o modelo de ML pode gerar classificações com baixa probabilidade (baixa confiança) ou simplesmente errar em casos complexos ou limítrofes.
* **Soluções:**
  * **Limiar de Confiança e Revisão Humana:** Implementar um limiar de confiança para a classificação automática. Se a confiança do modelo estiver abaixo do limiar, o certificado deve ser automaticamente encaminhado para revisão manual pelo professor responsável.
  * **Interface de Override:** Fornecer uma interface clara e eficiente para que os professores possam revisar as classificações automáticas e corrigi-las (fazer um override) quando necessário.
  * **Refinamento do Modelo:** Se certas ambiguidades são recorrentes, incluir exemplos mais claros e distintos dessas categorias no dataset de fine-tuning pode ajudar o modelo a aprender a diferenciação.
  * **Uso de NER como Contexto:** Extrair entidades como nome do evento, instituição promotora e datas via NER [Insight 8] pode fornecer contexto adicional para desambiguar a classificação principal.
  * **Clarificação das Regras:** Confirmar as regras oficiais do IFMS para casos de múltiplas classificações é fundamental [Insight 1]. O sistema deve implementar a regra oficial (escolha única pelo aluno ou divisão permitida).

### 7.3 Gerenciamento de Casos Especiais (Edge Cases)

O sistema deve ser projetado para lidar com situações menos comuns, como certificados muito antigos com formatos obsoletos, documentos emitidos por instituições estrangeiras (se aplicável e permitido), certificados com pouquíssimo texto descritivo, ou documentos que não são certificados válidos. Estratégias incluem ter uma categoria "Não Classificado" ou "Requer Análise Manual" e garantir que o fluxo de revisão do professor possa tratar essas exceções.

### 7.4 Escalabilidade

Embora o projeto possa começar focado no Campus Naviraí, é prudente considerar a escalabilidade futura. O número de alunos e cursos pode aumentar. A arquitetura deve ser pensada para suportar um volume maior de uploads e processamento. O uso de processamento assíncrono (filas), bancos de dados eficientes e, potencialmente, serviços de nuvem para OCR/NLP ou armazenamento ^48^ pode facilitar a escalabilidade.

### 7.5 Segurança e Privacidade

Certificados contêm dados pessoais dos alunos (nome, CPF talvez, detalhes de atividades). O sistema deve:

* **Proteger os Dados:** Implementar armazenamento seguro para os arquivos de certificados e os dados extraídos, utilizando criptografia em repouso e em trânsito.^37^
* **Controlar Acessos:** Garantir que apenas usuários autorizados (o próprio aluno, professores/coordenadores responsáveis) possam acessar os dados relevantes.
* **Conformidade Legal:** Estar em conformidade com as leis de proteção de dados aplicáveis no Brasil (como a LGPD - Lei Geral de Proteção de Dados).

### 7.6 Experiência do Usuário (UX)

Um sistema tecnicamente perfeito pode falhar se for difícil de usar. É importante projetar interfaces intuitivas para:

* **Alunos:** Processo de upload simples, feedback claro sobre o status do processamento (pendente, processando, validado, requer revisão, erro), visualização fácil das horas acumuladas e validadas.
* **Professores:** Interface eficiente para revisar certificados sinalizados, visualizar a classificação automática e os dados extraídos, e realizar overrides de forma rápida e clara.

Dada a complexidade inerente ao processamento de documentos do mundo real com IA, onde a perfeição é inatingível devido a erros de OCR ^37^ e ambiguidades de classificação, torna-se evidente que um sistema 100% automatizado é irrealista neste contexto. A validação de atividades complementares tem impacto direto nos créditos acadêmicos dos alunos. Portanto, um mecanismo de  **"human-in-the-loop"** , onde um professor pode revisar e corrigir as decisões do sistema, não é apenas desejável, mas **essencial** para a confiabilidade e aceitação da ferramenta [Insight 13]. O design do fluxo de trabalho e das interfaces de revisão para o professor é tão importante quanto a precisão dos modelos de IA.

Além disso, as incertezas identificadas (regras complexas, tratamento de atividades em grupo, desempenho real do OCR/NLP em certificados do IFMS) e a necessidade de feedback dos usuários finais (alunos e professores) apontam para a inadequação de um modelo de desenvolvimento em cascata (waterfall). Uma  **abordagem iterativa** , começando com funcionalidades básicas (MVP) e refinando o sistema em ciclos curtos com base em testes e feedback contínuo, é muito mais apropriada e reduz os riscos do projeto [Insight 14].

## 8. Recomendações e Próximos Passos Estratégicos

Com base na análise abrangente dos regulamentos, tecnologias, sistemas existentes e desafios, as seguintes recomendações e próximos passos são sugeridos para guiar o desenvolvimento do sistema de gerenciamento de certificados no IFMS Naviraí.

### 8.1 Sugestões de Pilha Tecnológica (Technology Stack)

Considerando o forte ecossistema de IA/ML e a familiaridade comum em ambientes acadêmicos, recomenda-se:

* **Linguagem de Backend:**  **Python** , devido à vasta disponibilidade de bibliotecas para OCR, NLP, ML e desenvolvimento web.
* **Framework Web:** **Flask** ou  **Django** , ambos frameworks Python populares e robustos para construir a API do backend e a interface web.
* **OCR:**
  * **Recomendação Principal:** Iniciar com  **EasyOCR** .^33^ Sua combinação de API amigável, instalação simples e performance baseada em deep learning a torna um excelente ponto de partida para lidar com a variedade de certificados.
  * **Alternativa/Comparação:** Manter **Tesseract** (via `pytesseract`) ^47^ como uma alternativa viável, especialmente se combinado com um pipeline de pré-processamento robusto usando  **OpenCV** . A capacidade do Tesseract de fornecer bounding boxes pode ser útil para abordagens baseadas em layout.
* **NLP / Classificação:**
  * **Recomendação Principal:** Utilizar a biblioteca **Hugging Face Transformers** ^68^ para **fine-tuning** de um modelo de linguagem pré-treinado para português (ex: `neuralmind/bert-base-portuguese-cased` ^65^, `PORTULAN/albertina-900m-portuguese-ptbr-encoder` ^66^) na tarefa específica de classificação de certificados.^58^ Esta abordagem oferece o maior potencial de precisão semântica.
  * **Pré-processamento e NER (Opcional):** Usar **spaCy** ^55^ para as etapas de pré-processamento de texto (tokenização, lematização) antes de alimentar o modelo Transformer e, se a extração de entidades for implementada, spaCy oferece uma solução eficiente e fácil de usar para NER.
* **Banco de Dados:** **PostgreSQL** ou  **MySQL** , bancos de dados relacionais open-source, maduros e adequados para armazenar os dados estruturados do sistema.
* **Armazenamento de Documentos:** Começar com o **sistema de arquivos local** para simplicidade durante o desenvolvimento. Para implantação e escalabilidade, considerar serviços de **armazenamento em nuvem** como AWS S3 ou Google Cloud Storage.

### 8.2 Considerações Chave de Design

* **Flexibilidade do Motor de Regras:** O componente que aplica as regras do IFMS (limites, excesso, etc.) deve ser projetado para ser  **configurável e facilmente atualizável** , preferencialmente através de parâmetros no banco de dados ou arquivos de configuração, em vez de regras hardcoded no código. Isso é crucial dada a necessidade de confirmação de algumas regras e a possibilidade de mudanças futuras nos regulamentos [Insight 11].
* **Priorizar Pré-processamento de Imagem:** Investir tempo e esforço na construção de um pipeline de pré-processamento de imagem eficaz antes do OCR é fundamental para a qualidade do texto extraído e, consequentemente, para todo o sistema.^37^
* **Workflow Human-in-the-Loop:** Projetar cuidadosamente a interface e o fluxo de trabalho para a revisão e override por parte dos professores. Tornar este processo o mais eficiente e intuitivo possível é vital para a adoção do sistema [Insight 13].
* **Consciência do Layout (Layout Awareness):** Embora possa adicionar complexidade, explorar formas de incorporar informações de layout na análise (mesmo que inicialmente com heurísticas simples baseadas em coordenadas OCR ou palavras-chave ^50^) provavelmente levará a um sistema mais robusto. Planejar a possível integração futura de modelos multimodais (como os da Hugging Face ^43^) é uma boa estratégia [Insight 12].
* **Tratamento Explícito de Atividades em Grupo:** A funcionalidade para lidar com certificados de atividades em grupo precisa ser explicitamente projetada. Dada a falta de regras claras, uma abordagem inicial pragmática pode ser a alocação manual de horas pelo professor através da interface de revisão, até que uma política oficial seja definida ou uma solução automatizada (se viável) seja desenvolvida [Insight 5].

### 8.3 Áreas para Investigação Adicional

Antes ou durante as fases iniciais de desenvolvimento, é crucial investigar:

* **Confirmação das Regras do IFMS Naviraí:** **Prioridade máxima.** O aluno deve buscar confirmação oficial junto à coordenação do curso ou setor administrativo relevante sobre:
  * A política exata para tratamento de horas que excedem o limite de uma categoria (são desconsideradas, redistribuídas, ou outra regra?).
  * Se uma única atividade/certificado pode ser usada para pontuar em múltiplas categorias simultaneamente.
  * Como as horas de um certificado emitido para uma atividade em grupo devem ser divididas entre os participantes.
* **Aquisição de Dados para Treinamento/Teste:** Investigar a viabilidade de obter um conjunto, mesmo que pequeno, de certificados reais (anonimizados, se necessário) do IFMS Naviraí para realizar o fine-tuning do modelo de classificação e para testes realistas.
* **Modelos Multimodais:** Pesquisar implementações específicas de modelos que consideram layout (ex: LayoutLM, LiLT na biblioteca Transformers) e avaliar a complexidade de fine-tuning e integração no contexto do projeto.
* **Sistemas Internos do IFMS:** Verificar se o IFMS (Campus Naviraí ou institucionalmente) utiliza alguma plataforma ou sistema (além do possível SIGAA/SUAP) com o qual este novo sistema poderia ou deveria se integrar.

### 8.4 Abordagem de Desenvolvimento por Fases (MVP)

Recomenda-se fortemente uma abordagem iterativa, começando com um Produto Mínimo Viável (MVP) e evoluindo a partir daí:

1. **Fase 1 (MVP Core):**
   * Interface básica de upload para alunos e login para professores.
   * Armazenamento simples de arquivos e metadados básicos no banco de dados.
   * Integração de uma biblioteca OCR (ex: EasyOCR) com pré-processamento básico.
   * Implementação de um classificador de texto inicial (pode ser baseado em regras simples ou fine-tuning de um modelo Transformer com dados limitados/genéricos).
   * Implementação das regras do IFMS que foram **claramente confirmadas** (ex: carga horária total por curso, limites simples por categoria, se confirmados).
   * Interface básica para o professor visualizar o certificado, o texto OCRizado, a classificação automática e **aprovar/rejeitar** a validação (sem override complexo ainda).
2. **Fases Subsequentes (Iterações):**
   * Melhorar pipeline de pré-processamento de imagem com base em testes.
   * Refinar o modelo de classificação NLP com mais dados (se obtidos) ou técnicas mais avançadas.
   * Implementar regras de negócio mais complexas (excesso de horas, múltiplas classificações, grupos)  **após confirmação oficial** .
   * Desenvolver funcionalidades de override mais detalhadas para professores.
   * Implementar extração de entidades (NER) para validação e dados estruturados.
   * Aprimorar a interface do usuário (UX) com base no feedback de alunos e professores.
   * Investigar e potencialmente integrar abordagens layout-aware.
   * Otimizar performance e escalabilidade.

Esta abordagem permite entregar valor mais cedo, obter feedback real e ajustar o desenvolvimento com base nos desafios encontrados e nas regras confirmadas, reduzindo os riscos inerentes a um projeto com componentes de IA e requisitos regulatórios específicos.
