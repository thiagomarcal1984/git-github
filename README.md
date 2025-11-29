# Compartilhando projetos
## Conheça o GitHub
São apenas instruções para criação da conta site do GitHub.

## Criando um repositório
Nada de novo: nome único de repositório por usuário.

Destaque para a definição da visibilidade do repositório: ele pode ser público ou privado.

## Instalando o Git
Difeenciando: Git é a ferramenta de linha de comando para versionar código; GitHub é um servidor para armazenamentos de repositórios criados com a ferramenta Git.

[Link para download da ferramenta Git](https://git-scm.com/downloads)

Após a configuração do Git, pode ser necessário modificar as configurações das variáveis de ambiente.


O comando abaixo inicializa um novo repositório Git (caso ele não tenha sido inicializado):
```bash
git init
```
## Sincronizando repositórios
Como realizar o commit inicial:
```bash
# Adiciona todos os arquivos que estão dentro do diretório corrente.
git add .
# Realiza o commit do repositório e acrescenta uma mensagem.
git commit -m "Projeto inicial"
```
Caso o commit não seja realizado, possivelmente é porque falta configurar o e-mail e o nome da pessoa que está atuando no código: 
```bash
# Configura o e-mail e o nome do usuário que fará o commit: 
git config --global user.email meu@email.com
git config --global user.name "Meu Nome"
```
> A flag `--global` indica que essas configurações se aplicarão a qualquer repositório no computador do usuário, ele não se limita a um repositório local.

Mudando o nome do branch (a flag `-m` significa `move`, de renomear):
```bash
# Mudando o nome do ramo atual para main.
git branch -m main
```

Definindo o servidor git remoto e empurrando (pushing) o código para o remoto: 
```bash
git remote add ssh https://github.com/thiagomarcal1984/git-github
git push -u ssh main
```
> A flag `-u` ou `--set-upstream` cria um vínculo entre o branch remoto e o branch local. Assim, o envio do código é simplificado porque você não precisa informar qual o branch local (origem) e o branch remoto (destino).

> Desta vez a ideia é usar o SSH ao invés do HTTPS. Perceba a mudança do nome do servidor remoto para `ssh` ao invés de `origin`
> Para se comunicar com o GitHub via SSH, você vai precisar de uma chave SSH. Para criá-la, acesse as configurações da conta e em seguida procure a opção "SSH and GPG keys". Ao clicar nessa opção, haverá 3 campos:
> 1. Title (nome do computador);
> 2. Key type (deixe marcada a opção Authentication Key); e
> 3. Key (a chave SSH que será gerada a partir do terminal).
> 
> A chave é gerada com o seguinte comando:
> ```
> ssh-keygen -t ed25519 -C "tma@cdtn.br"
> ```
> 
> ```
> # Saída: 
> Generating public/private ed25519 key pair.
> Enter file in which to save the key (C:\Users\Thiago/.ssh/id_ed25519): 
> Enter passphrase (empty for no passphrase): 
> Enter same passphrase again: 
> Your identification has been saved in C:\Users\Thiago/.ssh/id_ed25519
> Your public key has been saved in C:\Users\Thiago/.ssh/id_ed25519.pub
> The key fingerprint is:
> SHA256:R/Q25Jvzfe0AeCgko/2wdr7pFEX7k3yApYoNPlmjRlk thiagomarcal1984@gmail.com
> The key's randomart image is:
> +--[ED25519 256]--+
> |        E o o    |
> |       o o O     |
> |      * + * *    |
> |     = @ = * *   |
> |    . X S + @ .  |
> |     . = + . * ..|
> |      o +     o +|
> |     . + .     o.|
> |       .=.      .|
> +----[SHA256]-----+
> ```
> Após a execução do comando, copie o conteúdo da chave pública e cole no campo `Key` no GitHub:
> ```bash
> type C:\Users\Thiago\.ssh\id_ed25519.pub
> ```

# Colaborando em projetos
## Clonando um repositório
Para clonar o repositório, use o comando `git clone [url]`:
```bash
git clone https://github.com/thiagomarcal1984/git-github
```

## Realizando um commit
Use o `git status` para saber quais arquivos estão em processo de mudança.

Vamos adicionar todas as mudanças para a staging area com o comando:
```bash
git add .
```

Uma vez que os arquivos foram adicionados à staging area, fazemos o commit: 
```bash
git commit -m "Realizando um commit"
```

Para ver os commits, execute o comando `git log`.

## Enviando commits
Para ver os repositórios remotos, use o comando:
```bash
git remote
```
Caso queira ver a URL dos remotos, use o comando `git remote -v` (verboso).

Para adicionar colaboradores a um projeto, vá nas configurações do repositório e procure a opção `Collaborators`. Nela você pode acrescentar o nome do usuário que terá permissão para escrever no repositório do GitHub. 

A pessoa acrescentada ainda precisará aceitar o convite para ser colaborador do repositório.

## Baixando novos commits
Para baixar as alterações do repositório remoto, use o comando:
```bash
git pull origin main
```

# Utilizando Git na IDE
## Git no VSCode
Nada de muito novo: o uso do Source Control a partir do VS Code é mais simples que a linha de comando do Git.

## Simulando um conflito
A simulação é simples: 
1. dois usuários baixam o mesmo repositório e trabalham no mesmo branch;
2. uma pessoa muda uma linha no código, faz commit e sobe o commit para o repositório remoto;
3. outra pessoa muda a mesma linha, faz commit e faz um pull do repositório;
4. a pessoa que faz o pull tem seu arquivo modificado com a identificação dos conflitos.

## Resolvendo conflitos
Conflitos envolvem necessariamente em criar um novo commit sobre aquele que originou o conflito. Nada de usar a flag `--amend`: é necessário um novo commit mesmo.

Exemplo de conflito depois do `git pull`:
```js
<<<<<<< HEAD
let numeroLimite = 30;
=======
let numeroLimite = 50;
>>>>>>> 1f7e18e3f174e0441afecd4f24b7336107f759b1
```
A `<<< HEAD/Current Change` corresponde à mudança no repositório local, enquanto `>>> hash/Incoming Change` corresponde à mudança no repositório remoto.

Edite o arquivo com conflito e crie o novo commit com a resolução do conflito.

# Voltando no tempo
## Desfazendo um commit
O comando `git revert` desfaz o commit referenciado pela ID que você fornecer.

Exemplo: o snapshot A aparece antes do snapshot B. O `git revert [hash-B]` pega o ID do snapshot B e o código fica igual ao do snapshot A, mas com um novo ID de snapshot. No final, o log do git terá 3 ids de snapshots/commits.

## Resetando um commit
Para apagar um commit, use o comando abaixo:
```bash
git reset --hard [id-do-snapshot-para-onde-voltar]
# exemplo: git reset --hard 4a46fd9
```
> É por isso que geralmente usamos a variável `head` seguida de um til e o número de snapshots para voltar (ex.: `head~2`): a ideia não é apagar um commit, mas sim cortar os snapshots até um commit específico.

## Alterando o último commit
Para corrigir/emendar o commit atual, use o comando abaixo:
```bash
git commit --amend -m "Mensagem"
```

# Mais recursos
## Readme do repositório
Crie um arquivo com o nome `README.md` para criar um arquivo com a linguagem de marcação markdown.

- [Referências de Markdown no site do Wordpress](https://wordpress.com/support/markdown-quick-reference/)
- [Editor de Markdown MEditor.md](https://pandao.github.io/editor.md/en.html)

## Ignorando arquivos no repositório
O arquivo `.gitignore` lista todos os arquivos e diretórios que não queremos versionar no repositório git.

Para exemplificar, foi criado o arquivo `.gitignore` que menciona o diretório `temp/`. O diretório e seu conteúdo não serão versionados.

Há sites como o [gitignore.io](https://www.toptal.com/developers/gitignore/) que facilitam a criação de arquivos `.gitignore` a partir das linguagens que você informar para o site.

## Compartilhando códigos com Gist
O Gist é um recurso do GitHub que permite "criar arquivos de descrição de parte do código que você está desenvolvendo". Um Gist possui recursos de acrescentar vários arquivos diferentes com conteúdos diferentes do repositório original (o conteúdo dos arquivos costuma ser mais enxuto que o original).

Os Gists possuem uma URL, que pode ser pública ou privada. Daí você pode compartilhar essa URL para quem se interessar.
