# Alternância de Fontes de Entrada no Estilo Windows para Karabiner-Elements

[English](README.md) | [中文](README_zh.md) | Português | [日本語](README_ja.md)

Este repositório contém uma regra de complex modification para o Karabiner-Elements, pensada para usuários de macOS que estão acostumados ao fluxo de alternância de métodos de entrada do Windows.

Ela foi criada para quem usa várias fontes de entrada no macOS, por exemplo:

- Inglês
- Um método de entrada chinês de terceiros, como Squirrel
- Português ou outro método de entrada adicional

## Recursos

O arquivo principal da regra é:

```text
windows_style_input_source_switching.json
```

Ele oferece dois comportamentos:

1. Manter `Control` pressionado e tocar em `Shift` alterna para a próxima fonte de entrada.
2. Pressionar `Control + Space` alterna entre inglês e a segunda fonte de entrada na ordem configurada no macOS.

Mais especificamente:

- Quando a fonte de entrada atual é inglês, `Control + Space` alterna para a próxima fonte de entrada.
- Quando a fonte de entrada atual não é inglês, `Control + Space` volta para inglês.
- A segunda fonte de entrada não é fixa. Ela segue a ordem definida nas configurações do macOS.

Por exemplo, se a ordem das suas fontes de entrada for:

```text
English -> Squirrel -> Portuguese
```

Então:

- Manter `Control` pressionado e tocar em `Shift` percorre `English -> Squirrel -> Portuguese -> English`.
- `Control + Space` muda de English para Squirrel.
- `Control + Space` muda de Squirrel ou Portuguese de volta para English.

Se depois você mover Portuguese, Japanese ou outra fonte de entrada para a segunda posição, `Control + Space` passará a mudar de English para essa nova segunda fonte.

## Requisitos

- macOS

- [Karabiner-Elements](https://karabiner-elements.pqrs.org/)

- 
  O atalho do macOS para "Selecionar teclado anterior" configurado como `Control + Espaço`

  O atalho do macOS para "Selecionar o teclado seguinte no menu Tipo de teclado" configurado como `Control + Shift + /`
  Você pode verificar ou alterar esse atalho em:

```text
Definições do Sistema -> Teclado -> Atalhos de teclado -> Tipos de teclado
```

## Instalação

1. Abra o diretório de complex modifications do Karabiner-Elements:

```text
~/.config/karabiner/assets/complex_modifications/
```

2. Coloque este arquivo nesse diretório:

```text
windows_style_input_source_switching.json
```

3. Abra o Karabiner-Elements.
4. Acesse:

```text
Complex Modifications -> Add your own rule
```

5. Ative:

```text
Windows style input source switching
```

## Como Funciona

A regra mapeia manter `Control` pressionado e tocar em `Shift` para o atalho existente do macOS:

```text
Control + Shift + /
```

Isso significa que a ordem de alternância continua sendo controlada pelo macOS, não pelo Karabiner-Elements.

O comportamento de `Control + Space` usa condições de fonte de entrada do Karabiner-Elements:

- `input_source_if` detecta inglês.
- `input_source_unless` detecta fontes de entrada que não são inglês.
- `select_input_source` volta para inglês.

A fonte de entrada em inglês é identificada com:

```json
{
  "language": "^en$"
}
```

## Observações

Esta regra assume que sua fonte de entrada em inglês é reconhecida pelo Karabiner-Elements como `language = en`.

Se `Control + Space` não voltar corretamente para inglês, use o Karabiner-EventViewer para verificar o identificador real da sua fonte de entrada em inglês e substitua o seletor `language` por um `input_source_id` mais específico.

## Testes

Depois de ativar a regra, teste estes cenários:

1. Comece em inglês, mantenha `Control` pressionado e toque em `Shift` várias vezes. Ela deve percorrer todas as fontes de entrada.
2. Comece em inglês e pressione `Control + Space`. Ela deve mudar para a segunda fonte de entrada.
3. Comece em qualquer fonte de entrada que não seja inglês e pressione `Control + Space`. Ela deve voltar para inglês.
4. Altere a segunda fonte de entrada nas configurações do macOS e pressione `Control + Space` a partir de inglês novamente. Ela deve mudar para a nova segunda fonte.

## Licença

MIT License
