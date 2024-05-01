# Baseado no conversor kb2xbox (KeyBoard to Xbox), este é um fork customizado para jogar os jogos do Xbox Game Pass Cloud Gaming pelo teclado nos sistemas Linux
Convert a keyboard to (multiple) gamepads.

Fiz a maioria dos comandos e movimentação da câmera que um controle de Xbox no Linux pelo teclado precisaria. 

Você só precisa estar num sistema operacional Linux, instalar o Python Libevdev disponível na seção "# Requirements" e rodar o código conforme explicado no "### Example".

## Description
Usually, a second or third keyboard is treated the same way as the first keyboard.
kb2xbox allows you to emulate as many XBox Controllers as you like with your keyboards.
This is useful when you want to play local co-op games (aka couch games) with multiple players.

Se quiser customizar as teclas para o seu teclado ou jogo, você pode olhar a lista de entradas disponível no uinput aqui: https://github.com/torvalds/linux/blob/master/include/uapi/linux/input-event-codes.h
Aí escolhe as que quiser e substitui no arquivo xbox.cfg. Você pode criar outro arquivo .cfg para customizar para seu jogo e rodar o emulador com: `python kb2xbox.py -d /dev/input/event<KeyboardEventID> config/nome_do_seu_controle.cfg'

# Requirements
- Linux
- [python-libevdev](https://python-libevdev.readthedocs.io/en/latest/)

# Run
Check your available keyboards with `kb2xbox.py --list` | Veja quais teclados e mouses estão conectados no seu notebook/computador.

Make sure /dev/uinput is writable `sudo chmod 666 /dev/uinput`

## Syntax
`python kb2xbox.py -d KEYBOARD_DEVICE CONFIGS`

### Example
`python kb2xbox.py -d /dev/input/event<KeyboardEventID> config/xbox.cfg config/xbox2.cfg` | <KeyboardEventID> é o ID (Ex: 10 ou 4) do teclado que você encontrou na --list acima. Os outros argumentos são os controles que vc quer emular.

This lets you emulate 2 XBox Controllers:
- Arrow keys for the analogue stick (Controller 1)
- Right Alt, HENKAN and KATAKANAHIRAGANA keys for additional buttons (Controller 1)
- E,S,D,F keys for the analogue stick (Controller 2)
- Left Shift, Caps Lock and Tab keys for additional buttons (Controller 2)

## Keyboard over the Network. Caso seu controle não esteja conectado via USB
To connect a built-in keyboard from e.g. notebooks and turn them into Gamepads, use: (taken from [here](https://superuser.com/questions/67659/linux-share-keyboard-over-network))
- keyboard receiver:  
`nc -l -p 4444 > /dev/input/by-path/platform-i8042-serio-0-event-kbd`
- keyboard sender:  
`cat /dev/input/by-path/platform-i8042-serio-0-event-kbd | nc <IP> 4444`
