[![Typing SVG](https://readme-typing-svg.herokuapp.com?color=%2336BCF7&lines=All-in-one+V2)](https://git.io/typing-svg)

Огромная просьба сначала все прочитать на 10 раз, все протестировать, погуглить и только потом задавать вопросы в наш код чат. В личку админам с вопросами по коду просьба не писать, они не ответят.

Donate (evm) : `0xb7415DB78c886c67DBfB25D3Eb7fcd496dAf9021` or `donates-for-hodlmod.eth`

Паблик : https://t.me/hodlmodeth. [ code ] чат : https://t.me/code_hodlmodeth.

Идеальный скрипт-V2 для ведения фермы. Освоив его, ты сможешь (идет перечисление модулей) :

1. web3_checker : очень быстро (асинка) смотрит баланс монеты в любой evm сети.
2. debank_checker : около_быстро (асинка) смотрит все токены, нфт и протоколы во всех evm сетях (которые доступны на самом [debank](https://debank.com/)). 
3. exchange_withdraw : вывод монет с бирж : binance, mexc, kucoin, bybit, huobi.
4. okx_withdraw : вывод с биржи okx + в подарок вывод с субов. отдельным модулем из-за функции вывода с суб-аккаунтов.
5. transfer : вывод монет с кошельков в evm сетях (метамаск).
6. 1inch : свапы на 1inch во всех сетях, включая zksync era.
7. [orbiter](https://www.orbiter.finance/) : бридж eth во всех сетях, включая zksync era и starknet. чтобы бриджит на starknet, нужно добавить адреса кошельков старкнета в файл `starknet_address.txt`.
8. [woofi](https://fi.woo.org/) : бриджи и свапы. бридж проходит через stargate (layerzero). универсален, доступны все монеты и сети, которые там есть. 

Дополнительная информация :
1. Вместо принтов сделал logger. 
2. Добавил tg_sender. Все результаты прописываются не только в терминал, но и в тг-бота.
3. Для чекеров сделал csv файлы. 
4. Сделал нормальные try exceptы.
5. Сделал проверку транзакций на фейл (check_status_tx).
6. Сделал универсальный апрув и предварительную проверку на кол-во апрувнутых монет (check_allowance). то есть лишних апрувов делать не будет.

# Установка на Ubuntu
Обновляемся
```
sudo apt update && sudo apt upgrade -y
```
Скачиваем
```
git clone https://github.com/zaivanza/all-in-one-v2
```
Заходим в папку со скриптом
```
dir
cd all-in-one-v2
```
Устанавливаем pip:
```
apt install python3-pip
```
Устанавливаем библиотеки. Для заметки - для этого скриптаустанавливается `web3==6.2`
```sh
pip install telebot
pip3 install -r requirements.txt
# pip install web3==6.2
# pip install requests loguru web3 telebot tqdm ccxt termcolor tabulate
```


# Настройка.

1. Вся настройка модулей и апи ключей делается в файле `setting.py`, их настройка описана в этом же файле. 
2.  В папке data есть 3 файла : `wallets.txt`, `recipients.txt`, `proxies.txt`, `starknet_address.txt`, `rpc.py` :
- `wallets.txt` - сюда записываем кошельки (приватники / адреса).
- `recipients.txt` - сюда записываем адреса для трансфера, используется только в модуле transfer когда выводим с кошелька на адрес. 1 кошелек = 1 адрес.
- `proxies.txt` - сюда записываем прокси, они используются только в debank чекере, без них он работать не будет. Чем больше, тем лучше. Формат : http://login:password@ip:port
- `starknet_address.txt` - сюда записываем адреса кошельков старкнета. если не будете бриджить с орбитера на старкнет, можно не вставлять.
- `rpc.py` - здесь по желанию (необязательно, сейчас стоят паблик ankr rpc) нужно менять rpc.
3. Настраивать модули нужно в функциях value в файле `setting.py`.
4. Не забудь настроить тг-бота. Для этого в файле `setting.py` в переменную `TG_TOKEN` нужно вставить токен от тг бота. В переменную `TG_ID` нужно вставить свой id токен, узнать его можно здесь : https://t.me/getmyid_bot.
5. Запускать нужно файл `MAIN.py`

!!! ВНИМАНИЕ ЕСЛИ ВЫ ИСПОЛЬЗУЕТЕ ПОСЛЕДНЮЮ ВЕРСИЮ СКРИПТА, значит нужный модуль выбирается в интерактиве. Вот выдержка из [README](https://github.com/zaivanza/all-in-one-v2/blob/main/README.md):
> Запускать нужно файл main.py, **в терминале будет список с модулями, нужно будет выбрать один**.
> ![image](https://github.com/TatianaDEV7/all-in-one-v2/assets/98289003/608ddecc-dfd2-462e-bfd1-f84a15084c3b)
### Запуск интерактивного скрипта
```bash
# Лучше запустить фоном с помощью tmux. Установим tmux:
sudo apt update && sudo apt install tmux -y

# Запускаем сессию тмукс (название сессии любое, я выбрал `all-script-madule-3` так как запускаю третий модуль):
tmux new -s all-script-madule-3

# Переходим в папку со скриптом (папка у вас может по другому называться):
cd /root/all-in-one-v2

#Теперь запускаем сам скрипт:
python3 main.py
```
### tmux команды
[Шпаргался от Losst](https://losst.pro/shpargalka-po-tmux). 

Список частых команд:

Выходим из сессии `Ctrl+b d` (Но сессия ДАЛЬШЕ продолжит работать в фоне): 

Лист с сессиями:
```
tmux ls
```
Опять конектимся к сессии:
```
tmux attach -t all-script-module
```
> !Только не закрывайте скрипт с помощью CTRL+C

Создание новой сессии:
```
tmux new -s <name-of-session>
```
Закрыть все сессии:
```
tmux kill-server
```
закрыть определенную сессию:
```
tmux kill-session -t <name-of-session>
```

# Запуск (только после Настройки)

```bash
# Заходим в папку скрипта
cd /root/all-in-one-v2

# Запуск с выводом stdout в командную строку
python3 main.py
# Запуск в фоновом режиме. Записывает логи в файл log.log
nohup python3 main.py > log.log 2>&1 &

# Завершить процесс:
pgrep -a python
kill (номер процесса)
```
# Решение Ошибки
### 1) Знаки после запятой
Ошибка может выглядеть так: 
> error : binance {"code":-4007,"msg":"Address verification failed. Please confirm the network is matched with the withdrawal address. You may consult with recipient's platform for details."}


Для примера разберем модуль **exchange_withdrowal**:
Параметр записан либо в `all-in-one-v2/utils.py` либо в обновленном скрипте в файле `all-in-one-v2/modules/exchange_withdraw.py`

```
def exchange_withdraw(privatekey):

    try:

        cex, chain, symbol, amount_from, amount_to = value_exchange()
        amount_ = round(random.uniform(amount_from, amount_to), 7)

        logger.info(f"{cex}_withdraw")

        API_KEY     = CEX_KEYS[cex]['api_key']
        API_SECRET  = CEX_KEYS[cex]['api_secret']

        wallet = evm_wallet(privatekey)

        dict_ = {
            'apiKey': API_KEY,
            'secret': API_SECRET,
            'enableRateLimit': True,
            'options': {
                'defaultType': 'spot'
            }
        }

        if cex in ['kucoin']:
            dict_['password'] = CEX_KEYS[cex]['password']

        account = ccxt.__dict__[cex](dict_)

        account.withdraw(
            code = symbol,
            amount = amount_,
            address = wallet,
            tag = None, 
            params = {
                "network": chain
            }
        )
        logger.success(f"{cex}_withdraw success => {wallet} | {amount_} {symbol}")
        list_send.append(f'{STR_DONE}{cex}_withdraw')

    except Exception as error:
        logger.error(f"{cex}_withdraw unsuccess => {wallet} | error : {error}")
        list_send.append(f'{STR_CANCEL}{cex}_withdraw')
```
В строке `amount_ = round(random.uniform(amount_from, amount_to), 7)` заменить 7 на необходимое количество знаков после запятой.

### 2)  Приватники или адреса
Ошибка может выглядеть так: 
> error : binance {"code":-4007,"msg":"Address verification failed. Please confirm the network is matched with the withdrawal address. You may consult with recipient's platform for details."}


В файле обновленного скрипта есть строчка:
```
is_private_key = True # True если в wallets.txt вставил evm приватники. False если адреса (evm / не evm)
```
Ее обязательно надо учитывать при настройке иначе просто не пойдет скрипт.
# Полезно
Указаны все монеты и сети для вывода CEX Бирж: https://github.com/th0masi/all-cex-withdrawal
