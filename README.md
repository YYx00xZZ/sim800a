# sim800a
 working with sim800a module

<img src="https://user-images.githubusercontent.com/47386361/144780422-82cb579c-5db2-4714-8708-d06365506a59.png" width="300" height="300" />

Като цяло работата с GSM модул и ардуино е сравнително лесна, освен тогава когато не е...

На пазара има няколко разновидности модеми (TODO>списък), но по-интересен е броя на различните дизайни (PCB Designs) които се предлагат от различните производители (които също са безброи). Това което имам впредвид е, че два продукта базирани на един и същ модем (например SIM800), и именувани по сходен начин, са до-голяма степен различни в съшността си. (съответно и начина по който можем да ги използваме. )

Тези различия често са достатъчно осезаеми и напълно достатъчни, че да провалят проекта ни, в случей, че не знаем за тях и не ги предвидим в първоначалното планиране на цялостната работа. 

_Следващите редове се базират изцяло на наблюденията ми през последните три години на работа с различни "IoT" устройства (2018-2021)._

Въпросните различния се дължат на това, че: първо - различните производители имплементират (задължителната) периферията на модема по (достатъчно) различен начин, че това да промени крайното(по-нататъшЧното!?!?!?) действеи и/или начин на работа на/със въпросния продукт (за нас-девелопърите), второ - често биват добавяни различни функционалности към крайния (за developer-а е междинен...) продукт, които не са предвидени по начало в основния такъв (в случея това е GSM модема). Най-вероятно си мислите "какво от това? нали това е смисъла на развоините платки, пък и преди да излязат на пазара минават регулационен процес от съответните органи"...; трето-прекалено често крайният продукт съдържа критични грешки в дизайна на схемата (напр. LED в esp8266 платки), съответно и в начина на работа след това и четвърто, но не на последно място - много често липсва каквато и да е документация на продукта (било то схема на I/O или описание на наличните функционалности за следващия разработчик)

Модемът с който ще работим е един от най-известните _(най-често срещаните)_ - SIM800 и по-точно SIM800**A**, а модула (до колкото е възможно да бъде идентифициран) е по дизайн на ___UNV___, конкретно този с който разполагаме е версия _V3.9.3_ от _26-08-2016г._ (предполагам, че годината се отнася към версията, а не дата на производство)

_значението на буквата в края (как се нарича?индекс може би?), и какви други възможности има, ще разгледаме по-късно._

Предстой да разгледаме основни

# Тестване на модула
---
## Нужни
- USB Type Mini B
- SIM Card with atleast GPRS data
- Few jumper wires
- КОНВЕРТОР USB КЪМ UART ![converter-USB-to-UART](https://user-images.githubusercontent.com/47386361/146177393-e995ac82-2ae3-4912-b944-fa8ea13aa371.jpg)

## Свързване
<pre>
  SIM800A                            USB TO UART CONVERTER
 ──────────────────────────         ──────────────────────────

┌──────────────┬───────┐
│              ├──────┼│                USB
│              │sim   ││                 ┌────┐
│              │card  ││             ┌───┼┼┼┼┼┼──┐
│              │      ││             │   └────┘  │
│              │      ││             │           ├┐
│     ┌──┐     └──────┴┤             │        GND├┘ ──────┐
│     │┼┼│             │             │           │        │
│   SR│┼┼│ST ──────────┼────────┐    │           │        │
│   │ │┼┼│             │        │    │           │        │
│   │ │┼┼│             │        │    │           │        │
│   └─┴──┴─────────────┼───┐    │    │           │        │
│                      │   │    │   ┌┤           │        │
│ ┌──┐                 │   │    └──►└┤RX         │        │
└─┼┼┼┼─────────────────┘   │         │           │        │
  └──┘                     │        ┌┤           │        │
  PWR                      └───────►└┤TX         │        │
   ▲ ▲                               │           │        │
   │ │                               │         5V├┐       │
   │ │                               │           ├┴───┐   │
   │ │                               └───────────┘    │   │
   │ │                                                │   │
   │ └────────────────────────────────────────────────┘   │
   │                                                      │
   └──────────────────────────────────────────────────────┘
</pre>
![PINS-USED-ON-CONVERTER](https://user-images.githubusercontent.com/47386361/146182349-fcba18c2-f08c-4967-9919-4674dd1d6b3a.jpg)
![module-connected-to-converter](https://user-images.githubusercontent.com/47386361/146182940-eb4e954b-2b2c-40dc-9e7b-4c6545c84c8f.jpg)
