Инициализировать box.
vagrant init ubuntu/xenial64

Проверить файл Vagrantfile.
vagrant validate

Создать вирт машину.
vagrant up Vagrantfile

Установить плагин.
vagrant plugin install my-plugin

Посмотреть глобальный статус.
vagrant global-status

Соединиться по ssh с машиной в текущей директории.
vagrant ssh

Соединиться по ssh с машиной через имя.
vagrant ssh d1e80fa     

Пауза.
vagrant suspend 1f2a1a5
vagrant resume ubuntu1

Вкл/выкл.
vagrant halt 1f
vagrant up 1f

Перезагрузка.
vagrant reload

Удалить машину.
vagrant destroy d1e80fa

Посмотреть ssh конфигурацию.
vagrant ssh-config

Работа со снапшотами.
vagrant snapshot save 0f test
vagrant snapshot restore 0f test
vagrant snapshot delete 0f test

Скопировать файл в виртуальную машину.
vagrant upload source_file.txt dst_file.txt 0f

Обновление образов.
vagrant box update

Файл Vagrant
Имя в vagrant global-status.
config.vm.define "ubuntu1"   
Имя в VirtualBox.
config.vm.provider "virtualbox" do |v|
v.name = "ubuntu1"
Имя образа.
vagrant2.vm.box = "ubuntu/trusty64"
Проброс портов.
vagrant2.vm.network "forwarded_port", guest: 80, host: 8081
или
config.vm.network "forwarded_port", guest: 8080, host: 8080

Добавить физический адаптер и статический адрес.
config.vm.network "public_network",bridge: "enp2s0" , ip: "192.168.100.240"

Скопировать при развертывании.
config.vm.synced_folder "test/" , "/var/www/test"

Установить пакет при развертывание.
config.vm.provision "shell", inline: <<-SHELL
sudo apt-get update >/dev/null 2>&1 
sudo apt-get install -y mc >/dev/null 2>&1 
SHELL
или через скрипт
config.vm.provision "shell", path: "install.sh"

Использовать статический ssh ключ. (/home/user/.vagrant.d/insecure_private_key)
config.ssh.insert_key = false

Прописать маршрут по умолчанию.
inline: "ip route add default via 172.16.10.1"

Установить значение для ОЗУ.
config.vm.provider "virtualbox" do |vb|
vb.memory = "2048"
end
