<center>
<h1>Tutorial de Conexão Entre SASS e HTML</h1>

<h3> Leonardo de Souza Caetano </h3> 

19/05/2018

<br>

</center>

O Sass, como definido em sua própria documentação, é uma extensão do CSS com objetivo de acrescentar possibilidades e elegância ao CSS base. Acrescentado correções à falhas, funcionalidades e novas ferramentas, o Sass pode trazer grande melhorias e facilidades para o desenvolvedor front-end, como reusabilidade de código e melhor organização, e tudo isso com sintaxe completamente compatível ao CSS.

O Sass traz duas sintaxes diferentes: Sass e SCSS, sendo o SCSS o mais próximo do CSS. Mas apesar da proximidade, a conexão entre o SCSS e o HTML não é tão simples: é preciso usar [transpilers](https://en.wikipedia.org/wiki/Source-to-source_compiler) para converter o SCSS no CSS que alimenta o HTML. Felizmente, conseguir tal conversão é simples, e é descrita no passo-a-passo a seguir.

1. Nesse tutorial vamos usar módulos do Node.js e o editor Visual Studio Code. Certifique-se de ter o Node.js instalado antes de começar o tutorial. Caso não tenha o Node.js, você pode adquiri-lo na aba de [downloads no site da Node.js](https://nodejs.org/en/download/).
2. É preciso instalar o transpiler da Sass no seu projeto. Para isso abra o prompt de comando e, no caminho do seu projeto, digite o comando abaixo:
   
   ```cmd
    npm install -g node-sass 
    ```

3. Vamos usar [Gulp](https://gulpjs.com/) para automatizar a compilação dos nossos arquivos SCSS. Para instalá-lo abra o prompt de comando no caminho do seu projeto e digite os seguintes comandos:
   
   ```cmd
    npm install -g gulp
    npm install gulp gulp-sass
    ```

4. Crie um arquivo chamado ```gulpfile.js``` na pasta principal do seu projeto e insira o código:
   
   ```js
    var gulp = require('gulp');
    var sass = require('gulp-sass');
    gulp.task('sass', function() {
        gulp.src('./Style/*.scss')
            .pipe(sass())
            .pipe(gulp.dest(function(f) {
                return (f.base + "\CSSFile");
            }))
    });
    
    gulp.task('default', ['sass'], function() {
        gulp.watch('./Style/*scss', ['sass']);
    })

    ```
    Note que na linha 5 e 13 é informado o caminho onde os arquivos SCSS estão e na linha 8 o nome da pasta onde ficarão os arquivos CSS. Tal pasta ficará no mesmo diretório que os arquivos SCSS. Adapte os caminhos para a arquitetura do seu projeto. No exemplo acima, a arquitetura se configura da seguinte forma:
    
    ```cmd
    ├── ROOT
    │   ├── Style
    │   │   ├── CSSFile
    │   │   │   └── main.css
    │   │   └── main.scss
    │   ├── gulpfile.js
    │   └── main.html
    └───────
    ```


5. Agora precisamos configurar uma Gulp Task. No VSCode navegue ```Tasks > Configure Tasks``` e selecione ```Create tasks.json file from templates > Others```. Será gerado um arquivo tasks.json na pasta ```.vscode```. Substitua o código do arquivo pelo seguinte código:
    
    ```json
    {
        "version": "2.0.0",
        "tasks": [
            {
                "type": "gulp",
                "task": "default",
                "group": {
                    "kind": "build",
                    "isDefault": true
                }
            }
        ],
    }
    ```
    Assim que a task é executada, os arquivos CSS serão gerados e, toda vez que um
arquivo SCSS for alterado, as alterações serão feitas no arquivo CSS equivalente. Para
executar a task no VSCode tem-se o atalho ```Ctrl+Shift+B```.

5. Com os arquivos CSS gerados, o último passo é conectar o HTML principal ao CSS
principal. Para isso, cole o seguinte código na aba header do seu HTML: 

   
   ```html
    <link rel="stylesheet" href="./Style/CSSFile/main.css">
   ```

Ao final dos passos acima seu HTML está recebendo dados do SCSS, por intermédio de
arquivos CSS gerados automaticamente pela Gulp Taks. O projeto usado no tutorial encontra-se disponível no [GitHub](https://github.com/leonardoszct/SCSS-Tutorial). Para aprender mais sobre Sass e SCSS visite o [guia oficial](https://sass-guidelin.es/pt/).

Leonardo Caetano.