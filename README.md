# Documentação dos comandos GIT: rebase, cherry pick, revert e squash

## Sumário:
1. ### [git rebase](#git-rebase-1)
    - [Funcionamento](#funcionamento)
    - [Sintaxe](#sintaxe)
    - [Aplicação](#aplicação)

2. ### [git cherry pick](#git-cherry-pick-1)
    * [Funcionamento](#funcionamento-1)
    - [Sintaxe](#sintaxe-1)
    - [Aplicação](#aplicac3a7c3a3o-1)

3. ### [git revert](#git-revert-1) 
    - [Funcionamento](#funcionamento-2)
    - [Sintaxe](#sintaxe-2)
    - [Aplicação](#aplicação-algumas-opções)

4. ### [git squash](#git-squash-1)
    - [Funcionamento](#funcionamento-3)
    - [Sintaxe](#sintaxe-3)
    - [Aplicação](#aplicac3a7c3a3o-2)

<br>

# git rebase

### Funcionamento
Tem a função de integrar dois branchs em um, refazendo o histórico de commits, movendo de uma ramificação para outro ponto de uma árvore.

### Sintaxe
git rebase <base>

### Aplicação
git rebase main

### Referência
https://git-scm.com/docs/git-rebase
documentação da aula de Git parte 2, PDF disponibilizado no Tech Up Challenge

<br>

# git cherry pick

### Funcionamento

#### O git cherry-pick funciona copiando o conteúdo do commit selecionado e aplicando-o ao branch atual. Ele cria um novo commit com as mesmas alterações do commit original, mas com um novo hash de commit.

#### Dado um ou mais commits existentes, aplique a mudança que cada um introduz, registrando um novo commit para cada um. Isso requer que sua árvore de trabalho esteja limpa (sem modificações do commit HEAD).

#### Quando não é óbvio como aplicar uma mudança, acontece o seguinte:
1. A ramificação atual e HEADo ponteiro permanecem no último commit feito com sucesso.
2. A CHERRY_PICK_HEADreferência é definida para apontar para o commit que introduziu a alteração que é difícil de aplicar.
3. Os caminhos nos quais a alteração foi aplicada corretamente são atualizados no arquivo de índice e na sua árvore de trabalho.
4. Para caminhos conflitantes, o arquivo de índice registra até três versões, conforme descrito na seção "TRUE MERGE" do git-merge[1] . Os arquivos da árvore de trabalho incluirão uma descrição do conflito entre colchetes pelos marcadores de conflito usuais <<<<<<<e >>>>>>>.
5. Nenhuma outra modificação é feita.

### Sintaxe

#### A sintaxe básica do comando git cherry-pick é a seguinte:
```
 git cherry-pick <commit>
 ```
 * commit: O hash do commit que você deseja aplicar.

```
 git cherry-pick [--edit] [-n] [-m <número-pai>] [-s] [-x] [--ff]
		  [-S[<keyid>]] <commit>…​
 ```
 ```
 git cherry-pick (--continue | --skip | --abort | --quit)
 ```

### Aplicação

```
 git cherry-pick master
 ```
* Aplique a alteração introduzida pelo commit na ponta do branch master e crie um novo commit com esta alteração.

```
git cherry-pick ..master
```
```
git cherry-pick ^HEAD master
```
* Aplique as alterações introduzidas por todos os commits que são ancestrais do master, mas não do HEAD para produzir novos commits.

```
git cherry-pick maint next ^master
```
```
git cherry-pick maint master..next
```
* Aplique as alterações introduzidas por todos os commits que são ancestrais de maint ou next, mas não master ou qualquer um de seus ancestrais. Observe que o último não significa mainte tudo entre mastere next; especificamente, maintnão será usado se estiver incluído em master.

```
git rev-list --reverse master -- README | git cherry-pick -n --stdin
```
* Aplique as alterações introduzidas por todos os commits no branch master que tocou em README na árvore de trabalho e no índice, para que o resultado possa ser inspecionado e transformado em um único novo commit, se adequado.

### Referência
#### [Documentação Git](https://git-scm.com/docs/git-cherry-pick)

<br>

# git revert

### Funcionamento

#### Dado um ou mais commits já existentes, reverta as alterações introduzidas pelos patches relacionados e registre alguns novos commits que registram neles. Isso requer que a sua árvore de trabalho esteja limpa (nenhuma alteração a partir do commit HEAD).


#### Nota: git revert é utilizado para registrar alguns commits novos para reverter o efeito de alguns commits anteriores (geralmente apenas um com problema). 
### Sintaxe

```
git revert <commit> (commit: O hash do commit que você deseja aplicar)
```
* Commits que serão revertidos.

```
git revert -e ou git revert --edit
```
* Com esta opção, o comando git revert permitirá a edição da mensagem do commit antes de fazer a reversão do commit. Esta é a predefinição caso execute o comando em um terminal.

```
 git revert --abort
 ```
* Cancele a operação e retorne a condição pré-sequência.

### Aplicação (Algumas opções)
```
git revert HEAD~3
```
* Reverta as alterações informadas pelo quarto último commit no HEAD e crie um novo commit com as alterações revertidas.

```
git revert -n master~5..master~2
```
* Reverta as alterações feitas pelos commits do quinto último commit no master (incluso) para o terceiro último commit no master (incluso), porém não crie nenhum commit com as alterações revertidas. A reversão altera apenas a árvore de trabalho e o índice.


### Referência
#### [Documentação Git](https://git-scm.com/docs/git-revert/pt_BR)

<br>

# git squash

### Funcionamento
Após um tempo já desenvolvendo/construindo um recurso em uma ramificação, o histórico git de pequenas e idependentes confirmações acaba por sobrecarregar o histórico de construção do recurso. Sendo assim, para resolver este problema, existe o squash, ele basicamente combina os commits para garantir um histórico mais limpo usando squash.

- ### Onde pode ser usado:
  - Histórico de commits.
  - Branchs.

- ### Exemplo:

  - #### Commits:
    1. c98jo89
    2. c7820cm
    3. 8c1a12v
    
    <br> => 23ab609

### Sintaxe
 ```
 git rebase -i HEAD~{Quantidade}
 ```


| PARTE | FUNÇÃO |
|:------|-------:|
| rebase | Move commits para um novo commit base|
| -i | Interativo, ou seja, trará um menú com opções que auxiliam na junção dos commits ou branchs |
| HEAD~ | Referencia aos commits referentes ao atual |
| {Quantidade} | Quantidades de commits anteriores em que serão juntados | 

### Aplicação
+ Exemplos commits:
  + 28ddeb6
  + 28ddeb6

<br>

1. ```git rebase -i HEAD~3```

2. ![Alt text](image.png)

3. pick 28ddeb6 

4. squash 28ddeb6

5. esc + :wq

6. Editar mensagem da junção (OPCIONAL)

7. esc + :wq

Fim!