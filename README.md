# 1. Install ansible

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

# 2. Run playbook

```bash
cd roles/
git clone https://github.com/openwisp/ansible-openwisp2.git openwisp.openwisp2
git clone https://github.com/Stouts/Stouts.postfix
touch hosts playbook.yml
```

Inside playbook.yml :

```yaml
- hosts: openwisp2
  become: "{{ become | default('yes') }}"
  roles:
    - openwisp.openwisp2
  vars:
    postfix_myhostname: localhost
```

Inside hosts :

```yaml
[openwisp2]
localhost
```    

`ansible-playbook -i hosts playbook.yml -u aryaman -K --ask-pass`

# 2.1 Install ssh server (only if req)

```bash
 sudo apt-get install openssh-client openssh-server ssh-pass
 export ANSIBLE_HOST_KEY_CHECKING=False
```
# 2.2 Run influxdb on docker (only if req)

`docker-compose up -d influxdb`
 
# 2.3 For some ngnix issues (only if req)

```bash
sudo /etc/init.d/apache2 stop
sudo systemctl restart nginx
sudo fuser -k 80/tcp
sudo fuser -k 443/tcp
```

Login as root or:

```bash
sudo su -
```

#### Then uninstall nginx:

`apt-get purge nginx nginx-common nginx-full`  

#### And then install it:

`apt-get install nginx`


# 2.4 Openwisp PIP dependencies

```bash
cd /
cd opt/

sudo chown -R <user_name>:www-data openwisp2
sudo chmod -R g+s openwisp2
source env/bin/activate 
```

# NOTE : TASK [openwisp.openwisp2 : migrate] if any errors just do :

`sudo pip uninstall -r requirements.txt`	

```yaml
config controller 'http'
	option url 'https://192.168.56.1'
	option verify_ssl '0'
	option shared_secret 'wZzcfVybd3IeDLwGtk970zHQByHQLE1b'
```

# Using ngrok :

`ngrok http 127.0.0.1:8000`

```python
ALLOWED_HOSTS = [
    '*',
]

CSRF_TRUSTED_ORIGINS = ['https://19f1-103-172-72-246.ngrok.io']
```

# Method : Using /opt/openwisp2

```bash
sudo su
cd /opt/openwisp2/
source env/bin/activate
```

#### openwisp2/setting.py 

```python
ALLOWED_HOSTS = [
    '*',
]
SESSION_COOKIE_SECURE = False
CSRF_COOKIE_SECURE = False
```

`python manage.py runserver`

#### Open a new terminal window:

```bash
source env/bin/activate
celey -A openwisp2 worker -l info
```

#### Get IP from router page ex - 192.168.5.109 (In my case this one)

![Screenshot from 2022-03-25 17-15-13](https://user-images.githubusercontent.com/56113566/160115181-bc275238-daca-4e7f-b88a-1568db41e72c.png)
	
Then just change openwrt config of your VM or router

```bash
config controller 'http'
	option url 'http://192.168.5.109:8000'
	option verify_ssl '0'
	option uuid ''
	option key ''

/etc/init.d/openwisp_config restart
```

