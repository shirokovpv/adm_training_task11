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
<p>Узнаем нужные нам параметры с помощью команды vagrant ssh-config</p>
<img width="811" height="325" alt="image" src="https://github.com/user-attachments/assets/3b4b53a6-a8a0-44c0-975d-79fd507cda24" />
<p>&nbsp;</p>
<p><span style="font-weight: 300;">Создадим свой первый inventory файл ./staging/hosts</span></p>
<p><span style="font-weight: 300;">Со следующим содержимым:</span></p>
<p><span style="font-weight: 400;">nginx ansible_host=127.0.0.1 ansible_port=2222 ansible_user=vagrant ansible_private_key_file=.vagrant/machines/nginx/virtualbox/private_key</span></p>
<img width="811" height="93" alt="image" src="https://github.com/user-attachments/assets/f97bf05e-9676-45e9-9997-974b9aa8a7b8" />
<p>&nbsp;</p>
<p><span style="font-weight: 300;">Убедимся, что Ansible может управлять нашим хостом. Сделать это можно с помощью команды ansible nginx -i staging/hosts -m ping</span></p>
<img width="811" height="205" alt="image" src="https://github.com/user-attachments/assets/f162457e-5b4e-496a-abbb-c02f0b5f972b" />
<p>&nbsp;</p>
<p><span style="font-weight: 300;">Как видно, нам придется каждый раз явно указывать наш&nbsp;inventory-файл и вписывать в него много информации. Это можно обойти, используя </span><span style="font-weight: 300;">ansible.cfg </span><span style="font-weight: 300;">файл - прописав конфигурацию в нем.&nbsp;</span><span>Для этого в текущем каталоге создадим файл </span><span>ansible.cfg</span><span> со следующим содержанием:</span></p>
<p><span style="font-weight: 400;">[defaults]</span></p>
<p><span style="font-weight: 400;">inventory = staging/hosts</span></p>
<p><span style="font-weight: 400;">remote_user = vagrant</span></p>
<p><span style="font-weight: 400;">host_key_checking = False</span></p>
<p><span style="font-weight: 400;">retry_files_enabled = False</span></p>
<img width="496" height="169" alt="image" src="https://github.com/user-attachments/assets/f9561873-ba43-4b8c-a940-40169b44e68c" />
<p>&nbsp;</p>
Теперь из инвентори можно убрать информацию о пользователе:
nginx ansible_host=127.0.0.1 ansible_port=2222 ansible_private_key_file=.vagrant/machines/nginx/virtualbox/private_key
<img width="807" height="95" alt="image" src="https://github.com/user-attachments/assets/e1432085-f12f-448f-be7c-e85ae70b537b" />
<p>&nbsp;</p>
<p><span style="font-weight: 300;">Еще раз убедимся, что управляемый хост доступен, только теперь без явного указания inventory-файла:</span></p>
<img width="613" height="206" alt="image" src="https://github.com/user-attachments/assets/f118d338-c213-432c-a49c-1cd3b61fd64c" />
<p>&nbsp;</p>
<p><span style="font-weight: 300;">Теперь мы можем конфигурировать наш хост. Для начала воспользуемся Ad-Hoc командами и выполним некоторые удаленные команды на нашем хосте. Посмотрим, какое ядро установлено на хосте:</p>
<img width="683" height="96" alt="image" src="https://github.com/user-attachments/assets/c7d7ef4b-03d1-4946-bdff-767cc6776935" />
<p>&nbsp;</p>
<p><span style="font-weight: 300;">Проверим статус сервиса firewalld:</span></p>
<img width="735" height="621" alt="image" src="https://github.com/user-attachments/assets/63095394-81a2-432e-9bc3-2484ea314612" />
<p>&nbsp;</p>


