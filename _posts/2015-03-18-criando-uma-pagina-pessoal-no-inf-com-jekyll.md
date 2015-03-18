---
layout: post
title:  "Criando uma página pessoal no INF com Jekyll"
date:   2015-03-18 11:19:26
---

O [Jekyll](jekyllrb.com) é um gerador de códigos estáticos. A ideia é você criar páginas estáticas, usando HTML, CSS e JavaScript, pronto para serem publicados. Ele é baseado em vários formatos como [Markdown](http://daringfireball.net/projects/markdown/) para formatação de textos e posts e um padrão de template chamado [Liquid](http://liquidmarkup.org/) com um pouco de [YAML](http://yaml.org/) para exibir e guardar os dados das variáveis. 

Então, vamos lá! 

Criei um bootstrap no [github](https://github.com/acfreitas/pessoal.inf.ufg.br, basta fazer clone e começar usar. 

### Dependências

* [Ruby](https://www.ruby-lang.org/)
* [Bundler](http://bundler.io/)
* [Git](http://git-scm.com/)

### Fork

Primeiramente, faça fork do [projeto](https://github.com/acfreitas/pessoal.inf.ufg.br)  para sua conta do GitHub. Se você ainda não conhece o git, você pode conferir a [documentação](https://help.github.com/articles/fork-a-repo/)

### Travis CI

Acesse o [Travis CI](http://docs.travis-ci.com/user/getting-started/) com sua conta do GitHub e ative o projeto para o repositório que você fez fork no passo anterior. 

### Setup

    $ git clone https://github.com/acfreitas/pessoal.inf.ufg.br.git
    $ cd pessoal.inf.ufg.br
    $ bundle install

### Variáveis de Ambiente

Após criar o projeto no Travis CI, basta configurar as [variáveis de ambiente criptografadas](http://docs.travis-ci.com/user/build-configuration/#Secure-environment-variables) no Travis.

    $ travis encrypt DEPLOY_HOST="home.inf.ufg.br" --add
    $ travis encrypt DEPLOY_USERNAME="<sigadocurso><matricula>" --add # Exemplo: es2015000
    $ travis encrypt DEPLOY_PASSWORD="password" --add
    $ travis encrypt DEPLOY_REMOTEDIR="/home/alunos/msc/<sigadocurso><matricula>/public_html" --add

Feito isso, seu arquivo ```.travis.yml``` deve ficar assim: 

    language: ruby
    rvm: 2.0.0
    env:
      global:
      - secure: your-encrypt-key
      - secure: your-encrypt-key
      - secure: your-encrypt-key
      - secure: your-encrypt-key
    script:
    - bundle exec jekyll build --trace
    after_success: bundle exec travis-custom-deploy sftp service:jekyll

### _config.yml

O Jekyll possui um arquivo _config.ymk que guarda as configurações do seu projeto. Ele deve estar sempre na raiz do seu projeto. 

# Site settings
title: <Seu nome>
email: <nome>@inf.ufg.br
description: <Descrição da página>
baseurl: ""
url: "http://inf.ufg.br/~<nome>/" 
twitter_username: <twitter_username>
github_username:  <github_username>

# Build settings
markdown: kramdown