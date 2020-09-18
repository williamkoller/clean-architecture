# FILES-API MICROSERVICE
### Iremos Falar sobre Arquitetura Limpa que foi desenvolvido no FILES-API;

#### O que é esse monte de pastas? Cadê a pasta de helpers, controllers, models e etcs.

Ótima pergunta, nesse projeto estamos aplicando conceitos do Clean Architecture, Design Pattern, SOLID e TDD, essa organização de pastas e arquitetura nos permite isolar nossa regra de negócio, ou seja, as libs que utilizamos aqui nesse repositório podem ser trocadas sem gerar impacto ao funcionamento do nosso projeto! E é esse mindset que nos permite utilizar o mesmo código do FILES-API não somente com express, bcrypt, datastore e bucket, mas com outras libs e frameworks também. E qual a finalidade de cada uma das pastas?

### Domain:
Como o nome diz, é responsável pelo nosso domínio, mas afinal o que isso significa? Isso quer dizer que todas as nossas models de entidades (informações do certificado por exemplo) e suas respectivas interfaces para manipulação das mesmas ficam aqui.

* UseCases: São os nossos casos de uso para o nosso domínio, ou seja, é o que nossos métodos irão receber e retornar para manipular alguma entidade do nosso domínio, pense como se fosse alguma ação que o nosso sistema deveria fazer.
* Models: São as interfaces de cada entidade que iremos utilizar.

### Data:
Aqui nós implementamos os métodos que manipulam o nosso domínio, e como funcionam as nossas ferramentas auxiliares (Hasher por exemplo) e aqui vem um dos conceitos mais importantes, no nosso data layer nós não podemos ter libs externas e nem referênica direta a códigos externos, para isso iremos utilizar o conceito de Adapter, que será explicado na pasta de Infra.

* UseCases: São a implementação dos casos de uso definidos no nosso domínio.
Hasher: É responsável por ter a assinatura que irá receber a lib do bcrypt do Infra Layer.
* RemoteCreateFile: É responsável por ter a classe que irá implementar CreateFile com suas respectivas responsabilidades.
* DbLoadTemplateId: É responsável por ter a classe que contém assinatura que receberá como paramêtro a interface do templateHTML e templateSlug.

### Infra:
Aqui nós iremos fazer a ponte entre as libs desse mundão e o nosso código. E por falar nisso, enquanto você lê esse readme, nasceu mais um framework Javascript. Aqui nós iremos criar os nossos Adapters das libs que iremos utilizar no projeto, e o que são esses adapters? Os adapters são responsáveis por adequar o funcionamento de um código externo com a nossa aplicação. Vamos pegar por exemplo o HMTL-PDF, ele é uma lib para gerar PDFs na aplicação. Dessa forma quando formos trocar de lib, nós só precisamos criar um adapter que implemente essas interfaces, sem tocar em uma linha da nossa regra de negócio, legal né?

* HTML-PDF-ADAPTER: Contém os nossos adapter do HTML-PDF para gerar nossos PDFs.
Google-Storage: Contém a função que recebe o PDF gerado e envia-o para Bucket/Storage.
* Http: Contém os nossos adapter do axios para fazer requests.

### Presentation:
Aqui nós iremos fazer a ponte entre o Domain Layer utilizando seus UseCases e AxiosAdapters, podemos notar que iremos utilizar os controllers com suas assinaturas e utilizando suas interfaces HttpRequest e HttpResponse. Iremos utilizar os Https para receber e enviar os dados que vem do body da requisição e nenhum momento desta camada irá perceber que utilizaremos o express.
CreateFileController: É responsãvel por ter a assinatura que irá implementar as interfaces dos Https;
* Helpers/Http: É responsável por ter as implementações do HttpResponse com seus respecetivos statusCode e body.
* Protocols: É responsável por ter as assinaturas dos  Controller, Https e Middlewares.

### Main: 
E aqui estamos na camada mais esperada, Main Layer é responsável pelo Adapter do Express, aqui criamos o adapter que irá receber os dados da requisição. E por via de qualquer bug na lib, este é a camada que poderemos trocar o express por exemplo pelo Hapi e por isso não ficamos preso ao Express. Main Layer é responsável por toda parte de configuração da aplicação tais como: BodyParser, Cors, noCache, Routes e Middlewares. Podemos notar que aqui utilizamos o Pattern Factory, criamos uma função que irá receber como retorno o controler do Presentation Layer e ela receberá os:
* RemoceCreatefile: É responsável por ter a classe que irá implementar CreateFile com suas respectivas responsabilidades.
* CreateFileController: É responsãvel por ter a assinatura que irá implementar as interfaces dos Https;
* E é no Main Layer que estartamos o projeto com o express;


## Tá, mas e os testes? :microscope:
Gostei de você! Um projeto importante como esse deveria se preocupar com testes, e esse aqui foi desenvolvido com TDD desde o início! Nossos testes ficam em pastas dentro de dada camada.

## E eu posso contribuir? :rocket:

É claro! Basta você solicitar a equipe de security acesso ao repo. Como a estrutura desse projeto é um pouco diferente do normal, fique a vontate para usar os nossos canais no zoom para tirar suas dúvidas. PRs são extremamente bem vindas, Happy Coding!
## E o pai da criança? :baby:
Caso você queira entender o objetivo do projeto, por onde começar a contribuir, dar sugestões, ou precisa de alguma indicação pra começar a estudar os conceitos aplicados nesse projeto, é só chamar o @WilliamKoller que ele irá explicar com o maior prazer!
