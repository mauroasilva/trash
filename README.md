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

**Consultar:** Style Guide for Python Code - https://www.python.org/dev/peps/pep-0008/

**Exemplo 1 - WRONG:**
```
def process_line(self, evt):
    line = evt["raw"]["line"]
```
**Exemplo 1 - RIGHT:**
```
def process_line(self, event):
    line = event["raw"]["line"]
```

**Exemplo 2 - WRONG:**
```
n_evt = evt.deep_copy()
```
**Exemplo 2 - RIGHT:**
```
local_event = event.deep_copy()
```

## Harmonização de Eventos

É obrigatório respeitar a harmonização de eventos. Qualquer ‘key’ deve ser representada sempre em minúsculas.

**Consultar:** IntelMQ Data Harmonization - https://github.com/certtools/intelmq/blob/master/docs/DataHarmonization.md


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
**Nota:** assumindo que foi criado um bot para o feed ‘Abuse.ch Zeus’ é necessário criar diferentes parsers para os respectivos tipos de eventos (C&C, Binaries, Dropzones). Neste caso, a hierarquia ‘intelmq\bots\parser\abusech\parser.py’ não seria adequada porque são necessários mais parsers como referido anteriormente. A solução para estes casos é continuar a usar a mesma hierarquia, apenas acrescentando ao nome do ficheiro ‘_zeus_cc.py’, ‘_zeus_binaries.py’ e ‘_zeus_dropzones.py’. O resultado final na hierarquia será:
```
\intelmq\bots\parser\abusech\parser_zeus_cc.py
\intelmq\bots\parser\abusech\parser_zeus_binaries.py
\intelmq\bots\parser\abusech\parser_zeus_dropzones.py
```
