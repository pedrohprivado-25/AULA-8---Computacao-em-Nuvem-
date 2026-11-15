# Aula 08 – Computação em Nuvem | Saúde e observação de aplicações no Kubernetes

## Sobre a aula

Nesta aula, aprendemos sobre a saúde e o acompanhamento das aplicações dentro do Kubernetes.

Na aula anterior, estudamos como controlar o acesso aos recursos utilizando Secrets e RBAC. Já nesta aula, o foco foi entender como acompanhar o funcionamento de uma aplicação e identificar quando ela está funcionando, mas ainda não está pronta para receber acessos.

A professora apresentou três pontos importantes para acompanhar uma aplicação: **estado do Pod, eventos e logs**. Também estudamos duas verificações chamadas Liveness Probe e Readiness Probe.

O conteúdo foi apresentado no slide da professora e utilizado como base para realizar as atividades práticas registradas neste repositório.

## O que vimos na aula?

### 1. Estado, eventos e logs

Aprendemos que existem diferentes maneiras de acompanhar o que está acontecendo com uma aplicação no Kubernetes:

- **Estado (`kubectl get pods`):** permite verificar como o Pod está, se está pronto e se aconteceu algum reinício.
- **Eventos (`kubectl describe pod`):** mostram informações mais detalhadas e avisos sobre possíveis problemas.
- **Logs (`kubectl logs`):** mostram os registros da aplicação e ajudam a entender o que aconteceu.

A professora explicou que é importante primeiro identificar o problema e depois consultar os detalhes e os logs para entender sua causa.

### 2. Liveness Probe e Readiness Probe

Também aprendemos a diferença entre essas duas verificações:

- **Liveness Probe:** verifica se a aplicação continua funcionando. Quando identifica que ela deixou de funcionar, pode provocar o reinício do contêiner.
- **Readiness Probe:** verifica se a aplicação está pronta para receber acessos. Quando essa verificação falha, o Pod deixa de ser considerado pronto para receber tráfego por um Service.

Uma das partes mais importantes da aula foi entender que uma aplicação pode estar em execução e, mesmo assim, não estar pronta para receber solicitações.

### 3. Verificações HTTP

No exemplo apresentado pela professora, utilizamos dois caminhos de uma aplicação Nginx:

- `/`: utilizado pela Liveness Probe para verificar se o servidor continuava respondendo.
- `/health`: utilizado pela Readiness Probe para verificar se a aplicação estava pronta.

Também aprendemos que, nas verificações HTTP, respostas com códigos entre 200 e 399 são consideradas bem-sucedidas.

## O que fizemos nas atividades práticas?

Nas atividades práticas, utilizamos o Kubernetes Playground do Killercoda para trabalhar com um Pod chamado `web-saude`, dentro do namespace `aula8`.

O objetivo foi acompanhar o comportamento do Nginx e observar o que acontecia quando a verificação de prontidão estava funcionando e quando apresentava falhas.

As atividades registradas no GitHub foram:

1. **Preparação do ambiente:** verificamos os Nodes do cluster e criamos o namespace `aula8` para organizar os recursos da atividade.

2. **Criação do Pod:** criamos o arquivo `web-saude.yaml`, utilizando a imagem `nginx:alpine` e configurando as verificações Liveness Probe e Readiness Probe.

3. **Primeira verificação:** criamos o Pod e observamos que ele estava com o status `Running`, mas apresentava `READY 0/1`, porque o caminho `/health` ainda não existia.

4. **Criação do arquivo health:** criamos o arquivo dentro do diretório utilizado pelo Nginx e colocamos o conteúdo `OK`. Depois disso, o Pod passou para `READY 1/1`.

5. **Teste de acesso:** utilizamos o comando `wget` para acessar o endereço `/health` e confirmar que o arquivo estava respondendo corretamente.

6. **Remoção do arquivo:** removemos o arquivo `health` e observamos que o Pod voltou para `READY 0/1`, mas continuou com o status `Running` e sem reinicializações.

7. **Verificação dos logs:** consultamos os registros do Nginx e identificamos que o caminho `/` continuava respondendo com código 200, enquanto `/health` retornava 404.

8. **Limpeza do ambiente:** excluímos o namespace `aula8` e verificamos que ele não existia mais no cluster.

## Desafios da Aula 08

Além dos testes práticos, também registramos os desafios propostos pela professora.

Na comparação do experimento, organizamos os resultados em três situações:

- Antes de criar o arquivo `/health`: Pod em execução, mas não pronto para receber acessos.
- Com o arquivo `/health`: Pod em execução e pronto, com resposta `OK`.
- Após remover o arquivo: Pod continuou em execução, mas deixou de estar pronto novamente.

Também respondemos às perguntas sobre decisões de operação, explicando por que reiniciar o Pod não resolveria o problema, quais seriam os riscos de enviar tráfego para uma aplicação que não está pronta e quais comandos utilizaríamos para investigar uma falha.

## O que eu aprendi com essa aula?

O que mais me chamou a atenção foi entender que o status `Running` não significa necessariamente que a aplicação está pronta para receber acessos.

Durante a prática, consegui visualizar isso acontecendo. O Nginx continuava funcionando, mas a verificação de prontidão falhava porque o arquivo `/health` não existia.

Quando criamos o arquivo, o Pod passou para `1/1`. Depois, ao removê-lo, voltou para `0/1`, sem precisar reiniciar o contêiner.

Também entendi melhor a importância dos comandos `get`, `describe` e `logs`. Cada um apresenta informações diferentes e ajuda a identificar o que está acontecendo com a aplicação.

Essa atividade me ajudou a compreender melhor como o Kubernetes acompanha a saúde de uma aplicação e como podemos investigar problemas antes de tomar alguma decisão.

## Conclusão

A Aula 08 foi importante para entender melhor como acompanhar aplicações dentro do Kubernetes.

Com o conteúdo apresentado pela professora e os testes realizados na prática, consegui compreender a diferença entre uma aplicação estar funcionando e estar pronta para receber acessos.

Também aprendi a observar o estado do Pod, consultar eventos e verificar logs para encontrar possíveis problemas.

**Material registrado: Aula 08 – Computação em Nuvem | Verificações de Saúde no Kubernetes.**
