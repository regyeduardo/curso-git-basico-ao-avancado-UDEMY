# Git - Básico ao avançado (2021)

URL: https://www.udemy.com/course/git-basico-ao-avancado-2021/

Cria um repositório local

```bash
git init
```

Adiciona um repositório remoto ao repositório local

```bash
git remote add <nome-origin> <ORIGIN>
```

Verifica a URL de um repositório remoto

```bash
git remote get-url <nome-origin>
```

## Configurações iniciais

- A primeira coisa a ser feita e definir o nome e e-mail (que sera exibido nos commits)
- git config [—local | —global | —system] `<key>` [`<value>`]
  - — system aplica as configurações para todo repositório **de todos os usuários** no seu computador
  - —global aplica para todo repositório **do usuário corrente**
  - —local aplica para o repositório corrente
- As configurações serão aplicadas na seguinte ordem
  - Caso tenha local, prevalece local
  - Caso tenha global, prevalece global
  - Se não, prevalece a do sistema

Altera o nome que vai aparecer nos commits

```bash
git config --global user.name "<SEU NOME>"
```

Altera o email que vai aparecer nos commits

```bash
git config --global user.email <seu-email>
```

Retorna a configuração definida no repositório

```bash
git config user.name
git config user.email
```

## Pedindo ajuda

- Para acessar o manual de ajuda do Git voce pode considerar o seguinte comando:
- git `<verb>` —help

## Primeiro commit

- Acesse o diretório do projeto por linha de comando
- echo “hello world” > hello.txt

Adicionando um arquivo para que possa ser adicionado ao commit

```bash
git add hello.txt
```

Ou adicionar todos os arquivos e alterações

```bash
git add .
```

Criando um commit (todo commit necessita de uma mensagem)

```bash
git commit -m "SUA MENSAGEM"
```

## Git show

- Para visualizar as alterações envolvidas em um determinado commit, pode ser utilizado o comando git show (por padrão ira exibir as alterações do ultimo commit)

```bash
git show
```

Também e possível visualizar o intervalo de alterações entre commits, basta adicionar ... (três pontos) entre cada hash de commit

```bash
git show <commitN>...<commitM>
```

## Empurrando modificações para o servidor remoto

- git push
- Se tentar executar este comando ira ocorrer um erro, pois a branch master local ainda não possui uma branch de rastreamento

Criando uma branch de rastreamento

```bash
git push --set-upstream origin master
```

Caso de erro verifique se o local esta correto e mude a URL caso precise (eu mudei de ssh para HTTPS)

```bash
git remote set-url origin <"URL HTTPS">
```

## Git id

- git id e o nome de um objeto git
- git log
- git hist
- O comando git log mostrara strings de 40 caracteres como o que vemos aqui. Essa string e o nome de um objeto de commit (contigo em .git\objects)
- Git id’s são o que e conhecido como algoritmo de hash seguro um ou valores SHA-1. A string hexadecimal de 40 caracteres e o resultado de uma computação matemática baseada no conteúdo. Estatisticamente falando, o valor SHA-1 e exclusivo para um determinado conteúdo
- O mesmo conteúdo sempre resultara no mesmo valor SHA-1, mas e praticamente impossível encontrar dois arquivos de conteúdos diferentes que produzem o mesmo valor SHA-1
- Os valores SHA-1 são projetados para avalanche. **O que significa que pequenas mudanças no conteúdo, leva a uma grande diferença nos valores SHA-1**.
- Por isso podemos reduzir de 40 para 7 caracteres no comando criado git hist, que dificilmente existira outro commit com os mesmos 7 caracteres.

## Git objects

- Pensamos no GIT como um sistema que deve manter as imagens de arquivos e diretórios
- Dentro do repositório raiz do git podemos ter arquivos e diretórios. Diretórios que geralmente contem outros diretórios.
- No GIT o conteúdo dos arquivos são armazenados em objetos chamados **blobs**. (A diferença entre blobs e arquivos e que os arquivos também contem metadados. E blob não.)
- Diretórios são equivalentes a uma arvore (tree)
  - Uma arvore e basicamente uma lista de diretórios referindo-se a blobs, bem como a outras arvores.
- Um commit, e uma imagem (snapshot) desse sistema de arquivos em um determinado momento
- git hist

Retorna o tipo do objeto

```bash
git cat-file -t <Hash do commit>
# commit
```

Retorna o conteúdo com base nesse tipo e o identificador da arvore

```bash
git cat-file -p <Hash do commit>
# tree 9f78408dcd931da5a859c08278904f98b60d9f87
# author Regy Eduardo <regyeduardo@gmail.com> 1642614773 -0300
# committer Regy Eduardo <regyeduardo@gmail.com> 1642614773 -0300
```

```bash
git cat-file -t <Hash da arvore (retornada com git cat-file -p <Hash do commit>)>
# tree
```

```bash
git cat-file -p <Hash da arvore>
# 100644 blob 45ea4c41fc808328203b741acccc19d50149da19    hello.txt
```

```bash
git cat-file -t <Hash do blob>
# blob
```

```bash
git cat-file -p <Hash do blob>
# "Hello world"
```

```bash
mkdir docs
cd docs
echo "orientacoes" > orientacoes.txt
echo "pendencias" > pendencias.txt
git add .
git commit -m "creating docs"
```

```bash
git hist
# * 8985748 2022-01-19 | creating docs (HEAD -> master) [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt (origin/master) [Regy Eduardo]
```

```bash
git cat-file -p 8985748
# tree 59784a07a3651cdfe2ed636cc961fdab98828e0f
# parent 15da1cc22a99ad5d47221ffb819e864f8bd5ecf3
# author Regy Eduardo <regyeduardo@gmail.com> 1642618626 -0300
# committer Regy Eduardo <regyeduardo@gmail.com> 1642618626 -0300

# creating docs
```

```bash
git cat-file -p 59784a0
# 040000 tree fae732f1458f4a7abfbf1ea85c83c9db59603194    docs
# 100644 blob 45ea4c41fc808328203b741acccc19d50149da19    hello.txt
```

```bash
git cat-file -p fae732f
# 100644 blob 88204fe8f88d753b4f346fce5c91e4075c6cb590    orientacoes.txt
# 100644 blob 6e8605dde092bffa10469721f7c4f545f905abba    pendencias.txt
```

```bash
git cat-file -p 88204fe
# orientacoes
```

```bash
git cat-file -p 6e8605d
# pendencias
```

## Status dos arquivos

```bash
touch primeiroacesso.txt
```

```bash
git status
# On branch master
# Your branch is ahead of 'origin/master' by 1 commit.
#  (use "git push" to publish your local commits)

# Untracked files:
#  (use "git add <file>..." to include in what will be committed)
#        primeiroacesso.txt

# nothing added to commit but untracked files present (use "git add" to track)
```

```bash
git add .
```

```bash
git status
# On branch master
# Your branch is ahead of 'origin/master' by 1 commit.
#  (use "git push" to publish your local commits)

# Changes to be committed:
#  (use "git restore --staged <file>..." to unstage)
#        new file:   primeiroacesso.txt
```

```bash
echo "conteudo do arquivo" > primeiroacesso.txt
```

Caso fosse executado um git commit neste instante seria considerado somente o arquivo sem modificação que esta no staging

```bash
git status
# On branch master
# Your branch is ahead of 'origin/master' by 1 commit.
#   (use "git push" to publish your local commits)
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         new file:   primeiroacesso.txt
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
#         modified:   primeiroacesso.txt
```

```bash
git add .
```

```bash
git status
# On branch master
# Your branch is ahead of 'origin/master' by 1 commit.
#   (use "git push" to publish your local commits)
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         new file:   primeiroacesso.txt
```

```bash
git commit -m "creating primeiroacesso.txt"
```

```bash
git status
# Your branch is ahead of 'origin/master' by 2 commits.
#   (use "git push" to publish your local commits)
# nothing to commit, working tree clean
```

## Branches

- Em teoria, uma branch e uma ramificação de um projeto
- Na pratica, em GIT, uma branch e um ponteiro para um commit
- Veja que o conteúdo do arquivo master em \.git\refs\heads possui uma hash e essa hash e exatamente a hash do nosso ultimo commit ate o momento

```bash
cat .git/refs/heads/master
```

- Benefícios no Git
  - Rapido e fácil de ser criada:  1 arquivo que contem uma referencia
  - Permitem a experimentação: Os membros da equipe pode isolar seu trabalho para não afete os outros ate que o trabalho esteja pronto.
  - Suporte a varias versões: As ramificações permite suporte a varias versões do projeto simultaneamente
  - Permite processos de revisão de arquivo: As ramificações permitem facilmente incluir processos de revisão de arquivos

## Head

- A HEAD no GIT representa a versão que voce esta trabalhando no momento
- E através do HEAD que sabemos em qual branch estamos trabalhando

```bash
git hist
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (HEAD -> master, origin/master) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

## Checkout

- O checkout e utilizado principalmente para alternar entre versões
- git checkout [`<referencia>` | `<commit>`]
  - E possível utilizar referencias com a opção ~n, sendo n um inteiro qualquer. Por exemplo: git checkout `<branch>`~1, alterna para o penúltimo commit da branch em questão.
- git checkout -b <new_branch>
  - Cria uma nova branch e alterna para a mesma.

## Criando uma nova branch

- Vamos criar uma nova branch com o comando abaixo

Nome errado de proposito. Iremos corrigir mais a frente.

```bash
git checkout -b baner
```

Lista todas as branches (locais e rastreamento)

```bash
git branch -a
# * baner
#   master
#   remotes/origin/master
```

- Voce ira visualizar as branches de rastreamento em vermelho, branches locais em branco e o * em verde indica a branch corrente
- Note que a branch baner foi criada mas ainda não existe uma rastreamento correspondente

```bash
touch banner.txt
git add banner.txt
git commit -m "adding banner.txt"
```

```bash
git push
# fatal: The current branch baner has no upstream branch.
# To push the current branch and set the remote as upstream, use
#     git push --set-upstream origin baner
```

- Isso ocorreu pois como vimos anteriormente não ha uma branch de rastreamento que esteja rastreando nossa branch local
- Logo, para a primeira vez sera necessário executar o push conforme abaixo

```bash
git push --set-upstream origin baner # Opcao 1
git push -u origin baner # Opcao 2
```

```bash
git branch -a
# * baner
#   master
#   remotes/origin/baner
#   remotes/origin/master
```

- Note que a branch baner de rastreamento foi criada e ja esta rastreando baner local

## Nota: outras formas de criar uma branch

- Na aula anterior foi mostrado como criar uma branch utilizando o comando:

Esse comando cria uma nova branch e altera a referência HEAD para a mesma, tornando-a branch corrente.

```bash
git checkout –b <nome da nova branch>
```

- Porém há outras formas de criar uma branch, **sem necessariamente alternar para a recém criada**. Para criar uma nova branch a partir da branch **corrente**, você pode utilizar o comando:

```bash
git branch <nome da nova branch>
```

- Para criar uma nova branch a partir de uma branch **especifica** utilize o comando

```bash
git branch -c <nome da branch específica> <nome da nova branch>
```

## Renomear uma branch

- Repositório local

```bash
git branch -m "banner"
```

- Caso a branch que o nome sera alterado não seja a branch atual, execute:

```bash
git branch -m <Nome antigo> <Nome novo>
```

- Repositório remoto

Deleta a branch com o nome errado da plataforma de hospedagem.

```bash
git push origin :baner # opcao 1
git push origin --delete baner
# To https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git
#  - [deleted]         baner
```

```bash
git push -u origin banner
# Enumerating objects: 4, done.
# Counting objects: 100% (4/4), done.
# Delta compression using up to 4 threads
# Compressing objects: 100% (2/2), done.
# Writing objects: 100% (3/3), 350 bytes | 350.00 KiB/s, done.
# Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
# remote:
# remote: Create a pull request for 'banner' on GitHub by visiting:
# remote:      https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY/pull/new/banner
# remote:
# To https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git
#  * [new branch]      banner -> banner
# Branch 'banner' set up to track remote branch 'banner' from 'origin'.
```

## Tag

- As tags são etiquetas, geralmente são usadas para marcar lançamentos estáveis ou a conquista de marcas muito importantes
- Podem ajudar os usuários do repositório a navegar facilmente para as partes importantes do histórico de códigos, como pontos de liberação
- Branches x Tags
  - A area de trabalho e (quase sempre) associada a uma branch. Quando ocorre um commit a branch atual ira automaticamente atualizar a referencia para apontar para o novo commit; em outras palavras, branches são referencias mutáveis.
  - Uma tag, por outro lado, e criada para apontar para um commit especifico e, portanto, não muda, mesmo se a branch seguir em frente. Em outras palavras, tags são referencias imutáveis.
- O GIT oferece suporte a dois tipos de tags: leves e anotadas

### Tags anotadas

- Tags anotadas são aquelas que devem ser publicadas para outros contribuidores, provavelmente novas versões (que também devem ser assinadas). Nao apenas para ver quem marcou e quando foi marcado mas também o porque (geralmente um registro de alterações)

-a: indica que e uma tag anotada; -m: indica que o próximo parâmetro sera a descrição da tag

```bash
git tag -a "v1.0" -m "first version"
```

- Por padrão a tag sera adicionado no commit corrente (HEAD). Mas voce pode adicionar tags a qualquer outro commit do passado, basta adicionar a hash do commit

```bash
git hist
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (HEAD -> master, tag: v1.0, origin/master) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

- E possível visualizar as anotações de uma tag anotada com o git show

```bash
git show v1.0
# tag v1.0
# Tagger: Regy Eduardo <regyeduardo@gmail.com>
# Date:   Thu Jan 20 16:10:45 2022 -0300
# first version
# commit 98e5cad0ce86fc3b9264fd06af46f4e309afa765 (HEAD -> master, tag: v1.0, origin/master)
# Author: Regy Eduardo <regyeduardo@gmail.com>
# Date:   Thu Jan 20 14:49:20 2022 -0300
#     creating primeiroacesso.txt
# diff --git a/primeiroacesso.txt b/primeiroacesso.txt
# new file mode 100644
# index 0000000..4ebddeb
# --- /dev/null
# +++ b/primeiroacesso.txt
# @@ -0,0 +1 @@
# +conteudo do arquivo
```

### Tags leves

- As tags leves são mais apropriadas para uso **privado ou temporário,** o que significa marcar commits especiais para poder encontra-los novamente. Que seja para revê-los, verifica-los para testar alguma coisa ou qualquer outra coisa.
- Voce não deve definir -m e -a

```bash
git tag todo-primeiro-acesso
```

```bash
git hist
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (HEAD -> master, tag: v1.0, tag: todo-primeiro-acesso, origin/master) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

- Note que se for utilizado o git show abaixo não haverá anotações para essa tag

```bash
git show todo-primeiro-acesso
# commit 98e5cad0ce86fc3b9264fd06af46f4e309afa765 (HEAD -> master, tag: v1.0, tag: todo-primeiro-acesso, origin/master)
# Author: Regy Eduardo <regyeduardo@gmail.com>
# Date:   Thu Jan 20 14:49:20 2022 -0300
#     creating primeiroacesso.txt
# diff --git a/primeiroacesso.txt b/primeiroacesso.txt
# new file mode 100644
# index 0000000..4ebddeb
# --- /dev/null
# +++ b/primeiroacesso.txt
# @@ -0,0 +1 @@
# +conteudo do arquivo
```

- Por fim vamos deletar essa tag temporária da seguinte forma

```bash
git tag -d todo-primeiro-acesso
# Deleted tag 'todo-primeiro-acesso' (was 98e5cad)
```

## Empurrar as tags para o repositório remoto

- Agora vamos publicar todas as alterações feitas ate o momento para o Github

O push sem argumentos não envia as tags para o repositório remoto

```bash
git push
# Everything up-to-date
```

```bash
git push --tags
# Enumerating objects: 1, done.
# Counting objects: 100% (1/1), done.
# Writing objects: 100% (1/1), 166 bytes | 166.00 KiB/s, done.
# Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
# To https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git
#  * [new tag]         v1.0 -> v1.0
```

## Descartando mudanças locais

```bash
echo "oi" > hello.txt
```

```bash
git status
# On branch master
# Your branch is up to date with 'origin/master'.
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
#         modified:   hello.txt
# no changes added to commit (use "git add" and/or "git commit -a")
```

- Seguindo a orientação do próprio git para descartar as mudanças locais pode ser utilizado

```bash
git restore hello.txt
```

```bash
git status
# On branch master
# Your branch is up to date with 'origin/master'.
# nothing to commit, working tree clean
```

```bash
touch login.txt
```

```bash
git status
# On branch master
# Your branch is up to date with 'origin/master'.
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         login.txt
# nothing added to commit but untracked files present (use "git add" to track)
```

Este comando não tera efeito pois o novo arquivo não e rastreado pelo git (untracked)

```bash
git restore login.txt
# error: pathspec 'login.txt' did not match any file(s) known to git
```

- Para descartar neste caso utilize

```bash
git clean login.txt -f
# Removing login.txt
```

- Neste comando, se e desejável remover todos os arquivos não rastreados basta usar

```bash
git clean -f
```

- não sendo necessário trocar o nome do arquivo por ‘.’. Cuidado ao realizar esse comando! Se desejar visualizar todos os arquivos que serão apagados substitua -f por -n.

## Descartando mudanças do stage

```bash
echo “oi” > hello.txt
```

```bash
touch login.txt
```

```bash
git status
# On branch master
# Your branch is up to date with 'origin/master'.
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
#         modified:   hello.txt
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         login.txt
# no changes added to commit (use "git add" and/or "git commit -a")
```

```bash
git add .
```

```bash
git status
# On branch master
# Your branch is up to date with 'origin/master'.
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         modified:   hello.txt
#         new file:   login.txt
```

- Seguindo a orientação do próprio git para descartar as mudanças do stage use:

```bash
git restore --staged .
```

```bash
git status
# On branch master
# Your branch is up to date with 'origin/master'.
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
#         modified:   hello.txt
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         login.txt
# no changes added to commit (use "git add" and/or "git commit -a")
```

## Nota: exibir um arquivo em uma versão antiga

- Além de desfazer alterações, o comando restore também pode ser utilizado para restaurar versões anteriores em um arquivo.
- Para isso, você pode utilizar o seguinte comando:

```bash
git restore --source <hash do commit> <nome do arquivo>
```

## Revert de commit

- Vamos remover os arquivos não rastreados pelo git e somente “commitar” as alterações de hello.txt

```bash
git clean -f
```

```bash
git add .
```

```bash
git commit -m "changes in hello.txt"
```

```bash
git hist
```

- Vamos desfazer o ultimo commit

```bash
git revert HEAD --no-edit
# [master e680b94] Revert "changes in hello.txt"
#  Date: Thu Jan 20 17:51:10 2022 -0300
#  1 file changed, 1 insertion(+), 1 deletion(-)
```

- O git revert pode receber um id de commit ou qualquer referencia tag, branch ou HEAD. No caso acima foi revertido o commit corrente. Para reverter o penúltimo commit pode-se utilizar “git revert HEAD~1”.
- E interessante que o novo commit de uma reversão utilize a mensagem “revert `<mensagem do commit original>`”. O parâmetro —no-edit fara que o GIT não abra o editor de texto padrão

```bash
git hist
# * e680b94 2022-01-20 | Revert "changes in hello.txt" (HEAD -> master) [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0, origin/master) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo
```

## Reset de commit

- O comando git reset e uma ferramenta complexa e versátil para desfazer alterações
- Ha três tipos de reset: soft, mixed e hard
- Quando não definido parâmetro o GIT assume como padrão o mixed.
- Antes de apresentar cada um dos tipos de reset, e importante entender que os três tipos irão mudar o ponteiro HEAD e a branch atual para o commit referenciado. A diferença entre elas sera nas duas areas logicas que poderão ser afetadas pelo comando dependendo do parâmetro adicionado: a area de stage e a area de trabalho

### Reset de commit - SOFT

- Vamos adicionar um novo arquivo chamado soft.txt

```bash
touch soft.txt
```

```bash
git add soft.txt
```

```bash
git commit -m "adding soft test"
```

```bash
git hist
# * e680b94 2022-01-20 | Revert "changes in hello.txt" (HEAD -> master) [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0, origin/master) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

```bash
git status
# On branch master
# Your branch is ahead of 'origin/master' by 2 commits.
#   (use "git push" to publish your local commits)
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         new file:   soft.txt
```

- Note que após resetarmos para um commit anterior ao que adicionou soft.txt, o git esta solicitando o commit de soft.txt
- Isso ocorreu porque no soft as areas de stage e area de trabalho não são alterados

### Reset de commit - MIXED

- Vamos adicionar um novo arquivo chamado mixed.txt

```bash
touch mixed.txt
```

```bash
git add mixed.txt
```

```bash
git commit -m "adding mixed test"
```

```bash
git hist
# * f06d355 2022-01-20 | adding mixed test (HEAD -> master) [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0, origin/master) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
# On branch master
# Your branch is ahead of 'origin/master' by 2 commits.
#   (use "git push" to publish your local commits)
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         new file:   soft.txt
```

```bash
git reset HEAD~
```

```bash
git status
# On branch master
# Your branch is ahead of 'origin/master' by 2 commits.
#   (use "git push" to publish your local commits)
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         mixed.txt
# nothing added to commit but untracked files present (use "git add" to track)
```

- Note que após resetarmos para um commit anterior ao que adicionou mixed.txt, o git esta solicitando o add de mixed.txt
- Isso ocorreu porque no mixed a area de stage e alterada para o momento do commit selecionado, que não tinha o arquivo mixed.txt no stage

Pronto, entendido o mixed vamos remover as alterações da area de trabalho

```bash
git clean mixed.txt -f
```

### Reset de commit - HARD

- Vamos adicionar um novo arquivo hard.txt e nele adicionar o conteúdo “hard”

```bash
echo "hard" > hard.txt
```

```bash
git add hard.txt
```

```bash
git commit -m "adding hard test"
```

```bash
git hist
# * 32d78cb 2022-01-20 | adding hard test (HEAD -> master) [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0, origin/master) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

```bash
git reset --hard head~
# HEAD is now at e680b94 Revert "changes in hello.txt"
```

```bash
git hist
# * e680b94 2022-01-20 | Revert "changes in hello.txt" (HEAD -> master) [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0, origin/master) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

```bash
git status
# On branch master
# Your branch is ahead of 'origin/master' by 2 commits.
#   (use "git push" to publish your local commits)
# nothing to commit, working tree clean
```

- Note que após resetarmos para um commit anterior ao que adicionou hard.txt, o git status mostra que o diretório esta limpo.
- Isso ocorreu porque no hard a area stage e a area de trabalho são alterados para o momento do commit selecionado, logo o arquivo hard.txt foi descartado

## Reflog

- Cenário: Voce fez uma git reset —hard para algumas alterações indesejadas, mas depois percebeu que realmente precisava delas
- Solução: git reflog vem em seu socorro! E um comando incrível para recuperar o histórico de projetos e recuperar quase tudo
- Vamos recuperar o arquivo hard com o conteúdo hard

```bash
git reflog
# e680b94 (HEAD -> master) HEAD@{0}: reset: moving to head~
# 32d78cb HEAD@{1}: commit: adding hard test
# e680b94 (HEAD -> master) HEAD@{2}: reset: moving to head~
# f06d355 HEAD@{3}: commit: adding mixed test
# e680b94 (HEAD -> master) HEAD@{4}: reset: moving to HEAD~
# 02b8cf2 HEAD@{5}: commit: adding soft test
# e680b94 (HEAD -> master) HEAD@{6}: revert: Revert "changes in hello.txt"
# 8172798 HEAD@{7}: commit: changes in hello.txt
# 98e5cad (tag: v1.0, origin/master) HEAD@{8}: checkout: moving from banner to master
# af9b339 (origin/banner, banner) HEAD@{9}: Branch: renamed refs/heads/baner to refs/heads/banner
# af9b339 (origin/banner, banner) HEAD@{11}: commit: adding banner.txt
# 98e5cad (tag: v1.0, origin/master) HEAD@{12}: checkout: moving from master to baner
# 98e5cad (tag: v1.0, origin/master) HEAD@{13}: commit: creating primeiroacesso.txt
# 8985748 HEAD@{14}: commit: creating docs
# 15da1cc HEAD@{15}: commit (initial): Creating hello.txt
```

- Conseguimos a referencia do commit que adicionou o arquivo hard. Ufa o git salva tudo.

```bash
git checkout 32d78cb 
# Note: switching to '32d78cb'.
# You are in 'detached HEAD' state. You can look around, make experimental
# changes and commit them, and you can discard any commits you make in this
# state without impacting any branches by switching back to a branch.
# If you want to create a new branch to retain commits you create, you may
# do so (now or later) by using -c with the switch command. Example:
#   git switch -c <new-branch-name>
# Or undo this operation with:
#   git switch -
# Turn off this advice by setting config variable advice.detachedHead to false
# HEAD is now at 32d78cb adding hard test
```

## Reset x revert

- O comando revert gera um novo commit que desfaz o commit selecionado

  - Ou seja, para uma alteração que nao deveria existir são gerados 2 commits no histórico
  - Para ter um histórico mais limpo e melhor que os dois commits acima não pertençam ao histórico
- O comando reset remove commit(s) como se eles nunca tivessem existido no histórico

  - Torna o histórico mais limpo sem commits adicionais
  - Pode ser utilizado para remover os últimos commits de um determinado histórico
  - Nao remova commits que outros contribuidores possam ter baseado trabalho

  ## Detached head

```bash
git hist
# * 32d78cb 2022-01-20 | adding hard test (HEAD) [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" (master) [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0, origin/master) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

```bash
git status
# HEAD detached at 32d78cb
# nothing to commit, working tree clean
```

- Analisando esse histórico percebemos uma situação incomum. O repositório não esta mais na branch master e o ponteiro de head esta apontando para o primeiro commit que não esta na branch master
- Esse cenário ocorreu logo após utilizarmos checkout em um commit especifico (Nesta situação baseado no reflog)
- O HEAD deve apontar para a branch atual
- E através do HEAD que sabemos em qual branch estamos trabalhando
- O comportamento normal e o HEAD sempre estar apontando para alguma branch e a branch aponta para o ultimo commit
- Essa situação caracteriza um detached HEAD
- A solução para incluir novamente o hard.txt dentro da master seria criar uma branch que aponte para o ultimo desses commits e depois fazer um merge com o master. A sequencia de comandos seria:

```bash
git branch branch-temp
```

```bash
git checkout master
```

```bash
git merge branch-temp
# Updating e680b94..32d78cb
# Fast-forward
#  hard.txt | 1 +
#  1 file changed, 1 insertion(+)
#  create mode 100644 hard.txt
```

```bash
git hist
# * 32d78cb 2022-01-20 | adding hard test (HEAD -> master) [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0, origin/master) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

- Merge e uma pratica comum para desenvolvedores que usam sistemas de controle de versão
- Trata-se de um procedimento de mesclagem que o GIT realiza para unificar um histórico bifurcado
- A ação merge e usada para combinar mudanças de uma branch para outra
- Iremos criar uma nova branch e fazer checkout nela para entender melhor como podemos mescla-la com nossa branch principal

```bash
git checkout -b icone
```

```bash
touch icone.txt
```

```bash
git add icone.txt
```

```bash
git commit -m "adding icon"
```

- Fast-forward (Avanço rápido) e uma estratégia de merge. Vamos entende-lo melhor na pratica

```bash
git hist
# * e0b7f81 2022-01-21 | adding icon (HEAD -> icone) [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test (origin/master, master) [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

```bash
git checkout master
# * 32d78cb 2022-01-20 | adding hard test (HEAD -> master, origin/master) [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

```bash
git merge icone
# Updating 32d78cb..e0b7f81
# Fast-forward
#  icone.txt | 0
#  1 file changed, 0 insertions(+), 0 deletions(-)
#  create mode 100644 icone.txt
```

- Nesse caso, como após criarmos a ramificação ícone não houveram alterações na master, o git assume que não e necessário realizar um commit de merge para fundir ícone na master
- Basta mover o ponteiro do HEAD para frente porque não ha trabalho divergente para mesclar. Por isso e chamado de “avanço rápido”.
- Após isso, e possível verificar a linearidade com git hist

```bash
git hist
# * e0b7f81 2022-01-21 | adding icon (HEAD -> master, icone) [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test (origin/master) [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

```bash
git push
# Enumerating objects: 4, done.
# Counting objects: 100% (4/4), done.
# Delta compression using up to 4 threads
# Compressing objects: 100% (2/2), done.
# Writing objects: 100% (3/3), 269 bytes | 269.00 KiB/s, done.
# Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
# remote: Resolving deltas: 100% (1/1), completed with 1 local object.
# To https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git
#    32d78cb..e0b7f81  master -> master
```

- Na aula anterior, foi realizado checkout na branch master seguido do comando git hist.
- Vale destacar que é possível visualizar o log de qualquer branch, **mesmo que não seja a branch corrente**. Para isso, basta executar:

```bash
git log <nome da branch>
```

- Lembrando que o comando git hist é um atalho criado a partir do git log personalizado. Por isso, o git hist também irá aceitar o nome da branch.

```bash
git hist <nome da branch>
```

- Three-Way (Estratégia recursiva) e outra estratégia de merge
- Vamos criar uma nova branch e fazer checkout nela

```bash
git checkout -b menu
```

```bash
touch menu.txt
```

```bash
git add menu.txt
```

```bash
git commit -m "adding menu.txt"
```

```bash
git checkout master
```

- Agora, antes de realizar um merge dessa branch para a master iremos gerar um novo commit na master

```bash
echo "icone" > icone.txt
```

```bash
git add icone.txt
```

```bash
git commit -m "changes in icone.txt"
```

```bash
git merge menu
```

```bash
git hist
# *   e76d8f9 2022-01-21 | Merge branch 'menu' (HEAD -> master) [Regy Eduardo]
# |\
# | * 2a68ea5 2022-01-21 | adding menu.txt (menu) [Regy Eduardo]
# * | c96964b 2022-01-21 | changes in icone.txt [Regy Eduardo]
# |/
# * e0b7f81 2022-01-21 | adding icon (origin/master, icone) [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

```bash
git push
# Enumerating objects: 9, done.
# Counting objects: 100% (9/9), done.
# Delta compression using up to 4 threads
# Compressing objects: 100% (6/6), done.
# Writing objects: 100% (7/7), 631 bytes | 631.00 KiB/s, done.
# Total 7 (delta 4), reused 0 (delta 0), pack-reused 0
# remote: Resolving deltas: 100% (4/4), completed with 1 local object.
# To https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git
#    e0b7f81..e76d8f9  master -> master
```

## No fast-forward

- Vimos que na primeira ocasião o GIT assumiu que deveria utilizar a técnica fast-forward
- No entanto, ocasionalmente, voce deseja impedir que esse comportamento aconteça, normalmente porque voce deseja manter uma topologia de ramificação especifica
- Por exemplo, voce esta mesclando uma ramificação e deseja garantir que quando esta branch for incorporada na master, isso esteja visível no seu histórico
- Para isso, basta utilizar a opção —no-ff no merge
- Neste caso o GIT utilizara a técnica recursiva

## DAG

- A relação entre commits em um repositório e baseado em grafos
- Grafos e uma area da matematica extremamente importante para a computacao
- Veremos que um tipo especial de grafos e usado pelo GIT para modelar o histórico de commit de um projeto. Por isso e importante estabelecer alguns conceitos básicos:
  - Grafos
    - E uma forma de modelar coisas conectadas
    - Os grafos contem nos conectados por arestas
    - Pode conter algumas características como:
      - Direcionado
        - Tipo de grafo em que os nos estão conectados em uma determinada direção
      - Grafo acíclico
        - Acíclico significa que não ha ciclos ou não circulares
      - Grafo acíclico direcionado (DAG)
        - E um tipo de grafo que atende as seguintes características: direcionado e acíclico
  - DAG x GIT
    - O git modela a relação de commits com o grafo acíclico dirigido (DAG)
    - Cada no representa um commit
    - As setas apontam para os pais de um commit
- Em aulas anteriores foi visto que e possível utilizar HEAD~n, para considerar n commits anteriores
- Acabamos de ver que que o historio de commits e baseado em um DAG, ou seja, ele pode não ser linear
- Sendo assim, se um commit atual possuir 2 commits pais (pois tal commit e resultado de um merge), como podemos “navegar” para o segundo pai?

```bash
git hist
# *   e76d8f9 2022-01-21 | Merge branch 'menu' (HEAD -> master, origin/master) [Regy Eduardo]
# |\
# | * 2a68ea5 2022-01-21 | adding menu.txt (menu) [Regy Eduardo]
# * | c96964b 2022-01-21 | changes in icone.txt [Regy Eduardo]
# |/
# * e0b7f81 2022-01-21 | adding icon (icone) [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

```bash
git show HEAD~
# commit c96964b7c3f933d244b208cd69688694a92f1280
# Author: Regy Eduardo <regyeduardo@gmail.com>
# Date:   Fri Jan 21 14:39:50 2022 -0300
#     changes in icone.txt
# diff --git a/icone.txt b/icone.txt
# index e69de29..1744952 100644
# --- a/icone.txt
# +++ b/icone.txt
# @@ -0,0 +1 @@
# +icone
```

```bash
git show HEAD^
# commit c96964b7c3f933d244b208cd69688694a92f1280
# Author: Regy Eduardo <regyeduardo@gmail.com>
# Date:   Fri Jan 21 14:39:50 2022 -0300
#     changes in icone.txt
# diff --git a/icone.txt b/icone.txt
# index e69de29..1744952 100644
# --- a/icone.txt
# +++ b/icone.txt
# @@ -0,0 +1 @@
# +icone
```

```bash
git show HEAD^2
# commit 2a68ea582f94c86685869f706de1b476a9956aa9 (menu)
# Author: Regy Eduardo <regyeduardo@gmail.com>
# Date:   Fri Jan 21 14:37:00 2022 -0300
#     adding menu.txt
# diff --git a/menu.txt b/menu.txt
# new file mode 100644
# index 0000000..e69de29
```

## Conflitos

- Os conflitos surgem em situações de mesclagem em que o Git não pode determinar automaticamente o que esta correto e e necessário decisões humanas
- E importante destacar que não e necessário dois desenvolvedores no projeto para existir conflitos, duas branches criadas pelo menos desenvolvedor podem conflitar
- Os conflitos afetam apenas o desenvolvedor que conduz a mesclagem, o resto da equipe não tem conhecimento do conflito
- O GIT marcara o arquivo como conflitante e interrompera o processo de fusão. E responsabilidade dos desenvolvedores resolver o conflito.
- Vamos gerar um conflito

```bash
echo -e "consulta web service\nloga" > login.txt
```

```bash
git add login.txt
```

```bash
git commit -m "login algorithm"
```

```bash
git checkout -b login-persist
```

- Altere o arquivo login.txt adicionado na terceira linha o conteúdo “salva na tabela”

```bash
echo "salva na tabela" >> login.txt # (>> insere no final do arquivo)
```

```bash
git add login.txt
```

```bash
git commit -m "including persist on login.txt"
```

```bash
git checkout master
```

- Altere o arquivo login.txt adicionando na terceira linha o conteúdo “gambiarra”

```bash
echo "gambiarra" >> login.txt
```

```bash
git add login.txt
```

```bash
git commit -m "emergency adjustment"
```

```bash
git merge login-persist
# Auto-merging login.txt
# CONFLICT (content): Merge conflict in login.txt
# Automatic merge failed; fix conflicts and then commit the result.
```

```bash
git status # Apos resolver o conflito
# On branch master
# Your branch is ahead of 'origin/master' by 2 commits.
#   (use "git push" to publish your local commits)
# You have unmerged paths.
#   (fix conflicts and run "git commit")
#   (use "git merge --abort" to abort the merge)
# Unmerged paths:
#   (use "git add <file>..." to mark resolution)
#         both modified:   login.txt
# no changes added to commit (use "git add" and/or "git commit -a")
```

```bash
git add login.txt
```

```bash
git commit -m "resolving login conflict"
# [master b668c7a] resolving login conflict
```

```bash
git hist
# *   b668c7a 2022-01-21 | resolving login conflict (HEAD -> master) [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt (login-persist) [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
# |/
# * 266c4f4 2022-01-21 | login algorithm [Regy Eduardo]
# *   e76d8f9 2022-01-21 | Merge branch 'menu' (origin/master) [Regy Eduardo]
# |\
# | * 2a68ea5 2022-01-21 | adding menu.txt (menu) [Regy Eduardo]
# * | c96964b 2022-01-21 | changes in icone.txt [Regy Eduardo]
# |/
# * e0b7f81 2022-01-21 | adding icon (icone) [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

## Nota: merge sem checkout, é possível?

Durante as aulas práticas sobre merge desta seção, a branch master era a branch
corrente no momento do merge. No exemplo de fast forward, após o merge,
o rótulo da branch master avançou referenciando o mesmo commit que a
branch ícone referenciava.

Já no exemplo de merge three way, a
branch master avançou referenciando o novo commit de merge, que foi
gerado pela divergência de histórico entre as branches envolvidas na
mesclagem (master e menu).

Neste contexto, uma pergunta que pode surgir é sobre a possibilidade de fazer uma mesclagem entre duas branches, **que não são a branch corrente**. A resposta para esta dúvida pode ser respondida pelo texto a seguir, retirado diretamente da fonte informada.

"...*Você não pode mesclar uma branch B na branch A sem fazer o checkout de A primeiro, **caso isso resulte em uma mesclagem sem fast forward** (three way). Isso ocorre porque uma cópia de trabalho é necessária para resolver quaisquer conflitos em potencial.*

*No entanto, **no caso de mesclagens fast forward, isso é possível** , porque tais mesclagens nunca podem resultar em conflitos, por definição...*"

[Fonte](https://stackoverflow.com/questions/3216360/merge-update-and-pull-git-branches-without-using-checkouts)

## Simulando um segundo colaborador

- Agora vamos simular outro contribuidor atuando no projeto, fazendo um novo clone do repositório em outro diretório
- E interessante notar que no repositório a única branch local que existe e a da master

```bash
git branch -a # No novo repositorio clonado
# * master
#   remotes/origin/HEAD -> origin/master
#   remotes/origin/banner
#   remotes/origin/master
```

```bash
touch logo.txt
```

```bash
git add logo.txt
```

```bash
git commit -m "adding logo.txt"
```

```bash
git push
```

## Fetch

- Agora no repositório principal, execute:

```bash
git fetch
# remote: Enumerating objects: 3, done.
# remote: Counting objects: 100% (3/3), done.
# remote: Compressing objects: 100% (1/1), done.
# remote: Total 2 (delta 1), reused 2 (delta 1), pack-reused 0
# Unpacking objects: 100% (2/2), 218 bytes | 3.00 KiB/s, done.
# From https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY
#    b668c7a..f408c80  master     -> origin/master
```

- O fetch fara o download do conteúdo remoto, mas não atualizara o estado de funcionamento do seu repositório local, deixando intacto o trabalho atual
- Ou seja, nesta situação nossa area de trabalho se manteve intacta, novos GIT objects foram baixados e a branch de rastreamento da master esta apontando para o ultimo commit feito no slide anterior

```bash
ls
# docs/  hard.txt  hello.txt  icone.txt  login.txt  menu.txt  primeiroacesso.txt
```

- Para visualizar as alterações execute

```bash
git checkout origin/master
```

```bash
ls
# docs/     hello.txt  login.txt  menu.txt
# hard.txt  icone.txt  logo.txt   primeiroacesso.txt
```

- Executamos o comando anterior somente para visualizar as alterações na master remota
- Para trazer as alterações para a master local faca o procedimento seguinte:

```bash
git checkout master
# Previous HEAD position was f408c80 adding logo.txt
# Switched to branch 'master'
# Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
#   (use "git pull" to update your local branch)
```

```bash
git merge --no-ff origin/master
# Merge made by the 'ort' strategy.
#  logo.txt | 0
#  1 file changed, 0 insertions(+), 0 deletions(-)
#  create mode 100644 logo.txt
```

```bash
git hist
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' (HEAD -> master) [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt (origin/master) [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt (login-persist) [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
# |/
# * 266c4f4 2022-01-21 | login algorithm [Regy Eduardo]
# *   e76d8f9 2022-01-21 | Merge branch 'menu' [Regy Eduardo]
# |\
# | * 2a68ea5 2022-01-21 | adding menu.txt (menu) [Regy Eduardo]
# * | c96964b 2022-01-21 | changes in icone.txt [Regy Eduardo]
# |/
# * e0b7f81 2022-01-21 | adding icon (icone) [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]
```

```bash
ls
# docs/     hello.txt  login.txt  menu.txt
# hard.txt  icone.txt  logo.txt   primeiroacesso.txt
```

- Então toda vez que desejo atualizar minha area de trabalho preciso fazer um fetch e um merge?
- A resposta e sim. Mas ja ha um comando que realiza esses dois passos chamado git pull

## Pull

- O git pull baixa as alterações do repositório remoto e mescla com sua area de trabalho

```bash
echo "logo" > logo.txt
```

```bash
git add logo.txt
```

```bash
git commit -m "changes in logo.txt"
```

```bash
git push
# Enumerating objects: 7, done.
# Counting objects: 100% (7/7), done.
# Delta compression using up to 4 threads
# Compressing objects: 100% (3/3), done.
# Writing objects: 100% (4/4), 488 bytes | 488.00 KiB/s, done.
# Total 4 (delta 1), reused 0 (delta 0), pack-reused 0
# remote: Resolving deltas: 100% (1/1), completed with 1 local object.
# To https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git
#    f408c80..d543364  master -> master
```

- Agora no outro repositório, execute:

```bash
git pull
```

```bash
git hist
# * d543364 2022-01-21 | changes in logo.txt (HEAD -> master, origin/master, origin/HEAD) [Regy Eduardo]
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
# |/
# * 266c4f4 2022-01-21 | login algorithm [Regy Eduardo]
# *   e76d8f9 2022-01-21 | Merge branch 'menu' [Regy Eduardo]
# |\
# | * 2a68ea5 2022-01-21 | adding menu.txt [Regy Eduardo]
# * | c96964b 2022-01-21 | changes in icone.txt [Regy Eduardo]
# |/
# * e0b7f81 2022-01-21 | adding icon [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0) [Regy Eduardo]
# * 8985748 2022-01-19 | creating docs [Regy Eduardo]
# * 15da1cc 2022-01-19 | Creating hello.txt [Regy Eduardo]

```

## Fetch x pull

- O fetch sera usado para baixar os git objects do repositório remoto sem afetar o seu código local
  - Esta e uma forma segura de atualizar o espelho do repositório remoto em seu repositório local sem fazer merge
  - Para ter a versão remota na area de trabalho, voce ainda precisara fazer merge com a branch de rastreamento remoto (o que não e necessário no pull)
- O pull executa um git fetch seguido de um git merge origin/<current_branch>
  - Pode ser considerado “pouco seguro” no sentido que o conteúdo remoto e baixado e, de imediato, e feito uma tentativa de alterar o estado local para que ele corresponda aquele conteúdo
  - Isso pode causar um estado conflituoso acidental no repositório local

## Bloqueio de push

```bash
touch dadospessoais.txt # No outro repositorio
```

```bash
git add dadospessoais.txt
```

```bash
git commit -m "adding dadospessoais.txt"
```

```bash
git push
```

- Retorne ao repositório principal

```bash
touch dadospublicos.txt
```

```bash
git add dadospublicos.txt
```

```bash
git commit -m "adding dadospublicos.txt"
```

```bash
git push
# To https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git
#  ! [rejected]        master -> master (fetch first)
# error: failed to push some refs to 'https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git'
# hint: Updates were rejected because the remote contains work that you do
# hint: not have locally. This is usually caused by another repository pushing
# hint: to the same ref. You may want to first integrate the remote changes
# hint: (e.g., 'git pull ...') before pushing again.
# hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

- O push foi bloqueado porque existe um commit do “outro contribuidor” que ainda não foi baixado no seu repositório local. Nesta situação o GIT pede que voce faca um pull antes, pois caso exista um conflito no merge este sera o momento de resolver.

```bash
git pull
# remote: Enumerating objects: 3, done.
# remote: Counting objects: 100% (3/3), done.
# remote: Compressing objects: 100% (1/1), done.
# remote: Total 2 (delta 1), reused 2 (delta 1), pack-reused 0
# Unpacking objects: 100% (2/2), 238 bytes | 4.00 KiB/s, done.
# From https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY
#    d543364..e8b34c2  master     -> origin/master
# Merge made by the 'ort' strategy.
#  dadospessoais.txt | 0
#  1 file changed, 0 insertions(+), 0 deletions(-)
#  create mode 100644 dadospessoais.txt
```

- Nao ha conflito pois o merge foi realizado com arquivos diferentes

```bash
git push
# Enumerating objects: 6, done.
# Counting objects: 100% (6/6), done.
# Delta compression using up to 4 threads
# Compressing objects: 100% (4/4), done.
# Writing objects: 100% (4/4), 521 bytes | 521.00 KiB/s, done.
# Total 4 (delta 2), reused 0 (delta 0), pack-reused 0
# remote: Resolving deltas: 100% (2/2), completed with 1 local object.
# To https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git
#    e8b34c2..6af3ac2  master -> master
```

## Rebase

```bash
git checkout -b notificacoes
```

```bash
touch notificacoes.txt
```

```bash
git add notificacoes.txt
```

```bash
git commit -m "adding notificacoes.txt"
```

```bash
git hist
# * fa23b4b 2022-01-21 | adding notificacoes.txt (HEAD -> notificacoes) [Regy Eduardo]
# *   6af3ac2 2022-01-21 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY (origin/master, master) [Regy Eduardo]
# |\
# | * e8b34c2 2022-01-21 | adding dadospessoais.txt [Regy Eduardo]
# * | 9b0b5cb 2022-01-21 | adding dadospublicos.txt [Regy Eduardo]
# |/
# * d543364 2022-01-21 | changes in logo.txt [Regy Eduardo]
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt (login-persist) [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
# |/
# * 266c4f4 2022-01-21 | login algorithm [Regy Eduardo]
# *   e76d8f9 2022-01-21 | Merge branch 'menu' [Regy Eduardo]
# |\
# | * 2a68ea5 2022-01-21 | adding menu.txt (menu) [Regy Eduardo]
# * | c96964b 2022-01-21 | changes in icone.txt [Regy Eduardo]
# |/
# * e0b7f81 2022-01-21 | adding icon (icone) [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0) [Regy Eduardo]
# Enumerating objects: 6, done.
# Counting objects: 100% (6/6), done.
# Delta compression using up to 4 threads
# Compressing objects: 100% (4/4), done.
# Writing objects: 100% (4/4), 521 bytes | 521.00 KiB/s, done.
# Total 4 (delta 2), reused 0 (delta 0), pack-reused 0
# remote: Resolving deltas: 100% (2/2), completed with 1 local object.
# To https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git
#    e8b34c2..6af3ac2  master -> master
```

```bash
git checkout master
```

```bash
echo "menu" > menu.txt
```

```bash
git add menu.txt
```

```bash
git commit -m "changes in menu"
```

```bash
git hist
# * 8e009fd 2022-01-21 | changes in menu (HEAD -> master) [Regy Eduardo]
# *   6af3ac2 2022-01-21 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY (origin/master) [Regy Eduardo]
# |\
# | * e8b34c2 2022-01-21 | adding dadospessoais.txt [Regy Eduardo]
# * | 9b0b5cb 2022-01-21 | adding dadospublicos.txt [Regy Eduardo]
# |/
# * d543364 2022-01-21 | changes in logo.txt [Regy Eduardo]
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt (login-persist) [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
# |/
# * 266c4f4 2022-01-21 | login algorithm [Regy Eduardo]
# *   e76d8f9 2022-01-21 | Merge branch 'menu' [Regy Eduardo]
# |\
# | * 2a68ea5 2022-01-21 | adding menu.txt (menu) [Regy Eduardo]
# * | c96964b 2022-01-21 | changes in icone.txt [Regy Eduardo]
# |/
# * e0b7f81 2022-01-21 | adding icon (icone) [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
# * 98e5cad 2022-01-20 | creating primeiroacesso.txt (tag: v1.0) [Regy Eduardo]
```

```bash
git checkout notificacoes
```

Essa opção recria os commits inserindo-os em cima do topo de outra base

```bash
git rebase master
```

```bash
git hist
# * a17de26 2022-01-21 | adding notificacoes.txt (HEAD -> notificacoes) [Regy Eduardo]
# * 8e009fd 2022-01-21 | changes in menu (master) [Regy Eduardo]
# *   6af3ac2 2022-01-21 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY (origin/master) [Regy Eduardo]
# |\
# | * e8b34c2 2022-01-21 | adding dadospessoais.txt [Regy Eduardo]
# * | 9b0b5cb 2022-01-21 | adding dadospublicos.txt [Regy Eduardo]
# |/
# * d543364 2022-01-21 | changes in logo.txt [Regy Eduardo]
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt (login-persist) [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
# |/
# * 266c4f4 2022-01-21 | login algorithm [Regy Eduardo]
# *   e76d8f9 2022-01-21 | Merge branch 'menu' [Regy Eduardo]
# |\
# | * 2a68ea5 2022-01-21 | adding menu.txt (menu) [Regy Eduardo]
# * | c96964b 2022-01-21 | changes in icone.txt [Regy Eduardo]
# |/
# * e0b7f81 2022-01-21 | adding icon (icone) [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
# * 8172798 2022-01-20 | changes in hello.txt [Regy Eduardo]
```

- Note que o commit foi recriado e inserido na ponta mantando um histórico linear

```bash
git checkout master
```

```bash
git merge notificacoes
```

```bash
git hist
# * a17de26 2022-01-21 | adding notificacoes.txt (HEAD -> master, notificacoes) [Regy Eduardo]
# * 8e009fd 2022-01-21 | changes in menu [Regy Eduardo]
# *   6af3ac2 2022-01-21 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY (origin/master) [Regy Eduardo]
# |\
# | * e8b34c2 2022-01-21 | adding dadospessoais.txt [Regy Eduardo]
# * | 9b0b5cb 2022-01-21 | adding dadospublicos.txt [Regy Eduardo]
# |/
# * d543364 2022-01-21 | changes in logo.txt [Regy Eduardo]
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt (login-persist) [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
# |/
# * 266c4f4 2022-01-21 | login algorithm [Regy Eduardo]
# *   e76d8f9 2022-01-21 | Merge branch 'menu' [Regy Eduardo]
# |\
# | * 2a68ea5 2022-01-21 | adding menu.txt (menu) [Regy Eduardo]
# * | c96964b 2022-01-21 | changes in icone.txt [Regy Eduardo]
# |/
# * e0b7f81 2022-01-21 | adding icon (icone) [Regy Eduardo]
# * 32d78cb 2022-01-20 | adding hard test [Regy Eduardo]
# * e680b94 2022-01-20 | Revert "changes in hello.txt" [Regy Eduardo]
```

## The Golden Rule of Rebase

- Depois de entender os conceitos envolvidos no rebase e extremamente importante saber quando não utilizar este comando
- A regra de ouro do Rebase e:
  - Nao recrie commits que existam fora de seu repositório e nos quais as pessoas possam ter trabalhado
- No exemplo anterior foi preservado o histórico da master, nenhum commit dessa branch foi recriado. Agora considere o exemplo a seguir em caso de um rebase da master em outra branch

```bash
touch a.txt
```

```bash
git add a.txt
```

```bash
git commit -m "a"
```

```bash
git checkout -b rebranding
```

```bash
touch b.txt
```

```bash
git add b.txt
```

```bash
git commit -m "b"
```

```bash
git checkout master
```

```bash
touch c.txt
```

```bash
git add c.txt
```

```bash
git commit -m "c"
```

- Agora acesse o repositório do outro contribuidor novamente

```bash
git pull
```

```bash
touch d.txt
```

```bash
git add d.txt
```

```bash
git commit -m "d.txt"
```

- Retorne para o repositório principal

```bash
git rebase rebranding
```

```bash
git hist
# * 05a6915 2022-01-21 | c (HEAD -> master) [Regy Eduardo]
# * bb54275 2022-01-21 | b (rebranding) [Regy Eduardo]
# * 6cce631 2022-01-21 | a [Regy Eduardo]
# * a17de26 2022-01-21 | adding notificacoes.txt (notificacoes) [Regy Eduardo]
# * 8e009fd 2022-01-21 | changes in menu [Regy Eduardo]
# *   6af3ac2 2022-01-21 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
# |\
# | * e8b34c2 2022-01-21 | adding dadospessoais.txt [Regy Eduardo]
# * | 9b0b5cb 2022-01-21 | adding dadospublicos.txt [Regy Eduardo]
# |/
# * d543364 2022-01-21 | changes in logo.txt [Regy Eduardo]
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt (login-persist) [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
# |/
# * 266c4f4 2022-01-21 | login algorithm [Regy Eduardo]
# *   e76d8f9 2022-01-21 | Merge branch 'menu' [Regy Eduardo]
# |\
# | * 2a68ea5 2022-01-21 | adding menu.txt (menu) [Regy Eduardo]
# * | c96964b 2022-01-21 | changes in icone.txt [Regy Eduardo]
# |/
# * e0b7f81 2022-01-21 | adding icon (icone) [Regy Eduardo]
```

```bash
git push
# To https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git
#  ! [rejected]        master -> master (non-fast-forward)
# error: failed to push some refs to 'https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY.git'
# hint: Updates were rejected because the tip of your current branch is behind
# hint: its remote counterpart. Integrate the remote changes (e.g.
# hint: 'git pull ...') before pushing again.
# hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

- Rejeitado porque a branch de rastreamento da master possui o commit C. E a branch local não possui (o rebase destruiu esse commit). Então o GIT entende que voce deveria dar um pull antes. Contudo, voce deseja reescrever o histórico, para isso voce deveria forcar o push.

```bash
git push -f
```

- Agora acesse novamente o repositório do outro contribuidor

```bash
git pull
```

```bash
git hist
# *   d6986c7 2022-01-22 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY (HEAD -> master) [Regy Eduardo]
# |\
# | * 05a6915 2022-01-21 | c (origin/master, origin/HEAD) [Regy Eduardo]
# | * bb54275 2022-01-21 | b [Regy Eduardo]
# * | e5d259e 2022-01-21 | d [Regy Eduardo]
# * | 6283e70 2022-01-21 | c [Regy Eduardo]
# |/
# * 6cce631 2022-01-21 | a [Regy Eduardo]
# * a17de26 2022-01-21 | adding notificacoes.txt [Regy Eduardo]
# * 8e009fd 2022-01-21 | changes in menu [Regy Eduardo]
# *   6af3ac2 2022-01-21 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
# |\
# | * e8b34c2 2022-01-21 | adding dadospessoais.txt [Regy Eduardo]
# * | 9b0b5cb 2022-01-21 | adding dadospublicos.txt [Regy Eduardo]
# |/
# * d543364 2022-01-21 | changes in logo.txt [Regy Eduardo]
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
# |/
# * 266c4f4 2022-01-21 | login algorithm [Regy Eduardo]
# *   e76d8f9 2022-01-21 | Merge branch 'menu' [Regy Eduardo]
```

- Note que o grande beneficio do rebase que e a linearidade do histórico não acorreu. E ainda gerou commits duplicados no histórico tornando-o confuso (com dois SHA-1 diferentes que fazem a mesma coisa)

## Merge x rebase

- O merge e uma operação não destrutiva. As ramificações existentes não são alteradas de forma alguma. Isso evita todas as possíveis armadilhas do rebase.
- Por outro lado, isso também significa a existência de bifurcações em merges não fast-forward, o que ira poluir o histórico do projeto
- O rebase e uma operação destrutiva. Um ou mais commits serão eliminados e criados novamente
- Por isso, para não gerar problemas maiores que um histórico poluído, e importante seguir a regra de ouro do rebase. Se voce utilizar esse comando de forma consciente voce tera um histórico mais linear e fácil de ser entendido

### Conclusoes

- Se voce preferir um histórico limpo e linear, sem confirmações desnecessárias de mesclagem, utilize git rebase ao invés de git merge
- Por outro lado, se voce deseja preservar o histórico completo do seu projeto e evitar o risco de reescrever commits públicos, voce pode utilizar o git merge.
- Qualquer uma das opções e perfeitamente valida.

## Pull com rebase

- Acesse o repositório principal

Para tornar o histórico desse repositório igual ao do outro contribuidor

```bash
git pull
```

```bash
touch 1.txt
```

```bash
git add 1.txt
```

```bash
git commit -m "1"
```

- Agora acesse o repositório do outro contribuidor

```bash
touch 2.txt
```

```bash
git add 2.txt
```

```bash
git commit -m "2"
```

```bash
git push
```

- Acesse novamente o repositório principal

```bash
git pull
```

```bash
git hist
# *   63c1881 2022-01-22 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY (HEAD -> master) [Regy Eduardo]
# |\
# | * 9aa06c4 2022-01-22 | 2 (origin/master) [Regy Eduardo]
# * | d7861fc 2022-01-22 | 1 [Regy Eduardo]
# |/
# *   d6986c7 2022-01-22 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
# |\
# | * 05a6915 2022-01-21 | c [Regy Eduardo]
# | * bb54275 2022-01-21 | b (rebranding) [Regy Eduardo]
# * | e5d259e 2022-01-21 | d [Regy Eduardo]
# * | 6283e70 2022-01-21 | c [Regy Eduardo]
# |/
# * 6cce631 2022-01-21 | a [Regy Eduardo]
# * a17de26 2022-01-21 | adding notificacoes.txt (notificacoes) [Regy Eduardo]
# * 8e009fd 2022-01-21 | changes in menu [Regy Eduardo]
# *   6af3ac2 2022-01-21 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
# |\
# | * e8b34c2 2022-01-21 | adding dadospessoais.txt [Regy Eduardo]
# * | 9b0b5cb 2022-01-21 | adding dadospublicos.txt [Regy Eduardo]
# |/
# * d543364 2022-01-21 | changes in logo.txt [Regy Eduardo]
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
```

- Lembre que o pull por padrão realiza um fetch + merge. E nesta situação, não era possível avanço rápido pois seu histórico local divergiu com o remoto
- Voce consegue alterar o padrão e utilizar pull com rebase
- Retorne para o momento antes do merge ocorrer

```bash
git reset --hard HEAD~1 # Opcao 1
git reset --hard <HASH do commit> # Opcao 2
```

```bash
git hist
# * d7861fc 2022-01-22 | 1 (HEAD -> master) [Regy Eduardo]
# *   d6986c7 2022-01-22 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
# |\
# | * 05a6915 2022-01-21 | c [Regy Eduardo]
# | * bb54275 2022-01-21 | b (rebranding) [Regy Eduardo]
# * | e5d259e 2022-01-21 | d [Regy Eduardo]
# * | 6283e70 2022-01-21 | c [Regy Eduardo]
# |/
# * 6cce631 2022-01-21 | a [Regy Eduardo]
# * a17de26 2022-01-21 | adding notificacoes.txt (notificacoes) [Regy Eduardo]
# * 8e009fd 2022-01-21 | changes in menu [Regy Eduardo]
# *   6af3ac2 2022-01-21 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
# |\
# | * e8b34c2 2022-01-21 | adding dadospessoais.txt [Regy Eduardo]
# * | 9b0b5cb 2022-01-21 | adding dadospublicos.txt [Regy Eduardo]
# |/
# * d543364 2022-01-21 | changes in logo.txt [Regy Eduardo]
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt (login-persist) [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
# |/
```

```bash
git pull --rebase
# * 460aae7 2022-01-22 | 1 (HEAD -> master) [Regy Eduardo]
# * 9aa06c4 2022-01-22 | 2 (origin/master) [Regy Eduardo]
# *   d6986c7 2022-01-22 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
# |\
# | * 05a6915 2022-01-21 | c [Regy Eduardo]
# | * bb54275 2022-01-21 | b (rebranding) [Regy Eduardo]
# * | e5d259e 2022-01-21 | d [Regy Eduardo]
# * | 6283e70 2022-01-21 | c [Regy Eduardo]
# |/
# * 6cce631 2022-01-21 | a [Regy Eduardo]
# * a17de26 2022-01-21 | adding notificacoes.txt (notificacoes) [Regy Eduardo]
# * 8e009fd 2022-01-21 | changes in menu [Regy Eduardo]
# *   6af3ac2 2022-01-21 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
# |\
# | * e8b34c2 2022-01-21 | adding dadospessoais.txt [Regy Eduardo]
# * | 9b0b5cb 2022-01-21 | adding dadospublicos.txt [Regy Eduardo]
# |/
# * d543364 2022-01-21 | changes in logo.txt [Regy Eduardo]
# *   34c8d84 2022-01-21 | Merge remote-tracking branch 'origin/master' [Regy Eduardo]
# |\
# | * f408c80 2022-01-21 | adding logo.txt [Regy Eduardo]
# |/
# *   b668c7a 2022-01-21 | resolving login conflict [Regy Eduardo]
# |\
# | * 67ff1d8 2022-01-21 | including persist on login.txt (login-persist) [Regy Eduardo]
# * | 6c14eca 2022-01-21 | emergency adjustment [Regy Eduardo]
```

## Emendar commit

```bash
git checkout -b brindes
```

```bash
touch brindes.txt
```

```bash
git add brindes.txt
```

```bash
git commit -m "adding brindes.txt"
```

```bash
git hist
# * dfe461d 2022-01-22 | adding brindes.txt (HEAD -> brindes) [Regy Eduardo]
# * 460aae7 2022-01-22 | 1 (master) [Regy Eduardo]
# * 9aa06c4 2022-01-22 | 2 (origin/master) [Regy Eduardo]
# *   d6986c7 2022-01-22 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
```

```bash
echo "meus brindes" > brindes.txt
```

```bash
git add brindes.txt
```

```bash
git commit --amend -m "adding brindex.txt with content"
```

```bash
git hist
# * db2f90f 2022-01-22 | adding brindex.txt with content (HEAD -> brindes) [Regy Eduardo]
# * 460aae7 2022-01-22 | 1 (master) [Regy Eduardo]
# * 9aa06c4 2022-01-22 | 2 (origin/master) [Regy Eduardo]
# *   d6986c7 2022-01-22 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
```

- Note que o segundo commit tem um SHA-1 diferente do primeiro. Ou seja, o git recriou este commit baseado nas alterações dos commits 1 e 2
- Poderíamos ter utilizado o amend somente para editar a mensagem do commit sem alterar o conteúdo do arquivo brindes.txt. Ou so ter alterado o conteúdo do arquivo.

## Squash merge

```bash
git checkout master
```

```bash
git checkout -b esquecisenha
```

```bash
touch esquecisenha.txt
```

```bash
git add esquecisenha.txt
```

```bash
git commit -m "adding esquecisenha.txt"
```

```bash
echo "esqueci a senha codigo" > esquecisenha.txt
```

```bash
git add esquecisenha.txt
```

```bash
git commit -m "finishing forgot my password"
```

```bash
git hist
# * 7ecfb60 2022-01-22 | finishing forgot my password (HEAD -> esquecisenha) [Regy Eduardo]
# * e907626 2022-01-22 | adding esquecisenha.txt [Regy Eduardo]
# * 460aae7 2022-01-22 | 1 (master) [Regy Eduardo]
# * 9aa06c4 2022-01-22 | 2 (origin/master) [Regy Eduardo]
# *   d6986c7 2022-01-22 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
```

```bash
git checkout master
```

```bash
git merge --squash esquecisenha
```

```bash
git commit -m "adding forgot my password feature"
```

```bash
git hist
# * 564851c 2022-01-22 | adding forgot my password feature (HEAD -> master) [Regy Eduardo]
# * 460aae7 2022-01-22 | 1 [Regy Eduardo]
# * 9aa06c4 2022-01-22 | 2 (origin/master) [Regy Eduardo]
# *   d6986c7 2022-01-22 | Merge branch 'master' of https://github.com/regyeduardo/Git---B-sico-ao-avan-ado-UDEMY [Regy Eduardo]
```

## Rebase iterativo

- Gut rebase iterativo e quando o git rebase utiliza o parâmetro —i
- O rebase iterativo faz que ao invés de de mover cegamente todos os commits para a nova base, oferece a oportunidade de alterar os commits individuais no processo
- Sera averto um editor no qual voce pode inserir comandos para cada commit a ser rebaseado. Esses comandos determinam como os commits individuais serão transferidos para a nova base.

```bash
git checkout -b promocoes
```

```bash
touch promocoes.txt
```

```bash
git add promocoes.txt
```

```bash
git commit -m "adding promocoes.txt"
```

```bash
echo "codigo finalizado de promocoes" > promocoes.txt
```

```bash
git add promocoes.txt
```

```bash
git commit -m "finishing promotions development"
```

```bash
echo "codigo finalizado de promocoes sem bug" > promocoes.txt
```

```bash
git add promocoes.txt
```

```bash
git commit -m "bug xixes"
```

```bash
echo "inclusao de gambiarra" > promocoes.txt
```

```bash
git add promocoes.txt
```

```bash
git commit -m "adding a bad workaround"
```

```bash
git hist
# * b8a664b 2022-01-22 | adding a bad workaround (HEAD -> promocoes) [Regy Eduardo]
# * c0840b1 2022-01-22 | bug xixes [Regy Eduardo]
# * b18cf24 2022-01-22 | finishing promotions development [Regy Eduardo]
# * 05ee54c 2022-01-22 | adding promocoes.txt [Regy Eduardo]
```

```bash
git rebase -i master
```

```bash
git hist
# * 8e79986 2022-01-22 | bug fixes (HEAD -> promocoes) [Regy Eduardo]
# * 20f9f9d 2022-01-22 | adding promotions feature [Regy Eduardo]
```

```bash
git checkout master
```

```bash
git merge promocoes
```

```bash
git push
```

## Stash

```bash
git checkout -b noticias
```

```bash
echo "minha primeira noticia" > noticias.txt
```

```bash
git add noticias.txt
```

```bash
git commit -m “adding noticias.txt”
```

```bash
echo "minha segunda noticia" >> noticias.txt
```

```bash
git checkout banner
# error: Your local changes to the following files would be overwritten by checkout:
#         noticias.txt
# Please commit your changes or stash them before you switch branches.
# Aborting
```

- O comando stash salva suas modificações locais em uma pilha e limpa sua area de trabalho para o ultimo commit feito

```bash
git stash
```

```bash
git stash list
# stash@{0}: WIP on noticias: 95ef1eb adding noticias.txt
```

```bash
git checkout banner
```

- Por padrão arquivos com status untracked (que não são monitorados pelo GIT) não são salvos no stash, mas voce pode usar —include-untracked

```bash
git checkout noticias
```

```bash
git stash pop
# On branch noticias
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
#         modified:   noticias.txt
# no changes added to commit (use "git add" and/or "git commit -a")
# Dropped refs/stash@{0} (648ac3d6bdffb87f158e6bee59368790f0b90ce1)
```

```bash
git stash list
# Nao retorna nada
```

- Voce pode ter varias entradas na pilha

```bash
git stash
```

```bash
echo "minha terceira noticia" >> noticias.txt
```

```bash
git stash
```

```bash
git stash list
# stash@{0}: WIP on noticias: 95ef1eb adding noticias.txt
# stash@{1}: WIP on noticias: 95ef1eb adding noticias.txt
```

- E pode aplicar as alterações de uma entrada especifica

```bash
git stash apply stash@{1}
# On branch noticias
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
#         modified:   noticias.txt
# no changes added to commit (use "git add" and/or "git commit -a")
```

```bash
cat noticias.txt
# minha primeira noticia
# minha segunda noticia
```

```bash
git stash list
# stash@{0}: WIP on noticias: 95ef1eb adding noticias.txt
# stash@{1}: WIP on noticias: 95ef1eb adding noticias.txt
```

```bash
git restore noticias.txt
```

```bash
git stash clear
```

```bash
git stash list
# Nao retorna nada
```

## Blame

- E possível verificar qual commit e autor modificara pela ultima vez cada linha de um arquivo

```bash
git blame login.txt
# 266c4f4f (Regy Eduardo 2022-01-21 16:49:05 -0300 1) consulta web service
# 266c4f4f (Regy Eduardo 2022-01-21 16:49:05 -0300 2) loga
# 6c14eca3 (Regy Eduardo 2022-01-21 16:58:55 -0300 3) gambiarra
# 67ff1d8b (Regy Eduardo 2022-01-21 16:54:03 -0300 4) salva na tabela
```

- Outra opção e passar o intervalo de linha por parâmetro

```bash
git blame -L 3,3 login.txt
# 6c14eca3 (Regy Eduardo 2022-01-21 16:58:55 -0300 3) gambiarra
```

## Bisect

- Este comando usa um algoritmo de pesquisa binaria para descobrir qual commit no histórico do seu projeto introduziu um bug
- Voce o usa dizendo primeiro a uma confirmação “ruim” que e conhecida por conter o bug, e uma confirmação “boa” que e conhecida por ser antes da introdução do bug
- Em seguida, git bisect escolhe um commit entre esses dois pontos de extremidade e pergunta se o commit selecionado e “bom” ou “ruim”. Ele continua estreitando o intervalo ate encontrar o commit exato que introduziu a alteração

```bash
git checkout master
```

```bash
git checkout -b rodape
```

```bash
touch rodape.txt
```

```bash
git add rodape.txt
```

```bash
git commit -m "adding rodape.txt"
```

```bash
echo "Everything is good!" >> rodape.txt
git add rodape.txt
git commit -m "everything is good"
echo "Oh yeah" >> rodape.txt
git add rodape.txt
git commit -m "oh yeah"
echo "magnific" >> rodape.txt
git add rodape.txt
git commit -m "magnific"
echo "Bug" >> rodape.txt
git add rodape.txt
git commit -m "bug"
echo "Oh no!" >> rodape.txt
git add rodape.txt
git commit -m "oh no"
echo "Terrible." >> rodape.txt
git add rodape.txt
git commit -m "terrible"
```

- Agora vamos iniciar a busca para descobrir qual commit originou o bug

```bash
git bisect start
```

```bash
git bisect bad
```

- Agora precisamos identificar qualquer commit que esteja em uma versão estável
- Neste exemplo, vamos utilizar o commit que criou rodape.txt

```bash
git bisect good <hash_commit_criacao_rodape>
# Bisecting: 2 revisions left to test after this (roughly 2 steps)
# [f9b75d87d62253ba420b32f868b639ab6f8c02e2] magnific
```

```bash
cat rodape.txt
# Everything is good!
# Oh yeah
# magnific
```

- Note que este commit e o magnific, o melhor momento deste repositório. Logo antes da catástrofe
- Iremos marcar como um commit seguro

```bash
git bisect good
```

```bash
cat rodape.txt
# Everything is good!
# Oh yeah
# magnific
# Bug
# Oh no!
```

```bash
git bisect bad
```

```bash
cat rodape.txt
# Everything is good!
# Oh yeah
# magnific
# Bug
```

```bash
git bisect bad
# 0ab3bc8aaf06a4a7245a63ea2a3ddf8b2b0bd23c is the first bad commit
# commit 0ab3bc8aaf06a4a7245a63ea2a3ddf8b2b0bd23c
# Author: Regy Eduardo <regyeduardo@gmail.com>
# Date:   Sat Jan 22 15:23:27 2022 -0300
#     bug
#  rodape.txt | 1 +
#  1 file changed, 1 insertion(+)
```

- Para finalizar e retornar ao estado antes do bisect start execute o comando:

```bash
git bisect reset
```

```bash
start rodape.txt
```

```bash
git add rodape.txt
```

```bash
git commit -m "resolving a bug"
```

```bash
git push --set-upstream origin rodape
```

## Cherry-pick

- Cherry-pick permite que 1 ou mais commits arbitrários sejam escolhidas e aplicados na branch corrente
  - E uma ferramenta útil, mas em muitos casos não e uma pratica recomendada
  - Problema: pode gerar commits duplicados
  - Sempre que possível utilize Merge ou Rebase tradicional para integrar os commits
  - Mesmo assim, o cherry-pick e uma ferramenta útil para cenários específicos

### Cherry-pick - casos de uso

- Ajustando commit em uma branch incorreta
- Colaboração em equipe
- Hotfixes de bug
- Restaurando commits perdidos
- Vamos realizar um exemplo deste ultimo caso

```bash
git checkout master
```

```bash
git pull
```

```bash
git checkout -b branch-sera-descartada
```

```bash
touch blog.txt
```

```bash
git add blog.txt
```

```bash
git commit -m "adding blog.txt"
```

```bash
git checkout master
```

```bash
git branch -D branch-sera-descartada
```

```bash
git reflog
# 0cdac62 (HEAD -> master, origin/master) HEAD@{0}: checkout: moving from branch-sera-descartada to master
# ee71170 HEAD@{1}: commit: adding blog.txt
# 0cdac62 (HEAD -> master, origin/master) HEAD@{2}: checkout: moving from master to branch-sera-descartada
# 0cdac62 (HEAD -> master, origin/master) HEAD@{3}: pull: Fast-forward
# 8e79986 (promocoes) HEAD@{4}: checkout: moving from rodape to master
# af23002 (origin/rodape, rodape) HEAD@{5}: commit: resolving a bug
# 35633f3 HEAD@{6}: checkout: moving from 0ab3bc8aaf06a4a7245a63ea2a3ddf8b2b0bd23c to rodape
```

```bash
git cherry-pick ee71170
# [master c45714c] adding blog.txt
#  Date: Sat Jan 22 16:05:23 2022 -0300
#  1 file changed, 0 insertions(+), 0 deletions(-)
#  create mode 100644 blog.txt
```

```bash
git hist
# * c45714c 2022-01-22 | adding blog.txt (HEAD -> master) [Regy Eduardo]
# *   0cdac62 2022-01-22 | Merge pull request #1 from regyeduardo/rodape (origin/master) [Regy Eduardo]
```

## Ignorando arquivos e diretorios

- Pode existir arquivos dentro do repositório que desejamos que o GIT ignore. Por exemplo, arquivos compilados, temporários e arquivos de configurações
- Para isso, podemos criar um arquivo chamado .gitignore que geralmente vai estar na raiz do repositório
- Dentro do .gitignore colocamos todos os arquivos e pastas para serem ignorados pelo GIT

```bash
touch .gitignore
```

```bash
echo "arquivo que sera ignorado" > arquivoIgnorado.txt
```

```bash
git status
# On branch master
# Your branch is up to date with 'origin/master'.
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         .gitignore
#         arquivoIgnorado.txt
# nothing added to commit but untracked files present (use "git add" to track)
```

```bash
echo "arquivoIgnorado.txt" > .gitignore
```

```bash
git status
# On branch master
# Your branch is up to date with 'origin/master'.
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         .gitignore
# nothing added to commit but untracked files present (use "git add" to track)
```
