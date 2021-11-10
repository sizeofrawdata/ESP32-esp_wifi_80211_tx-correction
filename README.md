# ESP32-esp_wifi_80211_tx-correction

<html>

<head>
<meta http-equiv=Content-Type content="text/html; charset=windows-1251">
<meta name=Generator content="Microsoft Word 15 (filtered)">

</head>

<body lang=RU>

<div class=WordSection1>

<p class=MsoNormal>Разработчики <span lang=EN-US>SDK</span><span lang=EN-US> </span><span
lang=EN-US>espressif</span>, решили ограничить отправку произвольных пакетов,
путём добавления функции <b>ieee80211_raw_frame_sanity_check</b> проверки типа
пакета (кадра), в функцию <b>esp_wifi_80211_tx, а</b> я решил исправить это
ограничение.</p><p>В данном примере рассматривается ESP-IDF 4.3.</p>

<p class=MsoNormal>Для того чтобы обойти это ограничение, необходимо исправить
готовый образ или исполняемый файл <span lang=EN-US>elf</span><span lang=EN-US>
</span>вашего проекта.</p>

<p class=MsoNormal>Необходимо найти и исправить последовательность опкодов (*на
ваше усмотрение):</p>

<p class=MsoNormal align=center style='text-align:center'><img width=533
height=231 id="Рисунок 1" src="Correct_esp_wifi_80211_tx.files/image001.jpg"></p>

<p class=MsoNormal>так:</p>

<p class=MsoNormal align=center style='text-align:center'><img width=533
height=227 id="Рисунок 2" src="Correct_esp_wifi_80211_tx.files/image002.jpg"></p>

<p class=MsoNormal>или просто:</p>

<p class=MsoNormal align=center style='text-align:center'><img width=465
height=220 id="Рисунок 3" src="Correct_esp_wifi_80211_tx.files/image003.png"></p>

<p class=MsoNormal>* на ваше усмотрение – имеются ввиду варианты логики</p>

<p class=MsoNormal>Для получения образа прошивки (для последующей загрузки в
память <span lang=EN-US>ESP</span>-32) используется утилита <span lang=EN-US>esptool</span>
с параметрами:</p>

<p class=MsoNormal><span lang=EN-US>esptool.py --chip esp32 elf2image
my_esp32_app.elf</span></p>

<p class=MsoNormal>Результаты:</p>

<p class=MsoNormal align=center style='text-align:center'><img width=623
height=131 id="Рисунок 8" src="Correct_esp_wifi_80211_tx.files/image004.jpg"></p>

<p class=MsoNormal>до (вывод в монитор порта):</p>

<p class=MsoNormal align=center style='text-align:center'><img width=414
height=79 id="Рисунок 6" src="Correct_esp_wifi_80211_tx.files/image005.png"></p>

<p class=MsoNormal>после исправления:</p>

<p class=MsoNormal align=center style='text-align:center'><img width=275
height=57 id="Рисунок 7" src="Correct_esp_wifi_80211_tx.files/image006.png"></p>

<p class=MsoNormal>Как уже упоминалось, основная проверка производится в
функции <b>ieee80211_raw_frame_sanity_check</b>, которая находится в объектном
файле <b>ieee80211_output.o</b>  <span style='font-family:"Arial",sans-serif;
color:#202124;background:white'>библиотеки</span> <b>libnet80211.a, </b>её
исправление, позволит собирать последующие проекты без ограничений на тип
отправляемого пакета (кадра). </p>

<p class=MsoNormal>В моём случае, необходимо было проверить <b>возможность</b>
отправки <span lang=EN-US>deauth</span><span lang=EN-US> </span>пакетов в обход
ограничения.</p>

<p class=MsoNormal>Исправление проверено на версии ESP-IDF 4.3, модуля <span
lang=EN-US>ESP</span>-32, но я думаю, что этот вариант применим и к предыдущим
версиям (имеется ввиду обход <b>ieee80211_raw_frame_sanity_check</b>).</p>

<p class=MsoNormal>Использованные инструменты:</p>

<p class=MsoNormal><span lang=EN-US>Ghidra</span></p>

<p class=MsoNormal><span lang=EN-US>https</span>://<span lang=EN-US>ghidra</span>-<span
lang=EN-US>sre</span>.<span lang=EN-US>org</span>/</p>

<p class=MsoNormal><span lang=EN-US>ghidra</span>-<span lang=EN-US>xtensa</span></p>

<p class=MsoNormal><span lang=EN-US>https</span>://<span lang=EN-US>github</span>.<span
lang=EN-US>com</span>/<span lang=EN-US>yath</span>/<span lang=EN-US>ghidra</span>-<span
lang=EN-US>xtensa</span></p>

<p class=MsoNormal>&nbsp;</p>

</div>

</body>

</html>
