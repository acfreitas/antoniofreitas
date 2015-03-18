---
layout: post
title:  "Criando uma página pessoal no INF com Jekyll"
date:   2015-03-18 11:19:26
---

O [Jekyll](http://jekyllrb.com) é um gerador de códigos estáticos. A ideia é você criar páginas estáticas, usando HTML, CSS e JavaScript, pronto para serem publicados. Ele é baseado em vários formatos como [Markdown](http://daringfireball.net/projects/markdown/) para formatação de textos e posts, [Liquid](http://liquidmarkup.org/) para templates e [YAML](http://yaml.org/) para exibir e guardar os dados das variáveis. 

### Dependências

* [Ruby](https://www.ruby-lang.org/)
* [Bundler](http://bundler.io/)
* [Git](http://git-scm.com/)

### Fork

Primeiramente, faça fork do [projeto](https://github.com/acfreitas/pessoal.inf.ufg.br)  para sua conta do GitHub. Se você ainda não conhece o git, você pode conferir a documentação [aqui](https://help.github.com/articles/fork-a-repo/)

### Travis CI

Acesse o [Travis CI](http://docs.travis-ci.com/user/getting-started/) com sua conta do GitHub e ative o projeto para o repositório que você fez fork no passo anterior. 

### Clone

Faça clone do projeto para seu repositório local e execute o Bundler para instalar as dependências.

    $ git clone https://github.com/<username>/pessoal.inf.ufg.br.git
    $ cd pessoal.inf.ufg.br
    $ bundle install

### Variáveis de Ambiente

Após criar o projeto no Travis CI, basta configurar as [variáveis de ambiente criptografadas](http://docs.travis-ci.com/user/build-configuration/#Secure-environment-variables) .

    $ travis encrypt DEPLOY_HOST="home.inf.ufg.br" --add
    $ travis encrypt DEPLOY_USERNAME="<sigadocurso><matricula>" --add # Exemplo: es2015000
    $ travis encrypt DEPLOY_PASSWORD="password" --add
    $ travis encrypt DEPLOY_REMOTEDIR="/home/alunos/<sigadocurso><matricula>/public_html" --add

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

O Jekyll possui um arquivo _config.yml que guarda as configurações do seu projeto. Ele deve estar sempre na raiz do seu projeto. 

    # Site settings
    title: <Seu nome>
    email: <sigladocurso><matricula>@inf.ufg.br
    description: <Descrição da página>
    baseurl: ""
    url: "http://inf.ufg.br/~<nome>/" 
    twitter_username: <twitter_username>
    github_username:  <github_username>

    # Build settings
    markdown: kramdown

### Deploy 

Para fazer deploy, basta fazer commit no GitHub que o Travis CI irá publicar o projeto automaticamente. 

    $ git add .
    $ git commit -m "Descrição" 
    $ git push

### Teste

Acessa a URL ```inf.ufg.br/~<sigladocurso><matricula>/```

### Referências 

* [Jekyll](http://jekyllrb.com/) 
* [Markdown](http://daringfireball.net/projects/markdown/)
* [Liquid](http://liquidmarkup.org/) 
* [YAML](http://yaml.org/)
* [Git](http://git-scm.com/)
* [GitHub](https://github.com/)
* [Ruby](https://www.ruby-lang.org/)
* [Bundler](http://bundler.io/)
* [Travis CI](https://travis-ci.org/) 
* [Servindo sites estáticos com Jekyll](http://tableless.com.br/jekyll-servindo-sites-estaticos/)
* [Deploy a Jekyll page with Travis to your own server](http://jens-na.github.io/2014/01/22/jekyll-deploy-own-server/)