# Pipeline e IaC – Equipe: Grupo 2
**Integrantes e papéis:**
- Arthur Moreira dos Santos – Relator
- Arthur Miguel – Pipeline
- José Rolim – IAC
- Kayke – Coordenador

## Evidências
   **1. Print da execução verde (jobs testar e infraestrutura)**
   <img width="945" height="265" alt="image" src="https://github.com/user-attachments/assets/f2278935-a46b-4f1f-9c70-31bce2ca2a7d" />

   **2. Print do passo terraform plan com a linha &quot;Plan: 1 to add&quot;**
   <img width="945" height="749" alt="image" src="https://github.com/user-attachments/assets/55a7ac03-2fdf-49de-b71f-65b1b80de7d2" />

   **3. Print do pull request com o teste falhando (vermelho)**
   <img width="945" height="422" alt="image" src="https://github.com/user-attachments/assets/d93cab2b-e180-4e46-96e0-8f2e9cb552b3" />

   **4. Print do pull request corrigido (verde) e do merge**
   <img width="922" height="553" alt="image" src="https://github.com/user-attachments/assets/a9a7b05a-09ba-4971-8e7a-1e156acfff4c" />

## Respostas
**Pergunta 1:** O primeiro teste verifica se sem desconto, o resultado será igual a 30, o segundo teste vê se o desconto funciona de forma correta, onde deve ser igual a 180. Se a pessoa trocar a fórmula, algum dos testes irá falhar (mudando os sinais de multiplicação, ambos irão falhar, mas se remover o sinal de menos por um sinal de mais, não altera o teste sem desconto, apenas o teste COM desconto)

**Pergunta 2:** A linha de gatilho é a 2 linha (comando on:), a cada push na branch main ou pull-request feita. O Job é "testar", seguido dos passos: "uses: actions/checkout@v4" (Usado para baixar o código do repositório), "uses: actions/setup-python@v5" (Usado para baixar o Python, com o with indicando a versão), "run: pip install pytest" (Baixa o pytest), "run: pytest" (Usado para iniciar o pytest)

**Pergunta 3:**  Ele é declarativo, pois declara apenas o resultado final que deseja e o Terraform descobre como criá-lo. Utiliza-se do local_file para criar o arquivo ambiente.txt (com nome do grupo e o ambiente "homologacao"), além da saída com o caminho do arquivo, sem precisarmos programar o passo a passo de como o sistema vai criar e gravar esse arquivo no disco

**Pergunta 4:** Antes do apply, o terraform plan mostrou que criaria o local_file.ambiente, utilizando-se da linha Plan: 1 to add, 0 to change, 0 to destroy. Já o arquivo baixado contém apenas as duas linhas que contem o Ambiente: homologacao e Equipe: Grupo2.

**Pergunta 5:** O erro acontece no job testar, porque o teste deu 220 em vez de 180 (calculo tinha que ser == a 180) após trocar os sinais. A criação da infraestrutura nem acontece, pois o programa PRECISA que a linha testar funcione (podemos colocar a prova pela linha needs: testar), caso não, ele simplesmente dá um "skip", assim podemos provar que a regra de "Falhou, parou" realmente funcionou.

## O que aprendemos
O uso do pipeline elimina a necessidade de rodar testes de forma manual a cada modificação, ou seja, qualquer commit ou pull request acaba sendo verificado pelo github actions.
A infraestrutura do código IAC no terraform nos mostra que descrever o ambiente em um arquivo padroniza a criação de recursos/logs e evita nossos erros manuais.
A necessidade do "needs: testar" (condição que impede progressão caso não cumprida) mostra na prática a regra do "falhou, parou", o que impede automaticamente que o erro da fórmula da loja prosseguisse para a alteração do ambiente da equipe. 
No fim, automatizar esses processos deixa o trabalho muito mais ileso de erros, rápido e confiável do que ter que ver linha por linha manualmente, gerando erros e perda de tempo em correções atrás de correções.
