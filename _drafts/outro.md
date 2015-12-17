

Falar do stackedit e sobre o problema de timezones no jekyll

## Dia 0

Desafio: criar um app de criação e compartilhamento de playlists, em rails.
Conhecimento em ruby e ruby on rails: 0.
Status: challenge accepted.

### Update 1

Podemos usar várias versões do Ruby com RVM.

Ok, Ruby parece ser uma linguagem muito legal. Diz-se que é uma linguagem genuinamente orientada a objetos. Tudo é objeto. Por exemplo, em C, se você tem uma string e quer saber seu tamanho, escreveria **strlen(name)** enquanto que no Ruby, a própria string tem um método para isso, então seria **name.length**.

Outra coisa legal é que ao definir funções no Ruby não precisa usar chaves, e o ponto e vírgula não é necessário também, basta ir para a próxima linha.

E ao chamar um método, você pode passar os argumentos dentro de parênteses ou não.

Uso de #{expressão}

O retorno de funções também pode ser a última expressão.

Arrays e Hashes

Symbols

Statement modifiers

Regex no Ruby, .sub e .gsub

Blocks and Iterators

Yields, bloco associado

Reading and Writing

Controle de acesso

## Começando com Rails

No Rails tudo é automático, e é MVC por natureza.

ORM transforma tabelas em classes e objetos.

Hora de começar, Depot

Scaffolding

rails generate class

Criar um BD de desenvolvimento
Usar o migration pra modificar o scheme em nosso BD
Criar a tabela de produtos e usaro o scaffold pra criar um app pra mante-lo
Mudar o layout geral e o view do controller específico

Validações

rails generate scaffold Product title:string description:text image_url:string price:decimal

Como usar o devise
- mackenzie link

## Terceiro dia (segunda)

- Quais tabelas preciso ter no banco, playlists e músicas ou só músicas?
- Como implementar os favoritos?
	- http://stackoverflow.com/questions/13240109/implement-add-to-favorites-in-rails-3-4
	- http://guides.rubyonrails.org/association_basics.html


## Quarto dia (terça)

Hora de começar
- Controller Welcome pra página inicial
- Controller playlist
- Como é o link_to mesmo?
	- http://guides.rubyonrails.org/routing.html
	- butto_to não funcionou, mesmo usando :method => get
- Criando o scaffold das playlists e musicas
	- Como relacionar as duas?
		- rails g scaffolding Nome atributo:references
	-
