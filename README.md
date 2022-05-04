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

#### Access Credentials (example):

##### Public Key:

```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDbts/CuWhRXL+TS+xKRqpnUDCfBTsq3UNgLXIAZRA08fXnBYAOpzY5pvFkHDHQnxIVxpdR8wCusqrt3G+Dm2wAr15BhtZHhXLlVeggP+Z2PrR26HZojIFAgBSV4Kqx5ti3oK/ZrPTb88Q0dwJ5YL0wtiYTxLv0V6uIOnLE0ii5XZOChr2BFxK3S6TELfoijOQ6ifl99DDZN4m8FdKhuLIazBE+v7s1xL0G8mn/wL/WZzGUlWNZi7KrabGAfSjTtbvSDkJlSaoeF6vA5B5f9EgRK3U5cbH8W2F36KwdpxnGB3yktXmJfXZWHfMo9uO0cJVeURuujdgRjpzvXzfQmBbl1mZmcRefC601SieJ0LVKMM9u4kIqPVlU8kcCXyjX43V+tyw9sr0V/1ukgEbWk5wXtJuWG/yvhcDcLO+exYW/A6zkgjWeVkrosJcWFVxCHYmxVDH67ltrS0wu/ifsP5HV8dur+cety5dXmUA62PN+RbhdhkkNoGzB8+S+RpzGbckAlx0l0AbIo5Fde0KWCiz2nnL+SVFrcPYrR9WL0XteDpKoYXPm9/mo3qUYm0XdxBHFHRWw85dCpmw2PWvLfpxDnUyCVrPh926wxCCBnAZHvu42q2FBPZ6OoKZWjrB4t0awiWXWgt4j8OC4ZKyU3jhE631UxSuMfiaZ0l66rn8T+Q== ansible-generated on aryaman-desktop
```

##### Private Key:

```bash
-----BEGIN OPENSSH PRIVATE KEY-----
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
QFBg==
-----END OPENSSH PRIVATE KEY-----
```

