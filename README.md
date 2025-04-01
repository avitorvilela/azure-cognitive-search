# azure-cognitive-search
Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados

Aqui está a tradução e modulação do conteúdo solicitado:

---

**Explorando um índice de Pesquisa de IA do Azure (UI)**  
Vamos imaginar que você trabalha para a Fourth Coffee, uma cadeia nacional de cafés. Você foi solicitado a ajudar a construir uma solução de mineração de conhecimento que facilite a busca por insights sobre a experiência dos clientes. Você decide criar um índice de Pesquisa de IA do Azure usando dados extraídos de avaliações de clientes.

Neste laboratório, você irá:

- Criar recursos do Azure
- Extrair dados de uma fonte de dados
- Enriquecer os dados com habilidades de IA
- Usar o indexador do Azure no portal do Azure
- Consultar seu índice de pesquisa
- Revisar os resultados salvos em um Knowledge Store

**Recursos do Azure necessários**  
A solução que você criará para a Fourth Coffee requer os seguintes recursos em sua assinatura do Azure:

- Um recurso de Pesquisa de IA do Azure, que gerenciará o indexamento e as consultas.
- Um recurso de serviços de IA do Azure, que oferece serviços de IA para habilidades que sua solução de pesquisa pode usar para enriquecer os dados na fonte de dados com insights gerados por IA.

**Observação:** Seus recursos de Pesquisa de IA do Azure e de serviços de IA do Azure devem estar na mesma região!

- Uma conta de Armazenamento com contêineres de blobs, que armazenará documentos brutos e outras coleções de tabelas, objetos ou arquivos.

**Criar um recurso de Pesquisa de IA do Azure**  
Faça login no portal do Azure.

Clique no botão **+ Criar um recurso**, pesquise por **Pesquisa de IA do Azure** e crie um recurso de Pesquisa de IA do Azure com as seguintes configurações:

- **Assinatura:** Sua assinatura do Azure.
- **Grupo de recursos:** Selecione ou crie um grupo de recursos com um nome único.
- **Nome do serviço:** Um nome exclusivo.
- **Localização:** Escolha qualquer região disponível. Se for na região leste dos EUA, use “East US 2”.
- **Plano de preços:** Básico

Selecione **Revisar + criar** e, após ver a resposta **Validação bem-sucedida**, selecione **Criar**.

Após a conclusão da implantação, selecione **Ir para o recurso**. Na página de visão geral do **Pesquisa de IA do Azure**, você pode adicionar índices, importar dados e buscar os índices criados.

**Criar um recurso de serviços de IA do Azure**  
Você precisará provisionar um recurso de serviços de IA do Azure que esteja na mesma localização que o seu recurso de Pesquisa de IA do Azure. Sua solução de pesquisa usará este recurso para enriquecer os dados na fonte de dados com insights gerados por IA.

Volte à página inicial do portal do Azure. Clique no botão **+ Criar um recurso** e pesquise por **Serviços de IA do Azure**. Selecione criar um plano de **Serviços de IA do Azure**. Você será direcionado a uma página para criar um recurso de **Serviços de IA do Azure**. Configure-o com as seguintes opções:

- **Assinatura:** Sua assinatura do Azure.
- **Grupo de recursos:** O mesmo grupo de recursos do seu recurso de Pesquisa de IA do Azure.
- **Região:** A mesma localização do seu recurso de Pesquisa de IA do Azure.
- **Nome:** Um nome exclusivo.
- **Plano de preços:** Standard S0
- Marque a caixa **Eu reconheço que li e entendi todos os termos abaixo:** Selecionado.
- Selecione **Revisar + criar**. Após ver a resposta **Validação bem-sucedida**, selecione **Criar**.

Aguarde a conclusão da implantação e visualize os detalhes da implantação.

**Criar uma conta de armazenamento**  
Volte à página inicial do portal do Azure e, em seguida, clique no botão **+ Criar um recurso**.

Pesquise por **conta de armazenamento** e crie um recurso de **conta de armazenamento** com as seguintes configurações:

- **Assinatura:** Sua assinatura do Azure.
- **Grupo de recursos:** O mesmo grupo de recursos dos seus recursos de Pesquisa de IA do Azure e Serviços de IA do Azure.
- **Nome da conta de armazenamento:** Um nome único.
- **Localização:** Escolha qualquer local disponível.
- **Desempenho:** Padrão
- **Redundância:** Armazenamento localmente redundante (LRS)

Clique em **Revisar** e depois em **Criar**. Aguarde a conclusão da implantação e depois vá para o recurso implantado.

Na conta de armazenamento do Azure que você criou, no menu à esquerda, selecione **Configuração** (em **Configurações**).  
Alterar a configuração de **Permitir acesso anônimo a blobs** para **Ativado** e depois selecione **Salvar**.

**Carregar documentos no Armazenamento do Azure**  
No menu à esquerda, selecione **Contêineres**.

Clique em **+ Contêiner**. Um painel à direita será aberto.

Digite as seguintes configurações e clique em **Criar**:

- **Nome:** coffee-reviews
- **Nível de acesso público:** Contêiner (acesso de leitura anônima para contêineres e blobs)
- **Avançado:** sem alterações.

Em uma nova aba do navegador, baixe os **coffee reviews** compactados de [https://aka.ms/mslearn-coffee-reviews](https://aka.ms/mslearn-coffee-reviews) e extraia os arquivos para a pasta **reviews**.

No portal do Azure, selecione seu contêiner **coffee-reviews**. Dentro do contêiner, selecione **Carregar**.

No painel **Carregar blob**, selecione **Selecionar um arquivo**.

Na janela do Explorer, selecione todos os arquivos na pasta **reviews**, selecione **Abrir** e depois selecione **Carregar**.

Após o upload ser concluído, você pode fechar o painel **Carregar blob**. Seus documentos agora estão no seu contêiner de armazenamento **coffee-reviews**.

**Indexar os documentos**  
Após os documentos estarem no armazenamento, você pode usar a Pesquisa de IA do Azure para extrair insights dos documentos. O portal do Azure fornece um assistente de **Importar dados**. Com esse assistente, você pode criar automaticamente um índice e um indexador para fontes de dados compatíveis. Você usará o assistente para criar um índice e importar seus documentos de pesquisa para o índice de Pesquisa de IA do Azure.

No portal do Azure, vá até o seu recurso de Pesquisa de IA do Azure. Na página **Visão Geral**, selecione **Importar dados**.

Na página **Conectar-se aos seus dados**, na lista **Fonte de dados**, selecione **Armazenamento de Blobs do Azure**. Complete os detalhes da fonte de dados com os seguintes valores:

- **Fonte de dados:** Armazenamento de Blobs do Azure
- **Nome da fonte de dados:** coffee-customer-data
- **Dados para extrair:** Conteúdo e metadados
- **Modo de análise:** Padrão
- **String de conexão:** *Selecione Escolher uma conexão existente. Selecione sua conta de armazenamento, selecione o contêiner **coffee-reviews** e depois clique em **Selecionar**.
- **Autenticação por identidade gerenciada:** Nenhuma
- **Nome do contêiner:** esta configuração será preenchida automaticamente após você escolher uma conexão existente.

Selecione **Próximo: Adicionar habilidades cognitivas (Opcional)**.

Na seção **Anexar Serviços de IA**, selecione seu recurso de **Serviços de IA do Azure**.

Na seção **Adicionar enriquecimentos**:
- Alterar o nome do **Skillset** para **coffee-skillset**.
- Selecione a caixa **Ativar OCR e mesclar todo o texto no campo merged_content**.

Certifique-se de que o campo **Fonte de dados** esteja configurado como **merged_content**.  
Altere o nível de granularidade do **Enriquecimento** para **Páginas** (fragmentos de 5000 caracteres).  
Não selecione **Ativar enriquecimento incremental**.

Selecione os seguintes campos enriquecidos:

- **Habilidade Cognitiva**:  
  - **Extrair nomes de localizações** → **locations**
  - **Extrair frases-chave** → **keyphrases**
  - **Detectar sentimento** → **sentiment**
  - **Gerar tags de imagens** → **imageTags**
  - **Gerar legendas de imagens** → **imageCaption**

Em **Salvar enriquecimentos para um Knowledge Store**, selecione:

- Projeções de imagens
- Documentos
- Páginas
- Frases-chave
- Entidades
- Detalhes da imagem
- Referências de imagem

Após isso, siga com a configuração para criar o indexador, executar as consultas e revisar os resultados.
