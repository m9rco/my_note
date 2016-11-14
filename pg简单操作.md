# 备份和导入

```

pg_restore -h localhost -p 5000 -U tt67wq -W -d odoo_own -v "/Users/tt67wq/Documents/odoo_own.backup"

pg_dump -h 120.27.152.95 -p 5432 -U root -W -F c -b -v -f "/Users/tt67wq/Documents/odoo_own.backup" odoo_own

```
