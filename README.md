# Приложение ServCardPr
Windows служба отправки уведомлений (хуков) по изменениям аккаунтов и транзакций БД R-Keeper CRM на cardpr.com

> О настройках связки R-Keeper CRM с CardPr в части создания и редактирования карт можно узнать [здесь](http://help.cardpr.com/ru/articles/2689332-%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B0%D1%86%D0%B8%D1%8F-%D1%81-r-keeper)

Сопоставление клиентов между сервисом и локальной базой данной происходит по номеру телефона. Если клиент уже был создан в базе R-Keeper CRM (то есть по номеру телефона указанному при регистрации в CardPr есть клиент в базе CRM), то никакие данные клиента не обновятся. 

## Системные требования
ОС Windows 7 и выше

Microsoft .NET Framework 4.5 [загрузить с сайта Microsoft](https://www.microsoft.com/ru-ru/download/details.aspx?id=30653)

R-Keeper CRM v7 [руководство пользователя](https://docs.ucs.ru/uploads/r-keeper_crm_V7_user_manual.pdf)

## Дистрибутив
Актуальную версию приложения можно загрузить [здесь](bin)

## Лицензия
Для работы приложения нужно получить лицензию. Стоимость лицензии включена в стоимость тарифного плана CardPr. Лицензия нужна только для контроля количества установок.
Для получения лицензии необходимо написать письмо на адрес rkkey@dk-soft.ru c указанием что требуется лицензия на сервер синхронизации CardPr, необходимо в письме указать код ресторана и юрлицо.

## Установка и настройка приложения
Для установки необходимо распаковать дистрибутив с помощью программы 7-Zip.
В папке назначения запустить файл **InstallLs.bat** для установки службы лицензирования
Далее в перейти во вложенную папку **LS**, и в файле **LS.xml** прописать идентификатор полученного ключа лицензии в секцию **LicenseID**. Например, 

`<add key="LicenseID" value="50019" />`

Далее скопировать присланный файл ключа в папку **LS**.
Далее запустить Службу **Client.LS**.

Далее в папке назначения необходимо в файле **settingLs** прописать в секцию **Host** текущее имя компьтера.

Далее для настройки приложения необходимо запустить файл **ServCardPr.exe**.
Далее потребуется заполнить параметры приложения.

**!Внимание! Настройка параметров приложения будет возможна только при остановленной службе лицензирования. Если служба приложения запущена, то при запуске настроек приложения появится сообщение об отсутсвующей лицензии.**  
 
![Окно настроек приложения](images/settings_empty.jpg?raw=true "Окно настроек приложения")

### Описание параметров

**CardPr ApiKey** - API-ключ, можно скопировать из личного кабинета клиента в сервисе CardPr (Настройки -> API -> Private Key).

**Период запуска** - периодичность опроса службой базы данных R-Keeper CRM на изменения. Часы:минуты:секунды. Не рекомендуется указывать время меньше 1 минуты. Фактически для пользователей данный параметр отвечает за то, как быстро клиент ресторана получит уведомление об изменении баланса карты после начисления/списания бонусов.

**Начальный номер карты диапазона/ Конечный номер карты диапазона** - Первый и последний номера карт, транзакции по которым отслеживаются службой. Транзакции по картам с другими номерами пропускаются. В реальности нужно сразу определиться какие номера карт выдаются под электронные карты CardPr, так как в этой же базе CRM могут быть заведены физические (пластиковые) карты.

**Тип контакта телефон** - Идентификатор типа контакта "Телефон" из базы данных CRM. Можно посмотреть в Конфигураторе R-Keeper CRM (см. скриншот ниже). Номер телефона является основным идентификатором владельца электронной карты для CardPr. Так как регистрация электронных карт, создание владельцев и карт в CRM происходит автоматически, важно, чтобы для всех электронных карт в базе данных R-Keeper CRM номер телефона и email не заполнялся и не редактировался вручную пользователем, а только передавался автоматически из CardPr.

**Тип контакта электронной почты** - Идентификатор типа контакта "Электронная почта" из базы данных CRM. Можно посмотреть в Конфигураторе R-Keeper CRM (см. скриншот ниже).

**Тип бонусного счета** - Идентификатор типа бонусного счета R-Keeper CRM. Можно посмотреть в Конфигураторе R-Keeper CRM (см. скриншот ниже). Баланс указанного счета будет передаваться как баланс бонусов держателя электронной карты в CardPr. Процент текущего бонусного уровня в CRM передается как процент бонуса в CardPr.

**Тип дисконтного счета** - Идентификатор типа дисконтного счета R-Keeper CRM. Можно посмотреть в Конфигураторе R-Keeper CRM (см. скриншот ниже). Процент текущего дисконтного уровня гостя в CRM передается как процент скидки в CardPr.

![Конфигуратор CRM](images/crm_configurator_2.jpg?raw=true "Конфигуратор CRM")
![Конфигуратор CRM](images/crm_configurator_1.jpg?raw=true "Конфигуратор CRM")

Проценты текущего бонусного и дисконтного уровня будут переданы в CardPr, если они заполнены в поле "Базовая ставка" в R-Keeper CRM для каждой схемы счета. См. скришот.

![Схемы счетов CRM](images/schemas_crm.jpg?raw=true "Схемы счетов CRM")

**Подключение к базе CRM** - Настройки подключения к базе данных R-Keeper CRM
- Сервер - ip-адрес и порт сервера MSSQL, например "127.0.0.1,1433"
- База - название базы данных R-Keeper CRM, например "Card_System"
- UserId - Имя пользователя MSSQL, например "sa"
- Password - Пароль пользователя MSSQL

**Время последней синхронизации** - дата и время последней синхронизации счетов и транзакций. Определяет глубину запроса данных по времени из базы данных R-Keeper CRM. Если требуется провести пересинхронизацию данных за прошлый период (для того чтобы обновить данные по клиентов в CardPR принудительно за весь период), то допустимо установить в обоих полях нужную дату и время "в прошлом", после чего перезапустить службу. В обычной работе данные параметры изменять не требуется.

После установки праметров нужно сохранить изменения, закрыть окно, установить и запустить службу. **Любые дальнейшие изменения параметров применяются только после перезапуска службы.**

Все настройки приложения хранятся в файле setting. Рекомендуется сделать резервную копию файла после настройки и запуска приложения.

## Установка/удаление службы
Для установки службы необходимо запустить файл **ServCardPr.exe** с параметром **-i**. Служба будет установлена под именем **CardPrService**.
Для удаления службы необходимо запустить файл **ServCardPr.exe** с параметром **-u**.

## Логирование
Служба логирует свою работу в папку .\log. Логи создаются за каждую дату отдельно в текстовый файл ServCardPr.log. Приложение записывает в лог все запросы в адрес сервера CardPr, также ответы на эти запросы.

## Замечания

Существуют лимиты по количеству и частоте обращений внешних приложений к API CardPR.

> Тариф Старт – 1 / сек, 60 / час.
> Тариф Бизнес – 1 / сек, 600 / час.
> Тариф Корпорация – 1 / сек, 3600 / час.
> Увеличение лимитов по запросу в техподдержку.

Служба соблюдает лимит не чаще одного запроса в 1сек. Если же транзакций много и служба не укладывается в лимит по количеству запросов в час, то все неотправленные транзакции служба будет  пытаться отправить до тех пор, пока они не будут приняты.
Все не отправленные на текущий момент транзакции хрянятся в файле Errors.xml до успешной отправки.  

# Поддержка

По вопросам работоспособности приложения можно обращаться по адресу asu-help@dk-soft.ru. В обращении требуется указать суть проблемы, а также номер телефона для связи с вами.
