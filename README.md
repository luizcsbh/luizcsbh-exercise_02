# Exercicio 2 de Python
 
## 1 - Como organizar modelos em modulos?

Um scripty Python é considerao como Módulo, um diretório(pasta) é indicado como pacote quando este conta com a presença do arquivo __int__.py
Um pacote pode conter vários outros módulos
Então pasta criar uma pasta models e dentro desta pasta criar um arquivo arquivo __int__.py
 onde terá os import dos módulos

## 2 - Como criar modelos com heranças? De quais tipos?
```
from django.db import models

class Pessoa(models.Model):
    nome = models.CharField(max_length=100)
    cpf = models.CharField(max_length=11)
    datanascimento = models.DateField()
    endereco = models.CharField(max_length=100)
    genero = models.CharField(max_length=9)

class Cliente(Pessoa):
    compra_sempre = models.BooleanField(default=False)

class Funcionario(Pessoa):
    matricula = models.DecimalField(max_digits=5)
    ctps = models.CharField(max_length=25)
    salario = models.DecimalField(max_digits=15, decimal_places=2)
    departamento = models.CharField(max_length=30)

class Gerente(Funcionario):
    plr = models.DecimalField(max_digits=15, decimal_places=2)
    
```

## 3 - Como criar Enumeration type e usar como choices?

```
from django.db import modelsclass UF(models.Model):
    # Constants in Model class
    ACRE = 'AC'	
    ALAGOAS = 'AL'
    AMAPA = 'AP'
    AMAZONAS = 'AM'
    BAHIA = 'BA'
    CEARA = 'CE'
    DISTRITO FEDERAL = 'DF'
    ESPIRITO SANTO = 'ES'
    GOAIS = 'GO'
    MARANHAO = 'MA'
    MATO GROSSO = 'MT'
    MATO GROSSO DO SUL = 'MS'
    MINAS GERAIS = 'MG'
    PARA = 'PA'
    PARAIBA = 'PB'
    PARANA = 'PR'
    PERNAMBUCO = 'PE'
    PIAUI = 'PI'
    RIO DE JANEIRO = 'RJ'
    RIO GRANDE DO NORTE = 'RN'
    RIO GRANDE DO SUL = 'RS'
    RONDONIA = 'RO'
    RORAIMA = 'RR'
    SANTA CATARINA = 'SC'
    SAO PAULAO = 'SP'
    SERGIPE = 'SE'
    TOCANTIS = 'TO'
    ESTADOS_BRASIL_CHOICES = (
        (ACRE,'Acre'),
        (ALAGOAS, 'Alagoas'),
        (AMAPA = 'Amapa'),
        (AMAZONAS, 'Amazonas'),
        (BAHIA, 'Bahia'),
        (CEARA, 'Ceara'),
        (DISTRITO FEDERAL, 'Distrito Federal')
        (ESPIRITO SANTO, 'Espirito Santo'),
        (GOAIS, 'Goias'),
        (MARANHAO, 'Maranhao'),
        (MATO GROSSO, 'Mato Grosso'),
        (MATO GROSSO DO SUL, 'Mato Grosso do Sul'),
        (MINAS GERAIS, 'Minas Gerais'),
        (PARA, 'Para'),
        (PARAIBA, 'Paraiba'),
        (PARANA, 'Parana'),
        (PERNAMBUCO, 'Pernambuco'),
        (PIAUI, 'Piaui')
        (RIO DE JANEIRO, 'Rio de Janeiro'),
        (RIO GRANDE DO NORTE, 'Rio Grande do Norte'),
        (RIO GRANDE DO SUL, 'Rio Grande do Sul'),
        (RONDONIA, 'Rondonia'),
        (RORAIMA, 'Roraima'),
        (SANTA CATARINA, 'Santa Catarina'),
        (SAO PAULAO, 'Sao Paulo'),
        (SERGIPE, 'Sergipe'),
        (TOCANTIS, 'Tocantis'),
    )
    estados_brasil = models.CharField(
        max_lenght=2,
        choices=ESTADOS_BRSIL,
        default=ACRE,
    )
```

## 4 - Queries: https://docs.djangoproject.com/en/3.0/topics/db/queries/

```
from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def __str__(self):
        return self.name

class Author(models.Model):
    name = models.CharField(max_length=200)
    email = models.EmailField()

    def __str__(self):
        return self.name

class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateField()
    mod_date = models.DateField()
    authors = models.ManyToManyField(Author)
    number_of_comments = models.IntegerField()
    number_of_pingbacks = models.IntegerField()
    rating = models.IntegerField()

    def __str__(self):
        return self.headline
```

## 5 - O que é, para que serve e como usar um Proxy no modelo

Ao usar a herança de várias tabelas , uma nova tabela de banco de dados é criada para cada subclasse de um modelo. Esse é geralmente o comportamento desejado, pois a subclasse precisa de um local para armazenar quaisquer campos de dados adicionais que não estão presentes na classe base. Às vezes, no entanto, você deseja alterar apenas o comportamento Python de um modelo - talvez alterar o gerenciador padrão ou adicionar um novo método.

É para isso que serve a herança do modelo de proxy: criar um proxy para o modelo original. Você pode criar, excluir e atualizar instâncias do modelo de proxy e todos os dados serão salvos como se você estivesse usando o modelo original (sem proxy). A diferença é que você pode alterar itens como a ordem do modelo padrão ou o gerente padrão no proxy, sem precisar alterar o original.

Modelos de proxy são declarados como modelos normais. Você diz ao Django que é um modelo de proxy configurando o proxyatributo da Metaclasse para True.

```
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)

class MyPerson(Person):
    class Meta:
        proxy = True

    def do_something(self):
        # ...
        pass
```
fonte: https://docs.djangoproject.com/en/3.0/topics/db/models/#proxy-models

## 6 - O que faz o método __srt__()?

__str__ é um método especial, como __init__, usado para retornar uma representação de string de um objeto.

Por exemplo, aqui está um método str para objetos Time:

```
# dentro da classe Time:

    def __str__(self):
        return '%.2d:%.2d:%.2d' % (self.hour, self.minute, self.second)
```

## 7 - Quais e para que servem as propriedades adicionais dos tipos de relacionamento/mapeamento?

To define a many-to-one relationship, use ForeignKey
on_delete=models.CASCADE
CASCADE (Cascade deletes.)
PROTECT (Prevent deletion of the referenced object by raising ProtectedError, a subclass of django.db.IntegrityError.)
SET_NULL (Set the ForeignKey null; this is only possible if null is True.)
SET_DEFAULT (Set the ForeignKey to its default value; a default for the ForeignKey must be set.)
SET() (Set the ForeignKey to the value passed to SET(), or if a callable is passed in, the result of calling it.)
DO_NOTHING (Take no action. If your database backend enforces referential integrity, this will cause an IntegrityError unless you manually add an SQL ON DELETE constraint to the database field.)

To define a many-to-many relationship, use ManyToManyField.
ManyToManyField.symmetrical (Only used in the definition of ManyToManyFields on self. Consider the following model:)

To define a one-to-one relationship, use OneToOneField.
OneToOneField.parent_link (When True and used in a model which inherits from another concrete model, indicates that this field should be used as the link back to the parent class, rather than the extra OneToOneField which would normally be implicitly created by subclassing.)
