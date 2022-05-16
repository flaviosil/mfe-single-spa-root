# MicroFrontend com SingleSPA

### Criar uma aplicação shell (root)

```bash
npx create-single-spa --moduleType shell
```
responda as perguntas para configurar o módulo de acordo com a necessidade

ex.:
```bash
? Directory for new project .
? Which package manager do you want to use? npm
? Will this project use Typescript? Yes
? Would you like to use single-spa Layout Engine Yes
? Organization name (can use letters, numbers, dash or underscore) tagme
```

### Criar uma aplicação react com SingleSPA
```bash
npx create-single-spa --framework react
```

### Criar uma aplicação angular com SingleSPA
```bash
npx create-single-spa --framework angular
```

Após a criação do projeto single-spa, você já pode acessar o diretório do projeto e rodar a aplicação

```bash
cd {{projeto}}
npm start
```

## Integrando a apliação MFE à aplicação shell

## React

Assim que a aplicação estiver inicializada, ao acessar a página raiz teremos uma página informando que a aplicação está em modo "integrado"

na aplicação `shell`, edite o arquivo src/index.js no trecho onde definimos o importmap.

ex.:
```html
<script type="systemjs-importmap">
	{
		"imports": {
			"@tagme/shell": "//localhost:9000/tagme-shell.js",
			"@tagme/app1": "//localhost:8080/tagme-app1.js",
			"tagme-app2": "//localhost:4200/main.js"
		}
	}
</script>
```

é possivel também utilizar um arquivo externo para definir o importmap. nesse caso, devemos utilizar

```html
<script type="systemjs-importmap" src="/importmap.json"></script>
```

### Modo Standalone
Para rodar a aplicação no modo `standalone`
```bash
npm run start:standalone
```

## Angular

na aplicação `shell`, edite o arquivo src/index.js no trecho onde definimos o importmap adicionando o host e o arquivo `main.js` caso não haja um nome customizado para o mesmo.

```html
<script type="systemjs-importmap">
	{
		"imports": {
			"tagme-app2": "//localhost:4200/main.js"
		}
	}
</script>
```
### Modo Standalone
Para rodar a aplicação no modo `standalone` precisamos: 

- configurar o `angular.json` para um projeto single-spa outro standalone.
- criar um nodo tsconfig para a versão standalone
- alterar os scripts no package.json para os comandos single-spa e standalone

** Deixarei um projeto com essa característica

## Problemas conhecidos
### React
- Para migrar uma aplicação existente para o single-spa, você pode seguir as instruções: 
 	- https://single-spa.js.org/docs/faq/#create-react-app
	- https://single-spa.js.org/docs/migrating-existing-spas#converting-spas-into-registered-applications

### Angular
- Verifique sempre a compatibilidade entre sua versão do Angular e a versão do single-spa
- Certifique se o `zone.js` está no incluso no arquivo `src/index.ejs` no app `shell`
```html
<script src="https://cdn.jsdelivr.net/npm/zone.js@0.11.3/dist/zone.min.js"></script>
```
- Nos primeiros testes acessando um MFE Angular sem o angular-router, o MFE foi carregado com sucesso porém, sem o wrapper