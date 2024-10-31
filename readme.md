журнал работ в файле, там приведена подробная очередность команд и вывода
[журнал](/res/домашнее1.txt)

#################
1)
Скачайте все необходимые зависимости, использованные в проекте:

устанавливать terraform пришлось из репозитория яндекса

#################
2)
#Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд):

хранить секреты надо в файлах начинающихся на .terraform* которые будут исключены из синхронизации с Git. Либо добавить исключения.

#################
3)
Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение:

"result": "3sMMPBxMDW6kpM0s"

#################
4)
Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate.
Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их:

│ Error: Missing name for resource
нету имени ресурса!


│ Error: Invalid resource name
│ A name must start with a letter or underscore and may contain only letters, digits, underscores, and dashes.
у терраформа есть требования к синаксису имен ресурса. нужно начинать с буквы или символа _. так же нельзя использовать сиволы кроме _- букв и цифр
меняем 1nginx на nginx

│ Error: Reference to undeclared resource
│ A managed resource "random_password" "random_string_FAKE" has not been declared in the root module.
сылка на несуществующее имя. у нас в коде имя random_string. меняем random_string_FAKE на random_string. так же меняем атрибут resulT на правильный result

#################
5)
Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps :

[исправленный код](/res/исправленный_код.txt)
вывод docker ps
![docker ps](/res/dockerps.png)

#################
6)
Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа.
 Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду terraform apply -auto-approve.
 Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. 
 Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды docker ps:

При terraform apply -auto-approve нет ручного подтверждения. соответсвенно полезно при автоматическом разворачивании. например в неизменных или тестовых средах. по этой же причине не использовать для продакшена!
![docker ps](/res/dockerps_2.png)

#################
7)
Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate:

убедимся что ресурсы удалены :
список изменений нам покажут при выполнении terraform destroy. так же удобно посмотреть по выводу terraform plan после terraform destroy
[terraform.tfstate](/res/after_destroy.terraform.tfstate)

#################
8)
Объясните, почему при этом не был удалён docker-образ nginx:latest.
 Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker:

образ не был удален изза блока keep_locally = true в блоке 

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

описано в документации ресурса :
https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs/resources/image

"Optional
keep_locally (Boolean) If true, then the Docker image won't be deleted on destroy operation. If this is false, it will delete the image from the docker local storage on destroy operation."
