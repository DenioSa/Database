# Домашнее задание к занятию "`Отказоустойчивость в облаке`" - `Сайфиев Денис`


### Задание 1

Возьмите за основу решение к заданию 1 из занятий «Подъём в Яндекс Облаке» .

Теперь вместо одного устройства машины создайте сборник сценариев terraform, который:
создаст 2 определённые виртуальные машины. Используйте аргумент count для создания таких ресурсов;
создатель таргет-группы . Поместите в нее созданные на шаге 1 виртуальные машины;
создаст сетевой балансировщик нагрузки , который прослушивает порт 80, отправляет трафик на порт 80 виртуальных машин и http Healthcheck на порт 80 виртуальных машин.
Рекомендуем изучить документацию нагрузки на сетевой балансировщик , чтобы было понятно, что вы сделали.

Установите созданный виртуальный пакет Nginx любым удобным способом и запустите веб-сервер Nginx на порту 80.

Перейдите в веб-консоль Yandex Cloud и убедитесь, что:

созданный балансировщик находится в статусе Активный,
обе виртуальные машины в закрытой группе находятся в работоспособном состоянии.
Сделайте запрос на 80 порт на внешний IP-адрес балансировщика и убедитесь, что вы продолжаете отвечать в видео дефолтной страницы Nginx.
В качестве результата пришли:

1. Пособие по терраформированию.

2. Скриншот привода балансировщика и выключите группу.

3. Скриншот страницы, которая открылась при запросе IP-адреса балансировщика.

### Решение 1

1. main.tf
```tf
 terraform {
  required_version = "= 1.8.3"
 
  required_providers {
    yandex = {
      source = "yandex-cloud/yandex"

    }
  }
}

variable "yandex_cloud_token" {
  type = string
  description = "При запуске terraform plan/apply требуется ввести секретный токен"
}

provider "yandex" {
  token     = var.yandex_cloud_token 
  cloud_id  = "ab1g0po3fak0dkf0j026i"
  folder_id = "b1gnlq4ka26sn0occfcv"
  zone      = "ru-central1-a"
}

resource "yandex_compute_instance" "vm" {
  count = 2
  name = "vm${count.index}"
  platform_id = "standard-v1"

  boot_disk {
    initialize_params {
      image_id = "fd81mpc969gbg44vndkv"
      size = 5       
    }
  } 

  network_interface {
    subnet_id = yandex_vpc_subnet.subnet-1.id
    nat       = true
  }

  resources {
    core_fraction = 5
    cores  = 2
    memory = 2
  }
  
  metadata = {
    user-data = file("./meta.yml")
  }

}
resource "yandex_vpc_network" "network-1" {
  name = "network1"
}

resource "yandex_vpc_subnet" "subnet-1" {
  name           = "subnet1"
  zone           = "ru-central1-a"
  network_id     = "${yandex_vpc_network.network-1.id}"
  v4_cidr_blocks = ["192.168.10.0/24"]
}

resource "yandex_lb_network_load_balancer" "lb-1" {
  name = "lb-1"
  deletion_protection = "false"
  listener {
    name        = "my-lb1"
    port        = 80
    external_address_spec {
      ip_version = "ipv4"
    }
  }
  attached_target_group {
    target_group_id = yandex_lb_target_group.test-1.id 
    healthcheck {
      name = "http"
      http_options {
        port = 80
        path = "/"
      }
    }
  }
}

resource "yandex_lb_target_group" "test-1" {
  name      = "test-1"
  target {
    subnet_id = yandex_vpc_subnet.subnet-1.id
    address   = yandex_compute_instance.vm[0].network_interface.0.ip_address
  }
  target {
    subnet_id = yandex_vpc_subnet.subnet-1.id
    address   = yandex_compute_instance.vm[1].network_interface.0.ip_address
  }
}
```
1.2. meta.yml
```yml
cloud-config
disable_root: true
timezone: Europe/Moscow
repo_update: true
repo_upgrade: true
apt:
  preserve_sources_list: true

packages:
  - nginx

runcmd:
  - [ systemctl, nginx-reload ]
  - [ systemctl, enable, nginx.service ]
  - [ systemctl, start, --no-block, nginx.service ]
  - [ sh, -c, "echo $(hostname | cut -d '.' -f 1 ) >> /var/www/html/index.html" ]

users:
  - name: denio
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa  
```
2.![балансировщик и целевая группа](https://github.com/DenioSa/O-O/blob/b7785ed35011af87ad441f6229d73d4c2c626c0f/img/capture_20240525001207341.bmp))
  ![балансировщик и целевая группа 2](https://github.com/DenioSa/O-O/blob/d9417539b1b85e3352a1334b2ec9ce95d9cc2dd6/img/capture_20240525001804514.bmp))
  
3.![IP-адреса балансировщика](https://github.com/DenioSa/O-O/blob/d9417539b1b85e3352a1334b2ec9ce95d9cc2dd6/img/capture_20240525010759008.bmp))`


---

