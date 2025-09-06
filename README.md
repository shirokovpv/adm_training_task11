# adm_training_task11
<h1 align="center">Занятие 11. Первые шаги с Ansible</h1>
<h3 class="western"><a name="_heading=h.h6i87lkp3f19"></a> <span style="font-family: Roboto, serif;"><span style="font-size: small;">Описание домашнего задания</span></span></h3>
<p><span style="font-weight: 300;">Подготовить стенд на Vagrant как минимум с одним сервером. На этом сервере, используя Ansible необходимо развернуть nginx со следующими условиями:</span></p>
<ul>
<li style="font-weight: 300;"><span style="font-weight: 300;">необходимо использовать модуль yum/apt</span></li>
<li style="font-weight: 300;"><span style="font-weight: 300;">конфигурационный файлы должны быть взяты из шаблона jinja2 с переменными</span></li>
<li style="font-weight: 300;"><span style="font-weight: 300;">после установки nginx должен быть в режиме enabled в systemd</span></li>
<li style="font-weight: 300;"><span style="font-weight: 300;">должен быть использован notify для старта nginx после установки</span></li>
<li style="font-weight: 300;"><span style="font-weight: 300;">сайт должен слушать на нестандартном порту - </span><strong>8080</strong><span style="font-weight: 300;">, для этого использовать переменные в Ansible</span></li>
</ul>
<h3 class="western"><a name="_heading=h.df570rpzx1qg"></a><span style="font-family: Roboto, serif;"><span style="font-size: small;">Используемые ОС</span></span></h3>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Хостовая ОС Ubuntu 24.04 Desktop. Гостевая ОС Ubuntu 22.04. VirtualBox версия 7.0.16_Ubuntu r162802</span></span></p>
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<h3 class="western"><span style="font-family: Roboto, serif;"><span style="font-size: small;">Выполнение</span></span></h3>
<p dir="auto">Ввиду невозможности в нынешних реалиях воспользоваться стандартными репозиториями Vagrant (в РФ они сейчас не доступны), предложено обходное решение. Использован репозиторий, развернутый на&nbsp;<a href="https://vagrant.elab.pro/" rel="nofollow">https://vagrant.elab.pro/</a></p>
<p dir="auto">Так как официальный пакет последней версии Vagrant также не доступен для скачивания, пакет взят оттуда же. Версия 2.3.5.</p>
<img width="411" height="88" alt="image" src="https://github.com/user-attachments/assets/96c1b2d6-ac2b-47c1-a0ac-165646da94c3" />
<p>&nbsp;</p>
<p dir="auto">Для подключения репозитория надо добавить в Vagrant-файл строку:</p>
<p dir="auto"><code>ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'</code></p>
<p dir="auto">И изменить box_version (как в репозитории, если туда зайти).</p>
<img width="247" height="410" alt="image" src="https://github.com/user-attachments/assets/bfea51fd-1044-4f90-8399-6c2b43c83879" />
<p>&nbsp;</p>
<p dir="auto">Версия ansible:</p>
<img width="810" height="317" alt="image" src="https://github.com/user-attachments/assets/c46fa86b-bcd8-40f2-8770-e6202c921eda" />
<p>&nbsp;</p>
<p dir="auto">Создадим каталог ansible и положим в него Vagrant-файл, взятый с https://drive.google.com/file/d/17MEtg20TFSjKil6ih7PvPez7jmCvo6fb/view?usp=share_link</p>
<p>Изменим Vagrant-файл, как написано выше, также изменим параметр box_name => "ubuntu/22.04", как в репозитории. Измененный Vagrant-файл прикладываю сюда.</p>
<p>Установим ВМ с помощью команды vagrant up</p>
<p>shirokovpv@SPB300:~/ansible$ vagrant up<br />Bringing machine 'nginx' up with 'virtualbox' provider...<br />==&gt; nginx: Box 'ubuntu/22.04' could not be found. Attempting to find and install...</p>
<p>...</p>
<p>==&gt; nginx: Running provisioner: shell...<br /> nginx: Running: inline script<br />shirokovpv@SPB300:~/ansible$ </p>
<p>Видим, что ВМ установлена и запущена:</p>
<img width="1092" height="410" alt="image" src="https://github.com/user-attachments/assets/0cf0466d-a723-4536-91f9-48a5a054967e" />
<p>&nbsp;</p>
<p>Подключаемся к ВМ с помощью команды vagrant ssh</p>
<img width="768" height="666" alt="image" src="https://github.com/user-attachments/assets/a39caf89-fa11-4aa1-bbc3-b4ccdbc85c93" />
<p>&nbsp;</p>
<p>Все работает.</p>
