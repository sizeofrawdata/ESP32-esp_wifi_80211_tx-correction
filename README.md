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
пакета (кадра), в функцию <b>esp_wifi_80211_tx, </b>а я решил исправить это
ограничение.</p><p>В данном примере рассматривается <b>ESP-IDF 4.3.</b></p>

<p class=MsoNormal>Для того чтобы обойти это ограничение, необходимо исправить
готовый образ или исполняемый файл <span lang=EN-US>elf</span><span lang=EN-US>
</span>вашего проекта.</p>

<p class=MsoNormal>Необходимо найти и исправить (*<i>на ваше усмотрение</i>) последовательность опкодов:</p>

  <img align="center" src="https://github.com/sizeofrawdata/ESP32-esp_wifi_80211_tx-correction/blob/main/1.png"  alt="1">
  
<p class=MsoNormal>так:</p>
  
  <img align="center" src="https://github.com/sizeofrawdata/ESP32-esp_wifi_80211_tx-correction/blob/main/2.png"  alt="1">

<p></p>
<p></p>
<p>или просто:</p>

  <img align="center" src="https://github.com/sizeofrawdata/ESP32-esp_wifi_80211_tx-correction/blob/main/3.png"  alt="1">



<p class=MsoNormal>Для получения образа прошивки (для последующей загрузки в
память <span lang=EN-US>ESP</span>-32) используется утилита <span lang=EN-US>esptool</span>
с параметрами:</p>

<p class=MsoNormal><span lang=EN-US><b>esptool.py --chip esp32 elf2image
my_esp32_app.elf</b></span></p>

<p class=MsoNormal>Результаты:</p>

  <img align="center" src="https://github.com/sizeofrawdata/ESP32-esp_wifi_80211_tx-correction/blob/main/4.png"  alt="1">

<p class=MsoNormal>до (вывод в монитор порта):</p>

  <img align="center" src="https://github.com/sizeofrawdata/ESP32-esp_wifi_80211_tx-correction/blob/main/5.png"  alt="1">

<p class=MsoNormal>после исправления:</p>

  <img align="center" src="https://github.com/sizeofrawdata/ESP32-esp_wifi_80211_tx-correction/blob/main/6.png"  alt="1">

<p class=MsoNormal>Как уже упоминалось, основная проверка производится в
функции <b>ieee80211_raw_frame_sanity_check</b>, которая находится в объектном
файле <b>ieee80211_output.o</b>  <span style='font-family:"Arial",sans-serif;
color:#202124;background:white'>библиотеки</span> <b>libnet80211.a, </b>её
исправление, позволит собирать последующие проекты без ограничений на тип
отправляемого пакета (кадра). </p>

<p class=MsoNormal>В моём случае, необходимо было <b>проверить</b> возможность
отправки <span lang=EN-US>deauth</span><span lang=EN-US> </span>пакетов в "обход"
ограничения.</p>

<p class=MsoNormal>Исправление проверено на версии <b>ESP-IDF 4.3</b>, модуля <span
lang=EN-US><b>ESP</span>-32</b>, но я думаю, что этот вариант применим и к предыдущим
версиям (имеется ввиду обход <b>ieee80211_raw_frame_sanity_check</b>).</p>

<p class=MsoNormal>Использованные инструменты:</p>

<p><a href="https://ghidra-sre.org/" target="_blank">Ghidra</a></p>   
<p><a href="https://github.com/yath/ghidra-xtensa" target="_blank">ghidra-xtensa</a></p>  
<p class=MsoNormal>&nbsp;</p>

  
<p class=MsoNormal>прим. *<i>на ваше усмотрение</i> – имеются ввиду варианты логики</p>  
</div>

</body>

</html>
