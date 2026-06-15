# Padrões de Projeto GOF

## Contexto

Os padrões de projeto são soluções reaproveitáveis para problemas recorrentes no desenvolvimento de software orientado a objetos. Eles ajudam equipes a projetar sistemas robustos, flexíveis e fáceis de manter, evitando decisões arquiteturais ad hoc.

Os padrões GOF são um conjunto clássico descrito no livro "Design Patterns: Elements of Reusable Object-Oriented Software", publicado em 1994 pelos autores Erich Gamma, Richard Helm, Ralph Johnson e John Vlissides. Esses padrões são utilizados até hoje como referência para a construção de software de qualidade.

## Problemática

Projetar software sem padrões pode levar a:

- Código rígido, difícil de modificar.
- Alto acoplamento entre classes.
- Repetição de lógica e duplicação de código.
- Difícil extensão de funcionalidades existentes.
- Baixa clareza sobre responsabilidades entre objetos.

Os padrões GOF surgiram para resolver essas fragilidades, oferecendo soluções testadas para problemas comuns de arquitetura, criação e comportamento de objetos.

## Objetivos

Ao final deste material, o aluno deverá ser capaz de:

1. Entender a motivação histórica por trás dos padrões GOF.
2. Reconhecer a classificação dos padrões em Criação, Estruturais e Comportamentais.
3. Identificar quando aplicar cada padrão.
4. Implementar exemplos em Python para cada padrão GOF.
5. Avaliar vantagens e limitações de cada padrão.

## História dos padrões GOF

O termo "padrão de projeto" foi popularizado por Christopher Alexander no contexto da arquitetura. No desenvolvimento de software, os quatro autores do livro GOF formalizaram 23 padrões comuns em desenvolvimento orientado a objetos.

A publicação em 1994 consolidou uma linguagem comum entre desenvolvedores e projetistas. Desde então, esses padrões foram adotados em várias linguagens, incluindo C++, Java, Python e outras, tornando-se componente essencial no currículo de engenharia de software.

## Classificação dos padrões GOF

Os 23 padrões GOF são divididos em três grandes grupos:

1. Padrões de Criação
   - Abstract Factory
   - Builder
   - Factory Method
   - Prototype
   - Singleton

2. Padrões Estruturais
   - Adapter
   - Bridge
   - Composite
   - Decorator
   - Facade
   - Flyweight
   - Proxy

3. Padrões Comportamentais
   - Chain of Responsibility
   - Command
   - Interpreter
   - Iterator
   - Mediator
   - Memento
   - Observer
   - State
   - Strategy
   - Template Method
   - Visitor

## Exemplos de código em Python

A seguir, cada padrão é apresentado com um exemplo em Python, explicando o propósito e a aplicação. Para cada padrão há também um trecho que mostra a violação comum e a correção usando o padrão.

---

### 1. Abstract Factory

**Propósito:** Fornecer uma interface para criar famílias de objetos relacionados, sem especificar suas classes concretas.

**Quando usar:** Quando uma aplicação precisa ser independente de como seus objetos são criados e deseja garantir que produtos relacionados sejam compatíveis.

**Exemplo que viola o padrão:**
```python
sofa = ModernSofa()
chair = VictorianChair()
print(sofa.style())
print(chair.style())
```

**Problema:** O cliente conhece as classes concretas e pode montar famílias incompatíveis. A lógica de criação fica espalhada e aumenta o acoplamento.

**Solução usando Abstract Factory:**
```python
from abc import ABC, abstractmethod

class Sofa(ABC):
    @abstractmethod
    def style(self) -> str:
        pass

class Chair(ABC):
    @abstractmethod
    def style(self) -> str:
        pass

class ModernSofa(Sofa):
    def style(self) -> str:
        return "Sofá moderno"

class ModernChair(Chair):
    def style(self) -> str:
        return "Cadeira moderna"

class VictorianSofa(Sofa):
    def style(self) -> str:
        return "Sofá vitoriano"

class VictorianChair(Chair):
    def style(self) -> str:
        return "Cadeira vitoriana"

class FurnitureFactory(ABC):
    @abstractmethod
    def create_sofa(self) -> Sofa:
        pass

    @abstractmethod
    def create_chair(self) -> Chair:
        pass

class ModernFurnitureFactory(FurnitureFactory):
    def create_sofa(self) -> Sofa:
        return ModernSofa()

    def create_chair(self) -> Chair:
        return ModernChair()

class VictorianFurnitureFactory(FurnitureFactory):
    def create_sofa(self) -> Sofa:
        return VictorianSofa()

    def create_chair(self) -> Chair:
        return VictorianChair()


def client_code(factory: FurnitureFactory) -> None:
    sofa = factory.create_sofa()
    chair = factory.create_chair()
    print(sofa.style())
    print(chair.style())


if __name__ == "__main__":
    print("Usando móveis modernos:")
    client_code(ModernFurnitureFactory())
    print("\nUsando móveis vitorianos:")
    client_code(VictorianFurnitureFactory())
```

---

### 2. Builder

**Propósito:** Separar a construção de um objeto complexo de sua representação, permitindo a criação passo a passo.

**Quando usar:** Quando a construção de um objeto deve permitir variações independentes da representação interna.

**Exemplo que viola o padrão:**
```python
pizza = Pizza()
pizza.add("massa fina")
pizza.add("molho picante")
pizza.add("pepperoni")
```

**Problema:** O cliente deve conhecer cada etapa da montagem, o que dificulta a reutilização e gera código de montagem duplicado.

**Solução usando Builder:**
```python
from abc import ABC, abstractmethod

class Pizza:
    def __init__(self):
        self.ingredients = []

    def add(self, ingredient: str) -> None:
        self.ingredients.append(ingredient)

    def __str__(self) -> str:
        return f"Pizza com: {', '.join(self.ingredients)}"

class PizzaBuilder(ABC):
    @abstractmethod
    def add_dough(self) -> None:
        pass

    @abstractmethod
    def add_sauce(self) -> None:
        pass

    @abstractmethod
    def add_toppings(self) -> None:
        pass

    @abstractmethod
    def get_pizza(self) -> Pizza:
        pass

class MargheritaBuilder(PizzaBuilder):
    def __init__(self):
        self.pizza = Pizza()

    def add_dough(self) -> None:
        self.pizza.add("massa tradicional")

    def add_sauce(self) -> None:
        self.pizza.add("molho de tomate")

    def add_toppings(self) -> None:
        self.pizza.add("queijo")

    def get_pizza(self) -> Pizza:
        return self.pizza

class SpicyBuilder(PizzaBuilder):
    def __init__(self):
        self.pizza = Pizza()

    def add_dough(self) -> None:
        self.pizza.add("massa fina")

    def add_sauce(self) -> None:
        self.pizza.add("molho picante")

    def add_toppings(self) -> None:
        self.pizza.add("pepperoni")
        self.pizza.add("pimenta")

    def get_pizza(self) -> Pizza:
        return self.pizza

class Cook:
    def __init__(self, builder: PizzaBuilder) -> None:
        self._builder = builder

    def make_pizza(self) -> Pizza:
        self._builder.add_dough()
        self._builder.add_sauce()
        self._builder.add_toppings()
        return self._builder.get_pizza()


if __name__ == "__main__":
    print(Cook(MargheritaBuilder()).make_pizza())
    print(Cook(SpicyBuilder()).make_pizza())
```

---

### 3. Factory Method

**Propósito:** Definir uma interface para criar um objeto, mas deixar as subclasses decidirem qual classe concreta instanciar.

**Quando usar:** Quando uma classe não pode antecipar a classe de objetos que precisa criar.

**Exemplo que viola o padrão:**
```python
if document_type == "pdf":
    document = PDFDocument()
elif document_type == "word":
    document = WordDocument()
```

**Problema:** A lógica de criação permanece no cliente. Toda nova variante exige alteração no cliente, violando o princípio aberto/fechado.

**Solução usando Factory Method:**
```python
from abc import ABC, abstractmethod

class Document(ABC):
    @abstractmethod
    def create(self) -> str:
        pass

class PDFDocument(Document):
    def create(self) -> str:
        return "Documento PDF criado"

class WordDocument(Document):
    def create(self) -> str:
        return "Documento Word criado"

class Application(ABC):
    @abstractmethod
    def factory_method(self) -> Document:
        pass

    def new_document(self) -> str:
        document = self.factory_method()
        return document.create()

class PDFApplication(Application):
    def factory_method(self) -> Document:
        return PDFDocument()

class WordApplication(Application):
    def factory_method(self) -> Document:
        return WordDocument()


if __name__ == "__main__":
    print(PDFApplication().new_document())
    print(WordApplication().new_document())
```

---

### 4. Prototype

**Propósito:** Criar novos objetos clonando instâncias existentes.

**Quando usar:** Quando a criação de um objeto é cara ou complexa, e é mais simples copiar um exemplo existente.

**Exemplo que viola o padrão:**
```python
shape = Shape("círculo", {"raio": 5, "cor": "azul"})
new_shape = Shape(shape.name, shape.properties.copy())
```

**Problema:** O cliente precisa conhecer os detalhes do objeto e fazer cópias manuais, o que é propenso a erros.

**Solução usando Prototype:**
```python
import copy

class Shape:
    def __init__(self, name: str, properties: dict):
        self.name = name
        self.properties = properties

    def clone(self):
        return copy.deepcopy(self)

    def __str__(self) -> str:
        return f"Shape({self.name}, {self.properties})"


if __name__ == "__main__":
    circle = Shape("círculo", {"raio": 5, "cor": "azul"})
    another_circle = circle.clone()
    another_circle.properties["cor"] = "vermelho"
    print(circle)
    print(another_circle)
```

---

### 5. Singleton

**Propósito:** Garantir que uma classe tenha apenas uma instância e fornecer um ponto de acesso global a ela.

**Quando usar:** Para objetos que representam recursos únicos do sistema, como configuração ou conexão de banco de dados.

**Exemplo que viola o padrão:**
```python
config1 = Configuration()
config2 = Configuration()
```

**Problema:** Sem controle de criação, múltiplas instâncias podem existir, gerando estados inconsistentes para um recurso que deveria ser único.

**Solução usando Singleton:**
```python
class SingletonMeta(type):
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Configuration(metaclass=SingletonMeta):
    def __init__(self):
        self.settings = {}


if __name__ == "__main__":
    conf1 = Configuration()
    conf2 = Configuration()
    print(conf1 is conf2)
    conf1.settings["tema"] = "escuro"
    print(conf2.settings)
```

---

### 6. Adapter

**Propósito:** Converter a interface de uma classe em outra interface que o cliente espera.

**Quando usar:** Para permitir que classes incompatíveis trabalhem juntas.

**Exemplo que viola o padrão:**
```python
socket = EuropeanSocket()
result = AmericanPlug().connect()
```

**Problema:** O cliente não tem uma forma única de usar objetos incompatíveis; a adaptação é feita no cliente e o código se torna frágil.

**Solução usando Adapter:**
```python
class EuropeanSocket:
    def connect(self) -> str:
        return "Tomada europeia conectada"

class AmericanPlug:
    def connect(self) -> str:
        return "Plug americano conectado"

class AmericanToEuropeanAdapter:
    def __init__(self, plug: AmericanPlug):
        self._plug = plug

    def connect(self) -> str:
        return self._plug.connect() + " via adaptador"


if __name__ == "__main__":
    plug = AmericanPlug()
    adapter = AmericanToEuropeanAdapter(plug)
    print(adapter.connect())
```

---

### 7. Bridge

**Propósito:** Separar abstração da implementação para que ambas possam variar independentemente.

**Quando usar:** Quando a abstração e a implementação devem ser estendidas separadamente.

**Exemplo que viola o padrão:**
```python
class VectorCircle:
    pass

class RasterCircle:
    pass
```

**Problema:** Cada combinação de forma e renderização gera uma nova classe, resultando em explosão combinatorial.

**Solução usando Bridge:**
```python
from abc import ABC, abstractmethod

class Renderer(ABC):
    @abstractmethod
    def render_circle(self, radius: int) -> str:
        pass

class VectorRenderer(Renderer):
    def render_circle(self, radius: int) -> str:
        return f"Desenhando círculo de raio {radius} com vetor"

class RasterRenderer(Renderer):
    def render_circle(self, radius: int) -> str:
        return f"Desenhando círculo de raio {radius} com pixels"

class Shape(ABC):
    def __init__(self, renderer: Renderer) -> None:
        self.renderer = renderer

    @abstractmethod
    def draw(self) -> str:
        pass

class Circle(Shape):
    def __init__(self, renderer: Renderer, radius: int) -> None:
        super().__init__(renderer)
        self.radius = radius

    def draw(self) -> str:
        return self.renderer.render_circle(self.radius)


if __name__ == "__main__":
    print(Circle(VectorRenderer(), 5).draw())
    print(Circle(RasterRenderer(), 5).draw())
```

---

### 8. Composite

**Propósito:** Compor objetos em estruturas de árvore para representar hierarquias parte-todo.

**Quando usar:** Quando clientes devem tratar objetos individuais e composições de forma uniforme.

**Exemplo que viola o padrão:**
```python
if node.is_file():
    print(node.name)
else:
    for child in node.children:
        #... processa subnós separadamente
```

**Problema:** O cliente precisa distinguir entre folha e composição, o que torna o código menos flexível e mais difícil de estender.

**Solução usando Composite:**
```python
from abc import ABC, abstractmethod

class FileSystemComponent(ABC):
    @abstractmethod
    def display(self, indent: int = 0) -> None:
        pass

class File(FileSystemComponent):
    def __init__(self, name: str):
        self.name = name

    def display(self, indent: int = 0) -> None:
        print(" " * indent + self.name)

class Directory(FileSystemComponent):
    def __init__(self, name: str):
        self.name = name
        self.children = []

    def add(self, component: FileSystemComponent) -> None:
        self.children.append(component)

    def display(self, indent: int = 0) -> None:
        print(" " * indent + self.name + "/")
        for child in self.children:
            child.display(indent + 2)


if __name__ == "__main__":
    root = Directory("root")
    root.add(File("arquivo1.txt"))
    sub = Directory("subdir")
    sub.add(File("arquivo2.txt"))
    root.add(sub)
    root.display()
```

---

### 9. Decorator

**Propósito:** Anexar responsabilidades adicionais a um objeto dinamicamente.

**Quando usar:** Para estender comportamentos em tempo de execução sem modificar classes existentes.

**Exemplo que viola o padrão:**
```python
class Notifier:
    def send_email(self, message):
        pass

    def send_sms(self, message):
        pass
```

**Problema:** A classe cresce com funcionalidades e o cliente precisa escolher entre muitos métodos; isso aumenta o acoplamento.

**Solução usando Decorator:**
```python
from abc import ABC, abstractmethod

class Notifier(ABC):
    @abstractmethod
    def send(self, message: str) -> None:
        pass

class EmailNotifier(Notifier):
    def send(self, message: str) -> None:
        print(f"Enviando email: {message}")

class NotifierDecorator(Notifier):
    def __init__(self, notifier: Notifier) -> None:
        self._notifier = notifier

    def send(self, message: str) -> None:
        self._notifier.send(message)

class SMSDecorator(NotifierDecorator):
    def send(self, message: str) -> None:
        print(f"Enviando SMS: {message}")
        super().send(message)

class SlackDecorator(NotifierDecorator):
    def send(self, message: str) -> None:
        print(f"Enviando Slack: {message}")
        super().send(message)


if __name__ == "__main__":
    notifier = SlackDecorator(SMSDecorator(EmailNotifier()))
    notifier.send("Evento importante")
```

---

### 10. Facade

**Propósito:** Fornecer uma interface unificada para um conjunto de interfaces em um subsistema.

**Quando usar:** Para simplificar a utilização de um subsistema complexo.

**Exemplo que viola o padrão:**
```python
cpu.freeze()
memory.load(0, hard_drive.read(0, 1024))
cpu.jump(0)
cpu.execute()
```

**Problema:** O cliente precisa conhecer vários objetos do subsistema e sua ordem de invocação, criando acoplamento desnecessário.

**Solução usando Facade:**
```python
class CPU:
    def freeze(self) -> str:
        return "CPU congelada"

    def jump(self, position: int) -> str:
        return f"Salto para {position}"

    def execute(self) -> str:
        return "Executando"

class Memory:
    def load(self, position: int, data: str) -> str:
        return f"Carregando {data} em {position}"

class HardDrive:
    def read(self, lba: int, size: int) -> str:
        return f"Lendo {size} bytes do LBA {lba}"

class ComputerFacade:
    def __init__(self):
        self.cpu = CPU()
        self.memory = Memory()
        self.hard_drive = HardDrive()

    def start(self) -> None:
        print(self.cpu.freeze())
        print(self.memory.load(0, self.hard_drive.read(0, 1024)))
        print(self.cpu.jump(0))
        print(self.cpu.execute())


if __name__ == "__main__":
    ComputerFacade().start()
```

---

### 11. Flyweight

**Propósito:** Reduzir o uso de memória compartilhando o máximo possível de dados entre objetos semelhantes.

**Quando usar:** Para suportar milhões de objetos leves que compartilham estado comum.

**Exemplo que viola o padrão:**
```python
pine1 = TreeType("Pinheiro", "verde", "áspero")
pine2 = TreeType("Pinheiro", "verde", "áspero")
```

**Problema:** Objetos idênticos são criados repetidamente, desperdiçando memória.

**Solução usando Flyweight:**
```python
class TreeType:
    def __init__(self, name: str, color: str, texture: str):
        self.name = name
        self.color = color
        self.texture = texture

    def draw(self, x: int, y: int) -> str:
        return f"Desenhando {self.name} em ({x}, {y}) com cor {self.color}"

class TreeFactory:
    _types = {}

    @classmethod
    def get_tree_type(cls, name: str, color: str, texture: str) -> TreeType:
        key = (name, color, texture)
        if key not in cls._types:
            cls._types[key] = TreeType(name, color, texture)
        return cls._types[key]

class Tree:
    def __init__(self, x: int, y: int, tree_type: TreeType):
        self.x = x
        self.y = y
        self.tree_type = tree_type

    def draw(self) -> str:
        return self.tree_type.draw(self.x, self.y)


if __name__ == "__main__":
    pine = TreeFactory.get_tree_type("Pinheiro", "verde", "áspero")
    oak = TreeFactory.get_tree_type("Carvalho", "verde", "liso")
    for tree in [Tree(1, 2, pine), Tree(3, 4, pine), Tree(5, 6, oak)]:
        print(tree.draw())
```

---

### 12. Proxy

**Propósito:** Fornecer um representante ou substituto para outro objeto para controlar o acesso a ele.

**Quando usar:** Para adicionar controle, proteção ou otimização ao acesso a um objeto.

**Exemplo que viola o padrão:**
```python
image = RealImage("foto.jpg")
image.display()
```

**Problema:** O cliente sempre carrega o objeto pesado, mesmo quando não precisa realmente usá-lo.

**Solução usando Proxy:**
```python
from abc import ABC, abstractmethod

class Image(ABC):
    @abstractmethod
    def display(self) -> None:
        pass

class RealImage(Image):
    def __init__(self, filename: str) -> None:
        self.filename = filename
        self._load_from_disk()

    def _load_from_disk(self) -> None:
        print(f"Carregando {self.filename}")

    def display(self) -> None:
        print(f"Exibindo {self.filename}")

class ProxyImage(Image):
    def __init__(self, filename: str) -> None:
        self.filename = filename
        self._real_image = None

    def display(self) -> None:
        if self._real_image is None:
            self._real_image = RealImage(self.filename)
        self._real_image.display()


if __name__ == "__main__":
    image = ProxyImage("foto.jpg")
    image.display()
    image.display()
```

---

### 13. Chain of Responsibility

**Propósito:** Permitir que diversos objetos tenham a chance de processar uma solicitação, encadeando os objetos receptores.

**Quando usar:** Para evitar acoplamento entre remetente e receptor, permitindo que múltiplos objetos possam tratar a solicitação.

**Exemplo que viola o padrão:**
```python
if request < 10:
    handle_request_1(request)
elif request < 20:
    handle_request_2(request)
else:
    handle_default(request)
```

**Problema:** A lógica de roteamento fica no cliente; o código não escala quando novos handlers são adicionados.

**Solução usando Chain of Responsibility:**
```python
from __future__ import annotations
from abc import ABC, abstractmethod

class Handler(ABC):
    def __init__(self, successor: "Handler" | None = None) -> None:
        self.successor = successor

    @abstractmethod
    def handle(self, request: int) -> str | None:
        pass

class ConcreteHandler1(Handler):
    def handle(self, request: int) -> str | None:
        if request < 10:
            return f"Handler1 processou {request}"
        if self.successor:
            return self.successor.handle(request)
        return None

class ConcreteHandler2(Handler):
    def handle(self, request: int) -> str | None:
        if request < 20:
            return f"Handler2 processou {request}"
        if self.successor:
            return self.successor.handle(request)
        return None


if __name__ == "__main__":
    handler = ConcreteHandler1(ConcreteHandler2())
    for request in [5, 15, 25]:
        print(handler.handle(request) or "Nenhum handler processou")
```

---

### 14. Command

**Propósito:** Encapsular uma solicitação como um objeto, permitindo parametrizar clientes com filas, comandos e operações reversíveis.

**Quando usar:** Para desacoplar emissor e receptor e permitir histórico de ações.

**Exemplo que viola o padrão:**
```python
light.on()
light.off()
```

**Problema:** O cliente chama métodos diretamente no receptor, o que impede armazenar, desfazer ou reexecutar comandos facilmente.

**Solução usando Command:**
```python
from __future__ import annotations
from abc import ABC, abstractmethod

class Command(ABC):
    @abstractmethod
    def execute(self) -> None:
        pass

class Light:
    def on(self) -> None:
        print("Luz acesa")

    def off(self) -> None:
        print("Luz apagada")

class LightOnCommand(Command):
    def __init__(self, light: Light) -> None:
        self.light = light

    def execute(self) -> None:
        self.light.on()

class LightOffCommand(Command):
    def __init__(self, light: Light) -> None:
        self.light = light

    def execute(self) -> None:
        self.light.off()

class RemoteControl:
    def __init__(self, command: Command) -> None:
        self.command = command

    def press(self) -> None:
        self.command.execute()


if __name__ == "__main__":
    light = Light()
    RemoteControl(LightOnCommand(light)).press()
    RemoteControl(LightOffCommand(light)).press()
```

---

### 15. Interpreter

**Propósito:** Definir uma representação gramatical para uma linguagem e um interpretador que avalia sentenças nessa linguagem.

**Quando usar:** Para implementar linguagens de domínio específico simples.

**Exemplo que viola o padrão:**
```python
if expression == "A or B":
    return context["A"] or context["B"]
elif expression == "A and B":
    return context["A"] and context["B"]
```

**Problema:** Incluir novas expressões exige modificar o código do cliente e aumenta a complexidade.

**Solução usando Interpreter:**
```python
from abc import ABC, abstractmethod

class Expression(ABC):
    @abstractmethod
    def interpret(self, context: dict) -> bool:
        pass

class TerminalExpression(Expression):
    def __init__(self, key: str):
        self.key = key

    def interpret(self, context: dict) -> bool:
        return context.get(self.key, False)

class OrExpression(Expression):
    def __init__(self, left: Expression, right: Expression) -> None:
        self.left = left
        self.right = right

    def interpret(self, context: dict) -> bool:
        return self.left.interpret(context) or self.right.interpret(context)

class AndExpression(Expression):
    def __init__(self, left: Expression, right: Expression) -> None:
        self.left = left
        self.right = right

    def interpret(self, context: dict) -> bool:
        return self.left.interpret(context) and self.right.interpret(context)


if __name__ == "__main__":
    expr = AndExpression(
        TerminalExpression("A"),
        OrExpression(TerminalExpression("B"), TerminalExpression("C"))
    )
    context = {"A": True, "B": False, "C": True}
    print(expr.interpret(context))
```

---

### 16. Iterator

**Propósito:** Fornecer uma maneira de acessar elementos de um agregado sequencialmente sem expor sua representação interna.

**Quando usar:** Para iterar coleções usando uma interface uniforme.

**Exemplo que viola o padrão:**
```python
for i in range(len(items)):
    print(items[i])
```

**Problema:** O cliente depende da estrutura interna da coleção e precisa gerenciar índices.

**Solução usando Iterator:**
```python
from __future__ import annotations
from abc import ABC, abstractmethod

class Iterator(ABC):
    @abstractmethod
    def has_next(self) -> bool:
        pass

    @abstractmethod
    def next(self):
        pass

class Aggregate(ABC):
    @abstractmethod
    def create_iterator(self) -> Iterator:
        pass

class NumberIterator(Iterator):
    def __init__(self, collection: list[int]) -> None:
        self._collection = collection
        self._index = 0

    def has_next(self) -> bool:
        return self._index < len(self._collection)

    def next(self):
        item = self._collection[self._index]
        self._index += 1
        return item

class NumberCollection(Aggregate):
    def __init__(self, items: list[int]) -> None:
        self._items = items

    def create_iterator(self) -> NumberIterator:
        return NumberIterator(self._items)


if __name__ == "__main__":
    collection = NumberCollection([1, 2, 3])
    iterator = collection.create_iterator()
    while iterator.has_next():
        print(iterator.next())
```

---

### 17. Mediator

**Propósito:** Definir um objeto que encapsula a forma como um conjunto de objetos interage.

**Quando usar:** Para reduzir acoplamento entre objetos que se comunicam frequentemente.

**Exemplo que viola o padrão:**
```python
component_a.do_b()
component_b.do_a()
```

**Problema:** Componentes conhecem diretamente outros componentes, criando acoplamento rígido.

**Solução usando Mediator:**
```python
from __future__ import annotations
from abc import ABC, abstractmethod

class Mediator(ABC):
    @abstractmethod
    def notify(self, sender: object, event: str) -> None:
        pass

class ConcreteMediator(Mediator):
    def __init__(self, component1: "Component", component2: "Component") -> None:
        self._component1 = component1
        self._component1.mediator = self
        self._component2 = component2
        self._component2.mediator = self

    def notify(self, sender: object, event: str) -> None:
        if event == "A":
            print("Mediator reage a A e solicita B")
            self._component2.do_c()
        elif event == "B":
            print("Mediator reage a B e solicita A")
            self._component1.do_d()

class Component:
    def __init__(self, mediator: Mediator | None = None) -> None:
        self.mediator = mediator

    def do_a(self) -> None:
        print("Componente faz A")
        if self.mediator:
            self.mediator.notify(self, "A")

    def do_b(self) -> None:
        print("Componente faz B")
        if self.mediator:
            self.mediator.notify(self, "B")

    def do_c(self) -> None:
        print("Componente faz C")

    def do_d(self) -> None:
        print("Componente faz D")


if __name__ == "__main__":
    c1 = Component()
    c2 = Component()
    mediator = ConcreteMediator(c1, c2)
    c1.do_a()
    c2.do_b()
```

---

### 18. Memento

**Propósito:** Capturar e externalizar o estado interno de um objeto sem violar o encapsulamento.

**Quando usar:** Para implementar desfazer/redo em aplicações.

**Exemplo que viola o padrão:**
```python
history.append(originator.state)
```

**Problema:** O cliente acessa diretamente o estado interno, o que quebra encapsulamento e torna o sistema frágil.

**Solução usando Memento:**
```python
from __future__ import annotations

class Memento:
    def __init__(self, state: str) -> None:
        self._state = state

    def get_state(self) -> str:
        return self._state

class Originator:
    def __init__(self, state: str) -> None:
        self._state = state

    def set_state(self, state: str) -> None:
        self._state = state

    def save(self) -> Memento:
        return Memento(self._state)

    def restore(self, memento: Memento) -> None:
        self._state = memento.get_state()

    def __str__(self) -> str:
        return self._state

class Caretaker:
    def __init__(self):
        self._history: list[Memento] = []

    def backup(self, originator: Originator) -> None:
        self._history.append(originator.save())

    def undo(self, originator: Originator) -> None:
        if self._history:
            memento = self._history.pop()
            originator.restore(memento)


if __name__ == "__main__":
    originator = Originator("Estado 1")
    caretaker = Caretaker()
    caretaker.backup(originator)

    originator.set_state("Estado 2")
    caretaker.backup(originator)

    originator.set_state("Estado 3")
    print(originator)

    caretaker.undo(originator)
    print(originator)

    caretaker.undo(originator)
    print(originator)
```

---

### 19. Observer

**Propósito:** Definir uma dependência de um-para-muitos entre objetos, de modo que quando um objeto muda, seus dependentes são notificados.

**Quando usar:** Para implementar eventos e atualizações automáticas entre objetos.

**Exemplo que viola o padrão:**
```python
observer1.update(message)
observer2.update(message)
```

**Problema:** O remetente precisa conhecer todos os observadores e chamar cada um manualmente.

**Solução usando Observer:**
```python
from __future__ import annotations
from abc import ABC, abstractmethod

class Observer(ABC):
    @abstractmethod
    def update(self, message: str) -> None:
        pass

class Subject(ABC):
    def __init__(self) -> None:
        self._observers: list[Observer] = []

    def attach(self, observer: Observer) -> None:
        self._observers.append(observer)

    def detach(self, observer: Observer) -> None:
        self._observers.remove(observer)

    def notify(self, message: str) -> None:
        for observer in self._observers:
            observer.update(message)

class ConcreteObserver(Observer):
    def __init__(self, name: str) -> None:
        self.name = name

    def update(self, message: str) -> None:
        print(f"{self.name} recebeu: {message}")

class ConcreteSubject(Subject):
    def change_state(self, message: str) -> None:
        self.notify(message)


if __name__ == "__main__":
    subject = ConcreteSubject()
    subject.attach(ConcreteObserver("Observador 1"))
    subject.attach(ConcreteObserver("Observador 2"))
    subject.change_state("Novo evento disponível")
```

---

### 20. State

**Propósito:** Permitir que um objeto altere seu comportamento quando seu estado interno muda.

**Quando usar:** Quando um objeto deve mudar de comportamento sem usar condicionais complexos.

**Exemplo que viola o padrão:**
```python
if self.state == "A":
    self.do_a()
elif self.state == "B":
    self.do_b()
```

**Problema:** O objeto precisa manter condicionais de estado em várias partes do código, tornando a manutenção difícil.

**Solução usando State:**
```python
from __future__ import annotations
from abc import ABC, abstractmethod

class State(ABC):
    @abstractmethod
    def handle(self, context: "Context") -> None:
        pass

class Context:
    def __init__(self, state: State) -> None:
        self._state = state

    def request(self) -> None:
        self._state.handle(self)

    def transition_to(self, state: State) -> None:
        self._state = state

class ConcreteStateA(State):
    def handle(self, context: Context) -> None:
        print("Contexto no Estado A")
        context.transition_to(ConcreteStateB())

class ConcreteStateB(State):
    def handle(self, context: Context) -> None:
        print("Contexto no Estado B")
        context.transition_to(ConcreteStateA())


if __name__ == "__main__":
    context = Context(ConcreteStateA())
    context.request()
    context.request()
```

---

### 21. Strategy

**Propósito:** Definir uma família de algoritmos, encapsulá-los e torná-los intercambiáveis.

**Quando usar:** Para variar o algoritmo usado por um objeto sem modificar seu código.

**Exemplo que viola o padrão:**
```python
if strategy_type == "sum":
    result = sum(data)
elif strategy_type == "max":
    result = max(data)
```

**Problema:** O contexto escolhe explicitamente o algoritmo, dificultando a adição de novas estratégias.

**Solução usando Strategy:**
```python
from __future__ import annotations
from abc import ABC, abstractmethod

class Strategy(ABC):
    @abstractmethod
    def execute(self, data: list[int]) -> int:
        pass

class SumStrategy(Strategy):
    def execute(self, data: list[int]) -> int:
        return sum(data)

class MaxStrategy(Strategy):
    def execute(self, data: list[int]) -> int:
        return max(data)

class Context:
    def __init__(self, strategy: Strategy) -> None:
        self._strategy = strategy

    def set_strategy(self, strategy: Strategy) -> None:
        self._strategy = strategy

    def execute_strategy(self, data: list[int]) -> int:
        return self._strategy.execute(data)


if __name__ == "__main__":
    context = Context(SumStrategy())
    print(context.execute_strategy([1, 2, 3]))
    context.set_strategy(MaxStrategy())
    print(context.execute_strategy([1, 2, 3]))
```

---

### 22. Template Method

**Propósito:** Definir o esqueleto de um algoritmo em uma operação, delegando alguns passos para subclasses.

**Quando usar:** Para levar subclasses a reutilizar partes de um algoritmo sem alterar sua estrutura.

**Exemplo que viola o padrão:**
```python
class CSVProcessor:
    def process(self):
        self.read_csv()
        self.process_csv()
        self.save()

class JSONProcessor:
    def process(self):
        self.read_json()
        self.process_json()
        self.save()
```

**Problema:** A lógica de controle do algoritmo é duplicada em cada subclasse.

**Solução usando Template Method:**
```python
from abc import ABC, abstractmethod

class DataParser(ABC):
    def parse(self) -> None:
        self.read_data()
        self.process_data()
        self.save_data()

    @abstractmethod
    def read_data(self) -> None:
        pass

    @abstractmethod
    def process_data(self) -> None:
        pass

    def save_data(self) -> None:
        print("Dados salvos")

class CSVParser(DataParser):
    def read_data(self) -> None:
        print("Lendo CSV")

    def process_data(self) -> None:
        print("Processando CSV")

class JSONParser(DataParser):
    def read_data(self) -> None:
        print("Lendo JSON")

    def process_data(self) -> None:
        print("Processando JSON")


if __name__ == "__main__":
    CSVParser().parse()
    JSONParser().parse()
```

---

### 23. Visitor

**Propósito:** Representar uma operação a ser executada sobre elementos de uma estrutura de objetos, permitindo adicionar novas operações sem mudar as classes dos elementos.

**Quando usar:** Quando novas operações devem ser adicionadas a uma estrutura de objetos sem modificar esses objetos.

**Exemplo que viola o padrão:**
```python
if isinstance(element, ElementA):
    process_a(element)
elif isinstance(element, ElementB):
    process_b(element)
```

**Problema:** O cliente precisa saber o tipo concreto do elemento, tornando o código frágil e difícil de estender.

**Solução usando Visitor:**
```python
from __future__ import annotations
from abc import ABC, abstractmethod

class Visitor(ABC):
    @abstractmethod
    def visit_book(self, book: "Book") -> None:
        pass

    @abstractmethod
    def visit_dvd(self, dvd: "DVD") -> None:
        pass

class Element(ABC):
    @abstractmethod
    def accept(self, visitor: Visitor) -> None:
        pass

class Book(Element):
    def __init__(self, title: str, price: float) -> None:
        self.title = title
        self.price = price

    def accept(self, visitor: Visitor) -> None:
        visitor.visit_book(self)

class DVD(Element):
    def __init__(self, title: str, price: float) -> None:
        self.title = title
        self.price = price

    def accept(self, visitor: Visitor) -> None:
        visitor.visit_dvd(self)

class PriceVisitor(Visitor):
    def __init__(self) -> None:
        self.total = 0.0

    def visit_book(self, book: Book) -> None:
        # Apply possible book discount
        self.total += book.price * 0.9

    def visit_dvd(self, dvd: DVD) -> None:
        # DVDs have no discount
        self.total += dvd.price

class PrintVisitor(Visitor):
    def visit_book(self, book: Book) -> None:
        print(f"Livro: {book.title} - R${book.price:.2f}")

    def visit_dvd(self, dvd: DVD) -> None:
        print(f"DVD: {dvd.title} - R${dvd.price:.2f}")


if __name__ == "__main__":
    items: list[Element] = [Book("Clean Code", 50.0), DVD("Documentary", 30.0), Book("Design Patterns", 70.0)]

    # Usando PrintVisitor para exibir itens
    printer = PrintVisitor()
    for item in items:
        item.accept(printer)

    # Usando PriceVisitor para calcular total
    calculator = PriceVisitor()
    for item in items:
        item.accept(calculator)
    print(f"Total com descontos: R${calculator.total:.2f}")
```

Este exemplo mostra como novas operações (`PriceVisitor`, `PrintVisitor`) podem ser adicionadas sem modificar as classes `Book` e `DVD` — basta implementar um novo `Visitor`.

---

## Conclusão

Os padrões GOF trazem uma base sólida para arquitetar sistemas orientados a objetos. Eles não são regras rígidas, mas boas práticas que ajudam a tornar o código mais organizado, reutilizável e mais fácil de evoluir.

A melhor forma de aprender é aplicar cada padrão em pequenos exemplos e verificar quando o padrão reduz complexidade sem introduzir overhead desnecessário.

