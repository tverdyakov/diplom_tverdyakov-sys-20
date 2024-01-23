## 1.0 Установка и подготовка Terraform и Ansible.
### 1.1 Установка и подготовка Terraform.
Скачиваю и распаковываю последнюю стабильную версию на 23.01.2024 с сайта: https://hashicorp-releases.yandexcloud.net/terraform/
```bash
wget https://hashicorp-releases.yandexcloud.net/terraform/1.7.0/terraform_1.7.0_linux_amd64.zip
zcat terraform_1.7.0_linux_amd64.zip > terraform-diplom
chmod 744 terraform-diplom
mv terraform-diplom /usr/local/bin/
terraform-diplom -version
```
![](https://github.com/tverdyakov/diplom_tverdyakov-sys-20/blob/main/01_Установка%20и%20подготовка%20Terraform%20и%20Ansible/screenshots/01.png)
Создаю файл `.terraformrc` и добавляю блок с источником, из которого будет устанавливаться провайдер.
```bash
nano ~/.terraformrc
```
```terraform
provider_installation {
  network_mirror {
    url = "https://terraform-mirror.yandexcloud.net/"
    include = ["registry.terraform.io/*/*"]
  }
  direct {
    exclude = ["registry.terraform.io/*/*"]
  }
}
```
![](https://github.com/tverdyakov/diplom_tverdyakov-sys-20/blob/main/01_Установка%20и%20подготовка%20Terraform%20и%20Ansible/screenshots/02.png)
Для файла с метаданными, `meta.yaml`, необходим публичный SSH-ключ для доступа к ВМ. Для Yandex Cloud рекомендуется использовать алгоритм Ed25519: сгенерированные по нему ключи — самые безопасные. Ссылка: https://cloud.yandex.ru/ru/docs/glossary/ssh-keygen
```bash
ssh-keygen -t ed25519
```
![](https://github.com/tverdyakov/diplom_tverdyakov-sys-20/blob/main/01_Установка%20и%20подготовка%20Terraform%20и%20Ansible/screenshots/03.png)
Создаю файл `meta.yaml` с данными пользователя на создаваемые ВМ.
```bash
nano ~/meta.yaml
```
```terraform
#cloud-config
 users:
  - name: tverdyakov
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-ed25519
```
![](https://github.com/tverdyakov/diplom_tverdyakov-sys-20/blob/main/01_Установка%20и%20подготовка%20Terraform%20и%20Ansible/screenshots/04.png)
Создаю `playbook Terraform` c блоком провайдера.
```bash
nano ~/main.tf
```
```terraform
terraform {
  required_providers {
    yandex = {
      source = "yandex-cloud/yandex"
    }
  }
}
```
![](https://github.com/tverdyakov/diplom_tverdyakov-sys-20/blob/main/01_Установка%20и%20подготовка%20Terraform%20и%20Ansible/screenshots/05.png)
Инициализирую провайдера.
```bash
terraform-diplom init
```
![](https://github.com/tverdyakov/diplom_tverdyakov-sys-20/blob/main/01_Установка%20и%20подготовка%20Terraform%20и%20Ansible/screenshots/06.png)
#### Terraform готов к использованию. Продолжение в основной части.

---
### 1.2 Установка и подготовка Ansible.
Устанавливаю Ansible и проверяю версию.
```bash
apt install ansible
ansible --version
```
![](https://github.com/tverdyakov/diplom_tverdyakov-sys-20/blob/main/01_Установка%20и%20подготовка%20Terraform%20и%20Ansible/screenshots/07.png)
Создаю полностью прокомментированный пример `ansible.cfg` и заменяю содержимое файла на необходимые опции. Файл прикреплю в основной части.
```bash
ansible-config init --disabled > ansible.cfg
nano ~/ansible.cfg
```
![](https://github.com/tverdyakov/diplom_tverdyakov-sys-20/blob/main/01_Установка%20и%20подготовка%20Terraform%20и%20Ansible/screenshots/08.png)
Создаю файл `hosts` и добавляю в него начальные данные. Файл прикреплю в основной части.
```bash
nano ~/hosts
```
#### Ansible готов к использованию. Продолжение в основной части.

[Ссылка на основную часть дипломной работы.](https://github.com/tverdyakov/diplom_tverdyakov-sys-20/blob/main/02_Основная%20часть%20дипломной%20работы/README.md)
---
