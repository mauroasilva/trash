# Princípios
* Redução da complexidade de administração
* Redução da complexidade na criação de bots para novas feeds
* Redução da probabilidade da perda de eventos em todo o processo através da funcionalidade de persistência (mesmo num crash de sistema operativo)
* Adopção e melhoramento da Harmonização de eventos
* Adopção de um formato padrão para as mensagens (JSON)
* Integração com ferramentas já existentes (AbuseHelper, CIF)
* Disponibilizar mecanismos simples de armazenamento da informação em Log Collectors (Splunk, ElasticSearch, etc.)
* Disponibilizar mecanismos simples de criar black-lists
* Disponibilizar uma HTTP RESTFUL API para interação com o IntelMQ


# Rules

## Nome das variáveis
Nome das variáveis deve ser claro e percetível, não sendo permitido o uso de acrónimos, entre outras curtas representações de palavras.

Consultar: Style Guide for Python Code - https://www.python.org/dev/peps/pep-0008/

Exemplo 1 - WRONG:
```
def process_line(self, evt):
    line = evt["raw"]["line"]
```
Exemplo 1 - RIGHT:
```
def process_line(self, event):
    line = event["raw"]["line"]
```

Exemplo 2 - WRONG:
```
n_evt = evt.deep_copy()
```
Exemplo 2 - RIGHT:
```
local_event = event.deep_copy()
```

## Harmonização de Eventos

É obrigatório respeitar a harmonização de eventos. Qualquer ‘key’ deve ser representada sempre em minúsculas.

Consultar: IntelMQ Data Harmonization - https://github.com/certtools/intelmq/blob/master/docs/DataHarmonization.md


## Hierarquia de Diretórios no Repositório
```
intelmq\
  bin\
    intelmqctl
  lib\
    bot.py
    cache.py
    message.py
    pipeline.py
    utils.py
  bots\
    collector\
      <nome do bot>\
		    collector.py
    parser\
      <nome do bot>\
		    parser.py
    expert\
      <nome do bot>\
		    expert.py
    output\
      <nome do bot>\
		    output.py
    BOTS
  \conf
    pipeline.conf
    runtime.conf
    startup.conf
    system.conf
```
Nota: assumindo que foi criado um bot para o feed ‘Abuse.ch Zeus’ é necessário criar diferentes parsers para os respectivos tipos de eventos (C&C, Binaries, Dropzones). Neste caso, a hierarquia ‘intelmq\bots\parser\abusech\parser.py’ não seria adequada porque são necessários mais parsers como referido anteriormente. A solução para estes casos é continuar a usar a mesma hierarquia, apenas acrescentando ao nome do ficheiro ‘_zeus_cc.py’, ‘_zeus_binaries.py’ e ‘_zeus_dropzones.py’. O resultado final na hierarquia será:
```
\intelmq\bots\parser\abusech\parser_zeus_cc.py
\intelmq\bots\parser\abusech\parser_zeus_binaries.py
\intelmq\bots\parser\abusech\parser_zeus_dropzones.py
```


## Hierarquia de Diretórios da Instalação

Directório de ficheiros de configuração:
```
/etc/intelmq/
```

Directório de PID Files:
```
/var/run/intelmq/
```

Directório de ficheiros de logs:
```
/var/log/intelmq/
````

Directório de ficheiros adicionais usados por bots:
```
/var/lib/intelmq/
````


## Harmonização de Diretórios e Ficheiros

É obrigatório respeitar a harmonização de diretórios e ficheiros. Qualquer nome de pasta ou ficheiro deve:
* ser representado sempre em minúsculas e separado por underscores sempre que o nome contenha 2 palavras;
* ser auto explicativo do conteúdo da pasta ou ficheiro;


## Directórios e Ficheiros Hard-coded
Quaisquer directórios ou ficheiros hard-coded devem corresponder aos directórios ou ficheiros default do IntelMQ, respeitando por isso toda a hierarquia.
Directórios default de instalação que devem ser tidos em conta by default nas configurações e em PATHs hard-coded.
<Explicar melhor... Não percebi à primeira.>

## LICENCE Rules
Licença e autores não deve estar contida nos ficheiros de código...apenas no ficheiro LICENCE na root do repositório.

## Useless Code

Não pode existir código comentado.
<Tens de ter cuidado como escreves isto em Inglês... porque depois pode parecer que não queres que o pessoal comente o código, o que é mentira... tu queres que o código esteja comentado para ser mais fácil de ler.>

## Log Messages Format
Mensagens de log devem ser claras e bem escritas. Não devem ser em maiusculas nem tudo em minusculas. Deve ser uma frase que faça sentido para o utilizador
<Concordo. E além disto têm de seguir o padrão já implementado, ou seja: <Timestamp> - <Nome do bot> - <Nivel> - <Mensagem>. Que é para o caso de alguém que estende a lib de log e se lembra de meter uma cena marada qualquer.>

## Unicode
Garantir que todos os valores que vem de uma fonte externa são tranformados em unicode.

## Import Rules
Tentar ao máximo não estar sempre a repetir os mesmos imports caso as classes derivem umas das outras...basta um import na class base.
<Hmm... O que queres dizer com isto?

Prefiro:
from intelmq.message import Event
from Intelmq.message import Report

ou

from intelmq.message import Event,Report

do que

from intelmq.message import *
>

## Classes Names

Nome da class do bot (sub-class) deve ser o seu tipo. No caso de estarmos a criar um expert da Cymru, a class deve ser declarada da seguinte forma:

    class Expert(ExpertBot):
    
        ....
        
    if __name__ == "__main__":
        bot = Expert(sys.argv[1])
        bot.start()
    

## Private Methods Rules
Métodos privados com '_'

## Back-end independent
Qualquer código do IntelMQ Core, à excepção do ficheiro pipeline.py deve ser independente da tecnolgia de mensagens (Redis, RabbitMQ, etc...)

## Compatibility
O IntelMQ Core (incluíndo o intelmqctl) deve ser compatível com o IntelMQ Manager, IntelMQ UI e IntelMQ Mailer
