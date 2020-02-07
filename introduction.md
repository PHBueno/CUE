# O que é a CUElang?

- É uma linguagem Opensource para validação de dados.

- Possui muitas aplicações como:
  - validação de dados; 
  - modelagem de dados; 
  - configuração; 
  - consulta; 
  - geração de código;
  - scripts.
  
## Aplicações

A combinação de valores, em qualquer ordem sempre dará o mesmo resultado:
  - associativo; 
  - comutativo; 
  - idempotente.
  
## Filosofia e princípios

A CUElang não faz distinção entre tipos e valores, o que permite que ela faça restrições detalhadas.
As **restrições** na CUElang funcionam como **validadores de dados**


Exemplos:

1. Objeto JSON na sintaxe CUE

DADOS
```
moscow: {
  name:    "Moscow"
  pop:     11.92M
  capital: true
}
```

SCHEMA
```
municipality: {
  name:    string
  pop:     int
  capital: bool
}
```

Mistura entre dados e schema, como é o exemplo da CUE

CUE
```
largeCapital: {
  name:    string
  pop:     >5M
  capital: true
}
```

Em geral, na CUE, deve-se começar com uma ampla definição de um **tipo**, descrevendo todas as **instâncias possíveis**. 
Em seguida, reduz-se essas definições, combinando restrições de diferentes fontes (departamentos, usuários), até que uma instância de dados concreta permaneça.

# Instalação

O binário pode ser instalado usando um dos seguintes métodos:

## Download no GitHub
Binários para vários sistemas operacionais, incluindo Linux, Windows e Mac OS, podem ser baixados da seção de [versões da CUE no Github](https://github.com/cuelang/cue/releases)

## Instale usando o homebrew

```
brew install cuelang/tap/cue
```

## Instale a partir da origem

**Pré-requisitos**: Go 1.12 ou posterior


Para baixar e instalar a ferramenta de linha de comando cue, execute:

```
go get -u cuelang.org/go/cmd/cue

```

Para baixar a API e a documentação execute:

```
go get -u cuelang.org/go/cue
```

# Casos de uso

## Validação de dados

Valide dados programáticos ou baseados em texto.

A abordagem mais direta para específicar dados é nos arquivos JSON e YAML. Todo valor pode ser pesquisado exatamente onde precisa ser definido.
As ferramentas de validação de dados permitem verificar a consistência desses dados com base em um schema.

### Principais problemas abordados pela CUE
#### Validação de dados do lado do cliente
Não há muitas ferramentas úteis para verificar arquivos de dados simples. Frequentemente, a validação é feita no servidor. 
Se isso for feito no lado do cliente, ele se baseia em definições de schema bastante detalhadas ou no uso de ferramentas personalizadas que verificam o schema para um domínio específico.


A ferramenta de linha de comando cue fornece uma maneira bastante direta de definir o schema e verificá-los em uma coleção de arquivos de dados.

**Exemplo**:

```yaml
# ranges.yaml
min: 5
max: 10
---
min: 10
max: 5
```


```
// check.cue
min?: *0 | number    // 0 se o valor for indefinido
max?: number & >min  // se definido deve ser maior que "min"

```

O comando cue vet pode verificar se os valores em ranges.yaml estão corretos apenas mencionando os dois arquivos na linha de comandos.

```
$ cue vet ranges.yaml checks.cue
max: invalid value 5 (out of bound >10):
    ./check.cue:2:16
    ./ranges.yaml:5:7
```










