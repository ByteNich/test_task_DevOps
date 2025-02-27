## Структура проекта
```
test_task_DevOps/
├── app.py                  # Код Flask-приложения
├── Dockerfile              # Конфигурация Docker-образа
├── .github/
│   └── workflows/
│       └── cicd.yml        # CI/CD-конвейер для GitHub Actions
├── ansible/
│   ├── inventory.yml       # Инвентарь для Ansible
│   ├── playbook.yml        # Playbook для развертывания
│   └── roles/
│       └── deploy_app/
│           ├── tasks/
│           │   └── main.yml  # Задачи роли
│           └── vars/
│               └── main.yml  # Переменные роли
├── README.md
```

## Установка и настройка

### Требования
- Установлены: Python 3.9, Docker, Ansible, Git.
- Аккаунт на Docker Hub.
- Доступ к удалённому серверу с root-доступом через SSH.

### Локальная настройка
1. **Клонируйте репозиторий:**
   ```bash
   git clone https://github.com/trsnich/test_task_DevOps.git
   cd test_task_DevOps
   ```

2. **Создайте виртуальное окружение и установите зависимости:**
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   pip install flask
   ```

3. **Запустите приложение локально:**
   ```bash
   python app.py
   ```
   Откройте `http://localhost` в браузере.

### Создание Docker-образа
1. **Соберите образ:**
   ```bash
   docker build -t trsnich/my-python-app:latest .
   ```

2. **Опубликуйте на Docker Hub:**
   ```bash
   docker login -u <your_login>
   docker push <your_login>/my-python-app:latest
   ```

### Настройка CI/CD
- Файл `.github/workflows/cicd.yml` настроен для автоматической сборки и публикации образа.
- В GitHub добавьте секреты:
  - `DOCKER_USERNAME`: `<your_login>`
  - `DOCKER_PASSWORD`: ваш пароль или токен Docker Hub.

### Развертывание с Ansible
1. **Настройте SSH:**
   ```bash
   ssh-keygen -t rsa -b 4096 -C "ваш_email@example.com"
   ssh-copy-id -i ~/.ssh/id_rsa root@45.15.157.202
   ```

2. **Настройте инвентарь (`ansible/inventory.yml`):**
   ```yaml
   ---
   all:
     hosts:
       web_servers:
         ansible_host: <server IP>
         ansible_user: <name_user>
         ansible_ssh_private_key_file: <path>/.ssh/id_rsa
   ```

3. **Запустите playbook:**
   ```bash
   ansible-playbook -i inventory.yml playbook.yml
   ```

4. **Проверка:**
   Откройте `http://<server IP>` в браузере.
Вы должны увидеть: 
```html
Добро пожаловать в мое веб-приложение!
```
