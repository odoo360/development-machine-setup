# install on-primises
follow https://www.odoo.com/documentation/19.0/administration/on_premise/packages.html

###  db
```
 sudo apt install postgresql -y
```

### app
```
 wget -q -O - https://nightly.odoo.com/odoo.key | sudo gpg --dearmor -o /usr/share/keyrings/odoo-archive-keyring.gpg

 echo 'deb [signed-by=/usr/share/keyrings/odoo-archive-keyring.gpg] https://nightly.odoo.com/19.0/nightly/deb/ ./' | sudo tee /etc/apt/sources.list.d/odoo.list

 sudo apt-get update && sudo apt-get install odoo

```

### update linux & odoo
```
 sudo apt update
 apt-get upgrade
```

### first run 
First run will guide you through database creation (master password + admin user)
- Odoo runs as a systemd service (odoo)
- Default config file → /etc/odoo/odoo.conf
- Default port → 8069
- You can reach it at http://your-server-ip:8069


# install on local  
follow https://www.odoo.com/documentation/19.0/administration/on_premise/source.html
