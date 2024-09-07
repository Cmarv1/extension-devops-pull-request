# Use o modelo OpenAI GPT para revisar solicitações pull para Azure Devops
Task do Azure DevOps que adiciona comentários em portugues nas solicitações de PullRequest com a ajuda do ChatGPT.

## Instalação
A instalação da task pode ser feita usando o [Visual Studio MarketPlace](https://marketplace.visualstudio.com/publishers/jpcompcombr).

## Serviço Azure Open AI
A formatação do endpoint é a seguinte: https://{XXXXXXXX}.openai.azure.com/openai/deployments/{MODEL_NAME}/chat/completions?api-version={API_VERSION}

[Documentação API REST](https://learn.microsoft.com/pt-br/azure/ai-services/openai/reference).

### Dê permissão ao agente de serviço de build
Antes de usar esta task, certifique-se de que o serviço de build tenha permissões para contribuir em seu REPOSITORIO:

![contribute_to_pr](https://github.com/jpitapeva/extensao-devops-pull-request/blob/main/images/contribute_to_pr.png?raw=true)

### Permitir que a tarefa acesse o token do sistema
Adicione uma seção de checkout com persistCredentials definido como true.

#### Pipelines Yaml
```yaml
jobs:
- job:
  displayName: "JPCompcombr code review"
  pool:
    vmImage: ubuntu-latest 
 
  steps:
  - checkout: self
    persistCredentials: true

  - task: JPCompcombr@22
    displayName: GPTPullRequestReview
    inputs:
      api_key: 'YOUR_TOKEN'
      model: 'gpt-4'
      aoi_endpoint: 'https://{XXXXXXXX}.azure.com/openai/deployments/{MODEL_NAME}/chat/completions?api-version={API_VERSION}'
      aoi_tokenMax: 1000
      aoi_temperature: 0
      use_https: true
      file_excludes: 'file1.js,file2.py,secret.txt,*.csproj,src/**/*.csproj'
      additional_prompts: 'Prompt separado por virula, exemplo: corrija a nomenclatura de variaveis, garanta identacao consistente, revise a abordagem de tratamento de erros'
```

## License
[MIT](https://raw.githubusercontent.com/mlarhrouch/azure-pipeline-gpt-pr-review/main/LICENSE)

## Developer plugin Steps</br>
Into folder project, run command:  ```npm run build``` </br>
Bump version in vss-extension.json and task.json</br>
Run command for generate new package: ```npx tfx-cli extension create```</br>
Upload extension to marketplace: https://marketplace.visualstudio.com/manage/a</br>

## GPT (transformador pré-treinado gerativo)

## O que é a engenharia de prompts
Os modelos de IA generativa são treinados em grandes quantidades de dados e podem gerar texto, imagens, código e conteúdo criativo com base na continuação mais provável do prompt.

Engenharia de prompt é o processo de criação e otimização de prompts para utilizar melhor os modelos de IA. A criação de prompts eficazes é fundamental para o sucesso da engenharia de prompt e pode aprimorar significativamente o desempenho do modelo de IA em tarefas específicas. Fornecer prompts relevantes, específicos, inequívocos e bem estruturados pode ajudar o modelo a entender melhor o contexto e gerar respostas mais precisas.

Por exemplo, se quisermos que um modelo de OpenAI gere descrições de produto, poderemos fornecer uma descrição detalhada que descreve os recursos e os benefícios do produto. Quando esse contexto é fornecido, o modelo pode gerar descrições de produto mais precisas e relevantes.

A engenharia de prompt também pode ajudar a reduzir o viés e a aumentar a imparcialidade em modelos de IA. Ao criar prompts diversos e inclusivos, podemos garantir que o modelo não seja tendencioso em relação a um determinado grupo ou perspectiva.

## Curl
```
curl https://YOUR_ENDPOINT_NAME.openai.azure.com/openai/deployments/YOUR_DEPLOYMENT_NAME/chat/completions?api-version=2023-03-15-preview \
  -H "Content-Type: application/json" \
  -H "api-key: YOUR_API_KEY" \
  -d '{"messages":[{"role": "system", "content": "You are a helpful assistant, teaching people about AI."},
{"role": "user", "content": "Does Azure OpenAI support multiple languages?"},
{"role": "assistant", "content": "Yes, Azure OpenAI supports several languages, and can translate between them."},
{"role": "user", "content": "Do other Azure AI Services support translation too?"}]}'
```


### Author: Joao Paulo Moreira Antunes (jpitapeva@hotmail.com)</br>

